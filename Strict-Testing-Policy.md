# Strict Testing Policy

> The goal is not simply to have tests. The goal is to have tests that make the
> system safer to change.

Tests that assert implementation order, reach into private internals, or exist
purely to satisfy a coverage metric are not an asset. They are debt — coupling
the codebase to its own mechanics, making every refactor harder, and producing
false confidence when they pass.

MachineEnglish adopts a strict testing policy as a first-class engineering
commitment. This document defines what that means, why each rule exists, and what
the prohibited patterns look like in practice.

---

## Table of Contents

- [The Core Policy](#the-core-policy)
- [Principles](#principles)
- [Test Categories](#test-categories)
- [Anti-Patterns](#anti-patterns)
- [Review Standard](#review-standard)
- [Consequences](#consequences)
- [Organisation](#organisation)

---

## The Core Policy

MachineEnglish testing is governed by five rules:

1. **Tests validate contracts and observable behaviour.** Not implementation
   mechanics, not private helper sequencing, not internal state arrangement.

2. **Test scaffolding must not define production architecture by accident.** If a
   test requires an abstraction that does not exist in the product, that is a
   signal to create a real product abstraction — not a test-only one.

3. **Production abstractions introduced for tests must be legitimate product
   abstractions.** An interface that exists solely to enable mocking is a product
   liability, not a testing asset.

4. **Test suites must be organised for long-term extension, not short-term
   convenience.** A suite that works for one implementation should be reusable for
   the next implementation without duplication.

5. **Implementation details should be tested indirectly unless they are promoted
   into intentional contracts.** When an internal detail needs a test, the correct
   response is to name it as a contract, not to reach past the boundary.

This policy applies to all compiler, runtime, backend, interpreter, platform,
language-service, tooling, and standard-library work.

---

## Principles

### Contract First

Tests must be written against the contract that matters.

- Backend tests target the backend interface, not a specific LLVM implementation.
- Compiler tests target diagnostics, IR semantics, and result shape, not parser
  helper internals.
- LSP tests target request/response behaviour, not private scanner mechanics
  unless that scanner becomes a real product component.
- Tooling tests target command behaviour and filesystem effects, not incidental
  control flow.

If the contract is unclear, tests should force the contract to be named. Test
pressure is the cheapest form of interface review.

### Public Behaviour Over Private Mechanics

A passing test suite should mean the product still behaves correctly. It should
not mean that a private helper still happens to arrange its loops the same way.

Tests may use focused fixtures and test doubles, but the assertion target must be
observable behaviour or an intentional contract. A test that passes for the wrong
reason is worse than no test — it provides false assurance while hiding the real
risk.

### Test Pressure Is Design Feedback

Hard-to-test code is a signal. It is almost always a signal that a boundary is
poorly drawn, a responsibility is misallocated, or a contract has not been named.

The correct response to hard-to-test code is architectural thought.

**Acceptable responses:**
- Extract a real interface or value object
- Test at a higher integration level
- Split an oversized component into product-meaningful pieces
- Clarify the public contract
- Leave incidental implementation covered through caller behaviour

**Unacceptable responses:**
- `InternalsVisibleTo` for test access
- Public APIs that exist only for tests
- Test-only production flags or configuration
- Assertions that lock down incidental implementation order
- Mock-heavy tests that prove the mock setup rather than product behaviour

The first set of responses makes the product better. The second set makes the
product worse in exchange for a coverage number.

---

## Test Categories

### Semantic Tests

Validate language meaning: types, diagnostics, control flow, module behaviour,
generics, and compile results. These tests should be stable across parser or
internal representation changes unless the language contract itself changes. They
are the authoritative record of what the language guarantees.

### Contract Tests

Validate interfaces that must hold across multiple implementations. They should
be reusable without duplicating test logic. When a second implementation appears,
contract tests run against it for free.

Examples: backend contract tests that every backend implementation must pass,
platform abstraction tests that every OS implementation must pass.

### Integration Tests

Validate collaboration between real components: compiler pipeline, LSP handlers,
tools, filesystem layout, and package or runtime behaviour. Integration tests are
appropriate when correctness emerges from the interaction, not from any single
component in isolation.

### Regression Tests

Every fixed bug that could plausibly return should leave a regression test at the
lowest level that still exercises the real contract. Regression tests are not
optional. A bug fixed without a regression test has an unknown probability of
returning.

### Golden Tests

Golden or snapshot-style tests are allowed only when the output is a true product
artefact whose full shape matters — for example, a generated file format or a
specific diagnostic message structure. They must not become a substitute for
semantic assertions about behaviour.

---

## Anti-Patterns

The following are prohibited or strongly discouraged:

| Anti-pattern | Why it is harmful |
|---|---|
| Friend assemblies / `InternalsVisibleTo` for tests | Couples tests to internals; every refactor that moves internal details breaks tests |
| Test-only production APIs | Production code grows testing backdoors; the API surface lies about the product |
| Public setters or mutable state added only for tests | State that should be invariant is exposed; the product boundary is weakened |
| Mocking the unit under test's direct implementation details | The test proves mock setup, not product behaviour |
| Asserting private method sequencing | Tests break on every internal refactor regardless of whether behaviour changed |
| Duplicate suites per implementation | Divergence is inevitable; the second implementation drifts from the contract |
| Broad snapshot tests where only a few semantic properties matter | A snapshot test is a commitment to the entire output forever, including whitespace |
| Brittle line/column assertions unless source location is the feature | Every whitespace change in a fixture breaks unrelated tests |
| Relying on test execution order | Tests that must run in order are integration steps masquerading as unit tests |

---

## Review Standard

Every new test should be able to answer these questions clearly:

1. **What product contract or observable behaviour does this protect?**
   If the answer is "the implementation does X in this order," the test is likely
   wrong.

2. **Would a real consumer care if this test failed?**
   If the answer is no, the test may be coupling to implementation rather than
   behaviour.

3. **Can the implementation be refactored without rewriting the test?**
   If the implementation detail changes but the contract holds, the test must
   still pass.

4. **Is the test at the lowest level that still preserves honest coverage?**
   A test at too high a level is slow and fragile. A test at too low a level
   asserts implementation. Find the right boundary.

5. **Does any new abstraction this test requires exist for production reasons?**
   If the abstraction only exists to make the test compile, it does not belong in
   production code.

If any answer is unclear, the test design should be revisited before merging.

---

## Consequences

This policy produces specific, observable outcomes:

- Test projects mirror product contracts, not implementation folders by reflex.
  A test project named `ME.IR.Tests` tests the compiler contract, not the folder
  layout of `ME.IR`.

- New implementation variants reuse contract suites rather than fork them. The
  second backend does not require a second copy of the backend contract tests.

- Test helper abstractions remain small and honest. A test helper that grows to
  replicate production logic is a sign that production logic has not been properly
  extracted.

- Production code does not grow testing backdoors. The product surface is
  determined by product needs, not by what the test suite needs to reach.

- Refactors that improve testability by clarifying real boundaries are preferred
  over privileged access or brittle assertions.

- Coverage percentage is secondary to contract coverage and regression value.
  100% line coverage of the wrong things is noise. 70% coverage of the right
  contracts is safety.

---

## Organisation

When the same behavioural contract must be proven against more than one
implementation, the preferred model is:

- One abstract contract suite defining the full set of required behaviours
- Physical decomposition via file organisation for readability
- One concrete fixture per implementation that binds the abstract suite to the
  real component

This avoids duplication, ensures every implementation faces the same contract,
and makes adding a new implementation a mechanical step rather than a copy-paste
exercise.

This model applies wherever multiple implementations exist or are planned:
backend implementations, platform implementations, language-service transport
implementations, and any other dimension where the same contract is realised
differently.

---

## The Deeper Argument

Every shortcut in testing policy is a loan against future maintainability.
`InternalsVisibleTo` saves fifteen minutes today and costs an afternoon the next
time the internal structure changes. A test-only public API saves an hour today
and confuses the next engineer who reads the interface and wonders what the method
is for.

Strict testing policy is not about discipline for its own sake. It is about
ensuring that the test suite continues to do its job as the system grows. A test
suite that tightly couples to implementation becomes a liability as the system
scales. A test suite that couples to contracts scales with the system.

The strictness is the point. A test that can be trivially satisfied by a wrong
implementation is not a test. A test that fails only when the contract breaks is
an investment.

---

*This document is part of the MachineEnglish public documentation record.*
*The internal ADR is ADR-016, accepted 2026-05-19, authored by Mukesh Adhvaryu.*
