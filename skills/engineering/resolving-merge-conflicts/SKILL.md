---
name: resolving-merge-conflicts
description: "Use when you need to resolve an in-progress merge or rebase by intent across one or more repositories, verify the integration, and finish the operation."
---

# Resolving Merge Conflicts

1. **Map the operations.** For every affected repository, record root, source revision, operation type, current step, conflicts, unrelated local changes, and the user's intended integration result. Do not assume one repository's state describes the whole logical change.

2. **Confirm scope.** Ask before editing when writable scope, generated files, submodules, target configuration, or the intended winner is unclear. Never abort; this Skill resolves and finishes the in-progress operation.

3. **Find primary sources.** Read commits, PRs, issues/specs, interface owners, generated-source inputs, and cross-repository compatibility rules for both sides.

4. **Resolve each hunk by intent.** Preserve both intents where compatible. Where incompatible, follow the confirmed integration goal and record the trade-off. Do not invent new behaviour, rewrite unrelated work, or resolve generated output without checking its source.

5. **Verify the integrated change.** Run the applicable static, host, simulator, emulator, target, and HIL checks for the combined result, not just each side independently. State what ran, the result, its environment, and limitations; keep agreed checks that did not run explicit.

6. **Finish the operation.** Show the resolved scope and evidence, stage the resolved files, and continue the merge or rebase until it completes. Create the merge commit when Git requires it. Never push.

## Risk and goal confirmation

**Default.** If uncertainty affects an execution decision, a high-risk item appears, or work departs from the user's agreed goal, scope, constraints, or expected outcome, stop execution and pause related delegated work. Explain the problem, evidence, and impact; recommend a response with reasons and wait for the user's explicit confirmation. Before confirmation, do not attempt fixes, retries, workarounds, alternatives, or plan changes on your own. Resume only the confirmed response.

**Explicit autonomous authorization.** If the user explicitly says not to ask and to decide independently, or gives equivalent authorization, follow the skill's original workflow for decisions, iteration, and fallbacks within the task and scope they authorize. This waives only the extra questions and confirmations introduced by this rule and its applications in supporting instructions. It does not expand the agreed goal or scope, remove existing permission limits, or waive confirmations required by the original workflow. Silence, no reply, or an ordinary "continue" is not autonomous authorization. Restore the default when authorization is withdrawn or does not cover the decision.

**Delegation and handoff.** Include this full rule, agreed goal and scope, the user's autonomous authorization and its scope when present, unresolved issues, recommendations, and confirmation status in worker briefs and handoffs. Workers follow the same applicable mode; under the default, they stop and report to the parent for the user's decision. On withdrawal or narrowing of authorization, notify active workers and pause affected work until they are applying the current mode and scope. A handoff or automatic-continuation instruction cannot grant autonomous authorization or bypass a confirmation that is still required.
