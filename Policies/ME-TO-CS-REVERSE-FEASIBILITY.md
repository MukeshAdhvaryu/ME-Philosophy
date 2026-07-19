<p align="left">
  <img src="..\..\..\MnMLogo.png" alt="MachineEnglish logo" width="48" />&nbsp;&nbsp;&nbsp;<strong>MachineEnglish(ME) - a language of intent</strong>
</p>

> **Path Authority:** Repository file/folder paths are maintained only in [Document_Index_Page.md](Document_Index_Page.md) and [Document_Index_Page_Internal.md](Document_Index_Page_Internal.md). Do not rely on path literals in this document.


This document assesses how well ME declarations can be translated back into C# and
identifies where ME is richer than C#, producing lossy or approximate translations.

---

## Summary

**Overall verdict: Medium-High feasibility for structural code.**

The structural backbone of any ME codebase (specs, entities, choices, callables) maps
cleanly to C# interfaces, classes, enums, and methods. The harder cases are ME idioms
that have no C# equivalent: the failure model, CCC conditional compilation, the
capability system, and `emit`/`out` output modifiers. These require approximation.

---

## Direct mappings (lossless)

These ME constructs translate to C# with no semantic gap.

| ME | C# | Notes |
|---|---|---|
| `spec Foo` | `interface IFoo` | Add `I` prefix by convention |
| `spec Foo from Bar` | `interface IFoo : IBar` | |
| `entity Foo` | `class Foo` | |
| `entity Foo from Bar` | `class Foo : IBar` | Spec inheritance → interface implementation |
| `fix entity Foo` | `record Foo` / readonly class | `init`-only properties |
| `choice Foo` | `enum Foo` | |
| `fn Name [a as T] be R` | `R Name(T a)` | |
| `Name [a as T] be R` (spec callable) | `R Name(T a)` in interface | |
| `Name as T` (stored field) | `T Name { get; set; }` | |
| `Name by T` (computed field) | `T Name { get; }` | |
| `fix Name as T` | `T Name { get; init; }` | Or `readonly T Name` |
| `Name as T get` (spec field) | `T Name { get; }` | |
| `Name as T get set` (spec field) | `T Name { get; set; }` | |
| `of T` (generics) | `<T>` | |
| `action Foo [...]` | `delegate void Foo(...)` or `delegate R Foo(...)` | |
| `error Foo` | `class FooException : Exception` | |
| `module Foo.Bar` | `namespace Foo.Bar` | |
| `load Foo.Bar` | `using Foo.Bar` | |
| `ccc Foo` | `#if` / feature flag class | Lossy - see below |
| `decide ... when` | `switch` expression | |
| `for item in collection` | `foreach (var item in collection)` | |
| `if ... else` | `if ... else` | |
| `while`, `do ... while` | `while`, `do ... while` | |
| `try ... onfail ... always` | `try ... catch ... finally` | Approximate - see below |
| `with obj [...]` | `using (var obj = ...) { }` | |
| `await` | `await` | |

---

## Approximate mappings (lossy)

These ME constructs have no precise C# equivalent. The translation is a reasonable
approximation but loses ME-specific semantics.

### Failure model (`fails`, `throws`, not-ok state, `onfail`)

ME distinguishes:
- **`fails`** - expected, domain-relevant failure; callers must handle
- **`throws`** - unexpected/programmer error; can propagate
- **not-ok state** - value-level invalid state, not exception-based

C# has no checked-exception enforcement. The approximation:

```csharp
// ME
fn Divide [a as integer, b as integer] be integer fails DivideByZero

// C# approximation - callers are not forced to handle
/// <exception cref="DivideByZeroException">When b is zero.</exception>
int Divide(int a, int b)
```

The `onfail` handler becomes `catch`. Not-ok states that aren't exception-based become
`bool`-returning or `(T? result, bool ok)` tuple patterns.

**Loss:** ME's enforcement that callers handle `fails` channels is completely lost. C# has
no static analysis for this without external tooling.

### CCC (`ccc Config`, `if ccc.Config.Flag [...]`)

CCC is ME's compile-time configuration system - richer than `#if` because it is typed
(a real `entity`), can hold values, and can condition arbitrary declarations including
type members.

