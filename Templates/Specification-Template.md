<div style="display:inline-flex; align-items:center; gap:14px; border:1px solid rgba(164,175,186,0.35); border-radius:18px; padding:10px 16px; margin:0 0 12px 0; background:linear-gradient(135deg, rgba(255,255,255,0.06), rgba(255,255,255,0.02)); box-shadow: 0 8px 24px rgba(0,0,0,0.18);">
  <img src="..\..\..\MnMLogo.png" alt="MachineEnglish logo" width="48" />
  <span style="font-size: 2em; font-style: italic; font-weight: 700;">MachineEnglish(ME) - a language of intent</span>
</div>

> **Credit:** Sourced from [MukeshAdhvaryu/SDLC-Governance](https://github.com/MukeshAdhvaryu/SDLC-Governance) and adopted for this project.

> **Path Authority:** Repository file/folder paths are maintained only in [DIP.md](../../../DIP.md) and [DIP_Internal.md](../../../DIP_Internal.md). Do not rely on path literals in this document.

---

> **Status tracking:** Delivery status is maintained in the corresponding MAD document, not here. Specifications record the contract and examples only.

---

# Spc-AreaName: Specification Title

> One-line description of what this specification governs and its scope boundaries.

All rule IDs referenced below (`AREA-*`) are defined in `Sem-AreaName.md`.

---

## Table of Contents

- [N. First Major Area](#n-first-major-area)
  - [N.1 Sub-section One](#n1-sub-section-one)
  - [N.2 Sub-section Two](#n2-sub-section-two)
  - [N.3 Sub-section Three](#n3-sub-section-three)
- [N+1. Second Major Area](#n1-second-major-area)
  - [N+1.1 Sub-section One](#n11-sub-section-one)
  - [N+1.2 Sub-section Two](#n12-sub-section-two)
- [N+2. Summary](#n2-summary)
  - [Error code catalogue](#error-code-catalogue)
  - [Warning catalogue](#warning-catalogue)

---

## N. First Major Area

Brief paragraph describing what this area covers and the key invariants that always hold.
Keep it to two or three sentences. This is orientation, not specification - the rules below are the spec.

### N.1 Sub-section One

*(AREA-001, AREA-002)*

State what this sub-section governs. Use plain declarative sentences. If there is a key
invariant, state it in bold.

**The key invariant:** Every [construct] must [condition]. The compiler enforces this at
compile time - there are no runtime surprises.

Use a table when enumerating related properties:

| Property | Meaning |
|----------|---------|
| First property | What it means |
| Second property | What it means |

```me
// minimal valid example
ConstructName [param as Type] be ReturnType [
    be param
]
```

#### Invalid forms

```me
// brief comment describing what rule fires
BadConstruct [param as Type] [
    be param    // AREA-001: reason why this is wrong
]
```

[Back to TOC](#table-of-contents)

---

### N.2 Sub-section Two

*(AREA-003)*

Describe the rules for this sub-section. Prefer declarative sentences over lists.
Use a list only when enumerating genuinely parallel items - not for prose.

- First rule: condition and consequence.
- Second rule: condition and consequence.
- Third rule: condition and consequence.

```me
// valid form
x be TypeName { field1: value1, field2: value2 }
```

#### Valid forms

```me
// first valid variant
A [x as integer] be integer [
    be x plus 1
]

// second valid variant - same semantics, different surface
A(x as integer) = integer => x + 1
```

#### Invalid forms

```me
// AREA-003: reason for rejection
A [x as integer] be integer [
    // missing return - all paths must produce a value
]
```

[Back to TOC](#table-of-contents)

---

### N.3 Sub-section Three

*(AREA-004, AREA-005)*

Use sub-sub-sections when a topic has two genuinely distinct scenarios that would
be confusing as one block. Do not add nesting just for visual structure.

#### N.3.1 First scenario

Describe the first scenario. State which rule governs it inline in the paragraph
rather than in a separate heading when possible.

`AREA-004` fires when [precise condition]. The compiler detects this at [phase].

```me
// example of the scenario
entity Container from IContainable [
    items as List
]
```

#### N.3.2 Second scenario

Describe the second scenario. Cross-reference a related spec by name when the
detail lives elsewhere - do not duplicate it here.

Full rules for [related concept] are in `Spc-RelatedArea.md`.

[Back to TOC](#table-of-contents)

---

## N+1. Second Major Area

Brief orientation paragraph for this area.

### N+1.1 Sub-section One

*(AREA-010)*

When a sub-section defines multiple constraints on the same construct, prefer a
table over prose lists:

| Constraint | Enforcement | Rule |
|------------|-------------|------|
| First constraint | Compile-time | AREA-010 |
| Second constraint | Compile-time | AREA-011 |
| Third constraint | Runtime | AREA-012 |

```me
// valid example demonstrating the constraints
Callable [a as integer, b as integer] be integer fails DomainError [
    if a < 0 [
        fail DomainError { message: "a must be non-negative" }
    ]
    be a plus b
]
```

#### Invalid forms

```me
// AREA-010: first constraint violated
Callable [a as integer] be integer [
    // AREA-011: second constraint violated
]
```

[Back to TOC](#table-of-contents)

---

### N+1.2 Sub-section Two

*(AREA-013)*

Describe cross-spec dependencies explicitly. If this area's behaviour depends on
a decision recorded in an ADR or Major Decision, name it:

This behaviour was decided in [ADR-NNN](../ADRs/ADR-NNN-Title.md). The rule here
is the specification-layer consequence of that decision; the rationale lives in
the ADR.

```me
// example
```

[Back to TOC](#table-of-contents)

---

## N+2. Summary

### Error code catalogue

A consolidated reference for all error codes introduced by this spec. Every code that
appears inline in sections above must appear here. Codes not yet implemented are marked
with their implementation status.

| Code | Trigger condition | Severity | Status |
|------|------------------|----------|--------|
| AREA-001 | [precise trigger] | Error | ✅ |
| AREA-002 | [precise trigger] | Error | ✅ |
| AREA-003 | [precise trigger] | Error | ❌ |
| AREA-004 | [precise trigger] | Error | 🔶 |
| AREA-010 | [precise trigger] | Error | ✅ |
| AREA-011 | [precise trigger] | Warning | ✅ |
| AREA-012 | [precise trigger] | Warning | ❌ |
| AREA-013 | [precise trigger] | Error | ✅ |

### Warning catalogue

Only present if this spec introduces warnings. Remove this sub-section if none.

| Code | Condition | Suppressible with `accept` |
|------|-----------|---------------------------|
| AREA-W001 | [condition] | Yes |

[Back to TOC](#table-of-contents)

---

# End of Spc-AreaName.md

---

## Template usage notes

> **Remove this section before committing the spec.**

### Section numbering

Each spec owns a chapter number in the ME language specification. The chapter number
is assigned when the spec is created and is permanent. Check `DIP.md` for the existing
chapter assignments before choosing a number.

- `Spc-Core.md` - chapters 1–8 (language foundation)
- `Spc-Callables.md` - chapter 5 (functions and actions)
- `Spc-WarningsAndErrors.md` - chapters 1–4 (diagnostic model)

Use `N` as a placeholder above and replace with the assigned chapter number.

### Rule ID inline references

Every sub-section that is backed by a semantic rule must cite the rule IDs immediately
after the heading in the form `*(AREA-NNN)*`. Multiple rules on one line: `*(AREA-001, AREA-002)*`.
If a sub-section has no semantic rule yet, note `*(rule pending)*` as a reminder.

The rule bodies live in `Sem-AreaName.md`. The spec states *what* the rule checks;
the semantic document states *exactly when* it fires and what error code it emits.

### Back-to-TOC anchors

Every `###` sub-section ends with:

```
[Back to TOC](#table-of-contents)

---
```

This keeps navigation consistent across all specs.

### Invalid / valid forms

Use `#### Invalid forms` and `#### Valid forms` sub-sub-sections for code examples
that specifically demonstrate rule violations or correct patterns. Do not put
violation examples in the main prose block - they are easy to miss.

### Cross-spec references

When a rule is fully specified elsewhere, name the owning spec explicitly and do not
duplicate the rule. Use the form:

> Full [concept] rules are in `Spc-OtherArea.md`.

### Status ownership

Specification progress is tracked in the corresponding MAD document. The spec remains
the human-readable contract and should not carry a duplicate delivery table.
