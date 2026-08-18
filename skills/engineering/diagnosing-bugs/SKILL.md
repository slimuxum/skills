---
name: diagnosing-bugs
description: Diagnosis loop for hard bugs and performance regressions across C/C++, host, simulator, emulator, target, and HIL environments. Use when the user asks to diagnose/debug or reports broken, failing, intermittent, unsafe, or slow behaviour.
---

# Diagnosing Bugs

A discipline for hard bugs. Skip phases only when explicitly justified.

When exploring the codebase, read the configured glossary and ADRs for every active Logical Context. Do not infer the context from the current directory or a repository-root `CONTEXT.md`.

## Redact

This skill has you show commands, outputs and captured artifacts. **Redact every secret first** — write `<REDACTED>` in its place. Build loops against env vars, so the credential stays in the environment rather than in what you show. Captured artifacts carry auth headers: quote only the lines that carry the signal.

If the redacted output is not enough to diagnose the bug, say so and ask the user.

## Phase 1 — Build a feedback loop

**This is the skill.** Everything else is mechanical. If you have a **tight** pass/fail signal for the bug — one that goes red on _this_ bug — you will find the cause; bisection, hypothesis-testing, and instrumentation all just consume it. If you don't have one, no amount of staring at code will save you.

Spend disproportionate effort here. **Be aggressive. Be creative. Refuse to give up.**

### Ways to construct one — choose by risk and environment

1. **Static or build signal.** Reproduce a compiler/linker diagnostic, static-analysis finding, interface/ABI mismatch, or configuration failure.
2. **Failing test** at a seam that reaches the real bug on host, simulator, emulator, target, or HIL.
3. **CLI or protocol invocation** with controlled input and a known-good outcome.
4. **Trace/core/log replay** through the smallest faithful environment.
5. **Throwaway harness** around the affected C/C++ module, process, service, driver, or platform adapter.
6. **Property, fuzz, stress, or race loop** for intermittent output, concurrency, memory, or timing failures.
7. **Bisection or differential harness** across source revisions, toolchains, configurations, calibration, or datasets.
8. **Target/HIL procedure** when hardware, timing, electrical, peripheral, or integration behaviour is load-bearing.
9. **UI/browser driver** only when the symptom is genuinely UI-facing.
10. **HITL script or procedure** when a human must operate equipment or an unavailable interface.

Build the right feedback loop, and the bug is 90% fixed.

### Tighten the loop

Treat the loop as a product. Once you have _a_ loop, **tighten** it:

- Can I make it faster without removing the behaviour that reproduces the bug? (Cache setup, skip unrelated init, narrow the scope, move logic to host or simulation.)
- Can I make the signal sharper? (Assert on the specific symptom, not "didn't crash".)
- Can I make it more deterministic? (Pin time, seed RNG, isolate filesystem, freeze network.)

A tight loop is the fastest faithful loop available. Seconds are ideal, but a deterministic target or HIL procedure taking minutes is valid when cheaper environments cannot reproduce the symptom. Record that limitation.

### Non-deterministic bugs

The goal is not a clean repro but a **higher reproduction rate**. Use bounded repetition, stress, scheduling variation, or timing control appropriate to the environment. Do not parallelise equipment access, inject timing changes, or stress target/HIL systems without authorization, stop conditions, and recovery. Record the achieved rate and sample size.

### When you genuinely cannot build a loop

Stop and say so explicitly. List what you tried. Ask for the missing environment, target/lab access, a redacted trace/log/core dump, or permission for temporary instrumentation. Do not flash, change calibration, power-cycle shared equipment, mutate production state, or add instrumentation without authorization and recovery steps. Do **not** proceed to hypothesise without a loop.

### Completion criterion — a tight loop that goes red

Phase 1 is done when the loop is **tight** and **red-capable**: you can name one command or bounded procedure that you have already executed in its stated environment (show redacted evidence), and that is:

- [ ] **Red-capable** — it drives the actual bug code path and asserts the **user's exact symptom**, so it can go red on this bug and green once fixed. Not "runs without erroring" — it must be able to _catch this specific bug_.
- [ ] **Deterministic** — same verdict every run (flaky bugs: a pinned, high reproduction rate, per above).
- [ ] **Efficient** — no cheaper faithful environment or smaller safe procedure is available.
- [ ] **Repeatable** — agent-runnable where possible; otherwise a precise HITL/target/HIL procedure with controlled inputs and stop conditions.
- [ ] **Revision-bound** — records every involved repository revision, build/toolchain configuration, and target/environment identity needed to reproduce it.