```csharp
// ME
ccc Config [
    EnableAdvancedMode as truth be false
]

if ccc.Config.EnableAdvancedMode [
    fn AdvancedOp [] be Result
]

// C# approximation 1 - preprocessor (loses the typed value)
#if ENABLE_ADVANCED_MODE
int AdvancedOp() { ... }
#endif

// C# approximation 2 - feature flag (runtime, not compile-time)
public static class Config {
    public static bool EnableAdvancedMode { get; set; } = false;
}
// conditionally registered in DI or called with runtime guard
```

**Loss:** C# `#if` flags are stringly-typed. Feature flag libraries (LaunchDarkly,
Microsoft.FeatureManagement) are runtime, not compile-time. Neither perfectly captures
CCC's typed, compile-time conditional declarations.

### Capability system

ME capabilities describe cross-cutting behaviour that affects multiple types (like a
mixin/trait). C# has no direct equivalent.

```csharp
// ME
capability Logging affects Order, Customer [
    LogAction [message as text]
]

// C# approximation - interface + extension methods
interface ILoggable {
    void LogAction(string message);
}
// Order and Customer implement ILoggable
// Extension methods provide default behaviour via a base class or composition
```

**Loss:** ME capabilities can carry state and have precise "affects" declarations that
are checked by the compiler. C# interfaces require explicit implementation on each type.

### `emit` / `out` output modifiers

ME's `emit` and `out` mark specific output values in multi-return callables. The `emit`
modifier allows a callable to "emit" values to a collection without explicit accumulation.

```csharp
// ME
fn Scan [bounds as Bounds] be ScanLine emit

// C# approximation - IEnumerable<T> with yield return
IEnumerable<ScanLine> Scan(Bounds bounds) {
    // yield return each scan line
}
```

**Loss:** `emit` in ME is a first-class language concept that the type system tracks.
`IEnumerable<T>` is a library pattern; the compiler does not enforce the same semantics.

### Spec callables with body (ME default implementations)

ME allows spec callables to have bodies (default implementations). C# interfaces also
support default interface methods (since C# 8), so this maps reasonably well.

```csharp
// ME
public spec Countable [
    Count by integer
    IsEmpty by truth [
        be Count equals 0
    ]
]

// C# - default interface method
public interface ICountable {
    int Count { get; }
    bool IsEmpty => Count == 0;  // default implementation
}
```

**Fidelity:** High - C# 8+ default interface methods are semantically equivalent.

---

## Hard cases (manual migration required)

These ME constructs have no viable mechanical C# translation.

### Module `internal` visibility enforcement

In ME, `internal` specs cannot be implemented outside their module. C# `internal` is
assembly-scoped; there is no package-level visibility. Enforcement relies on
`InternalsVisibleTo` for tests, which is already avoided in this project.

**C# approximation:** Use `internal` + a sealed implementation class. Document the
contract manually. Cannot be mechanically enforced.

### Not-ok propagation chains

ME's not-ok state propagates through call chains without explicit exception raising. The
not-ok value travels as data, not as a stack-unwinding exception.

**C# approximation:** Result<T> pattern (`(T? Value, bool Ok, Error? Error)` or a
`Result<T>` library type). Requires every caller to explicitly unwrap.

**Loss:** ME's propagation is implicit and composable; C# requires explicit `if (!result.Ok) return ...` at every call site.

---

## Reverse migration verdict by ME construct

| ME construct | C# equivalent | Fidelity |
|---|---|---|
| `spec` | `interface` | ✅ High |
| `entity` | `class` / `record` | ✅ High |
| `choice` | `enum` | ✅ High |
| `error` | Exception subclass | ✅ High |
| `action` | `delegate` | ✅ High |
| `fn` callable | method | ✅ High |
| Generics (`of T`) | `<T>` | ✅ High |
| `fix` fields | `readonly` / `init` | ✅ High |
| `decide ... when` | `switch` expression | ✅ High |
| `with` | `using` statement | ✅ High |
| `try ... onfail ... always` | `try ... catch ... finally` | 🟡 Medium |
| `fails` channel | XML doc + convention | 🟡 Medium (unenforced) |
| `emit` output modifier | `IEnumerable<T>` + yield | 🟡 Medium |
| `out` output modifier | out params or ValueTuple | 🟡 Medium |
| `ccc` | `#if` / feature flags | 🟡 Medium (lossy) |
| Capability system | interfaces + composition | 🟡 Medium |
| Not-ok propagation | Result<T> pattern | 🔴 Hard |
| Module `internal` enforcement | Convention only | 🔴 Hard |

