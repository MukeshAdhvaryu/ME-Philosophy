<p align="left">
  <img src="..\..\..\MnMLogo.png" alt="MachineEnglish logo" width="48" />&nbsp;&nbsp;&nbsp;<strong>MachineEnglish(ME) - a language of intent</strong>
</p>
*MachineEnglish (ME) - a language of intent*

> **Credit:** Sourced from [MukeshAdhvaryu/SDLC-Governance](https://github.com/MukeshAdhvaryu/SDLC-Governance) and adopted for this project.

> **Path Authority:** Repository file/folder paths are maintained only in [DIP.md](../../../DIP.md) and [DIP_Internal.md](../../../DIP_Internal.md). Do not rely on path literals in this document.

---

# Testing Standards Policy

**Date:** 2026-05-19
**Status:** Accepted
**Author:** Mukesh Adhvaryu

---

## Goal

Define the testing standard for MachineEnglish: tests must protect product
contracts, compiler semantics, and user-visible behaviour with minimal coupling
to implementation mechanics.

The goal is not simply to have tests. The goal is to have tests that make the
system safer to change.

---

## Decision

MachineEnglish adopts a strict testing policy:

1. Tests validate contracts and observable behaviour.
2. Test scaffolding must not define production architecture by accident.
3. Production abstractions introduced for tests must be legitimate product
   abstractions.
4. Test suites must be organized for long-term extension, not short-term
   convenience.
5. Implementation details should be tested indirectly unless they are promoted
   into intentional contracts.

This policy applies to all compiler, runtime, backend, interpreter, platform,
language-service, tooling, and standard-library work.

---

## Principles

### Contract First

Tests must be written against the contract that matters.

Examples:

- Backend tests target `IBackend`, not a specific LLVM implementation.
- Compiler tests target diagnostics, IR semantics, and result shape, not parser
  helper internals.
- LSP tests should target request/response behaviour, not private scanner
  mechanics unless that scanner becomes a real product component.
- Tooling tests should target command behaviour and filesystem effects, not
  incidental control flow.

If the contract is unclear, tests should force the contract to be named.

### Public Behaviour Over Private Mechanics

A passing test suite should mean the product still behaves correctly. It should
not mean that a private helper still happens to arrange its loops the same way.

Tests may use focused fixtures and test doubles, but the assertion target must be
observable behaviour or an intentional contract.

### Test Pressure Is Design Feedback

Hard-to-test code is a signal. The response must be architectural thought, not
visibility bypasses or test-only hooks.

Acceptable responses include:

- extract a real interface or value object
- test at a higher integration level
- split an oversized component into product-meaningful pieces
- clarify the public contract
- leave incidental implementation covered through caller behaviour

Unacceptable responses include:

- `InternalsVisibleTo`, friend assemblies, or reflection hacks for test access
- public APIs that exist only for tests
- test-only production flags
- assertions that lock down incidental implementation order
- mock-heavy tests that prove the mock setup rather than product behaviour

---

## Organization

ADR-001 established the preferred model for contract suites that must run against
multiple implementations: one abstract contract suite, physically decomposed via
`partial` files, with one concrete fixture per implementation.

That model must be reused when the same behavioural contract must be proven
against more than one implementation.

Examples:

- backend contract suites across LLVM, native, WebAssembly, or interpreter-backed
  backends
- platform contract suites across OS abstractions
- language-service contract suites across transport or editor hosts, if such
  hosts emerge

The file system may organize test concerns. The type system should only encode
semantic distinctions.

---

## Test Categories

### Semantic Tests

Semantic tests validate language meaning: types, diagnostics, control flow,
module behaviour, generics, and compile results. These tests should be stable
across parser or internal representation changes unless the language contract
itself changes.

### Contract Tests

Contract tests validate interfaces such as `IBackend` or future comparable
product contracts. They should be reusable across implementations without
duplicating test logic.

### Integration Tests

Integration tests validate collaboration between real components: compiler
pipeline, LSP handlers, tools, filesystem layout, and package/runtime behaviour.
They are appropriate when correctness emerges from the interaction.

### Regression Tests

Every fixed bug that could plausibly return should leave a regression test at the
lowest level that still exercises the real contract.

### Golden Tests

Golden or snapshot-style tests are allowed only when the output is a true product
artifact whose full shape matters. They must not become a substitute for semantic
assertions.

---

## Anti-Patterns

The following are prohibited or strongly discouraged:

- `InternalsVisibleTo`, friend assemblies, or reflection hacks for test access
- test-only production APIs
- public setters or mutable state added only for tests
- mocking the unit under test's direct implementation details
- tests that assert private method sequencing
- duplicate suites per implementation when a shared contract suite is possible
- broad snapshot tests for outputs where only a few semantic properties matter
- brittle line/column assertions unless source location is the feature under test
- relying on test execution order

---

## Review Standard

New tests should answer these questions:

- What product contract or observable behaviour does this protect?
- Would a real consumer care if this test failed?
- Can the implementation be refactored without rewriting the test?
- Is the test at the lowest level that still preserves honest coverage?
- Does any new abstraction exist for production reasons?

If the answer is unclear, the test design should be revisited.

---

## Consequences

- Test projects should mirror product contracts, not implementation folders by
  reflex.
- New implementation variants should reuse contract suites rather than fork them.
- Test helper abstractions must remain small and honest.
- Production code must not grow testing backdoors.
- Refactors that improve testability by clarifying real boundaries are preferred
  over privileged access or brittle assertions.
- Coverage percentage is secondary to contract coverage and regression value.

---

## Relationship to ADR-001 and ADR-015

ADR-001 is the concrete precedent: backend tests were corrected by moving from
implementation-scoped tests to contract-scoped tests.

ADR-015 establishes the boundary rule: tests may not use `InternalsVisibleTo` to
reach production internals.

This policy combines those ideas into the repository-wide testing standard.
