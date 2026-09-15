---
name: ask-matt
description: Ask which skill or flow fits a workspace-grounded engineering situation. A router over the skills in this repo.
disable-model-invocation: true
---

# Ask Matt

Route from the user's actual situation, not keywords. Before making a load-bearing claim about a skill, read that skill's `SKILL.md`. Do not treat omission from an injected skill list as proof that a user-invoked skill is unavailable.

Establish enough context to route correctly: workspace, repositories and revisions, logical contexts, current phase, expected artifact, applicable verification environments, and tracker needs. Workspace, Repository, and Logical Context are distinct.

Route through `setup-matt-pocock-skills` once per workspace before the first engineering flow, and again when issue-tracker, triage-label, or domain-document conventions change.

## Main engineering flow

1. Use `setup-matt-pocock-skills` before the first engineering flow in a workspace.
2. Use `grill-with-docs` to pressure-test an idea that fits one sustained design session. Use `grill-me` only when there is no workspace documentation to maintain.
3. Use `research` for a bounded fact-finding unit that must run in its required background worker.
4. When a design question needs a runnable answer, detour through `prototype` in an isolated workspace and fresh session: use `handoff` out, run `prototype`, then use `handoff` back with the answer and reference it from the original design thread.
5. Use `wayfinder` when the route is too foggy or large for one session. It resolves decision tickets and hands off; it does not implement the destination.
6. Use `to-spec` to turn settled decisions into a repository- and revision-aware engineering specification. A spec may describe interfaces, states, timing, resources, failure handling, generated artifacts, or other engineering requirements; it is not limited to user stories.
7. Use `to-tickets` when implementation spans sessions. It creates dependency-ordered, independently verifiable outcomes without assuming schema, API, or UI layers.
8. Use `implement` for one scoped change or frontier ticket. It drives `tdd` where executable behavior can be developed test-first and finishes with `code-review`.

Keep applicable verification explicit: static analysis, host, simulator, emulator, target, and HIL. A later environment may consume evidence from an earlier one; unavailable environments are limitations, not passes.

Keep grilling, specification, and ticket decomposition in one context while it remains reliable. Start implementation tickets in fresh contexts. At a phase boundary, use the decision below instead of clearing or compacting by habit.

## On-ramps

- Raw bugs and requests from other people → `triage`, then `implement`. Do not triage tickets produced by `to-tickets`.
- Broken behaviour, an intermittent flake, a regression, or another hard failure → `diagnosing-bugs`; hand architectural seams it exposes to `improve-codebase-architecture`.
- A huge, foggy effort → `wayfinder`, then usually `to-spec` → `to-tickets` → `implement`.
- A codebase-health opportunity → `improve-codebase-architecture`, then take the chosen idea to `grill-with-docs`.

## Vocabulary underneath

- `domain-modeling` resolves logical-context language, glossary conflicts, and hard-to-reverse ADR decisions.
- `codebase-design` supplies module, interface, seam, depth, adapter, leverage, and locality vocabulary.

These model-invoked skills support a flow; they do not replace its artifact or method boundaries.

## Phase boundaries

Choose in this order:

1. **Continue** when the next phase needs this context and it remains reliable.
2. **`/clear`** when nothing behind the boundary is needed.
3. **`handoff`** when context must become portable across a harness, directory, person, or mid-phase fork.
4. **Subagent** when one bounded unit can run independently and return evidence.
5. **`/compact`** when none of the earlier choices fits.

Read [PHASE-BOUNDARIES.md](PHASE-BOUNDARIES.md) for the full decision tree.

## Standalone routes

- `grill-me` — stateless interview outside a documented workspace.
- `grilling` — interview primitive with no wrapper.
- `resolving-merge-conflicts` — resolve an active merge or rebase by intent.
- `prototype` — answer one design question with a disposable artifact.
- `research` — primary-source investigation in a required background worker.
- `to-questionnaire` — obtain missing decisions from another person.
- `wizard` — guide steps that genuinely require a human.
- `wait-what` — re-explain the previous message in shared vocabulary.
- `teach` — learn a concept over several sessions.
- `writing-for-agents` — write instructions and documents for agent consumption.
- `tdd` — build a bounded behavior test-first.
- `code-review` — review a change against fixed baselines and, when one exists, an authoritative specification.

## Response

Name the recommended next skill or short flow and explain the deciding boundary. State prerequisites, context-transition point, expected artifact, and verification level.

Stop after routing. When the phase-boundary tree selects Subagent, recommend a real Subagent rather than a parent-agent or sequential substitute; the skill that owns the selected work retains its own dispatch rules.

## Risk and goal confirmation

**Default.** If uncertainty affects an execution decision, a high-risk item appears, or work departs from the user's agreed goal, scope, constraints, or expected outcome, stop execution and pause related delegated work. Explain the problem, evidence, and impact; recommend a response with reasons and wait for the user's explicit confirmation. Before confirmation, do not attempt fixes, retries, workarounds, alternatives, or plan changes on your own. Resume only the confirmed response.

**Explicit autonomous authorization.** If the user explicitly says not to ask and to decide independently, or gives equivalent authorization, follow the skill's original workflow for decisions, iteration, and fallbacks within the task and scope they authorize. This waives only the extra questions and confirmations introduced by this rule and its applications in supporting instructions. It does not expand the agreed goal or scope, remove existing permission limits, or waive confirmations required by the original workflow. Silence, no reply, or an ordinary "continue" is not autonomous authorization. Restore the default when authorization is withdrawn or does not cover the decision.

**Delegation and handoff.** Include this full rule, agreed goal and scope, the user's autonomous authorization and its scope when present, unresolved issues, recommendations, and confirmation status in worker briefs and handoffs. Workers follow the same applicable mode; under the default, they stop and report to the parent for the user's decision. On withdrawal or narrowing of authorization, notify active workers and pause affected work until they are applying the current mode and scope. A handoff or automatic-continuation instruction cannot grant autonomous authorization or bypass a confirmation that is still required.
