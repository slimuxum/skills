---
name: tdd
description: Test-driven development for C/C++ and other software. Use when the user wants a feature or bug built test-first, mentions red-green-refactor, wants integration tests, or has an executable seam for red-green work on host, simulator, emulator, target, or HIL.
---

# Test-Driven Development

TDD is the red → green loop. When this skill is selected, keep the requested test-first method and choose the fastest deterministic environment that faithfully exercises the agreed seam. Do not pretend a host test proves target behaviour.

When exploring the codebase, read the configured glossary and ADRs for every active Logical Context. Do not assume the current directory or a repository-root `CONTEXT.md` identifies the context.

## Select the environment and strategy

Before writing a test, identify the requirement or risk and choose the cheapest environment that preserves it:

Confirm every affected repository/subtree, its source revision and build/toolchain configuration.

- **Static** — compiler diagnostics, analysis, coding rules, interface/ABI checks.
- **Host** — pure logic, algorithms, parsers, state machines, and portable libraries.
- **Simulator/emulator** — platform interactions and integration unavailable on host.
- **Target/HIL** — timing, resources, concurrency, peripherals, electrical behaviour, fault injection, and real integration.

Choose the fastest deterministic loop that faithfully covers the agreed seam. If only simulator, emulator, target, or HIL preserves the property, run the red → green cycle there. If no faithful executable seam exists, stop and agree a different public seam with the user rather than silently abandoning the requested test-first method; retain any later target/HIL obligation that the chosen seam cannot close.

## What a good test is

Tests verify observable behaviour and runtime contracts through public owned interfaces, never private implementation details. Expected results come from a specification, standard, worked example, reference implementation, or other independent authority.

See [tests.md](tests.md) for examples and [mocking.md](mocking.md) for mocking guidelines.

## Seams — where tests go

A **seam** is the public owned boundary where behaviour can be observed without reaching inside. In automotive software it may be a published source, binary, process, repository, protocol, generated interface, or hardware-facing contract. Tests live at seams, never against internals; do not expose a production interface solely to make a test convenient.

**Test only at pre-agreed seams.** Before writing any test, list each proposed seam, the risk it covers, environment, cost, and what it will not prove, then confirm every seam with the user. No test is written at an unconfirmed seam.

Ask: "What's the public interface, and which seams should we test?"

When the shape of that interface is itself in question — how deep the module is, where the seam belongs, what the interface should expose — call the Skill tool with "codebase-design" for the vocabulary. It is the shared source of the module, interface, depth, seam, adapter, leverage and locality terms, and it is a reference to consult, not a session to run.

## Anti-patterns

- **Implementation-coupled** — mocks internal collaborators, tests private methods, or verifies through a side channel (querying the database instead of using the interface). The tell: the test breaks when you refactor but behavior hasn't changed.
- **Tautological** — the assertion recomputes the expected value the way the code does (`expect(add(a, b)).toBe(a + b)`, a snapshot derived by hand the same way, a constant asserted equal to itself), so it passes by construction and can never disagree with the code. Expected values must come from an independent source of truth — a known-good literal, a worked example, the spec.
- **Environment overclaim** — a host, mock, simulator, or emulator test is reported as proving target/HIL behaviour it cannot preserve.
- **Horizontal slicing** — writing all tests first, then all implementation. Work in risk-sized vertical slices instead.

## Rules of the loop

- **Red before green.** Write the failing test first, then only enough code to pass it. Don't anticipate future tests or add speculative features.
- **Prove the red.** Confirm the failure is caused by the missing or broken behaviour, not by a bad harness, unavailable target, or environment mismatch.
- **One slice at a time.** One risk, one justified seam, one test, one minimal implementation per cycle.
- **Re-run at required fidelity.** A host-green result does not close a target/HIL obligation. Run or explicitly classify every agreed environment.
- **Report evidence precisely.** State what ran, the result, the environment, and its limitations. Mark agreed checks that did not run explicitly rather than implying they passed.
- **Refactoring is not part of the loop.** It belongs to the review stage (see the `code-review` skill), not the red → green implementation cycle.

## Risk and goal confirmation

**Default.** If uncertainty affects an execution decision, a high-risk item appears, or work departs from the user's agreed goal, scope, constraints, or expected outcome, stop execution and pause related delegated work. Explain the problem, evidence, and impact; recommend a response with reasons and wait for the user's explicit confirmation. Before confirmation, do not attempt fixes, retries, workarounds, alternatives, or plan changes on your own. Resume only the confirmed response.

**Explicit autonomous authorization.** If the user explicitly says not to ask and to decide independently, or gives equivalent authorization, follow the skill's original workflow for decisions, iteration, and fallbacks within the task and scope they authorize. This waives only the extra questions and confirmations introduced by this rule and its applications in supporting instructions. It does not expand the agreed goal or scope, remove existing permission limits, or waive confirmations required by the original workflow. Silence, no reply, or an ordinary "continue" is not autonomous authorization. Restore the default when authorization is withdrawn or does not cover the decision.

**Delegation and handoff.** Include this full rule, agreed goal and scope, the user's autonomous authorization and its scope when present, unresolved issues, recommendations, and confirmation status in worker briefs and handoffs. Workers follow the same applicable mode; under the default, they stop and report to the parent for the user's decision. On withdrawal or narrowing of authorization, notify active workers and pause affected work until they are applying the current mode and scope. A handoff or automatic-continuation instruction cannot grant autonomous authorization or bypass a confirmation that is still required.