If you catch yourself reading code to build a theory before this command exists, **stop — jumping straight to a hypothesis is the exact failure this skill prevents.** No red-capable command, no Phase 2.

## Phase 2 — Reproduce + minimise

Run the loop. Watch it go red — the bug appears.

Confirm:

- [ ] The loop produces the failure mode the **user** described — not a different failure that happens to be nearby. Wrong bug = wrong fix.
- [ ] The failure is reproducible across multiple runs (or, for non-deterministic bugs, reproducible at a high enough rate to debug against).
- [ ] You have captured the exact symptom (error message, wrong output, slow timing) so later phases can verify the fix actually addresses it.

### Minimise

Once it's red, shrink the repro to the **smallest scenario that still goes red**. Cut inputs, callers, config, data, and steps **one at a time**, re-running the loop after each cut — keep only what's load-bearing for the failure.

Why bother: a minimal repro shrinks the hypothesis space in Phase 3 and becomes a reusable verification case where that is appropriate.

Done when **every remaining element is load-bearing** — removing any one of them makes the loop go green.

Do not proceed until you have reproduced **and** minimised.

## Phase 3 — Hypothesise

Generate **3–5 ranked hypotheses** before testing any of them. Single-hypothesis generation anchors on the first plausible idea.

Each hypothesis must be **falsifiable**: state the prediction it makes.

> Format: "If <X> is the cause, then <changing Y> will make the bug disappear / <changing Z> will make it worse."

If you cannot state the prediction, the hypothesis is a vibe — discard or sharpen it.

**Show the ranked list to the user before testing.** They often have domain knowledge that re-ranks instantly ("we just deployed a change to #3"), or know hypotheses they've already ruled out. Cheap checkpoint, big time saver. Don't block on it — proceed with your ranking if the user is AFK.

## Phase 4 — Instrument

Each probe must map to a specific prediction from Phase 3. **Change one variable at a time.**

Tool preference:

1. **Debugger, core dump, trace, profiler, sanitizer, or target probe** if the environment supports it.
2. **Targeted logs or diagnostic counters** at boundaries that distinguish hypotheses.
3. Never "log everything and grep".

Before target instrumentation, confirm timing/memory impact, writable scope, deployment method, rollback, and whether the probe can change the symptom.

**Tag every debug log** with a unique prefix, e.g. `[DEBUG-a4f2]`. Cleanup at the end becomes a single grep. Untagged logs survive; tagged logs die.

**Perf branch.** Establish a baseline with the appropriate clock, profiler, trace, counter, or timing harness in the environment where the regression matters. Measure first, fix second; do not infer target timing from host timing.

## Phase 5 — Fix + regression test

Create the regression check **before the fix** — but only when a correct, faithful seam and environment exist.

A correct seam is one where the test exercises the **real bug pattern** as it occurs at the call site. If the only available seam is too shallow (single-caller test when the bug needs multiple callers, unit test that can't replicate the chain that triggered the bug), a regression test there gives false confidence.

**If no correct seam exists, that itself is the finding.** Note it. The codebase architecture is preventing the bug from being locked down. Flag this for the next phase.

If a correct automated seam exists:

1. Turn the minimised repro into a failing test at that seam.
2. Watch it fail.
3. Apply the fix.
4. Watch it pass.
5. Re-run the Phase 1 feedback loop against the original (un-minimised) scenario.

If automation is not appropriate, preserve the minimal command or procedure as verification evidence and state why it could not be automated; do not manufacture a shallow host test.

## Phase 6 — Cleanup

Required before declaring done:

- [ ] Original repro no longer reproduces (re-run the Phase 1 loop)
- [ ] Regression test passes (or absence of seam is documented)
- [ ] All `[DEBUG-...]` instrumentation removed (`grep` the prefix)
- [ ] Throwaway prototypes deleted (or moved to a clearly-marked debug location)
- [ ] Every check states what ran, the result, its environment, and limitations; agreed checks that did not run remain explicit
- [ ] The correct hypothesis and its evidence are stated in the commit or PR message so the next debugger can learn from it