---

## Recommended approach for reverse migration

If a ME → C# reverse migration is needed:

1. **Structural pass:** Translate `spec → interface`, `entity → class`, `choice → enum`,
   `error → Exception`, `action → delegate` mechanically.
2. **Callable pass:** Translate all `fn` callables to methods, `[]` params to method
   params, `be T` return to `T`.
3. **Failure model pass:** Translate `fails X` to XML `<exception>` doc, `onfail` to
   `catch`, not-ok states to `bool ok` out params or `Result<T>`.
4. **CCC pass:** Translate `ccc` types to `#if` blocks or a feature-flag static class.
   Document which CCC fields were converted to runtime flags vs compile-time flags.
5. **Capability pass:** Translate ME capabilities to C# interfaces + extension methods or
   base classes. Flag for manual review.
6. **Review pass:** Every `🟡`/`🔴` construct in the table above is a review item.

The reverse migration tool (`me-tools reverse-migrate`) follows this order and annotates
all `🟡`/`🔴` translations with `// ME-LOSSY:` comments and a review report.

---

# ME → C# Migration Pipeline - To-Do List

Reverse migration pipeline status as of 2026-05-20.

Current capability: contracts and stub pipeline wired; no frontend or backend
implemented yet.  All items below are deferred pending implementation.

---

## Legend

| Symbol | Meaning |
|--------|---------|
| ✅ | Done |
| 🔵 | My work (neutral model / CLI / infrastructure) |
| 🟠 | Other AI's work (ME frontend / pipeline / semantic) |
| - | Not assigned yet |

---

## Done

| # | What | Owner |
|---|------|-------|
| D-01 | `ME.Migration.ToCSharp` project created; all contracts scaffolded | ✅ 🔵 |
| D-02 | `IMeFrontend`, `ICSharpTextBackend`, `IMeDeclarationReader` interfaces | ✅ 🔵 |
| D-03 | `IFailureModelTranslator`, `ICccTranslator`, `ICapabilityTranslator`, `ILossyAnnotator` interfaces | ✅ 🔵 |
| D-04 | `MeReverseEnums` - `CccTranslationMode`, `CapabilityTranslationMode`, `FailureTranslationMode` | ✅ 🔵 |
| D-05 | `MeLossyAnnotation` record | ✅ 🔵 |
| D-06 | `MeToCSharpMigrationPipeline` stub wiring frontend + backend | ✅ 🔵 |

---

## Remaining - Frontend (ME parser)

| # | What | Gap | Owner |
|---|------|-----|-------|
| F-01 | `MeFrontend` implementation | Reads `.me` files using ME language parser; produces `MigrationCompilation` | 🟠 |
| F-02 | `MeDeclarationReader` implementation | Walks ME AST nodes and lowers each declaration into `MigrationDeclaration` | 🟠 |
| F-03 | CCC declaration capture | `ccc` entities must be preserved in `MigrationCompilation` for the CCC pass | 🟠 |
| F-04 | Capability declaration capture | `capability ... affects [...]` must be preserved for the capability pass | 🟠 |
| F-05 | `fails` / `throws` channel capture | Record failure channels on `MigrationCallableDeclaration` | 🟠 |
| F-06 | Not-ok state detection | Identify callables returning not-ok state for `IFailureModelTranslator` | 🟠 |
| F-07 | `emit` / `out` modifier capture | Preserve `MigrationParameterModifier.Emit` / `.Out` from ME syntax | 🟠 |
| F-08 | Body lowering | Method bodies lowered to `MigrationBlock` (reuse forward-migration logic) | 🟠 |

---

## Remaining - Pass Implementations

