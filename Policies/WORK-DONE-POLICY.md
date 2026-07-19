<p align="left">
  <img src="..\..\..\MnMLogo.png" alt="MachineEnglish logo" width="48" />&nbsp;&nbsp;&nbsp;<strong>MachineEnglish(ME) - a language of intent</strong>
</p>
> **Credit:** Sourced from [MukeshAdhvaryu/SDLC-Governance](https://github.com/MukeshAdhvaryu/SDLC-Governance) and extended for this project.

# Work Done Policy

> A document without a status table is a document that cannot answer the question: "Is this actually done?"

Status must have one owner per document path. In this repository that means:
- ADR and MD delivery status lives in the corresponding RDMP document.
- MAD delivery status lives in the MAD document itself.
- DFR drift-tracking status lives in the DFR document itself.

Specs and Semantics carry the contract, not duplicate delivery status. Where a status table exists, it records what shipped, what did not, what is in flight, and the exact commit where each state was established. The table is the document's ground truth - not its prose, not its headings, and not its author's memory.

---

## Decision Flow (Mandatory)

Status ownership is split by source path:

1. `ADR/MD -> RDMP`
   ADRs and MDs record the decision and rationale. Delivery status lives in the bound roadmap.
2. `MAD -> Spec -> Semantic`
   MADs own delivery status for that architecture domain. Specs and semantics carry the contract, not duplicate status.
3. `DFR -> DFR`
   Drift reports keep their own correction-tracking status.

### What each layer means

| Layer | Document(s) | Answers | Owned by |
|---|---|---|---|
| **Decision** | ADR, MD, MAD, DFR | Why this design? What alternatives were rejected? | Mukesh (decision owner) |
| **Roadmap** | RDMP-*.md | How do ADR/MD-backed work items ship? | Blake / team |
| **Contract** | Spc-*.md, Sem-*.md | What behaviour must the implementation satisfy? | Blake / Alex |
| **Tests** | Spec-suites, contract tests | Does the implementation actually do what was decided? | Blake / Alex |
| **Source status owner** | RDMP for ADR/MD, MAD for MAD, DFR for DFR | Where the authoritative delivery state lives | Derived from tests |
| **Status Master** | ME_Status_Master.md | What is the current snapshot across all work? | Derived from status owners; never primary |

### Rules

- ADRs and MDs drive Roadmaps. A Roadmap with no backing ADR/MD is a plan without a rationale.
- MADs drive Specs and Semantics. A spec without a MAD home is a contract without an architecture owner.
- Tests prove behaviour. A feature is not done because the code compiles; it is done because the evidence exists.
- When tests pass, update the source status owner first, then refresh `ME_Status_Master.md`.
- `ME_Status_Master.md` is a snapshot, not the primary source. Discrepancies are resolved by trusting the source status owner.
- If work starts without a backing ADR/MD/MAD/DFR, stop and write the governing document first.

## Verification Chain

Two validation paths exist.

### MAD-backed language work

```
MAD decision
  -> 1. Spec         (ME/Architecture/Specs/Spc-*.md)
  -> 2. Semantic     (ME/Architecture/Semantics/Sem-*.md)
  -> 3. Abstractions (ME/Language/Journey/Tests.Abstractions/Suites/)
  -> 4. Implementation tests (ME/Language/Journey/Tests.Implementation/)
  -> 5. E2E native proof (examples/ME-console-testing or examples/ - see below)
  -> 6. Status propagation:
       a. MAD status table        -- work item marked [x]; test method(s) cited
       b. ME_Status_Master.md     -- snapshot refreshed from MAD/RDMP
```

### ADR/MD-backed delivery work

```
ADR / MD decision
  -> 1. RDMP        (ME/Architecture/Roadmap/RDMP-*.md)
  -> 2. Implementation/tests as applicable
  -> 3. E2E native proof (examples/ME-console-testing or examples/ - see below)
  -> 4. Status propagation:
       a. RDMP status table       -- relevant work item row updated
       b. ME_Status_Master.md     -- snapshot refreshed from MAD/RDMP
```

### Step definitions

**Spec / Semantic / Tests** apply when the work changes language behaviour. MAD-backed work must travel the full contract chain.

**E2E native proof** applies whenever the work changes anything a compiled ME program can observe at runtime (new syntax, new semantics, new stdlib/runtime behavior, a bug fix in codegen or the interpreter). It is not satisfied by unit or abstraction-suite tests alone, no matter how thorough - see "Why This Exists" below for why. Skip it only under the same conditions as step 3/4 in Allowed Exceptions (no backend-executable behavior yet, or compile-time-only rejection with nothing to run).

**Source status owner** is updated first:
- ADR/MD work updates the bound RDMP row.
- MAD work updates the MAD status row.
- DFR work updates the DFR row.

**ME_Status_Master.md** is refreshed only after the source status owner is correct.

## Allowed Exceptions

A verification chain step may be skipped only when one of the following applies,
and the deviation is documented inline at the point of skip:

| Skip | Condition |
|---|---|
| Skip step 3 (abstractions) | Feature has no backend-executable behavior yet (documented stub) |
| Skip step 4 backend pairing | Case is compile-time only (grammar rejection, type error, name error) - see ADR-043 |
| Skip step 3 or 4 entirely | Backend harness capability is missing for the scenario (state the gap) |
| Skip the E2E native proof step | Case is compile-time only (nothing to run), or the feature has no native-executable behavior yet (LSP/tooling-only, documented stub) - same conditions as skipping step 3/4 above |

No other exceptions are valid. "Not done yet" is not an exception - it means the work cycle is not closed.

---

## E2E Native Proof Requirement (Mandatory)

**Ratified 2026-07-13.** Every piece of compiler, runtime, or stdlib work that
has any native-observable behavior must end with a real compile-link-run
proof committed to `examples/ME-console-testing/` (or a new directory under
`examples/` for work that doesn't fit the console-testing shape) - not just
abstraction-suite or unit tests, no matter how thorough those are.

**Why:** unit and abstraction-suite tests in this repo run against the IR or
a fake backend snapshot - they prove the compiler *emits the right shape*,
not that a real compiled program *runs correctly*. This repo has repeatedly
shipped features that passed every compile-only test while being completely
broken (or entirely unimplemented) at actual runtime - discovered only when
someone finally tried to compile-link-run a real program:

- DFR-036/037 (2026-07-12): `show`/`to text` on any non-text value and `out`
  parameter write-back both had zero working native runtime, despite
  extensive prior compile-only test coverage.
- DFR-035 T10 (2026-07-13): `x is ok` / `x is not ok` had never had a runtime
  implementation for *any* binding - every VAL-003 test that existed before
  this date was compile-only and never caught it.

Compile-only proof is not proof (see [[feedback_no_compile_only_proof]]).

**How to apply:**
- Add a new "Testing `<name>`" block to `examples/ME-console-testing/main.me`
  (green header/footer convention, see the file itself) whenever a feature
  lands that a compiled program can exercise.
- Actually run the compiled `.exe` and read its output before marking the
  work item `[x]` - do not infer correctness from a clean compile alone.
- If the work only reaches the interpreter (native lowering deferred, as with
  DFR-035 T10's out-param not-ok producer), the status row must say so
  explicitly (`[~]`, not `[x]`) and the E2E step stays open until native
  lowering lands - an interpreter-only test is not a substitute for this step.

---

## Scope

This policy applies to:
- All new ADRs
- All Major Decisions (MD-*)
- All Drift Fix Reports (DFR-*) once accepted
- All MAD and RDMP rows added or promoted in `ME_Status_Master.md`

It does not apply to:
- Pure documentation changes with no language behavior (renaming an ADR, fixing a typo)
- Deferred/placeholder decisions that are explicitly marked `Status: Proposed`

---

## Enforcement

- Blake (Claude) is responsible for checking the verification chain is complete before
  closing any work cycle.
- Alex (Codex) is responsible for ensuring implementation tests exist before reporting
  a feature implemented.
- Mukesh is the final authority on exceptions.
- MAD, RDMP, and DFR status tables are the authoritative records - a row marked `[x]`
  without test evidence must be challenged.
- ME_Status_Master.md is the shared snapshot ledger. It is refreshed last and must
  match the ADR/MD/RDMP rows it derives from. Discrepancies are resolved by trusting
  the originating document, not ME_Status_Master.md.

---

## Why This Exists

Architecture documents accumulate stale content. A decision is made, partially
implemented, then the scope changes - but nobody updates the document. Six months
later a new contributor reads "Done" in a heading and trusts it. The code disagrees.

The status table solves this by making staleness visible:

- Every item has an explicit state symbol, not a prose claim.
- Every shipped item has a commit SHA - if no commit exists, it is not shipped.
- Every date is absolute, not relative ("last week", "recently").
- Partial and WIP states are first-class - they are not omitted out of embarrassment.

---

## Status Symbols

| Symbol | Meaning | When to use |
|--------|---------|-------------|
| `[x]` | **Shipped** | Complete, tested, in the build or runtime. A commit SHA is mandatory. |
| `[ ]` | **Not done** | Not started. No design, no code, no tests. |
| `[~]` | **Partial** | Some sub-items shipped, some have not. Notes must state what is missing. |
| `[*]` | **WIP** | Actively in progress this cycle. Expected to become `[x]` or `[~]` soon. |
| `[.]` | **Deferred** | Intentionally postponed. Notes must state the blocker or trigger condition. |
| `[!]` | **Cancelled** | Decided against. Notes must state why - do not delete the row. |

**Rules:**
- `[x]` without a commit SHA is invalid. Fill the commit or downgrade to `[~]`.
- `[*]` without an owner or active branch is invalid. Downgrade to `[ ]` or `[.]`.
- Never delete a row. `[!]` preserves the decision trail.

---

## Standard Status Table

Every status-owning document must include a section named **Status** containing this table.
Add it immediately after the document's opening summary or decision block.

```markdown
## Status

| # | Sub-topic | Related Area | Status | Notes | Commit | Date |
|---|-----------|--------------|--------|-------|--------|------|
| 1 | Brief label for this item | Component / layer this touches | [x] | One line: caveat, blocker, or "-" | `abc1234` | YYYY-MM-DD |
```

### Column definitions

| Column | Type | Rules |
|--------|------|-------|
| **#** | Integer | Sequential row number within that status table. |
| **Sub-topic** | Short label | Match the heading or rule ID where possible. No prose. |
| **Related Area** | Component name | Name the system layer, module, or tool. |
| **Status** | Symbol | One of the six symbols above. No free-text status. |
| **Notes** | One sentence | State the exception, blocker, or sub-item gap. Use `-` if none. |
| **Commit** | Short SHA | The commit that shipped this item. Format: inline code `abc1234`. Use `-` if not yet shipped. |
| **Date** | ISO 8601 | Date of the commit, or date the status was last reviewed. Format: `YYYY-MM-DD`. |

## Document-Type Rules

### ADR (Architecture Decision Records)

ADRs do **not** carry delivery status tables. Implementation status lives in the bound RDMP document. ADRs record decisions and rationale only.

### MD (Major Decisions)

MDs do **not** carry delivery status tables. Implementation status lives in the bound RDMP document. MDs record decisions and rationale only.

### MAD (Major Architectural Decisions)

MADs own the status table for the `MAD -> Spec -> Semantic` path. Each row tracks a concrete architectural consequence owned by that MAD.

### DFR (Drift Reports)

DFRs keep their own correction-tracking status table because the report itself is the work tracker.

### Roadmap (RDMP)

The status table is the delivery tracker for the `ADR/MD -> RDMP` path. Each phase or named item gets a row. The roadmap body may contain narrative, but the table is the canonical state of play.

## Maintenance Rules

1. **Update on merge, not on plan.** Change a row's status when the commit lands on the integration branch, not when the task is started.
2. **One row per atomic deliverable.** If a row would need two commits and two dates, it is two rows.
3. **The table owns the state.** If prose says "Done" but the table says `[ ]`, the table is authoritative and the prose must be corrected.
4. **Deferred rows must carry a trigger.** `[.]` without a condition is not allowed.
5. **Commits are immutable evidence.** A SHA in the Commit column is a permanent record.
6. **Review cadence.** At the start of any session that touches a document, scan its status table.
7. **Status propagates from evidence.** Update the source status owner first, then refresh `ME_Status_Master.md`.

## Method of Elimination (large test failure counts)

When a test run produces a large number of failures, do not attempt to fix them
individually. Most failures in a batch share a root cause - fix the root and the
rest resolve automatically.

**The practice:**

1. Run the suite. Note the failure count.
2. Pick the **first failing test**. Read only the snippet and the failure message -
   nothing else.
3. Diagnose the root cause from that one test. It is almost always representative
   of a whole category.
4. Fix it. Re-run.
5. Observe how many failures disappeared. The delta tells you whether you found the
   root cause or a symptom.
6. Repeat with the next first failing test until the count is zero or the remaining
   failures are unrelated noise.

**Why this works:** a large failure count is not a large number of problems. It is
usually one or two broken contracts propagating through every test that depends on
them. The first failing test exposes the contract. The fix collapses the cascade.

**Rule:** never file a status row as `[x]` based on a partial run or a filtered
subset. The full suite must pass, or the remaining failures must be explicitly
accepted as known pre-existing failures with a note in the status table.

---

## What Not To Do

These are the failure patterns this policy exists to prevent. Each one has been
observed in practice. None of them feel like mistakes while they are happening.

### Stay distant from test evidence

Architectural decisions made without reading test output are decisions made without
evidence. The test suite is not a CI gate to pass - it is the most precise
description of what the system actually does. Read it. Not all of it. The first
failure. Regularly.

*The symptom:* a feature is marked done because the code compiles and looks right.
The tests disagree. Nobody noticed.

### Commit without a roadmap item

Commits that are not traceable to a roadmap item are commits that cannot be
reviewed, reverted, or explained to a future contributor. "I was fixing things"
is not a commit message. "feat(modules): implement MDL-102 per DFR" is.

*The symptom:* 70+ commits on a branch, half of them `fix`, `update`, `wip`.
The branch cannot be cleanly squashed because nobody can tell what belongs together.

### Start strong, context-switch before done

A feature that is 80% implemented is not 80% valuable. It is a liability - it
occupies a status row, blocks dependent work, and requires future context
reconstruction. A `[~] Partial` row that stays partial for two weeks is a sign
that the feature was abandoned mid-flight.

*The symptom:* Status Master has more `[~]` rows than `[x]` rows. Roadmap phases
never fully close.

### Write specs after the code

A spec written after the code describes what was built, not what was decided.
It cannot catch implementation mistakes because the implementation already defined
what "correct" means. The spec becomes documentation, not a contract.

*The symptom:* specs that match the implementation exactly, including its bugs.
Tests pass because they were written against the code, not the spec.

---

## Legend Block (copy-paste for document headers)

Every document that carries a status table should include this legend directly above
the table:

```markdown
> **Status legend:** [x] Shipped - [ ] Not done - [~] Partial - [*] WIP - [.] Deferred - [!] Cancelled
```
