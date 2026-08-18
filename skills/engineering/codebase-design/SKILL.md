---
name: codebase-design
description: Shared vocabulary for designing deep modules and cross-repository interfaces. Use when the user wants to design or improve a module's interface, find deepening opportunities, decide where a seam goes, make code testable, verifiable, or AI-navigable, reason about C/C++, embedded, runtime, or integration constraints, or when another skill needs the deep-module vocabulary.
---

# Codebase Design

Design **deep modules**: a lot of behaviour behind a small interface, placed at a clean seam and verifiable through observable contracts. A module may live in one repository or span coordinated repositories inside one logical context. The aim is leverage for callers, locality for maintainers, and verification at the environments that carry the relevant risk.

## Glossary

Use these terms exactly — don't substitute "component," "service," "API," or "boundary." Consistent language is the whole point.

**Module** — anything with an interface and an implementation. Deliberately scale-agnostic: a function, class, library, process, firmware partition, or cross-repository slice. _Avoid_: unit, component, service.

**Interface** — everything a caller, integrator, or verifier must know to use the module correctly: source and binary contracts, invariants, ownership, ordering, concurrency, timing, resource limits, error modes, required configuration, and hardware assumptions. _Avoid_: API, signature (too narrow — they refer only to part of the surface).

**Implementation** — what's inside a module, its body of code. Distinct from **Adapter**: a thing can be a small adapter with a large implementation (a flash-backed calibration store) or a large adapter with a small implementation (an in-memory fake). Reach for "adapter" when the seam is the topic; "implementation" otherwise.

**Depth** — leverage at the interface: the amount of behaviour a caller (or test) can exercise per unit of interface they have to learn. A module is **deep** when a large amount of behaviour sits behind a small interface, **shallow** when the interface is nearly as complex as the implementation.

**Seam** _(Michael Feathers)_ — a place where you can alter behaviour without editing in that place; the *location* at which a module's interface lives. Where to put the seam is its own design decision, distinct from what goes behind it. _Avoid_: boundary (overloaded with DDD's bounded context).

**Adapter** — a concrete thing that satisfies an interface at a seam. Describes *role* (what slot it fills), not substance (what's inside).

**Leverage** — what callers get from depth: more capability per unit of interface they learn. One implementation pays back across N call sites and M tests.

**Locality** — what maintainers get from depth: change, bugs, knowledge, and verification concentrate in one place rather than spreading across callers. Fix once, fixed everywhere.

## Deep vs shallow

**Deep module** = small interface + lots of implementation:

```
┌─────────────────────┐
│   Small Interface   │  ← Few methods, simple params
├─────────────────────┤
│                     │
│  Deep Implementation│  ← Complex logic hidden
│                     │
└─────────────────────┘
```

**Shallow module** = large interface + little implementation (avoid):

```
┌─────────────────────────────────┐
│       Large Interface           │  ← Many methods, complex params
├─────────────────────────────────┤
│  Thin Implementation            │  ← Just passes through
└─────────────────────────────────┘
```

When designing an interface, ask:

- Can I reduce the number of methods?
- Can I simplify the parameters?
- Can I hide more complexity inside?

## Principles

- **Depth is a property of the interface, not the implementation.** A deep module can be internally composed of small, mockable, swappable parts — they just aren't part of the interface. A module can have **internal seams** (private to its implementation, used by its own tests) as well as the **external seam** at its interface.
- **The deletion test.** Imagine deleting the module. If complexity vanishes, it was a pass-through. If complexity reappears across N callers, it was earning its keep.
- **The interface is the verification surface.** Verify observable contracts at the cheapest environment that preserves the risk: static, host, simulator, emulator, target, or HIL. Use more than one environment when no single one covers the contract.
- **A seam must represent real variation or control.** Two adapters are strong evidence. A production-only hardware seam can also be real when substitution, fault containment, ownership, or independent verification requires it; do not invent an adapter only to satisfy a slogan.

## Designing for verifiability

Good interfaces make verification natural:

1. **Make dependencies and hardware resources explicit.** Do not hide clocks, buses, allocators, schedulers, devices, or remote services behind ambient construction.
2. **Expose observable outcomes.** Return status and data where appropriate; when the contract is a side effect, expose the acknowledgement, trace point, diagnostic, or read-back that verifies it.
3. **Specify runtime properties.** Record ownership, lifetime, thread/interrupt context, blocking behaviour, latency, memory bounds, startup/shutdown order, and failure behaviour when they matter.
4. **Keep the surface small.** Fewer entry points and modes reduce caller knowledge and the verification matrix.

## Relationships

- A **Module** has one aggregate **Interface**: the complete surface it presents across source, binary, process, repository, and hardware seams.
- **Depth** is a property of a **Module**, measured against its **Interface**.
- A **Seam** is where a **Module**'s **Interface** lives.
- An **Adapter** sits at a **Seam** and satisfies the **Interface**.
- **Depth** produces **Leverage** for callers and **Locality** for maintainers.

## Rejected framings

- **Depth as ratio of implementation-lines to interface-lines** (Ousterhout): rewards padding the implementation. We use depth-as-leverage instead.
- **"Interface" as a language keyword or a class's public methods**: too narrow — interface here includes every fact a caller, integrator, and verifier must know.
- **"Boundary"**: overloaded with DDD's bounded context. Say **seam** or **interface**.

## Going deeper

- **Deepening a cluster given its dependencies** — see [DEEPENING.md](DEEPENING.md): dependency categories, seam discipline, and verification consolidation.
- **Exploring alternative interfaces** — see [DESIGN-IT-TWICE.md](DESIGN-IT-TWICE.md): spin up parallel sub-agents to design the interface several radically different ways, then compare on depth, locality, and seam placement.
