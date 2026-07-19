<p align="left">
  <img src="..\..\..\MnMLogo.png" alt="MachineEnglish logo" width="48" />&nbsp;&nbsp;&nbsp;<strong>MachineEnglish(ME) - a language of intent</strong>
</p>
> **Credit:** Sourced from [MukeshAdhvaryu/SDLC-Governance](https://github.com/MukeshAdhvaryu/SDLC-Governance) and extended for this project.

# SOLIDE Design Policy

## The Indictment

The software industry has been lying to you.

Not through malice. Through comfort. Through the slow institutional amnesia that sets
in when a wrong decision becomes old enough to be called a convention.

For two decades, the industry has taught SOLID as gospel. Interface Segregation.
Liskov Substitution. Contracts, not implementations. Every bootcamp. Every
certification. Every design patterns book written since 2003.

And for that same two decades, every major language - including the one that helped
popularise SOLID - has quietly, systematically violated those principles, added
features to paper over the violations, and continued teaching SOLID with a straight
face.

MachineEnglish is done pretending this didn't happen.

## Exhibit A: System.Array

This is not a fringe case. This is not a legacy codebase from a team that didn't
know better. This is `System.Array` - the most fundamental collection type in the
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

That is not an interface. That is a lie the type system agreed to tell.

It violates Liskov: you cannot substitute `Array` wherever `IList<T>` is expected
without the risk of runtime failure. That is the textbook definition of an LSP
violation.

It violates Interface Segregation: `IList<T>` is too fat. It bundled read capability
and write capability into one contract, then forced fixed-size collections to
implement the write half as a runtime exception.

Microsoft's official guidance? Check `IsFixedSize` before calling mutation methods.
That is an admission of guilt dressed as documentation.

And then - and this is where the hypocrisy becomes architectural - Microsoft knew the
correct design. In .NET 4.5 (2012), they introduced `IReadOnlyList<T>` and
`IReadOnlyCollection<T>`. That was them saying, in public: the original design was
wrong and here is the correct split. But they left `Array : IList<T>` in place.
Backwards compatibility mattered more than honesty.

## The Defense That Isn't

In 2023, a conversation was had with a leading AI system about exactly this problem.
Its first instinct was to defend `Array : IList<T>`. Historical reasons.
Compatibility. "Arrays do align with the read-only aspect." It took sustained
pressure to get an admission that the design causes "confusion" and "surprise."

Notice what it never said: this was wrong from the beginning.

The fact that the most capable AI system in the world - trained on every textbook,
every tutorial, every Stack Overflow answer ever written - initially defended the lie
reveals something important: the lie has been told so many times it became the truth.
The industry's institutional memory encoded the mistake as a convention.

## The Correct Design Was Already Known - And Already Built

The correct design is not difficult. You need ISP applied honestly:

```
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

Then, for an array - a fixed-size collection with indexed read and write access:

```
MyArray<T> : IReadOnlyCollection<T>, IxIndexer<T>
```

No `IAdd<T>`. No `IRemove<T>`. No `NotSupportedException`. No lies. The type tells
the truth about what it can do because the contracts it implements are exactly as
large as they need to be and no larger.

This design is not theoretical. It has existed in production since 2016 - three years
before C# 8 introduced Default Interface Methods, and four years before Microsoft
added `IReadOnlyList<T>` as a public admission that the original BCL design was
flawed. The correct answer was not found after the mistake was acknowledged. It was
built independently, from first principles, before the mistake was publicly admitted.

## What The Industry Did Instead

C# 8.0 (2019) introduced Default Interface Methods. The argument was API evolution:
widely-implemented interfaces need to grow without forcing every implementer to
update. The mechanism: put default implementations inside the interface itself.

This is not a solution to the `IList<T>` problem. It is a tool for avoiding the
solution. Instead of decomposing fat interfaces into honest atomic contracts, you
patch them with fallback behaviour and tell implementers they can ignore the new
methods. The interface gets fatter. The lie gets quieter.

Kotlin has protocol extensions. Swift has extension functions on interfaces. Rust has
trait default implementations. Every modern language made the same trade-off:
ergonomics over contract purity. And every one of them kept teaching SOLID.

That is the hypocrisy. Not that the trade-offs were wrong. The hypocrisy is the
refusal to say plainly: SOLID as originally formulated is no longer the complete
description of what we actually do.

## SOLIDE: An Honest Successor

MachineEnglish proposes SOLIDE.

The extra **E** is deliberate.

---

### S - Single Role Per Construct

**Original:** A class should have one reason to change.

**Restated:** A language construct should have one architectural role.

- `spec` declares requirements. Only requirements.
- `capability` delivers shared behaviour. Only shared behaviour.
- `entity` owns concrete instance behaviour and state.
- `block` carries reusable structure.

If a language construct does two architectural jobs at once, the rest of the acronym
cannot save it. This is exactly how `IList<T>` became a lie: one interface tried to
represent readable collection, indexed collection, growable collection, shrinkable
collection, and mutable collection at the same time.

---

### O - Open Ecosystem, Stable Contract

**Original:** Open for extension, closed for modification.

**Restated:** A contract is closed to structural change. Its ecosystem is open to
explicit, named extension.

A spec does not need to change every time useful derived behaviour is discovered.
That behaviour lives in named capabilities. The spec remains stable. The ecosystem
grows. Extension is declared, named, conflict-detected, and visible to any reader of
the code.

This is more precise than the original formulation, which said nothing about how
extension should work. That silence became an escape hatch. The industry filled the
gap with extension methods and default interface members, then pretended the old
rules still described the new reality.

---

### L - Verified Substitutability

**Original:** Subtypes must be substitutable for their base types.

**Restated:** Substitutability is a compiler-verified conformance fact, not a design
aspiration.

In MachineEnglish, an entity either satisfies a spec or it does not. The compiler
decides. There is no runtime discovery. There is no `NotSupportedException` for
methods you promised but could not honour. There is no `IsFixedSize` property you
must check before trusting the contract.

If you implement a spec, you implement all of it. The compiler does not accept
partial conformance dressed as full conformance.

This is the principle that `Array : IList<T>` violated. MachineEnglish makes that
specific class of violation structurally impossible.

---

### I - Minimal Contracts, Composable Behaviour

**Original:** Clients should not be forced to depend on methods they do not use.

**Restated:** Contracts declare the smallest meaningful set of requirements. Shared
behaviour that derives from those requirements is composed explicitly, not inherited
silently.

`IList<T>` failed ISP because it was not minimal. It combined read access, indexed
access, mutation, insertion, and removal in one contract. A fixed-size collection
that honestly only supports read access and indexed write access has no contract it
can implement without lying.

The correct design - `ICount`, `IIndexer<T>`, `IxIndexer<T>`, `IAdd<T>`,
`IRemove<T>` as separate atomic contracts - is ISP applied honestly. Collections
declare exactly which capabilities they support. Nothing more.

This is where the hypocrisy must be named plainly: a fat interface with runtime
exceptions is not a "pragmatic trade-off" when a clean atomic design is available.
It is a failed contract protected by backwards compatibility and defended by
convention.

---

### D - Depend On Contracts, Not Constructs

**Original:** Depend on abstractions, not concretions.

**Restated:** Depend on verified contracts and named capability ecosystems, not
concrete implementation details.

MachineEnglish strengthens this by making contracts the primary architectural surface
and requiring shared behaviour to be expressed against contracts through named
capabilities rather than against concrete types. You cannot accidentally depend on an
implementation detail through a capability because capabilities attach to specs, not
to entities.

---

### E - Explicit Extension

**New principle. No SOLID equivalent.**

All behavioural extension to a contract must be named, declared, inspectable, and
conflict-checked. Nothing may silently attach behaviour to a contract.

This is the principle the original SOLID did not have. It is the one that would have
prevented extension methods on interfaces from being silent, would have forced Default
Interface Methods to require explicit opt-in naming, and would have made the
`Array : IList<T>` relationship impossible to compile unless `Array` could actually
honour `IList<T>`'s full contract.

In MachineEnglish:

- A capability that affects a spec must be named.
- It must be explicitly declared.
- If two capabilities define the same function for the same target, the compiler
  rejects the ambiguity.
- The entity always wins; base types win over capabilities; capabilities fill
  declared gaps.

Nothing is silent. Nothing is accidental. Nothing is inherited without a name the
reader can trace.
