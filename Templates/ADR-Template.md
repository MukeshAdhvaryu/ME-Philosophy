<div style="display:inline-flex; align-items:center; gap:14px; border:1px solid rgba(164,175,186,0.35); border-radius:18px; padding:10px 16px; margin:0 0 12px 0; background:linear-gradient(135deg, rgba(255,255,255,0.06), rgba(255,255,255,0.02)); box-shadow: 0 8px 24px rgba(0,0,0,0.18);">
  <img src="..\..\..\MnMLogo.png" alt="MachineEnglish logo" width="48" />
  <span style="font-size: 2em; font-style: italic; font-weight: 700;">MachineEnglish(ME) - a language of intent</span>
</div>

> **Credit:** Sourced from [MukeshAdhvaryu/SDLC-Governance](https://github.com/MukeshAdhvaryu/SDLC-Governance) and adopted for this project.

> **Path Authority:** Repository file/folder paths are maintained only in [DIP.md](../../../DIP.md) and [DIP_Internal.md](../../../DIP_Internal.md). Do not rely on path literals in this document.

---

# ADR-NNN: Short Decision Title

**Date:** YYYY-MM-DD
**Status:** Proposed | Accepted | Superseded | Deprecated
**Author:** Name, in deliberation with Name

---

> **Status tracking:** Implementation status is maintained in the corresponding RDMP document, not here. ADR documents record decisions and rationale only.

---

## Context

What is the situation that makes this decision necessary? What forces, constraints,
or requirements are at play? Who is affected?

Do not state the decision here. State the circumstances that made a decision unavoidable.

---

## Why Existing Approaches Fall Short

> **OPTIONAL** - include when this decision challenges an established pattern,
> prior art, or industry convention. Omit for uncontroversial implementation choices.

What does the prior approach, the industry convention, or the alternative do instead?
What goes wrong? Be specific - a concrete failure case is a reason; vague criticism is not.

---

## The Correct Design

> **OPTIONAL** - include when a concrete design is being established. Show it;
> do not just describe it.

What does the right answer look like? A code example, a table, a diagram - whatever
makes the correct design immediately visible.

```me
// minimal example of the correct design
```

---

## Positions

> **OPTIONAL** - include for contested decisions or when deliberation should be
> preserved for future reference. Omit for uncontested implementation choices.

### Decision Owner's Position

What is the primary decision maker's view? What led them here?

### Deliberating Contributor(s)

What did other contributors argue? Where did they agree or push back?

Record the strongest objections and how they were resolved - even if the resolution
was "accepted as a known trade-off." This prevents the same arguments being relitigated.

---

## Options Considered

### Option A: Name

Brief description and trade-offs. Why it was rejected or deferred.

### Option B: Name

Brief description and trade-offs. Why it was rejected or deferred.

### Option C: Chosen Direction

Why this option was selected. What it gives up and what it gains.

---

## Consequences

### Positive

- List expected benefits.
- One sentence each.

### Negative

- List known drawbacks or costs.
- Be honest - a drawback not recorded here will surprise the next contributor.

### Neutral

- List side effects that are neither clearly beneficial nor harmful.

---

## Allowed Exceptions

> **OPTIONAL** - omit if there are no legitimate bypasses of this decision.
> "Not done yet" is not an exception.

If certain situations legitimately bypass this decision, document them here.

| Skip | Condition |
|------|-----------|
| Skip step X | Only when condition Y is true and documented inline. |

---

## References

- Related ADR: ADR-NNN - Title
- Related Major Decision: MD-NNN - Title
- Related spec: [Spc-AreaName.md](../Specs/Spc-AreaName.md)
- Related policy: [POLICY-NAME.md](../Policies/POLICY-NAME.md)
- Related spec, ticket, or external document
