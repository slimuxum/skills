---
name: grill-with-docs
description: Relentlessly interview a workspace-grounded plan or design while maintaining its glossary and architectural decisions inline.
disable-model-invocation: true
---

# Grill with Docs

Establish the workspace root, relevant repositories and revisions, logical context, source authority, and writable documentation paths.

Call the Skill tool twice, for "grilling" and "domain-modeling". Both disciplines stay active together throughout the interview.

Use `grilling` to pressure-test goals, boundaries, failure modes, timing, resources, interfaces, and verification assumptions. Use `domain-modeling` to resolve terminology and update the canonical glossary inline as decisions land, offering ADRs for durable trade-offs.

Treat Repository and Logical Context as separate boundaries. Do not assume Node, Web, a single repository, or a single context. Keep applicable verification environments explicit: static analysis, host, simulator, emulator, target, and HIL.

Maintain a short list of unresolved decisions, source conflicts, repository revisions, verification constraints, and out-of-scope areas for `to-spec`.

Do not implement, mutate tracker state, create branches, or commit. Do not widen the selected workspace or Logical Context.

## Risk and goal confirmation

**Default.** If uncertainty affects an execution decision, a high-risk item appears, or work departs from the user's agreed goal, scope, constraints, or expected outcome, stop execution and pause related delegated work. Explain the problem, evidence, and impact; recommend a response with reasons and wait for the user's explicit confirmation. Before confirmation, do not attempt fixes, retries, workarounds, alternatives, or plan changes on your own. Resume only the confirmed response.

**Explicit autonomous authorization.** If the user explicitly says not to ask and to decide independently, or gives equivalent authorization, follow the skill's original workflow for decisions, iteration, and fallbacks within the task and scope they authorize. This waives only the extra questions and confirmations introduced by this rule and its applications in supporting instructions. It does not expand the agreed goal or scope, remove existing permission limits, or waive confirmations required by the original workflow. Silence, no reply, or an ordinary "continue" is not autonomous authorization. Restore the default when authorization is withdrawn or does not cover the decision.

**Delegation and handoff.** Include this full rule, agreed goal and scope, the user's autonomous authorization and its scope when present, unresolved issues, recommendations, and confirmation status in worker briefs and handoffs. Workers follow the same applicable mode; under the default, they stop and report to the parent for the user's decision. On withdrawal or narrowing of authorization, notify active workers and pause affected work until they are applying the current mode and scope. A handoff or automatic-continuation instruction cannot grant autonomous authorization or bypass a confirmation that is still required.
