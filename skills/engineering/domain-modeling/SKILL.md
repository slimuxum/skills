---
name: domain-modeling
description: Build and sharpen a workspace's domain model. Use when terminology, logical-context boundaries, a glossary, or an architectural decision must be clarified or recorded.
---

# Domain Modeling

Actively sharpen the language and decisions of the logical context in scope. Reading a glossary is ordinary grounding; use this skill when the model itself may change.

A Workspace, Repository, and Logical Context are different boundaries. A context may span repositories, and one repository may contain several contexts. Establish the workspace root, relevant repositories and revisions, target context, source authority, and writable documentation paths before editing.

## Find the canonical location

Read the configured context map, agent guidance, glossaries, ADRs, specifications, generated documentation, and relevant code within scope. Follow existing conventions.

If no convention exists, choose one canonical location for the active Logical Context and create files lazily when the first term or decision is resolved. Do not duplicate a mutable glossary or ADR in several repositories; keep one canonical artifact and link to it where practical.

## Work the model

### Challenge terminology

Call out conflicts with the canonical glossary immediately. Replace vague or overloaded terms with precise candidates, then ask which meaning is intended.

### Test concrete scenarios

Probe normal, boundary, failure, recovery, concurrency, timing, and resource-limit scenarios when relevant. Keep the language domain-neutral; do not assume a Web request, database transaction, UI, or cloud service.

### Cross-check authoritative sources

Compare statements with the sources that own them: code, interface definitions, specifications, generated artifacts, ADRs, verification evidence, and user decisions. When sources conflict, name each source, repository, revision, and affected context. Do not silently choose a winner.

### Update the glossary narrowly

When a term is resolved, update the canonical glossary immediately using [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md). Do not batch resolved terms for later. A glossary defines terms and relationships; it is not a specification, implementation plan, issue list, or verification record. Preserve unrelated content and local conventions.

### Offer ADRs sparingly

Offer an ADR only when all are true:

1. the decision is costly to reverse;
2. a future reader would otherwise find it surprising;
3. real alternatives and trade-offs were considered.

Offer the ADR to the user. When they accept, write it using [ADR-FORMAT.md](./ADR-FORMAT.md), the canonical ADR location, and the repository's numbering convention. Record affected repositories, logical contexts, authoritative inputs, consequences, and unresolved verification.

## Boundaries

Do not edit implementation, tracker state, branches, or commits. Do not widen repository or context scope without approval. Do not claim a verification result that was not run.

Finish with the repositories and revisions inspected, contexts affected, files changed, source conflicts, unresolved terms, and uninspected or `NOT_RUN` scope.

## Risk and goal confirmation

**Default.** If uncertainty affects an execution decision, a high-risk item appears, or work departs from the user's agreed goal, scope, constraints, or expected outcome, stop execution and pause related delegated work. Explain the problem, evidence, and impact; recommend a response with reasons and wait for the user's explicit confirmation. Before confirmation, do not attempt fixes, retries, workarounds, alternatives, or plan changes on your own. Resume only the confirmed response.

**Explicit autonomous authorization.** If the user explicitly says not to ask and to decide independently, or gives equivalent authorization, follow the skill's original workflow for decisions, iteration, and fallbacks within the task and scope they authorize. This waives only the extra questions and confirmations introduced by this rule and its applications in supporting instructions. It does not expand the agreed goal or scope, remove existing permission limits, or waive confirmations required by the original workflow. Silence, no reply, or an ordinary "continue" is not autonomous authorization. Restore the default when authorization is withdrawn or does not cover the decision.

**Delegation and handoff.** Include this full rule, agreed goal and scope, the user's autonomous authorization and its scope when present, unresolved issues, recommendations, and confirmation status in worker briefs and handoffs. Workers follow the same applicable mode; under the default, they stop and report to the parent for the user's decision. On withdrawal or narrowing of authorization, notify active workers and pause affected work until they are applying the current mode and scope. A handoff or automatic-continuation instruction cannot grant autonomous authorization or bypass a confirmation that is still required.
