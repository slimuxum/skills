---
name: to-tickets
description: Split a plan, specification, or settled current conversation into dependency-ordered, independently verifiable engineering tickets.
disable-model-invocation: true
---

# To Tickets

Turn a plan, specification, or settled current conversation into agent-sized implementation tickets. Produce the execution map; do not implement it.

## Preconditions

Read the complete source material: the referenced plan or specification, or the settled decisions in the current conversation. Also read the workspace configuration. Establish:

- source identity and revision (canonical path/link when one exists, otherwise the current conversation);
- repositories in scope and current revisions;
- logical contexts;
- source authority and revision drift;
- configured tracker and triage-label mapping.

Stop for clarification if the source has unresolved decisions that change decomposition. Do not silently repair it or require a separate spec when the plan or conversation is already sufficient.

If the codebase has not already been explored in the current context, inspect the affected repositories enough to understand their current interfaces, build and verification surfaces, active glossary, and ADRs. Use that vocabulary in ticket titles and descriptions. Look for prefactoring that makes the change easier, then include it only when it has its own observable acceptance.

## Slice by observable outcomes

Prefer tracer bullets: each ticket delivers a thin, coherent outcome that can be observed and verified independently. The outcome need not be user-facing. It may be a build property, generated artifact, interface behavior, state transition, diagnostic, migration stage, target observation, or other engineering evidence.

Do not default to schema/API/UI layers. Discover the actual change surfaces, which may include build configuration, C or C++ code, startup or linker behavior, drivers, operating-system integration, middleware, protocols, services, tools, generated code, documentation, tests, or hardware interaction.

A ticket may touch several layers or repositories when that is the smallest coherent outcome. Keep repository ownership explicit. Prefer one repository per ticket when it can remain independently verifiable; when a ticket must span repositories, state why and identify every baseline revision.

## Decomposition rules

1. Identify prefactoring that makes later outcomes safer or independently verifiable. Include it only when it has its own observable acceptance.
2. Give every ticket one outcome, one bounded scope, and acceptance evidence that is false at the baseline unless it protects an invariant.
3. Put real dependency edges between tickets. Do not encode sequencing merely because tickets were listed in order.
4. Keep a fresh agent able to execute the ticket from the plan, specification, or settled conversation plus linked evidence, without hidden conversation context.
5. Use stable source identifiers and paths only when the source evidence supports them; do not invent a file-level implementation plan.
6. For a wide mechanical refactor that cannot stay valid as a tracer bullet, use expand–migrate–contract. Define integration and verification gates explicitly.
7. Keep implementation, tracker closure, branch creation, and commits outside this skill.

Choose only applicable verification environments: static analysis, host, simulator, emulator, target, and HIL. A ticket may require more than one level, but never imply that an unavailable target or HIL check passed. State which evidence the ticket produces and which later gate consumes it.

## Ticket format

```markdown
# <outcome-oriented title>

## Outcome

<new observable capability, property, or evidence>

## Context and baseline

- Source: <plan/spec link or path, or current conversation>
- Logical context: <name>
- Repositories and revisions:
  - <repository>: <baseline revision>

## Scope

<owned change surfaces and explicit scope boundaries>

## Out of scope

<explicit exclusions and work owned elsewhere>

## Acceptance evidence

- <observation that can fail at the baseline>
- <required invariant that remains true>

## Verification

| Environment | Procedure or command | Expected evidence | Prerequisites or limitation |
| --- | --- | --- | --- |

## Dependencies

- Blocked by: <linked ticket titles or none>
- Blocks: <linked ticket titles or none>

## Status

ready-for-agent
```

Keep acceptance owned by the ticket. Do not make one ticket pass only through unowned work in another ticket.

Avoid implementation snippets. The exception is a small decision-rich snippet from a prototype when it communicates a validated state machine, reducer, schema, interface, or type shape more precisely than prose; trim it to the decision and identify its source.

## Preview and approve

Before any mutation, present:

- the numbered blocker-first ticket list;
- repository and logical-context scope for each ticket;
- dependency edges and available frontier;
- verification environment and evidence for each ticket;
- any cross-repository or expand–migrate–contract exception;
- every proposed tracker mutation and label.

Ask whether tickets should merge, split, reorder, or change ownership. Obtain explicit approval to publish.

## Publish

Use tracker-native blocking relationships when available; use the configured fallback only when they are not. Create blockers before dependants when the tracker requires identities first. A source parent issue may be referenced from each ticket, but do **not** close or modify the parent issue.

Publish the user-approved breakdown and apply the configured `ready-for-agent` triage label unless the user instructed otherwise. Local ticket files carry `Status: ready-for-agent`. Do not assign or close the new tickets, create branches, commit, or push.

After publication, report canonical ticket links or paths, per-repository baseline revisions, the initial frontier, and verification that has not run. Hand one frontier ticket at a time to `implement`.
