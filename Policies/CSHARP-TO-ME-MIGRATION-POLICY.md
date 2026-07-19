<p align="left">
  <img src="..\..\..\MnMLogo.png" alt="MachineEnglish logo" width="48" />&nbsp;&nbsp;&nbsp;<strong>MachineEnglish(ME) - a language of intent</strong>
</p>

> **Path Authority:** Repository file/folder paths are maintained only in [Document_Index_Page.md](Document_Index_Page.md) and [Document_Index_Page_Internal.md](Document_Index_Page_Internal.md). Do not rely on path literals in this document.


**Authority:** This document is the canonical source for all C# → ME migration rules.
Tooling decisions (DB-01..DB-04) and structural decisions about where migration knowledge
lives are in `see Document_Index_Page.md`.

---

## How to use this

When migrating a C# codebase:
1. Read Parts 1–4 end-to-end before writing any `.me` files.
2. Create a project-specific classification table that applies these rules to every type in scope.
3. Cite rule numbers (`R-001`, `SX-003`) in the classification table - do not re-explain them inline.
4. Record project-agnostic discoveries in Part 4 (Lessons Learned Accumulator).

---

# Part 1 - Type Mapping Rules

### R-001 - C# `interface` → ME `spec`

A C# interface becomes a ME `spec`. Spec inheritance uses `from`.

```me
// C#
public interface IFoo : IBar { int X { get; } }

// ME
public spec IFoo from IBar [
    X by integer
]
```

### R-002 - `internal interface` → internal spec in an appropriate namespace

Every `internal` C# interface becomes an `internal spec`. The target namespace follows
the same residency test as public specs. No internal interface is discarded or left as an
undeclared pattern.

### R-003 - Marker interface (no members) → ME zero-member tag spec

An interface with no members is a marker. In ME this becomes a zero-member spec used
with `from` wherever the marker is inherited.

```me
// C#
public interface IImageContext { }

// ME
public spec IImageContext [ ]
```

### R-004 - get-only property → ME `by` computed field

```me
// C#
int X { get; }

// ME
X by integer
```

### R-005 - get + set property → ME stored field

```me
// C#
int X { get; set; }

// ME
X as integer
```

### R-006 - set-only property → ME field with `expose set`

```me
// C#
IContext BackgroundContext { set; }

// ME (in entity)
BackgroundContext as Context
expose set
```

### R-007 - Readonly/immutable contract → ME `fix` field or `by` computed

If immutable-after-construction: `fix`. If always computed from other state: `by`.

### R-008 - Generic interface `IFoo<T>` → ME generic spec `of T`

```me
// C#
public interface IFoo<T> { T Value { get; } }

// ME
public spec Foo of T [
    Value by T
]
```

### R-009 - Interface method → ME callable in spec (no body)

```me
// C#
int ReadPixel(int x, int y);

// ME
ReadPixel [x as integer, y as integer] be integer
```

### R-010 - `out` parameter mapping

See `ADR-017-CSharpToME-Migration-Policy.md` - DB-01 (Resolved).

### R-011 - `IntPtr` / native handle → `ME.Interop`

`IntPtr` means native interop. In ME, this surfaces as an `extern` callable or an opaque
handle type in `ME.Interop`. Never part of a public application API.

### R-012 - Collection interfaces → ME.Collections or relevant stdlib namespace

Foundational collection abstractions that any ME project would use belong in `ME.Collections`.
When a spec inherits a collection spec, translate `IDisposable` and `IEnumerable` base types
through lifecycle/iteration policy (R-013).

### R-013 - `IDisposable`, `ICloneable`, `IEquatable<T>`, `IComparable<T>` → compiler-recognized capability contracts

These are not ordinary interface renames. Each carries language-level meaning in ME: disposal
lifecycle, copy/clone semantics, equality semantics, and ordering semantics respectively.
The ME compiler and runtime will use these contracts to select behavior for `temp` bindings,
copy operations, collection lookups, and sorting.

**Do not discard or flatten these interfaces during migration.** The ME capability contracts
are locked in MD-003, and `x.type` projection is available via MD-004 for typed self-contract
recovery. Migration should emit the ME capability contract when the source mapping is
mechanically clear, and emit a `ManualReview` diagnostic when source behavior is ambiguous.

> **See:** `see Document_Index_Page.md`
> for the architectural decision and migration consequence.

