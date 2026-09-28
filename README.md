# MachineEnglish (ME)

### The Language of Intent

> What the code says is exactly what it does, and anyone in the room can read it.

![Windows](https://img.shields.io/badge/Windows-x64-blue?logo=windows)
![Linux](https://img.shields.io/badge/Linux-x64-orange?logo=linux)
![macOS](https://img.shields.io/badge/macOS-arm64-black?logo=apple)
![Android](https://img.shields.io/badge/Android-arm64%20cross--compile-green?logo=android)

MachineEnglish is an intent-first programming language and toolchain for readable,
contract-driven domain logic. It is built around a simple claim: software should
be precise enough for machines, honest enough for compilers, and readable enough
for the people who actually understand the domain.

This repository is the public philosophy and design record for MachineEnglish: the
ambitions, the language design decisions, and the principles that distinguish ME
from the languages that came before it.

## Table of Contents

- [What ME Is](#what-me-is)
- [The Ambition](#the-ambition)
- [Why ME Is Different](#why-me-is-different)
- [SOLIDE And Spec Contracts](#solide-and-spec-contracts)
- [Feature Comparison](#feature-comparison)
- [Current Status](#current-status)
- [Test Coverage](#test-coverage)
- [CI And Artifacts](#ci-and-artifacts)
- [Tooling Status](#tooling-status)
- [LSP Status](#lsp-status)
- [Testing Policy](#testing-policy)
- [Policies And Templates](#policies-and-templates)
- [Roadmap](#roadmap)
- [Architecture Decisions](#architecture-decisions)
- [Major Decisions](#major-decisions)

---

## What ME Is

ME is a dual-representation language. Every major construct can be expressed in
plain English form or developer shorthand. Neither form is second-class.

```me
// English form
Add (a as integer, b as integer) [
    be a plus b
]

// Developer form
Add (a as integer, b as integer) => a plus b
```

That matters because most languages are only readable by developers. ME is trying
to make the implemented business rule readable by the analyst, operator, tester,
or product owner who knows whether the rule is actually true.

ME is currently strongest as a backend and domain-modelling language. The working
foundation includes the compiler pipeline, LLVM backend, tree-walk interpreter,
standard library wrappers, native runtime, LSP server, VS Code integration, MCP
tooling, and cross-platform CI.

---

## The Ambition

*We are allowed to be ambitious. This section says the quiet parts out loud.*

### The Honest Starting Point

Every programming language that changed the world was built by people who believed
they had seen something others hadn't. Dijkstra saw structured programming when
everyone was writing GOTOs. Wirth saw simplicity when C was considered clean.
Gosling saw platform independence when every platform was a silo. They were right.
The world caught up.

We have seen something.

Not one thing. Several things at once — and the fact that they combine is what
makes this different from another language project.

### What We Have Seen

#### 1. The readability wall has never been broken — only decorated

Every language since COBOL has claimed to be readable. What they mean is: readable
by developers. Python is readable by developers. Ruby reads like poetry — to
developers. TypeScript is an improvement — for developers.

None of them are readable by the person who actually knows the domain. The finance
analyst who knows exactly what a margin call means cannot read the code that
computes it. The operations team that wrote the business rules cannot verify the
implementation. The domain expert and the implementation exist in permanently
separate worlds, connected only by documents that go stale and meetings that end
in misunderstanding.

ME breaks the wall. Not by making developers write more verbose code — by giving
the same construct two valid surface forms simultaneously. The developer writes in
techie form. The domain expert reads in English form. Neither compromises. Neither
translates. The code is the documentation and the documentation is the code.

This has never been done before. Not like this.

#### 2. Null is a forty-year-old mistake that everyone accepted

Tony Hoare called it his billion-dollar mistake. He was being conservative. The
null pointer exception is the most common runtime error in the history of software.
Every language since Algol has inherited it. Rust got close to eliminating it.
Kotlin made it opt-in. Swift made it visible.

ME makes it structurally impossible. Not by convention. Not by linter rule. Not by
discipline. By design. A variable either has a value or it doesn't — and the
compiler tracks which. There is no third state. There is no way to accidentally
propagate a null through ME code because the concept does not exist in the language.

Forty years of null pointer exceptions. ME ends them.

#### 3. Failure has always been a lie

Functions lie. They say they return an integer. They actually return an integer, or
throw an IOException, or throw a NullPointerException, or throw a StackOverflowError,
or silently return -1 as a sentinel, or swallow an exception and return 0. The
signature is fiction. The reality is discovered at runtime, in production, at 3am.

ME puts failure in the signature. Every caller knows exactly what can go wrong
before they call. `fails` for expected, recoverable business failures. `throws` for
exceptional, programmer-error failures. The compiler enforces it. The tooling
understands it. The domain expert reading the English form can see it.

This is how software should have always worked.

#### 4. Rendering has been doing it wrong for decades

The painter's algorithm — draw back to front, overwrite pixels repeatedly — is so
ingrained that nobody questions it. Every game engine, every browser, every UI
framework does it this way. It is accepted as the cost of rendering.

We invented a better way. Patented it. Proved it — 735% faster entity creation,
2323% faster animated rendering. Front-to-back, single buffer, every pixel written
exactly once. Object identifiers stored per pixel, making hit-testing O(1) instead
of a traversal.

This is not an optimisation of the existing model. It is a different model entirely.

#### 5. The single window changes everything about where software runs

iOS gives you one window. iPadOS gives you one window. Every serious development
environment in history has required more than one window, so none of them run
natively on the platforms that billions of people actually carry with them every day.

Our virtual window system — built on the patented rendering model — runs an entire
multi-window environment inside a single OS window handle. The OS sees one window.
The user sees a full desktop. This works on iOS. On iPadOS. On any constrained
platform that provides a single surface.

Nobody has done this because nobody had the rendering model to do it efficiently.
We do.

### What This Combination Unlocks

Take these five things together and the picture that emerges is not "a better
programming language." It is something larger.

**ME is a complete software civilisation stack.**

- A language that domain experts and developers share
- A compiler that targets native binary, WebAssembly, and IaC simultaneously
- An interop layer that absorbs .NET, JVM, and npm without rewriting anything
- A migration path that converts existing code mechanically
- A rendering engine that runs everywhere, faster than anything currently shipping
- A windowing system that brings full development environments to platforms that
  have never had them
- A development environment — ME Dev Studio — that runs on the device in your bag

Each of these individually is a product. Together they are a platform.

| Problem accepted by the industry | ME's answer |
|---|---|
| Domain experts cannot read production code. | Dual English/techie syntax makes the code itself readable as the business rule. |
| Null is normal. | No null exists in the language; validity is tracked explicitly. |
| Failure is hidden until runtime. | `fails` and `throws` are declared in callable contracts. |
| Existing ecosystems block adoption. | Wrapper generators and migration tools let ME absorb .NET, JVM, npm, C#, Java, TypeScript, YAML, CSS, and HTML paths incrementally. |
| Rendering writes the same pixel repeatedly. | GWS targets the patented front-to-back model. |
| Serious tools require desktop-style windowing. | Virtual windows over one host surface are a planned route to full development environments on constrained platforms. |

| Layer | Intent |
|---|---|
| Language | Domain experts and developers share one readable source of truth. |
| Compiler | ME targets native binaries today, with WebAssembly and IaC on the roadmap. |
| Interop | Wrapper generators absorb .NET, JVM, and npm ecosystems instead of asking teams to rewrite everything. |
| Migration | Assisted C#, Java, TypeScript, YAML, CSS, and HTML migration removes the mechanical 80 percent. |
| Rendering | GWS targets a patented front-to-back renderer and virtual windowing model. |
| Dev environment | ME Dev Studio is the long-range goal: real development on constrained devices, including single-window platforms. |

### The Developer on the iPad

Picture a developer in 2027. She opens her iPad on a train. She opens ME Dev
Studio — a full development environment, running natively, no remote desktop, no
cloud dependency, no latency. She has her project open. The LSP is giving her
completions, showing her type errors, navigating her to definitions. She compiles.
The binary runs on-device in the simulator. She ships a fix.

This is not science fiction. Every component that makes this possible either exists
today or is the direct next step from what exists today. The LSP is shipped. The
compiler is cross-platform. The platform abstraction is done. The rendering engine
is patented and proven. The virtual window system is the next implementation
milestone.

The developer on the iPad is not a distant vision. She is eighteen to twenty-four
months of focused work away.

### The Enterprise Path

The reason most new languages fail in enterprise is not technical. It is ecosystem.
A CTO at a bank will not approve a rewrite of two million lines of C# into a
language that cannot consume their existing NuGet packages. Full stop.

ME's wrapper generator dissolves this objection. `me-tools wrap dotnet <package>`
generates idiomatic ME wrappers for any NuGet package. The .NET ecosystem — thirty
years of libraries, frameworks, and institutional knowledge — becomes available to
ME code immediately. Same for JVM. Same for npm via TypeScript definitions.

ME does not ask enterprise teams to start over. It asks them to start one service.
Then another. The wrapper generator means they keep everything they already have
while they do it.

This is the difference between a language project and a language adoption.

### The World ME Is Building Toward

A world where:

- The analyst who understands the domain can read the code that implements it —
  and catch the bugs that come from misunderstanding
- Production outages caused by null pointer exceptions are a historical artefact,
  like segfaults in managed languages
- Infrastructure configuration is type-checked before it is deployed, because it
  is written in the same language as the business logic
- Development tools run on the device in your pocket, not tethered to a cloud
  service or a desktop machine
- A team in France and a team in Japan work on the same codebase in their own
  languages, with deterministic round-trip translation, and the compiler never
  sees anything but the canonical form
- The rendering model in your mobile app is patented and measurably faster than
  everything it replaced

This is not utopia. It is a specific, achievable technical roadmap with a working
foundation, a proven rendering patent, and a clear path from here to there.

### On Being Ambitious

Ambition in technology is not arrogance. It is the refusal to accept that the way
things are is the way things must be.

Null exists because it was convenient in 1965. We are not in 1965.

The painter's algorithm exists because nobody had a better idea when it was
standardised. We have a better idea. We patented it.

Development tools don't run on iPads because the OS windowing model doesn't
permit it. Our windowing model doesn't need the OS.

Domain experts can't read production code because nobody built a language that
seriously tried to close that gap. We are closing it.

Every one of these problems has been accepted for so long that solving them feels
presumptuous. It isn't. It's just work.

We are doing the work.

---

## Why ME Is Different

| Principle | ME position |
|---|---|
| Readability | English and techie forms are both valid surface syntax for the same construct. |
| Nulls | ME has no null, nil, undefined, or None. A value is either assigned and ok, or it is not. |
| Failure | `fails` and `throws` are declared in the callable contract. Failure is visible before runtime. |
| Domain types | Finance, text-validation, choices, temporal, collections, and optional wrappers are first-class targets. |
| Inheritance | Multiple inheritance is deterministic, explicit, and conflict-aware. |
| Contracts | `spec` declarations stay pure: they declare requirements, not executable defaults. |
| Capabilities | Shared behavior belongs in named `capability` blocks that are explicit, inspectable, and conflict-checked. |
| Output values | Output parameters do not require noisy call-site ceremony. |
| Translation | English is the default canonical grammar, but the surface language is designed for deterministic natural-language translation. |
| Migration | Existing ecosystems are treated as inputs, not enemies. |

---

## SOLIDE And Spec Contracts

### The Indictment

The software industry has been lying to you.

Not through malice. Through comfort. Through the slow institutional amnesia that
sets in when a wrong decision becomes old enough to be called a convention.

For two decades, the industry has taught SOLID as gospel. Interface Segregation.
Liskov Substitution. Contracts, not implementations. Every bootcamp. Every
certification. Every design patterns book written since 2003.

And for that same two decades, every major language — including the one that helped
popularise SOLID — has quietly, systematically violated those principles, added
features to paper over the violations, and continued teaching SOLID with a straight
face.

MachineEnglish is done pretending this didn't happen.

### Exhibit A: `System.Array`

This is not a fringe case. This is not a legacy codebase from a team that didn't
know better. This is `System.Array` — the most fundamental collection type in the
entire .NET runtime, written by the people who built C#, used in every .NET program
ever written.

```csharp
// This compiles. The type system says this is valid.
IList<int> list = new int[] { 1, 2, 3 };

// This also compiles. The type system says this is valid.
list.Add(4);

// This is what actually happens at runtime:
// System.NotSupportedException: Collection was of a fixed size.
```

`System.Array` implements `IList<T>`. `IList<T>` declares `Add`, `Remove`,
`Insert`, `Clear`. Arrays cannot grow. Arrays cannot shrink. They never could.
Everyone knows this.

And yet `Array` implements `IList<T>` and throws `NotSupportedException` at runtime
when you call the methods the interface promises are there.

**That is not an interface. That is a lie the type system agreed to tell.**

It violates Liskov: you cannot substitute `Array` wherever `IList<T>` is expected
without the risk of runtime failure. That is the textbook definition of an LSP
violation.

It violates Interface Segregation: `IList<T>` is too fat. It bundled read
capability and write capability into one contract, then forced fixed-size
collections to implement the write half as a runtime exception.

Microsoft's official guidance? Check `IsFixedSize` before calling mutation methods.
That is an admission of guilt dressed as documentation.

And then — and this is where the hypocrisy becomes architectural — Microsoft *knew*
the correct design. In .NET 4.5 (2012), they introduced `IReadOnlyList<T>` and
`IReadOnlyCollection<T>`. That was them saying, in public: *the original design was
wrong and here is the correct split.* But they left `Array : IList<T>` in place.
Backwards compatibility mattered more than honesty.

### The Defense That Isn't

In 2023, a conversation was had with a leading AI system about exactly this
problem. Its first instinct was to defend `Array : IList<T>`. Historical reasons.
Compatibility. "Arrays do align with the read-only aspect." It took sustained
pressure to get an admission that the design causes "confusion" and "surprise."

Notice what it never said: *this was wrong from the beginning.*

The fact that the most capable AI system in the world — trained on every textbook,
every tutorial, every Stack Overflow answer ever written — initially defended the
lie reveals something important: **the lie has been told so many times it became
the truth.** The industry's institutional memory encoded the mistake as a
convention.

### The Correct Design Was Already Known — And Already Built

The correct design is not difficult. You need ISP applied honestly:

```csharp
ICount                           // just Count. One property.
IIndexer<T> : ICount             // read-only indexed access.
IxIndexer<T> : IIndexer<T>       // read-write indexed access.
IAdd<T>                          // Add and Insert. Nothing else.
IRemove<T>                       // Remove, RemoveAt, RemoveLast. Nothing else.
IAddRange<T>                     // AddRange. Nothing else.
IRemoveRange<T>                  // RemoveRange, RemoveAll. Nothing else.
ISwap<T>                         // Swap and Relocate. Nothing else.
IFind<TKey, TItem>               // Find, FindIndex, Exists, Contains.
IIndexOf<T>                      // IndexOf. Nothing else.
IReplace<T>                      // Replace. Nothing else.
IExist<T>                        // Exists. Nothing else.
IReverseIEnumerable<T>           // InReverse. Nothing else.
IResizeUnit                      // ResizeUnit, MakeZeroLength.
```

Then, for an array — a fixed-size collection with indexed read and write access:

```csharp
MyArray<T> : IReadOnlyCollection<T>, IxIndexer<T>
```

No `IAdd<T>`. No `IRemove<T>`. No `NotSupportedException`. No lies. The type tells
the truth about what it can do because the contracts it implements are exactly as
large as they need to be and no larger.

This design is not theoretical. It has existed in production since 2016 — three
years before C# 8 introduced Default Interface Methods, and four years before
Microsoft added `IReadOnlyList<T>` as a public admission that the original BCL
design was flawed. The correct answer was not found after the mistake was
acknowledged. It was built independently, from first principles, before the mistake
was publicly admitted.

### What The Industry Did Instead

C# 8.0 (2019) introduced Default Interface Methods. The argument was API evolution:
widely-implemented interfaces need to grow without forcing every implementer to
update. The mechanism: put default implementations inside the interface itself.

This is not a solution to the `IList<T>` problem. It is a tool for avoiding the
solution. Instead of decomposing fat interfaces into honest atomic contracts, you
patch them with fallback behaviour and tell implementers they can ignore the new
methods. The interface gets fatter. The lie gets quieter.

Kotlin has protocol extensions. Swift has extension functions on interfaces. Rust
has trait default implementations. Every modern language made the same trade-off:
ergonomics over contract purity. And every one of them kept teaching SOLID.

That is the hypocrisy. Not that the trade-offs were wrong. The hypocrisy is the
refusal to say plainly: *SOLID as originally formulated is no longer the complete
description of what we actually do.*

### SOLIDE: An Honest Successor

MachineEnglish proposes **SOLIDE**.

The extra `E` is deliberate.

#### S — Single Role Per Construct

*Original: A class should have one reason to change.*

Restated: **A language construct should have one architectural role.**

- `spec` declares requirements. Only requirements.
- `capability` delivers shared behaviour. Only shared behaviour.
- `entity` owns concrete instance behaviour and state.
- `block` carries reusable structure.

If a language construct does two architectural jobs at once, the rest of the
acronym cannot save it. This is exactly how `IList<T>` became a lie: one interface
tried to represent readable collection, indexed collection, growable collection,
shrinkable collection, and mutable collection at the same time.

#### O — Open Ecosystem, Stable Contract

*Original: Open for extension, closed for modification.*

Restated: **A contract is closed to structural change. Its ecosystem is open to
explicit, named extension.**

A spec does not need to change every time useful derived behaviour is discovered.
That behaviour lives in named capabilities. The spec remains stable. The ecosystem
grows. Extension is declared, named, conflict-detected, and visible to any reader
of the code.

This is more precise than the original formulation, which said nothing about *how*
extension should work. That silence became an escape hatch. The industry filled the
gap with extension methods and default interface members, then pretended the old
rules still described the new reality.

#### L — Verified Substitutability

*Original: Subtypes must be substitutable for their base types.*

Restated: **Substitutability is a compiler-verified conformance fact, not a design
aspiration.**

In MachineEnglish, an entity either satisfies a spec or it does not. The compiler
decides. There is no runtime discovery. There is no `NotSupportedException` for
methods you promised but could not honour. There is no `IsFixedSize` property you
must check before trusting the contract.

If you implement a spec, you implement all of it. The compiler does not accept
partial conformance dressed as full conformance.

This is the principle that `Array : IList<T>` violated. MachineEnglish makes that
specific class of violation structurally impossible.

#### I — Minimal Contracts, Composable Behaviour

*Original: Clients should not be forced to depend on methods they do not use.*

Restated: **Contracts declare the smallest meaningful set of requirements. Shared
behaviour that derives from those requirements is composed explicitly, not
inherited silently.**

`IList<T>` failed ISP because it was not minimal. It combined read access, indexed
access, mutation, insertion, and removal in one contract. A fixed-size collection
that honestly only supports read access and indexed write access has no contract it
can implement without lying.

The correct design — `ICount`, `IIndexer<T>`, `IxIndexer<T>`, `IAdd<T>`,
`IRemove<T>` as separate atomic contracts — is ISP applied honestly. Collections
declare exactly which capabilities they support. Nothing more.

This is where the hypocrisy must be named plainly: a fat interface with runtime
exceptions is not a "pragmatic trade-off" when a clean atomic design is available.
It is a failed contract protected by backwards compatibility and defended by
convention.

#### D — Depend On Contracts, Not Constructs

*Original: Depend on abstractions, not concretions.*

Restated: **Depend on verified contracts and named capability ecosystems, not
concrete implementation details.**

MachineEnglish strengthens this by making contracts the primary architectural
surface and requiring shared behaviour to be expressed against contracts through
named capabilities rather than against concrete types. You cannot accidentally
depend on an implementation detail through a capability because capabilities attach
to specs, not to entities.

#### E — Explicit Extension

*New principle. No SOLID equivalent.*

**All behavioural extension to a contract must be named, declared, inspectable,
and conflict-checked. Nothing may silently attach behaviour to a contract.**

This is the principle the original SOLID did not have. It is the one that would
have prevented extension methods on interfaces from being silent, would have forced
Default Interface Methods to require explicit opt-in naming, and would have made
the `Array : IList<T>` relationship impossible to compile unless `Array` could
actually honour `IList<T>`'s full contract.

In MachineEnglish:
- A capability that `affects` a spec must be named.
- It must be explicitly declared.
- If two capabilities define the same function for the same target, the compiler
  rejects the ambiguity.
- The entity always wins; base types win over capabilities; capabilities fill
  declared gaps.
- Nothing is silent. Nothing is accidental. Nothing is inherited without a name
  the reader can trace.

### MachineEnglish Walks The Talk

It is easy to write a manifesto. It is harder to build a language that enforces
what the manifesto claims.

MachineEnglish has been building this since before the manifesto was written.

**2016–2018** — Atomic interface decomposition is built in production C# code:
`ICount`, `IIndexer<T>`, `IxIndexer<T>`, `IAdd<T>`, `IAddRange<T>`,
`IInsertRange<T>`, `IRemove<T>`, `IRemoveRange<T>`, `ISwap<T>`,
`IFind<TKey, TItem>`, `IIndexOf<T>`, `IReplace<T>`, `IExist<T>`,
`IReverseIEnumerable<T>`, `IResizeUnit`. Each interface carries exactly one
responsibility. None of them could produce a `NotSupportedException` because none
of them are too fat for any honest implementer to carry.

**2019** — C# 8 ships Default Interface Methods. The industry moves further from
atomic contracts.

**2023** — The industry's canonical AI system initially defends `Array : IList<T>`
with "historical reasons" rather than calling it what it is. Sustained argument
eventually produces acknowledgment that the atomic design is better. The
institutional memory of the industry encodes the mistake as convention.

**2026** — MachineEnglish's spec system mirrors this interface hierarchy as
first-class language constructs. The compiler's conformance analyser enforces that
every entity implementing a spec must honour the full contract. There is no
`NotSupportedException` path. There is no `IsFixedSize` escape hatch. There is no
partial conformance the type system politely ignores.

The difference is not only design philosophy. The difference is enforcement. SOLID
asked you to apply the principles correctly. MachineEnglish's compiler refuses to
compile the violations.

**You cannot write an ME equivalent of `Array : IList<T>`. The language will not
let you.**

### The Interview Question That Should Not Exist

*"Do you know SOLID? Do you follow SOLID principles?"*

Every senior engineering interview. Every principal engineer screening. Every
architecture review checklist. The question sits there like a badge of rigour,
asked by people who expect a particular answer and would be genuinely alarmed if
they got an honest one.

The honest answer is: yes. I know SOLID well enough to know that the people asking
this question usually don't. Not because they are unintelligent. Because they
learned SOLID from the same bootcamps, certifications, and design patterns books
that were written by people who had already normalised the violations. They learned
SOLID from a culture that ships `Array : IList<T>` and then teaches ISP in the
same breath without noticing the contradiction.

What they are actually asking is: *can you recite the five letters and say
something plausible about each one?*

The urge — suppressed in every interview — is to turn the question around:

**Do YOU know SOLID?**

Do you know that the standard library of the language you use every day has been
violating Liskov and Interface Segregation since 2002 with `Array : IList<T>`? Do
you know that Microsoft's official response to that violation is to tell developers
to check `IsFixedSize` before trusting the interface — to ask the contract if it is
lying before relying on it? Do you know that C# 8 Default Interface Methods — the
feature your team probably celebrated as modern and expressive — move the language
further from correct interface design, not closer? Do you know that the correct
design was already built, in production, in 2016, by someone you might be
interviewing today?

The pun is not accidental. They want a *principal* engineer with *solid* engineering
skills. The word and the acronym sit in the same sentence and nobody laughs. SOLID,
the adjective, means reliable, dependable, built on a firm foundation. SOLID, the
acronym, was supposed to mean the same thing for software design. The industry took
the acronym, stripped it of the rigour that gave the adjective its meaning, turned
it into an interview checklist, and kept using the word *solid* as if the two
things were still connected.

They are not.

An engineer who has truly internalised SOLID — not memorised it, internalised it —
will see the BCL collections hierarchy and feel uncomfortable. They will see
`IList<T>` and immediately ask: why does this include `Add` and `Remove`? They will
see `Array : IList<T>` and immediately ask: how can a fixed-size collection honestly
implement a mutable list contract? They will not accept "historical reasons" as an
answer because historical reasons are not a design principle.

That discomfort is the signal. Most people who confidently answer "yes, I follow
SOLID" have never felt that discomfort.

The person building a language that makes those violations structurally uncompilable
does not answer "do you know SOLID?" with a five-bullet recitation.

They answer it with a compiler.

---

## Feature Comparison

| Feature | ME | Python | TypeScript | C# | Java | Rust |
|---|---|---|---|---|---|---|
| Dual English/techie syntax | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Domain experts can read code | ✅ | partial | ❌ | ❌ | ❌ | ❌ |
| Built-in fintech domain types | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Built-in text validation sugars | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Built-in choice types | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| No nulls, validity model only | ✅ | ❌ | ❌ | partial | ❌ | ✅ |
| No default value horror | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Failure declared in signature | ✅ | ❌ | ❌ | partial | partial | ✅ |
| Output params without call-site modifier | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Universal binding keyword | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Deterministic multiple inheritance | ✅ | complex | ❌ | ❌ | ❌ | ❌ |
| Named tuple partial selection | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Spec contracts enforced by compiler | ✅ | ❌ | ❌ | ❌ | ❌ | partial |

---

## Current Status

| Area | Status | Notes |
|---|---:|---|
| Core contracts and public interfaces | ✅ Shipped | Shared public API, diagnostics, interfaces, language content. |
| Compiler pipeline and semantic engine | ✅ Shipped | Parse, AST, semantic analysis, compile options, LSP query helpers. |
| LLVM backend | ✅ Shipped | Native LLVM facade and backend implementation. |
| Backend contract tests | ✅ Shipped | Contract suite pattern established. |
| Tree-walk interpreter | ✅ Shipped | Runtime execution path independent from LLVM. |
| Platform abstraction | ✅ Shipped | OS/toolchain abstraction layer. |
| CLI tooling | ✅ Shipped | Compile, grammar/bootstrap tools, MCP, Roslyn helper, VS Code packaging, LSP launcher. |
| LSP server | ✅ Shipped | stdio OmniSharp language server. |
| LSP protocol tests | ✅ Shipped | Black-box JSON-RPC tests against the built server. |
| VS Code extension | ✅ Shipped | File icons, TextMate grammar, language configuration, LSP client packaging. |
| ME standard library wrappers | ✅ Shipped | Collections, text, IO, process/sys, net, math, crypto, encoding, temporal, optional, graphics. |
| Native C runtime | ✅ Shipped | Runtime support for text, collections, IO, process, network, crypto, bitmap, etc. |
| MCP server | ✅ Shipped | Local tool server for code intelligence workflows. |
| Architecture docs | ✅ Shipped | ADRs, roadmaps, handoff briefs. |
| C# to ME migration work | 🚧 In progress | Migration policy and feasibility documents are being developed. |

---

## Test Coverage

| Test Project | Scope | Tests | Status |
|---|---|---:|---:|
| ME.IR.Tests | Compiler semantics and language rules | 1,868 | ✅ Active |
| ME.Language.Tests | Language definition/lexer/parser metadata | 37 | ✅ Active |
| ME.Backend.Tests | Backend contract suite (abstract base) | — | ✅ Active |
| ME.LLVM.Tests | LLVM backend binding to backend contracts | 560 | ✅ Active |
| ME.Interpreter.Tests | Interpreter behavior | 138 | ✅ Active |
| ME.Platforms.Tests | Platform abstraction | 21 | ✅ Active |
| ME.LSP.Tests | Black-box LSP protocol | 3 | ✅ Active |
| | **Total** | **2,627** | |

---

## CI And Artifacts

ME has a working end-to-end compiler proven on Windows x64, Linux x64, macOS
arm64, and Android arm64 cross-compilation. It compiles ME source to native
binaries via LLVM and is verified by CI on every push.

| Platform | Runner | Tests | Artifact |
|---|---|---|---|
| Windows x64 | `windows-latest` | ✅ | `me-tools-win-x64` |
| Linux x64 | `ubuntu-latest` | ✅ | `me-tools-linux-x64` |
| macOS arm64 | `macos-14` | ✅ | `me-tools-osx-arm64` |
| Android arm64 | `ubuntu-latest` with NDK r27c | ✅ smoke | `me-tools-android-arm64` |

Self-contained `me-tools` binaries are published as GitHub Actions artifacts on
every CI run with 30-day retention.

Recently shipped:

| What | Detail |
|---|---|
| Native LLVM backend | Replaced third-party .NET LLVM bindings with a lean C API facade and direct P/Invoke into `libLLVM`. |
| Cross-platform CI | GitHub Actions matrix covers Windows x64, Linux x64, and macOS arm64. |
| Android arm64 target | `OsPlatform.Android`, `AndroidToolchain`, NDK r27c, target `aarch64-linux-android33`, with CI smoke tests. |
| ME stdlib wrappers | Collections, Text, Math, Encoding, Crypto, Optional, Temporal, IO, Sys, Net, and Graphics wrappers over native runtime support. |
| Self-contained artifacts | Platform-specific `me-tools` artifacts are published by CI. |
| `extern fn` plus entity proof | Entity methods can call C ABI extern declarations, which is the foundation for stdlib wrappers. |

---

## Tooling Status

| Tooling capability | Status | Notes |
|---|---:|---|
| Compile `.me` files | ✅ Shipped | CLI entry point into the compiler/backend pipeline. |
| LSP server | ✅ Shipped | stdio server used by editor integrations. |
| Roslyn code intelligence | ✅ Shipped | Used for C# symbol analysis. |
| MCP setup/start | ✅ Shipped | Local machine intelligence server integration. |
| VS Code packaging/install | ✅ Shipped | Packages extension assets, grammar, language config, and client code. |
| ANTLR generation | ✅ Shipped | Current grammar toolchain support. |
| Lexer/parser TSV import/export | ✅ Shipped | Supports grammar migration/bootstrap work. |
| Lexer/parser/semantic bootstrap | ✅ Shipped | Supports self-hosting and grammar/spec audit work. |
| Document index generator | 🔜 Planned | Next tool task: regenerate public and internal document index pages automatically. |

---

## LSP Status

| Capability | Status | Notes |
|---|---:|---|
| Document sync | ✅ Shipped | Open/change/save/close wiring through DocumentStore. |
| Diagnostics | ✅ Shipped | Compiler diagnostics published on open/save and live change debounce. |
| Hover | ✅ Shipped | Uses compiler position queries. |
| Go to definition | ✅ Shipped | Same-file and cross-file declaration resolution. |
| Completion | ✅ Shipped | Keywords, current file declarations, and directly loaded module declarations. |
| Signature help | ✅ Shipped | ME square-bracket callable signatures. |
| Find references | ✅ Shipped | Text-token workspace scan with identifier fallback. |
| Rename | ✅ Shipped | Text-token workspace edit with prepareRename and identifier validation. |
| Syntax highlighting | ✅ Shipped | TextMate grammar in VS Code extension. |
| Semantic tokens | 🔜 Planned | Should layer on top of compiler-aware symbol resolution. |
| Code actions | 🔜 Planned | Candidate next LSP capability. |

---

## Testing Policy

Testing is strict by design:

- Tests must target public contracts or observable behavior.
- Do not use `InternalsVisibleTo`.
- Do not add production APIs only for tests.
- If test pressure exposes a bad boundary, improve the product boundary.

These rules are captured in ADR-015 (No InternalsVisibleTo For Tests) and
ADR-016 (Strict Testing Policy).

---

## Policies And Templates

The cross-project engineering policies and document templates for all
MachineEnglish repositories live here:

- [Policies](Policies) - engineering, SOLIDE design, naming, testing,
  documentation, drift-resolution, work-done, and migration policies.
- [Templates](Templates) - ADR, DFR, MAD, MD, RDMP, DIP, Specification, and
  Semantic-rule document templates.

This repository is their canonical cross-project home. Consuming repositories
(MachineEnglish, MachineEnglish-Native) are assumed to be checked out as
siblings and reference these folders by relative path. Snapshot refreshed
2026-07-19 from the MachineEnglish repository; the copies there remain in place
until that repository switches its links.

---

## Roadmap

| Phase | Feature | Status |
|---|---|---:|
| Now | Backend core: types, callables, failure, entities | ✅ Shipped |
| Now | Hello World end-to-end: LLVM pipeline | ✅ Shipped |
| Now | Multi-file compilation, `load` resolution, `.meproject` | ✅ Shipped |
| Now | Generics monomorphization | ✅ Shipped |
| Now | Error channels (`fail`/`throw`) end-to-end | ✅ Shipped |
| Now | Pattern matching (`decide`) full lowering | ✅ Shipped |
| Now | Diagnostics with file path and caret snippets | ✅ Shipped |
| Now | Tree-walk interpreter | ✅ Shipped |
| Now | `async`/`await` lowering | ✅ Shipped |
| Now | Debug information (DWARF / PDB) | ✅ Shipped |
| Now | Language Server Protocol (LSP) | ✅ Shipped |
| Now | Linux and macOS support | ✅ Shipped |
| Now | Native I/O, process, and network runtimes plus wrappers | ✅ Shipped |
| Now | `extern fn` ABI bindings and implicit void actions | ✅ Shipped |
| Now | Native LLVM backend with own C API facade | ✅ Shipped |
| Now | Cross-platform CI: Windows x64, Linux x64, macOS arm64 | ✅ Shipped |
| Now | Android arm64 cross-compilation target | ✅ Shipped |
| Now | Self-contained `me-tools` binaries published by CI | ✅ Shipped |
| Near | IaC compiler: ME to YAML | 🔜 Planned |
| Near | Standard library expansion in ME | 🔜 Planned |
| Near | Frontend: behavior-first UI | 🔜 Planned |
| Near | GWS: Graphics and Windowing System | 🔜 Planned |
| Near | Auto-generated document index pages | 🔜 Planned |
| Mid | Project scaffolding | 📋 Designed |
| Mid | Ecosystem interop: wrapper generation for .NET, JVM, npm | 📋 Designed |
| Mid | Migration tools: C#, Java, TypeScript, YAML, CSS, HTML to ME | 📋 Designed |
| Mid | Self-hosting bootstrap phases | 📋 Designed |

---

## Architecture Decisions

| ADR | Decision |
|---|---|
| ADR-001 | Backend test refactor |
| ADR-002 | Remove ANTLR and replace with pure C# lexer/parser |
| ADR-003 | C# as source of truth for language definitions |
| ADR-004 | `Word<T>` interned identity |
| ADR-005 | LLVM backend |
| ADR-006 | Remove Ubiquity native LLVM facade |
| ADR-007 | ME platform abstraction |
| ADR-008 | Generics monomorphization |
| ADR-009 | Bracketed generic syntax |
| ADR-010 | Else/otherwise unification |
| ADR-011 | Capability conflict resolution |
| ADR-012 | `me_runtime` C standard library |
| ADR-013 | LSP language server |
| ADR-014 | Syntax highlighting |
| ADR-015 | No `InternalsVisibleTo` for tests |
| ADR-016 | Strict testing policy |
| ADR-017 | C# to ME migration policy |

---

## Major Decisions

| Decision | Statement |
|---|---|
| MD-001 | Specs are pure contracts; default behavior belongs in named capabilities. |
| MD-002 | C# authored language definitions are a major project direction. |

---

*MachineEnglish: where intent becomes instruction, and instruction becomes code.*
*And one day, where the code is readable by everyone in the room.*
