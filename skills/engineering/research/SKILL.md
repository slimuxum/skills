---
name: research
description: Investigate a bounded question against high-trust primary sources in a required background Subagent and capture revision-aware findings in one Markdown file. Use for documentation, standards, APIs, or source-reading legwork.
---

# Research

Spin up exactly one **background Subagent** to do the research, so you keep working while it reads. Do not replace the Subagent with synchronous research.

Include the agreed objective and scope, the full risk and goal confirmation rule, any explicit autonomous authorization with its scope, and pending confirmations in the worker brief. Its job:

1. Investigate one bounded question against **primary sources** — authoritative specifications, official documentation, first-party APIs, and source code at identified revisions — not secondary summaries. Follow every claim back to the source that owns it; use secondary sources only to locate primary evidence and label any unavoidable exception.
2. Keep Workspace, Repository, and Logical Context distinct. For every inspected repository, record its revision and paths in scope. Do not assume Node, Web, one repository, or one context.
3. Record each conclusion with a source link or repository path, revision when applicable, retrieval date for external material, and a limitation where the source cannot settle the claim. Surface conflicting authorities instead of silently choosing one.
4. Write the findings to a single Markdown file, citing each claim's source. Include the question and scope, repositories and revisions, findings, implications, conflicts, unknowns, and uninspected scope.
5. Save it where the owning repository or Logical Context already keeps research notes. Match the existing convention; if there is none, choose one sensible location and report it.
6. Return the file path and a concise summary.

When Wayfinder delegates a research ticket, follow that ticket's original capture rule: place the findings on its throwaway `research/<name>` branch and leave a context pointer from the ticket. Otherwise, research writes only the findings file and does not mutate tracker state, implementation, or generated artifacts.

## Risk and goal confirmation

**Default.** If uncertainty affects an execution decision, a high-risk item appears, or work departs from the user's agreed goal, scope, constraints, or expected outcome, stop execution and pause related delegated work. Explain the problem, evidence, and impact; recommend a response with reasons and wait for the user's explicit confirmation. Before confirmation, do not attempt fixes, retries, workarounds, alternatives, or plan changes on your own. Resume only the confirmed response.

**Explicit autonomous authorization.** If the user explicitly says not to ask and to decide independently, or gives equivalent authorization, follow the skill's original workflow for decisions, iteration, and fallbacks within the task and scope they authorize. This waives only the extra questions and confirmations introduced by this rule and its applications in supporting instructions. It does not expand the agreed goal or scope, remove existing permission limits, or waive confirmations required by the original workflow. Silence, no reply, or an ordinary "continue" is not autonomous authorization. Restore the default when authorization is withdrawn or does not cover the decision.

**Delegation and handoff.** Include this full rule, agreed goal and scope, the user's autonomous authorization and its scope when present, unresolved issues, recommendations, and confirmation status in worker briefs and handoffs. Workers follow the same applicable mode; under the default, they stop and report to the parent for the user's decision. On withdrawal or narrowing of authorization, notify active workers and pause affected work until they are applying the current mode and scope. A handoff or automatic-continuation instruction cannot grant autonomous authorization or bypass a confirmation that is still required.