| # | What | Gap | Owner |
|---|------|-----|-------|
| P-01 | `FailureModelTranslator` - `XmlDocComment` mode | `fails X` → XML `<exception>` comment fragment | 🔵 |
| P-02 | `FailureModelTranslator` - `ResultPattern` mode | not-ok return → `(T? Value, bool Ok)` tuple type ref | 🔵 |
| P-03 | `CccTranslator` - `PreprocessorDirective` mode | `ccc` entity → `#if` / `#define` blocks | 🔵 |
| P-04 | `CccTranslator` - `RuntimeFeatureFlag` mode | `ccc` entity → static feature-flag class | 🔵 |
| P-05 | `CapabilityTranslator` - `Interface` mode | capability → `interface` + explicit type binding | 🟠 |
| P-06 | `CapabilityTranslator` - `ExtensionMethods` mode | capability → `interface` + static extension class | 🟠 |
| P-07 | `LossyAnnotator` implementation | Stamp `// ME-LOSSY:` comments; flush diagnostics to report | 🔵 |
| P-08 | `emit` → `IEnumerable<T>` / yield mapper | `MigrationParameterModifier.Emit` → `IEnumerable<T>` return + lossy note | 🔵 |
| P-09 | `out` / `ref` → ValueTuple mapper | Multi-output callables → `(T1, T2)` return or `out` params | 🔵 |

---

## Remaining - Backend (C# emitter)

| # | What | Gap | Owner |
|---|------|-----|-------|
| B-01 | `CSharpTextBackend` structural pass | Emit `interface`, `class`, `record`, `enum`, `delegate` from `MigrationTypeDeclaration` | 🟠 |
| B-02 | Callable emission | Emit method signatures from `MigrationCallable` and `MigrationCallableDeclaration` | 🟠 |
| B-03 | Property emission | Emit `{ get; }` / `{ get; set; }` / `{ get; init; }` from `MigrationProperty` | 🟠 |
| B-04 | Namespace grouping | Group emitted types by `TargetNamespace`; one `.cs` file per namespace | 🟠 |
| B-05 | `using` / `load` emission | Emit `using` directives from `module` / `load` declarations | 🟠 |
| B-06 | XML doc passthrough | Emit XML doc-comments for all public members | 🔵 |
| B-07 | Lossy annotation insertion | Call `ILossyAnnotator.Annotate` on each `🟡`/`🔴` fragment before writing | 🔵 |

---

## Remaining - CLI / Tooling

| # | What | Gap | Owner |
|---|------|-----|-------|
| C-01 | `me-tools reverse-migrate` CLI command | Wire `MeToCSharpMigrationPipeline` into `ME.Tools` | 🔵 |
| C-02 | `--ccc-mode` flag | Expose `CccTranslationMode` choice at the CLI | 🔵 |
| C-03 | `--capability-mode` flag | Expose `CapabilityTranslationMode` choice at the CLI | 🔵 |
| C-04 | `--failure-mode` flag | Expose `FailureTranslationMode` choice at the CLI | 🔵 |
| C-05 | `--report <path>` flag | Write lossy-annotation review report as Markdown or JSON | 🔵 |

---

## Remaining - Test Coverage

| # | What | Gap | Owner |
|---|------|-----|-------|
| T-01 | Fixture suite: spec → interface | `spec IShape`, `spec IShape from IGeo` | 🔵 |
| T-02 | Fixture suite: entity → class | `entity Order`, `fix entity Point` → `record` | 🔵 |
| T-03 | Fixture suite: choice → enum | `choice Colour`, `[Flags]`-equivalent | 🔵 |
| T-04 | Fixture suite: generics | `spec IBox of T`, `entity Pair of [K, V]` | 🔵 |
| T-05 | Fixture suite: fails channel | Assert `// ME-LOSSY:` comment and `ManualReview` diagnostic | 🟠 |
| T-06 | Fixture suite: CCC | Both `PreprocessorDirective` and `RuntimeFeatureFlag` modes | 🔵 |
| T-07 | Fixture suite: capability | `capability Logging affects Order` in each translation mode | 🟠 |
| T-08 | Fixture suite: emit modifier | `fn Scan ... be ScanLine emit` → `IEnumerable<ScanLine>` | 🔵 |
| T-09 | End-to-end: round-trip | C# → ME (forward) → C# (reverse); compare to original | 🟠 |

---

## Notes

- All contracts live in `see Document_Index_Page.md`.  Each pass item above maps to
  exactly one interface in that folder.
- `MeLossyAnnotation` and `MeReverseEnums` in `see Document_Index_Page.md` are the shared
  value types for all pass implementations.
- Fixture pairs (`.me` + `.cs`) will live in a `see Document_Index_Page.md`
  folder, mirroring the pattern in `see Document_Index_Page.md`.
- The `me-tools reverse-migrate` command is the user-facing contract.  Do not change its
  flag names without updating this doc.
