---
name: implement
description: "Implement scoped work from a spec, ticket, or agreed plan across one or more repositories, with risk-based verification, review, and a final commit."
disable-model-invocation: true
---

# Implement

Implement only the work the user authorized. Do not redesign the plan, advance workflow state, close tickets, or push. Finish the implemented slice by committing it to the current branch, as this Skill has always done.

## 1. Preflight

Before writing:

- Resolve the full workspace scope: repositories, logical contexts, relevant subtrees, source revisions, and cross-repository interfaces.
- Restate acceptance criteria and identify missing or contradictory inputs. Stop with `BLOCKED` when safe implementation would require guessing.
- Confirm writable paths, generated/vendor exclusions, permitted tools, authoritative compiler/linker/build/toolchain configuration, target/lab access, and the intended current branch in each repository.
- Record the initial status of every in-scope repository. Preserve unrelated user changes.
- Select verification environments by risk: static, host, simulator, emulator, target, and HIL. State what each can prove and what remains limited.

## 2. Execute the logical change

Order edits by interface compatibility and integration dependency, not by the current working directory. Keep repositories independently buildable when possible; when an integration point cannot be green independently, state the temporary condition and the final integration check.

Call the Skill tool with "tdd" only where a fast, deterministic red-green loop is appropriate. For configuration, generated code, hardware-only behaviour, timing/resource constraints, or expensive target/HIL checks, use the risk-appropriate static analysis, review, procedure, or staged verification instead of manufacturing a low-value test.

Make the smallest coherent change. After each slice, run the cheapest relevant check. Do not flash targets, mutate shared infrastructure, run destructive migrations, or change calibration without explicit authorization and recovery steps.

## 3. Verify and review

Run the agreed verification work. For each check, report the command or procedure, environment, source revision, result, output summary, and limitations. Keep agreed checks that did not run explicit.

Call the Skill tool with "code-review" against the complete logical change across all affected repositories, including committed, staged, unstaged, and untracked files. Address findings only within the approved scope.

## 4. Finish

Summarize changed repositories/files, acceptance coverage, evidence, and remaining risks. Commit the coherent reviewed slice to the current branch in each affected repository. Never push, merge, close a ticket, or mark product completion.

## Risk and goal confirmation

**Default.** If uncertainty affects an execution decision, a high-risk item appears, or work departs from the user's agreed goal, scope, constraints, or expected outcome, stop execution and pause related delegated work. Explain the problem, evidence, and impact; recommend a response with reasons and wait for the user's explicit confirmation. Before confirmation, do not attempt fixes, retries, workarounds, alternatives, or plan changes on your own. Resume only the confirmed response.

**Explicit autonomous authorization.** If the user explicitly says not to ask and to decide independently, or gives equivalent authorization, follow the skill's original workflow for decisions, iteration, and fallbacks within the task and scope they authorize. This waives only the extra questions and confirmations introduced by this rule and its applications in supporting instructions. It does not expand the agreed goal or scope, remove existing permission limits, or waive confirmations required by the original workflow. Silence, no reply, or an ordinary "continue" is not autonomous authorization. Restore the default when authorization is withdrawn or does not cover the decision.

**Delegation and handoff.** Include this full rule, agreed goal and scope, the user's autonomous authorization and its scope when present, unresolved issues, recommendations, and confirmation status in worker briefs and handoffs. Workers follow the same applicable mode; under the default, they stop and report to the parent for the user's decision. On withdrawal or narrowing of authorization, notify active workers and pause affected work until they are applying the current mode and scope. A handoff or automatic-continuation instruction cannot grant autonomous authorization or bypass a confirmation that is still required.
