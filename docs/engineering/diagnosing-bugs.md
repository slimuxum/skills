## What it does

`diagnosing-bugs` runs a six-phase diagnosis on a hard bug or performance regression across static/build, host, simulator, emulator, target, or HIL environments: build a feedback loop, minimise it, rank hypotheses, instrument, fix where authorized, and clean up.

It will not let the agent form a theory until a **tight** feedback loop exists — one named command or bounded target/HIL procedure, already executed in its stated environment, that goes red on *this* bug and green when it is fixed. If no red-capable signal exists, there is no Phase 2. Lower-fidelity evidence cannot be used to claim target-only timing, memory, concurrency, peripheral, or electrical behaviour.

## When to reach for it

Type `/diagnosing-bugs`, or the agent reaches for it on its own when a task fits — it is model-invoked, and fires on "diagnose" / "debug this" or on a report that something is broken, throwing, failing, or slow.

Reach for it on the hard ones: a bug that resists a first look, an intermittent flake, a regression that crept in between two known-good states. It is heavy by design, and the wrong tool for a question you want answered in one message.

| Your situation | Where to go |
| --- | --- |
| A specific defect you can describe as a symptom | This skill |
| A slow endpoint or a timing regression with a known before-and-after | This skill — it has a performance branch (measure a baseline, then bisect) |
| "Where are the bottlenecks in this codebase?" — no specific symptom | Not this skill. It diagnoses one known failure, it does not audit |
| A raw bug report from someone else, not yet confirmed or written up | [triage](https://aihero.dev/skills-triage) first |
| Throwaway code to answer a design question, not chase a defect | [prototype](https://aihero.dev/skills-prototype) |
| Building a planned behaviour test-first | [tdd](https://aihero.dev/skills-tdd) |
| No good seam exists to lock the bug down | [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) — this skill hands off there itself |

## The tight loop is the skill

Phase 1 gets disproportionate effort because it is the only phase that is hard. The skill gives a ladder of ways to construct the loop, roughly in order of preference:

1. A static-analysis, compiler, linker, configuration, or interface/ABI signal.
2. A failing check at a faithful seam on host, simulator, emulator, target, or HIL.
3. A CLI or protocol invocation with controlled input and a known-good outcome.
4. A replayed trace, core, log, request, payload, or event capture.
5. A throwaway C/C++ or service harness around the affected boundary.
6. A property, fuzz, stress, or race loop for intermittent output, concurrency, memory, or timing failures.
7. A bisection or differential harness across source revisions, toolchains, configurations, calibration, or datasets.
8. A bounded target/HIL procedure when hardware, timing, electrical, peripheral, or integration behaviour is load-bearing.
9. A UI/browser driver only for a genuinely UI-facing symptom.
10. A [human-in-the-loop](https://www.aihero.dev/ai-coding-dictionary/human-in-the-loop) script or procedure when the interface cannot be automated.

*A* loop is not the goal. **Tight** means the fastest faithful, deterministic, sharp, repeatable signal available. Seconds are ideal, but a target or HIL procedure taking minutes is valid when cheaper environments cannot reproduce the property. For an intermittent bug, raise and pin the reproduction rate until the signal is useful.

When it genuinely cannot build one, it is instructed to stop and say so, list what it tried, and ask you for [environment](https://www.aihero.dev/ai-coding-dictionary/environment) access, a captured artifact, or permission to add temporary instrumentation. It should not proceed to hypothesise anyway.

## The gates between phases

The phases are gates, not a checklist. Each one refuses to open until something specific is true.

| Gate | What has to be true |
| --- | --- |
| Into Phase 2 | A named command or bounded procedure, already executed with redacted evidence, that can go red on this bug and records repositories, revisions, toolchain/configuration, and environment identity |
| Into Phase 3 | The repro is reproduced *and* minimised — every remaining element is load-bearing |
| Into Phase 4 | 3–5 ranked, falsifiable hypotheses exist, each stating its prediction, shown to you before any is tested |
| Into Phase 5 | Probes map to a specific prediction, one variable at a time, every debug log tagged `[DEBUG-a4f2]`-style so cleanup is one grep |
| Done | Original repro is green, instrumentation is gone, every check names its environment and limitations, and the correct hypothesis and evidence are recorded in the commit or PR message |

Phase 5 has one escape hatch worth knowing about. A regression check is written before the fix only if a **correct, faithful seam and environment** exist. Where the only available check is too shallow, the skill records a command or procedure as evidence rather than manufacturing confidence.

## Common questions

**It fires on quick questions where I just wanted a direct answer.**
This is the most-reported problem with the skill, and it is real. On GPT-5.6-Sol especially, users report it triggering on a plain description of a problem: "the model triggers the rather formal diagnosing-bugs skill instead. It then goes on to construct a reproduction scenario — often building a mock scenario with limited value — before giving me a response or suggestion. This results in considerable reply delays." Four separate people reported the same shape on [issue #578](https://github.com/mattpocock/skills/issues/578). The accepted fix is to start with a lighter approach and graduate to the heavier one only where the problem warrants it, but that change has not landed. The skill is calibrated against Claude Code's invocation behaviour; a [model](https://www.aihero.dev/ai-coding-dictionary/model) with a lower activation threshold over-fires it. Until it is graduated, the practical fix is to say what you want ("just answer this, don't diagnose") or to disable model invocation for it in your [harness](https://www.aihero.dev/ai-coding-dictionary/harness).

**Can I point it at a codebase and ask where the performance problems are?**
No. It diagnoses one failure you can already name. Its performance branch is for a regression with a symptom — establish a baseline measurement, then bisect, measure first and fix second — not for a proactive sweep. A skill for the proactive version was [proposed and closed](https://github.com/mattpocock/skills/issues/431); there is currently no skill for it.

**Does it stop before it writes the fix?**
No. Only Phase 3 has a human checkpoint: the ranked hypotheses are shown before testing, and the skill proceeds on its own ranking if you are away. Once the cause is proven, it writes the regression check and fix. Target/HIL actions that can alter hardware, calibration, shared equipment, or deployed state still require explicit authorization and recovery steps.

**I already ran `/triage` on this bug report. Is this the same work again?**
Partly, and neither skill admits it. As one reader put it: "Triage's step 3 is essentially a shallow, bounded instance of diagnosing-bugs Phase 1–2, but neither file mentions the other." Triage does a bounded "is this actually a bug, and what is the surface" pass; this skill does the thorough version. Running triage first is not wasted — its verification often gives you most of Phase 1's raw material — but expect to redo it properly here, and expect no cross-reference to tell you that.

**Will the repro output it pastes leak secrets?**
It must redact them first. Commands should consume credentials through environment variables; quoted artifacts include only signal-bearing lines, with credentials, tokens, cookies, and personal data replaced by `<REDACTED>`. If the redacted artifact is insufficient, the agent asks rather than exposing the secret.

**My security scanner flagged this skill as high risk.**
Snyk flags it, and the flag is a false positive. It is the only skill in the set that ships an executable shell script (`hitl-loop.template.sh`) alongside instructions to run it and to curl a dev server. Shipped `.sh` plus run-it instructions plus outbound HTTP is enough to trip a static scanner. The script itself is about 30 lines of `read -r -p` prompts that pause for human input. The scanner is rating the capability surface, not a proven exploit.

**What happened to `/diagnose`?**
Renamed to `/diagnosing-bugs` in v1.0.0. The old name no longer exists. Anything of yours that chains `/diagnose` — a wrapper skill, a saved prompt — needs updating.

## It's working if

- It shows a command or bounded procedure, environment, source revisions, and redacted red evidence before it offers a theory.
- The failure it reproduces is the one you reported, not a nearby one it found on the way.
- It shrinks the repro before it starts guessing, and can tell you why each remaining piece is load-bearing.
- You are shown a ranked list of 3–5 hypotheses, each with a prediction you could falsify, before any of them is tested.
- Every debug log it adds carries a tag like `[DEBUG-a4f2]`, and a grep for that tag comes back empty when it declares done.
- The commit or PR message records which hypothesis was right and the evidence that distinguished it.
- When it cannot lock the bug down with a faithful automated check, it says so and preserves an exact procedure instead of writing a shallow test.
- Target instrumentation and equipment actions have explicit authorization, impact analysis, stop conditions, and recovery.
- Every check states what ran, its result, environment, and limitation; agreed checks that did not run remain explicit.

## Where it fits

`diagnosing-bugs` is a reach-for-it-anytime standalone. You drop into it when something is broken and drop out when the fix and its regression test are in; it holds no state and needs no prior setup. [ask-matt](https://aihero.dev/skills-ask-matt) routes "Something's broken" here.

Two neighbours matter. [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) takes the [handoff](https://www.aihero.dev/ai-coding-dictionary/handoff) when the real finding is that the code has no seam to lock the bug down — the recommendation is made after the fix is in, when there is more information. [triage](https://aihero.dev/skills-triage) sits upstream of it for bugs that arrive as raw reports from other people, and does a shallower version of the same first two phases.
