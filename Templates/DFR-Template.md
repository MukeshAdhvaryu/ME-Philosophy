<div style="display:inline-flex; align-items:center; gap:14px; border:1px solid rgba(164,175,186,0.35); border-radius:18px; padding:10px 16px; margin:0 0 12px 0; background:linear-gradient(135deg, rgba(255,255,255,0.06), rgba(255,255,255,0.02)); box-shadow: 0 8px 24px rgba(0,0,0,0.18);">
  <img src="..\..\..\MnMLogo.png" alt="MachineEnglish logo" width="48" />
  <span style="font-size: 2em; font-style: italic; font-weight: 700;">MachineEnglish(ME) - a language of intent</span>
</div>

> **Credit:** Sourced from [MukeshAdhvaryu/SDLC-Governance](https://github.com/MukeshAdhvaryu/SDLC-Governance) and adopted for this project.

# DFR-NNN: Short Drift Title

**Date:** YYYY-MM-DD
**Status:** Open | In Progress | Blocked | Resolved | Superseded
**Area:** Short subsystem or component name
**Owner:** Person or role

---

> **Status legend:** [x] Shipped - [ ] Not done - [~] Partial - [*] WIP - [.] Deferred - [!] Cancelled

## Status

| ID | Status | Task | Owner | Evidence |
|----|--------|------|-------|----------|
| DFR-NNN-T01 | Not Started | Correct the implementation to match intended design. | Role/person | File, test, or commit - pending. |
| DFR-NNN-T02 | Not Started | Add regression test protecting the corrected behaviour. | Role/person | Test file - pending. |
| DFR-NNN-T03 | Not Started | Update documentation to reflect canonical design. | Role/person | Doc file - pending. |

---

## Drift

State the observed gap between the intended design and what is currently present.

A drift is not just a bug. It is a divergence from a documented or understood
architectural contract. Be precise: which component, which behaviour, which
document or decision is being violated?

---

## Intended Design

State the canonical behaviour the system must preserve.

Reference the ADR, Major Decision, specification, or policy that defines this
behaviour. If no such document exists, creating one is part of resolving this drift.

---

## Current Behavior

Describe what the system actually does today. Point to:

- specific source files, functions, or modules
- test files that pass when they should fail, or fail when they should pass
- documentation that describes the wrong behaviour
- commit where the drift was introduced, if known

---

## Root Cause

Why did this drift occur?

Common causes:
- The decision was made after the code was written and was never backported.
- A refactoring changed the behaviour without checking the design contract.
- The design was ambiguous and two contributors interpreted it differently.
- The implementation was a workaround accepted under time pressure.

---

## Decision

State the resolution direction. Which of the following applies?

- **Correct the implementation** to match the intended design.
- **Revise the intended design** because circumstances have changed and the drift
  is actually acceptable.
- **Accept the drift as intentional** and record the reason (becomes a `Superseded`
  drift with explanation).

If the resolution requires a new ADR or Major Decision, note it here.

---

## Journey Log

Append dated entries as work proceeds. Do not edit past entries.

```
YYYY-MM-DD - Initial drift identified. Root cause analysis in progress.
YYYY-MM-DD - T01 started. File X modified; awaiting test coverage.
YYYY-MM-DD - T02 complete. Regression test added: TestSuiteName.TestMethodName.
YYYY-MM-DD - Drift resolved. T03 doc update committed. DFR marked Resolved.
```

---

## References

- Related ADR: ADR-NNN - Title
- Related Major Decision: MD-NNN - Title
- Related spec or policy: filename
- Affected source files: list paths
- Commit where drift was introduced: SHA (if known)
