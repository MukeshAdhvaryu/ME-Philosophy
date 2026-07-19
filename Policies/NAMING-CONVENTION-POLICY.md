<p align="left">
  <img src="..\..\..\MnMLogo.png" alt="MachineEnglish logo" width="48" />&nbsp;&nbsp;&nbsp;<strong>MachineEnglish(ME) - a language of intent</strong>
</p>
> **Credit:** Sourced from [MukeshAdhvaryu/SDLC-Governance](https://github.com/MukeshAdhvaryu/SDLC-Governance) and extended for this project.

# Naming Convention Policy

## Purpose

This policy records naming principles for MachineEnglish language design, standard
library contracts, migration output, and architecture documents.

Names in ME should describe the capability a construct delivers. They should avoid
absence-based or prohibitive wording when a positive capability name is available.

## Positive Capability Names

**Use positive capability names for contracts. A contract name should say what it
provides, not what it forbids.**

For example, prefer `Readable` over `ReadOnly`:

- `IReadableCollection<T>` means the contract provides collection reading.
- `IReadableList<T>` means the contract provides indexed list reading.
- A writable list can honestly inherit `IReadableList<T>` because readability is a
  delivered capability.
- A writable list inheriting `IReadOnlyList<T>` reads as a contradiction, even when the
  inherited members are technically only getters.

The same rule applies beyond collections. Prefer names such as `Counted`, `Indexed`,
`Writable`, `Resizable`, `Clearable`, `Disposable`, `Observable`, and `Queryable` when
those names state the offered capability directly.

`Disposable` is a valid positive capability name: it means the type offers deterministic
disposal/release behavior. A name such as `INotDisposable`, or a name that encodes what
the type cannot do, is the pattern this policy rejects.

## Interface Names

Interface and spec names are favored to start with `I`, but this is a convention, not an
enforced language rule.

Use the `I` prefix when it improves recognition of a capability contract, especially for
stdlib-style specs such as `ICount`, `IEnumerable`, `IIndexer`, `IReadableCollection`,
and `IDisposable`. Omit it when the domain name is clearer without the prefix, or when
preserving a migrated source name is required by migration policy.

## Namespace and Folder Hierarchy

Namespace naming must follow the folder hierarchy exactly. Both must reflect the same
path.

A namespace must be deducible from the folder path, and the folder path must be
deducible from the namespace.

## Documentation Path Authority

Repository path references are maintained in exactly two documents:

1. `Document_Index_Page.md`
2. `Document_Index_Page_Internal.md`

All other documents must reference DIPs for path lookup and must not be treated as the
source of truth for repository paths.

## Restriction Names

Use absence-based or restrictive names only when the restriction itself is the semantic
contract and cannot be expressed as a positive capability.

Acceptable restriction names must pass both tests:

1. The restriction is externally observable and part of the contract.
2. A positive capability name would hide or misrepresent the behavior.

## Reserved Exceptions

The following abbreviated or non-standard names are approved by explicit architectural
decision and must not be used as precedent for generalizing the exception.

| Name | Type | Approved by | Reason |
|------|------|-------------|--------|
| `temp` | binding keyword | MD-003 | Compact, widely understood, no viable 3-letter full-word alternative with identical semantics. Clearer than `end`, `here`, or `scoped` for scope-bound cleanup bindings. |

`temp` is the only approved exception to the no-abbreviation rule as of this writing.
Do not generalize this exception to other stripped or abbreviated forms without a new
architectural decision record.

## Migration Naming

Mechanical migration may preserve source names when exact preservation is required by
migration policy. However, new ME standard-library contracts should follow this policy.
When a migrated source name carries a negative convention, record the source name and the
chosen ME name in the project classification notes.

## C# to ME Naming Rules

### Construct casing

| Construct | Rule | Example |
|-----------|------|---------|
| Spec property contract | PascalCase | `Count`, `IsVisible` |
| Spec callable | PascalCase | `GetOriginBasedVersion`, `Clone` |
| Entity stored field | camelCase | `x`, `count`, `lineLength` |
| Entity `expose` → public name | auto-capitalizes | `x expose` → `X` |
| Choice member | PascalCase | `Equal`, `GreaterThan` |
| Generic type parameter | PascalCase | `T`, `TKey`, `TValue` |

### C# interface name preservation

Preserve the C# interface name exactly, including the `I` or `Ix` prefix. This is
deliberate traceability, not Hungarian notation. If the C# source declares
`IParameter<T>`, write `IParameter of T` - do not normalize to `Parameter`,
`TypedParameter`, or `ParameterContract`.

A non-generic spec and a generic spec with the same base name are distinct contracts
because `of` arity contributes to declaration identity:

```me
public spec IValue [ Value as any optional get ]
public spec IValue of T [ Value as T optional get ]
```

### Internal `Ix...` contracts

C# internal/extended interfaces named `IxSomething` become `internal spec IxSomething`,
not `xSomething`. The `Ix` prefix is part of the name; `internal` carries the visibility.

```me
// WRONG
internal spec xParameter from IParameter []

// CORRECT
internal spec IxParameter from IParameter []
```

### Partial interface preservation

If the C# source declares `partial interface`, the ME spec must also carry `partial`.
Preserve this even when the current migration batch only contains one part.

### `expose as` redundancy

Do not write `expose as PascalName` when the auto-capitalized form of the field name
already matches the desired public name. `x expose` automatically produces the public
property `X`. Only use `expose as Name` when the desired public name differs from the
capitalized field name.

### Reserved words as parameter names

The following ME keywords must not be used as parameter or field names:

| Reserved word | Use instead |
|---------------|-------------|
| `entity` | `other` |
| `key` | `k` |
