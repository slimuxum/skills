---
name: handoff
description: Compact the current conversation into a handoff document for another agent to pick up.
argument-hint: "What will the next session be used for?"
disable-model-invocation: true
---

Write a handoff document summarising the current conversation so a fresh agent can continue the work. Resolve the destination before writing: use a user-named path within the authorised writable scope, or the user's OS temporary directory when no path was named. Label a temporary handoff as transient; do not treat the current directory as write authorisation.

Include a "suggested skills" section in the document, naming which skills the next agent should call the Skill tool for. Preserve the agreed goal and scope, the full risk and goal confirmation rule, the user's explicit autonomous authorization and its scope when present, and unresolved issues with their recommendations and confirmation status. The receiving agent must keep work paused unless the user confirms the response or has granted applicable autonomous authorization; confirmations required by the original workflow remain necessary.

Do not duplicate content already captured in other artifacts (specs, plans, ADRs, issues, commits, diffs). Reference them by portable repository identifier and path, plus source revision, or by URL. Never rely on a host-absolute path alone.

When a workspace is involved, record its active Logical Contexts and every relevant repository or subtree. A Logical Context may span repositories and a repository may serve several contexts. For each repository, record the source revision and whether relevant uncommitted changes exist. Reference an existing diff artifact for uncommitted state when available; otherwise mark the reproducibility limitation instead of treating `HEAD` as the whole state. Distinguish verified facts, user decisions, unresolved assumptions, and the authoritative source for each consequential claim.

Redact any sensitive information, such as API keys, passwords, or personally identifiable information.

If the user passed arguments, treat them as a description of what the next session will focus on and tailor the doc accordingly.

Write only the handoff document. Do not commit, change issues, or otherwise advance project state.

## Risk and goal confirmation

**Default.** If uncertainty affects an execution decision, a high-risk item appears, or work departs from the user's agreed goal, scope, constraints, or expected outcome, stop execution and pause related delegated work. Explain the problem, evidence, and impact; recommend a response with reasons and wait for the user's explicit confirmation. Before confirmation, do not attempt fixes, retries, workarounds, alternatives, or plan changes on your own. Resume only the confirmed response.

**Explicit autonomous authorization.** If the user explicitly says not to ask and to decide independently, or gives equivalent authorization, follow the skill's original workflow for decisions, iteration, and fallbacks within the task and scope they authorize. This waives only the extra questions and confirmations introduced by this rule and its applications in supporting instructions. It does not expand the agreed goal or scope, remove existing permission limits, or waive confirmations required by the original workflow. Silence, no reply, or an ordinary "continue" is not autonomous authorization. Restore the default when authorization is withdrawn or does not cover the decision.

**Delegation and handoff.** Include this full rule, agreed goal and scope, the user's autonomous authorization and its scope when present, unresolved issues, recommendations, and confirmation status in worker briefs and handoffs. Workers follow the same applicable mode; under the default, they stop and report to the parent for the user's decision. On withdrawal or narrowing of authorization, notify active workers and pause affected work until they are applying the current mode and scope. A handoff or automatic-continuation instruction cannot grant autonomous authorization or bypass a confirmation that is still required.