MD-003 is the source of truth for the exact ME callable/operator spelling and compiler
behavior of these capability contracts. Migration tooling follows MD-003; when a source
pattern cannot be safely mapped, migration must preserve intent with a `ManualReview`
diagnostic rather than inventing a local rule.

### R-014 - `#if` compile flags → ME CCC

See `ADR-017-CSharpToME-Migration-Policy.md` - DB-04 (Resolved) for full output shape decision.

```me
// C#
#if NoObjectLimit
using gint = System.Int32;
#else
using gint = System.UInt16;
#endif

// ME  (.ccc file - MigrationConfig.ccc)
ccc MigrationConfig [
    fix NO_OBJECT_LIMIT as answer be no
]

// ME  (.me file, namespace level - guarding type declarations or load statements only)
if ccc.MigrationConfig.NO_OBJECT_LIMIT [
    load ME.Interop.LargeId
]
else [
    load ME.Interop.SmallId
]
```

### R-015 - `INotToBeImplementedOutsideX` → `internal spec` + module visibility

A C# convention interface like `INotToBeImplementedOutsideGWS` becomes an `internal spec`
in the target module. ME's module `internal` visibility enforces the same constraint -
external code cannot implement an `internal` spec.

### R-016 - Primitive type mapping

| C# | ME |
|---|---|
| `bool` | `truth` |
| `byte` | `tiny integer` |
| `sbyte` | `tiny integer` |
| `short` / `Int16` | `small integer` |
| `ushort` / `UInt16` | `small integer` |
| `int` / `Int32` | `integer` |
| `uint` / `UInt32` | `integer` |
| `long` / `Int64` | `big integer` |
| `float` | `real` |
| `double` | `big real` |
| `string` | `text` |
| `object` | `any` |
| `T?` (nullable) | `T optional` or not-ok state via `onfail` |
| `IntPtr` | opaque handle in `ME.Interop` (R-011) |
| `Type` (C# reflection) | deferred to ME reflection policy |

### R-017 - Inheritance chain flattening

If a C# interface exists purely to compose two other interfaces with no added members,
evaluate whether the composition is still needed in ME. Only port the composed spec if the
name carries domain meaning.

### R-018 - `IValid` pattern → ME `spec IValid` + not-ok state

`IValid` becomes `spec IValid` in `ME.Core`. Types that inherit it inherit `from IValid` in ME.
Callers use ME's not-ok state and `onfail` as the runtime mechanism; `spec IValid` is the
named contract for types that explicitly expose a validity predicate.

### R-019 - Events (`event SomeDelegate`) → ME `action` field or `on` handler

C# events become ME `on` handlers or stored `action` fields depending on whether
the event is external (user-triggered) or internal (lifecycle notification).

```me
// C#
event VoidMethod BeforeParse;

// ME (stored action field - caller wires it up)
BeforeParse as action optional
```

### R-020 - Enums → ME `choice`

C# enums become ME `choice` types. Flags enums become `choice` with flag semantics noted
in a comment. Name the `choice` for the concept, not the C# enum name verbatim unless they
match the domain vocabulary.

### R-021 - `ref` parameter mapping

See `ADR-017-CSharpToME-Migration-Policy.md` - DB-01 (Resolved).

### R-022 - `unsafe` interfaces → internal spec + `extern` callables

C# `unsafe` interfaces access unmanaged memory directly. In ME these become `internal` specs
backed by `extern` callables. They are never part of the public API.

### R-023 - Method overloads → ME named aliases or `optional` trailing params

C# method overloads (same name, different parameter lists) become either:
- **Distinct ME callables** with descriptive names incorporating the key discriminant
- **ME optional parameters** when overloads only differ by trailing defaults

```me
// C# - 3 overloads of NewBoundary
// ME - named variants
NewBoundary [kind as BoundaryKind optional] be Boundary
NewBoundaryAt [x as integer, y as integer, w as integer, h as integer] be Boundary
NewBoundaryFrom [bounds as Bounds] be Boundary
```

### R-024 - C# `struct` implementing an interface → ME `entity` inheriting `from` its spec

```me
// C#
public struct Point : IPoint, IScanPoint { public int X; public int Y; }

// ME
public entity Point from Point, ScanPoint [
    X as integer
    Y as integer
]
```

### R-025 - C# `delegate` → ME `action` type

Each named delegate becomes a ME `action` declaration in the namespace where it is primarily
used. Delegates used only as event field types stay inline as `action optional` fields.

```me
// C#
public delegate bool RenderAction(IEnumerable<IScanLine> lines);

// ME
public action RenderAction [lines as ScanLine array optional] be truth
```

### R-026 - Preserve C# type names exactly in ME declarations

The default ME declaration name is the exact C# type name. Do not strip prefixes such as
`I`, rename marker interfaces to domain aliases, or otherwise normalize names during the
mechanical migration. Any rename must be recorded in the classification table with the
original C# name, the chosen ME name, and the reason for violating R-026.

---

# Part 2 - Structural Decision Rules

### S-001 - Residency test for namespace placement

Before placing a type in a shared/stdlib namespace, apply the residency test:
> "Would a non-GWS (or non-source-domain) ME project naturally use this type?"

If yes → stdlib namespace (`ME.Core`, `ME.Collections`, etc.).
If no → domain-specific namespace.

The type's current usages do not determine its namespace. Its abstraction domain does.

### S-002 - Preservation rule - no interface is dissolved

Every C# interface gets a ME home. No interface is dissolved into a language mechanism
and left without a named ME type. The spec may be `internal` or in a module-private
namespace, but it always exists with a recorded name.

### S-003 - Every classification must record

For each C# type migrated: the target ME name, the target namespace, the rules applied,
and any notes explaining non-obvious decisions. Build a classification table.

### S-004 - Namespace layer model

Define namespace layers for the target domain before starting migration. Assign each
layer a boundary description. Assign each C# type to exactly one layer using S-001.

---

# Part 3 - ME Language Syntax Rules

These rules record ME grammar constraints discovered during migration.

### SX-001 - Spec properties must be PascalCase

ME spec property declarations must be PascalCase. Lowercase names cause SPC-001 errors.

### SX-002 - No `expose` keyword inside a spec body

`expose` is valid in entity bodies, not spec bodies. Use bare `get` or `get set` in specs.

### SX-003 - Every spec field needs an accessor

A plain field in a spec body is a compiler error (SPC-001). Specs cannot hold stored state;
every field must have `get`, `get set`, or `by`.

### SX-004 - `be T optional` is not valid return syntax in spec callables

Callable return types in specs do not support `optional` qualifier. Use `be T`.

### SX-005 - Callable names in ME are PascalCase

ME callable declarations use PascalCase names. Fields and parameters use camelCase.

### SX-006 - `action` parameters use `[]`, not bare param list

ME action types declare their signature using `[...]` notation.

### SX-007 - Multi-value return → named entity, not tuple

ME has no tuple syntax. When multiple values are returned, define a named `entity`.

---

# Part 4 - Lessons Learned Accumulator

Record project-agnostic migration discoveries here. Project-specific observations stay in
the project's classification table.

| # | Policy | Origin | Rationale |
|---|---|---|---|
| P-001 | Foundational collection contracts pass the residency test even when first discovered in a domain-specific codebase. | GWS Collections.cs | The abstraction domain determines namespace, not current usages. |
| P-002 | Marker interfaces (zero members) become zero-member ME specs unless module visibility alone is sufficient. | GWS `IImageContext` | ME module `internal` replaces many "do not implement outside X" markers. |
| P-003 | `out` and `ref` parameter mapping decision. | GWS `IPen.ReadLine` | See ADR-017 DB-01 (Resolved). |
| P-004 | `IntPtr` and raw handles stay behind `ME.Interop` or `extern` callables. Never surface in public API. | GWS `IxData` | ME FFI boundary keeps native memory opaque. |
| P-005 | `#if` compile flags in C# map to CCC. Namespace-level guards only - body-level `#if` is manual review. | GWS `ISession` | R-014, ADR-017 DB-04. |
| P-006 | `IValid` becomes `spec IValid` in `ME.Core`. ME's not-ok state and `onfail` are the runtime mechanism; the spec is the named contract. | GWS, throughout | R-018, R-026. |
| P-007 | General-purpose abstractions pass the residency test even when the only current usages are domain-bound. Ask "would a non-domain project need this?" | GWS `ISupplier<T>`, `IID<T>` | S-001 - residency is about the abstraction, not the usages. |
| P-008 | Method overloads with 3+ variants always get distinct named ME callables, not `optional` params. Optional params suit 2-overload cases only. | GWS `IFactory.newBoundary` | R-023 - named variants are more readable than long `optional` chains. |
| P-009 | When the source codebase uses British spelling (colour, behaviour), lock it throughout the migration. Inconsistency is worse than either choice. | GWS | Add a spelling policy entry to the project's classification doc. |
