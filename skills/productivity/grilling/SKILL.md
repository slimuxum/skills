---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

If this invocation carries `grilling_fact_worker: true`, do not run the interview. Perform only the bounded fact lookup in the worker brief, bind every finding to its source and revision, report it to the parent, and stop. Never dispatch another fact worker from a marked fact worker.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

Each question should be formatted like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent with `grilling_fact_worker: true` and one bounded fact brief; include the marked-worker instructions above, the agreed objective and scope, and the full risk and goal confirmation rule with any explicit autonomous authorization, its scope, and pending confirmations rather than relying on implicit Skill loading. Don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. If the required sub-agent capability is unavailable, mark that fact branch `BLOCKED`, name the missing capability and limitation, and keep working only the independent frontier; never replace the sub-agent with a sequential lookup or a question to the user. The _decisions_ are the user's: put each to them and wait.

When the subject uses a workspace, treat it as potentially multi-repository. Do not infer the active Logical Context from the current directory or one repository: a context may span repositories or select subtrees within them. Bind repository facts to the repository, path or subtree, and source revision they came from; surface conflicting sources instead of silently choosing one.

The session is done only when the frontier is empty and no fact branch is running or `BLOCKED`: every branch of the design tree has been visited and nothing is silently assumed. If a fact branch remains `BLOCKED`, report that shared understanding was not reached. Do not act on the result until the user confirms you have reached a shared understanding.

## Risk and goal confirmation

**Default.** If uncertainty affects an execution decision, a high-risk item appears, or work departs from the user's agreed goal, scope, constraints, or expected outcome, stop execution and pause related delegated work. Explain the problem, evidence, and impact; recommend a response with reasons and wait for the user's explicit confirmation. Before confirmation, do not attempt fixes, retries, workarounds, alternatives, or plan changes on your own. Resume only the confirmed response.

**Explicit autonomous authorization.** If the user explicitly says not to ask and to decide independently, or gives equivalent authorization, follow the skill's original workflow for decisions, iteration, and fallbacks within the task and scope they authorize. This waives only the extra questions and confirmations introduced by this rule and its applications in supporting instructions. It does not expand the agreed goal or scope, remove existing permission limits, or waive confirmations required by the original workflow. Silence, no reply, or an ordinary "continue" is not autonomous authorization. Restore the default when authorization is withdrawn or does not cover the decision.

**Delegation and handoff.** Include this full rule, agreed goal and scope, the user's autonomous authorization and its scope when present, unresolved issues, recommendations, and confirmation status in worker briefs and handoffs. Workers follow the same applicable mode; under the default, they stop and report to the parent for the user's decision. On withdrawal or narrowing of authorization, notify active workers and pause affected work until they are applying the current mode and scope. A handoff or automatic-continuation instruction cannot grant autonomous authorization or bypass a confirmation that is still required.
