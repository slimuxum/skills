## What it does

`tdd` builds behaviour test-first: one failing check, then just enough code to pass it, then the next behaviour. It works for C, C++, and other code by choosing the fastest deterministic environment that faithfully exercises the agreed seam.

It lists the public seam, risk, environment, cost and limitation before writing a test, then confirms every seam with you. No test is written at an unconfirmed seam. The seam must reflect real ownership or variation; do not create a production interface solely for test convenience. The other thing to know is that `tdd` is a **reference**, not a driver. It holds the rules of the loop, and something else (you, or [implement](https://aihero.dev/skills-implement)) runs the [session](https://www.aihero.dev/ai-coding-dictionary/session) that applies them.

## When to reach for it

Type `/tdd`, or the [agent](https://www.aihero.dev/ai-coding-dictionary/agent) reaches for it automatically when a task fits — building a feature or fixing a bug test-first. The familiar phrase "red-green-refactor" may still trigger the skill, but this skill deliberately runs red → green and leaves refactoring to review.

Reach for it when there is a concrete behaviour to build, with an input and an observable output, and you want tests that survive a refactor.

| Your situation | Where to go |
| --- | --- |
| Portable logic or a contract with known inputs and observable outputs, including C/C++ modules | `tdd` on host or the cheapest faithful environment |
| The behaviour isn't pinned down yet | [to-spec](https://aihero.dev/skills-to-spec), which also agrees the test seams before any code is written |
| The question is really the shape of the interface, not the tests | [codebase-design](https://aihero.dev/skills-codebase-design) |
| You have a [spec](https://www.aihero.dev/ai-coding-dictionary/spec) or [tickets](https://www.aihero.dev/ai-coding-dictionary/ticket) and want the whole build run for you | [implement](https://aihero.dev/skills-implement), which drives `tdd` per ticket |
| Config, generated code, hardware timing, ABI/layout, electrical, or integration behaviour | Agree a faithful executable seam and run the red-green loop in the required simulator, emulator, target, or HIL environment; if none exists, stop and resolve the seam instead of silently dropping TDD |

Run the loop only when the expected result has an independent source of truth: a requirement, protocol vector, worked example, prior trusted result, or other authoritative oracle. Otherwise a test merely restates the implementation. Choose the evidence environment by the risk being proved; if the requested TDD loop has no faithful seam, stop and agree one rather than substituting another method without the user.

## Prerequisites

[codebase-design](https://aihero.dev/skills-codebase-design) needs to be installed. `tdd` used to carry its own deep-module and interface-design notes; in v1.0 those were deleted in favour of the shared skill, and `tdd` now leans on it for interface-design vocabulary. Nothing else — the skill is [stateless](https://www.aihero.dev/ai-coding-dictionary/stateless) and writes no files of its own.

## The loop, and the seam it runs at

Three words carry this skill.

**Red-green.** Write the failing test, then only enough code to pass it. No anticipating the test after next. There is no refactor phase: it was dropped in June 2026 because agents essentially never performed it, and because review and implementation work better as separate sessions. Refactoring belongs to [code-review](https://aihero.dev/skills-code-review).

**Vertical slice.** One seam, one test, one minimal implementation, then repeat — the first cycle being a **tracer bullet** that proves a single path end to end. The opposite is horizontal slicing: all the tests first, then all the code. Bulk tests verify *imagined* behaviour, they check the shape of things rather than what a user does, and they commit you to a test structure before you understand the implementation.

**Pre-agreed public seam.** A seam is a published source, binary, process, repository, protocol, generated interface, or hardware-facing boundary where behaviour can be observed without reaching inside. Name the owner, risk, environment and limitation, then confirm the seam before any test. In the full chain the seam may be agreed during [to-spec](https://aihero.dev/skills-to-spec); invoked on its own, `tdd` asks you directly.

The three anti-patterns it is written to prevent:

| Anti-pattern | The tell |
| --- | --- |
| Implementation-coupled | The test breaks when you rename an internal function, though behaviour did not change. Mocked internal collaborators, asserted call counts, database queries used to verify instead of the interface. |
| Tautological | The expected value is computed the way the code computes it, so the test passes by construction. Expected values have to come from somewhere else — a known-good literal, a worked example, the spec. |
| Horizontal slicing | A batch of tests landed before any implementation. |

Mocks are for representable system boundaries — external APIs, clocks, randomness, filesystems, OS services, buses, or peripherals in host-scoped checks. They do not prove target timing, concurrency, ABI, electrical behaviour, or real hardware integration, and they do not replace simulator, emulator, target, or HIL evidence.

## Common questions

**Why doesn't it refactor, even if I asked for "red-green-refactor"?**

The separation is deliberate: this skill keeps each implementation cycle to red → green, then [code-review](https://aihero.dev/skills-code-review) handles refactoring findings independently. The trigger phrase is accepted as familiar vocabulary; it does not add a third phase to this loop.

**It asked me to choose a test seam and I had no idea which to pick.**

This is the most-reported friction with the skill ([issue #607](https://github.com/mattpocock/skills/issues/607)). The prompt lists candidate seams by name only, with nothing about what each one catches or misses, so you are choosing between labels. There is no fix shipped yet. The practical workaround is to ask the agent for the trade-offs before answering — what does the component-level seam miss that the integration seam catches, and how much slower is it. It is also why the chain agrees seams up front in `to-spec`, where you have the whole feature in view rather than one prompt.

**It wrote the implementation before the test, even though the skill says red first.**

That slice has not satisfied this skill. The agent must execute the new check, capture the expected failure for the right reason, and only then change production code. If the environment cannot support that loop, stop calling it TDD and state why another verification technique is required.

**Should it write browser or end-to-end tests first?**

Choose the cheapest faithful environment. A browser, simulator, emulator, target, or HIL loop is valid only when the property under test requires it and the loop is safe and deterministic enough. Prefer a host or component check for portable logic, but never use a lower-fidelity pass to claim target-only timing, memory, concurrency, peripheral, or electrical behaviour.

**Does `/tdd` replace `/implement`, or the course's `/do-work`?**

No. `/tdd` documents the methodology; `/implement` owns the change, selects TDD only where it fits the risk, finishes with review, and commits the reviewed slice to the current branch. If you are asking which one to run against a ticket, the answer is usually `/implement`.

**Where did the deep-modules and interface-design guidance go?**

Into [codebase-design](https://aihero.dev/skills-codebase-design) in v1.0, generalised so several skills share one vocabulary. `refactoring.md` left at the same time; refactoring is now [code-review](https://aihero.dev/skills-code-review)'s job, and that skill carries the Fowler smell baseline.

**Does it know about my other tickets?**

No. Run against one ticket, it will happily propose work that belongs to a sibling ticket, because it has no view of the rest of the issue graph ([issue #129](https://github.com/mattpocock/skills/issues/129)). Cross-ticket planning is outside `tdd`'s job. Passing the spec alongside the ticket helps; right-sizing the tickets in the first place helps more.

## It's working if

- The seam, risk, oracle, and environment are explicit before the first test.
- The first production change follows a failing check observed in that environment for the expected reason.
- Each cycle is one test, one minimal implementation, green, repeat — never a horizontal batch.
- Expected values come from an independent source, not a copy of the production algorithm.
- Mocks remain within the fidelity they can represent, and required simulator, emulator, target, or HIL evidence stays separate.
- Unsuitable or unavailable loops are reported honestly instead of being replaced by a shallow host test.
- Every check states what ran, the result, its environment, and limitations; agreed checks that did not run remain explicit.

## Where it fits

`tdd` is the engine inside the build step of the main chain, rather than a step of its own:

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review
```

[to-spec](https://aihero.dev/skills-to-spec) can agree verification seams up front, [implement](https://aihero.dev/skills-implement) selects `tdd` per risk, and [code-review](https://aihero.dev/skills-code-review) checks that the chosen evidence is faithful and complete. Its other neighbour is [codebase-design](https://aihero.dev/skills-codebase-design), the shared source of interface vocabulary. Use `tdd` directly whenever a concrete behaviour has an independent oracle and a suitable loop. When you are unsure which skill fits, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
