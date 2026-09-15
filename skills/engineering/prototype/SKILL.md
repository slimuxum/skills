---
name: prototype
description: Build a throwaway prototype to answer one design or feasibility question. Use for logic, state, interfaces, timing, integration, target behaviour, or UI/HMI exploration in Qt/QML, LVGL, native or instrument-cluster runtimes, Web, and other real rendering environments before production implementation.
---

# Prototype

A prototype is **throwaway code that answers a question**. The question decides the shape.

## Pick a branch

Identify which question is being answered — from the user's prompt, the surrounding code, or by asking if the user is around:

- **"Does this logic, state model, interface, timing, or integration approach work?"** → [LOGIC.md](LOGIC.md). Build the smallest artifact that preserves the question: a host executable, test harness, model, simulator/emulator scenario, target/HIL procedure, or a shareable HTML demo when human interaction is the point.
- **"What should this look like?"** → [UI.md](UI.md). First identify the actual UI runtime, display geometry, input model, and faithful rendering environment. Generate several radically different UI/HMI variations and compare them through an environment-appropriate development selector: for example a QML loader/property, an LVGL debug menu or build option, a native/instrument HMI harness, or a Web route/query selector.

The branches produce different artifacts. Do not turn a non-UI engineering question into a browser demo, or a native HMI question into a Web mockup. If the UI runtime or required fidelity is ambiguous, stop and state what cannot be learned from each available environment before choosing.

## Rules that apply to both

1. **Scope before writing.** Identify the repository and subtree whose design question the prototype answers. Keep the clearly marked prototype close to that code or UI so its context is obvious. Do not flash, provision, mutate real data, or operate target/HIL equipment without explicit authorization and a recovery path.
2. **Choose fidelity deliberately.** Prefer static or host execution when it answers the question. For UI/HMI work, use the real renderer or its faithful simulator at the intended resolution and input mode. Use emulator, target, or HIL only for behaviour the cheaper environment cannot preserve, and state the remaining limitations.
3. **Throwaway from day one.** Mark every artifact and non-production configuration clearly. Do not let prototype shortcuts silently become a production implementation.
4. **Trivial to run or repeat.** Record one command or bounded procedure, its environment, inputs, expected observation, and cleanup.
5. **Build only enough evidence.** Add only the assertions, instrumentation, error handling, and safety controls needed to answer the question reliably.
6. **Capture it when done.** Report the verdict, evidence, environment, source revisions, and limitations. Fold the validated decision into the real code and verify it under normal production constraints; do not carry prototype shortcuts, losing variants, or development selectors into production. Then commit the prototype itself to a throwaway branch outside main and leave a context pointer to that branch on the implementation issue. Capture the question and answer in the issue or commit. Do not push.

## Risk and goal confirmation

**Default.** If uncertainty affects an execution decision, a high-risk item appears, or work departs from the user's agreed goal, scope, constraints, or expected outcome, stop execution and pause related delegated work. Explain the problem, evidence, and impact; recommend a response with reasons and wait for the user's explicit confirmation. Before confirmation, do not attempt fixes, retries, workarounds, alternatives, or plan changes on your own. Resume only the confirmed response.

**Explicit autonomous authorization.** If the user explicitly says not to ask and to decide independently, or gives equivalent authorization, follow the skill's original workflow for decisions, iteration, and fallbacks within the task and scope they authorize. This waives only the extra questions and confirmations introduced by this rule and its applications in supporting instructions. It does not expand the agreed goal or scope, remove existing permission limits, or waive confirmations required by the original workflow. Silence, no reply, or an ordinary "continue" is not autonomous authorization. Restore the default when authorization is withdrawn or does not cover the decision.

**Delegation and handoff.** Include this full rule, agreed goal and scope, the user's autonomous authorization and its scope when present, unresolved issues, recommendations, and confirmation status in worker briefs and handoffs. Workers follow the same applicable mode; under the default, they stop and report to the parent for the user's decision. On withdrawal or narrowing of authorization, notify active workers and pause affected work until they are applying the current mode and scope. A handoff or automatic-continuation instruction cannot grant autonomous authorization or bypass a confirmation that is still required.
