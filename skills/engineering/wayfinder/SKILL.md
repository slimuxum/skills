---
name: wayfinder
description: Map an effort too large for one session as tracker-backed decision tickets, then resolve the frontier with revision-aware evidence and required parallel research Subagents.
disable-model-invocation: true
---

# Wayfinder

Use Wayfinder when the route to a destination is still unclear and the decision work exceeds one session. Build a shared map of decisions; do not disguise implementation slices as decision tickets.

## Establish scope and authority

Before tracker or file mutations, establish:

- workspace root;
- every repository in scope and its current revision;
- logical contexts and their repository or subtree membership;
- authoritative specifications, code, generated artifacts, ADRs, and prior evidence;
- configured tracker mapping and triage labels.

Workspace, Repository, and Logical Context are distinct. A context may span repositories, and a repository may contain several contexts. Do not assume GitHub, Node, Web, one repository, or one context.

If tracker configuration is absent, tell the user to run `setup-matt-pocock-skills`; if no tracker is ultimately provided, use the local-Markdown convention.

## Plan, do not implement

Name the destination first. A map is complete when nothing material remains to decide before execution. Wayfinder may perform research, prototypes, interviews, or enabling tasks needed to make decisions, but it does not implement the destination unless the user explicitly expands scope.

Chart and work the map in the order below. A Wayfinder invocation does not implement the destination or push any branch.

## The map

Create one canonical map in the configured tracker, label it `wayfinder:map`, and make its decision tickets child issues. Refer to maps and tickets by linked title in human-facing text, not by bare IDs.

```markdown
## Destination

<observable end state>

## Workspace baseline

- <repository>: <revision> — <scope and role>

## Logical contexts

<contexts and repository or subtree membership>

## Source authority

<ordered sources and conflict policy>

## Notes

<domain, skills every session should consult, constraints, verification environments, and standing instructions>

## Decisions so far

- [<closed ticket title>](link) — <one-line gist>

## Not yet specified

<in-scope fog that cannot yet be phrased as a ticket>

## Out of scope

<explicit exclusions>
```

The map is an index. Keep each full decision and its evidence in one ticket; place only a gist and link on the map.

## Decision tickets

Size each ticket for one agent session and make it answer one precise question.

```markdown
## Question

<decision or investigation>

## Scope

- Logical context: <name>
- Repositories and baseline revisions: <list>
- Relevant repositories, interfaces, artifacts, or environments: <set>

## Required evidence

<sources, observations, and applicable verification environments>
```

Label every ticket `wayfinder:<type>`, where `<type>` is `research`, `prototype`, `grilling`, or `task`. Use the tracker-native child and blocking relationships when available. Use a documented body convention only when the tracker lacks them. The frontier is the open, unblocked, unclaimed set.

Ticket types:

- **Research (AFK):** establish a fact from primary sources through the required research worker protocol.
- **Prototype (HITL):** create a disposable artifact that makes a design choice concrete.
- **Grilling (HITL):** call the Skill tool twice, for "grilling" and "domain-modeling"; the human supplies their side of the conversation.
- **Task (AFK or HITL):** perform an enabling action that must happen before a decision can be made. The agent drives it directly when it can run AFK; otherwise it gives the human a precise HITL checklist. Resolve the ticket only when the work is done, recording what happened and every resulting fact later tickets depend on.

A prototype or task remains an enabling decision activity, not implementation of the destination.

## Fog and out of scope

Keep in-scope questions that cannot yet be phrased precisely under **Not yet specified**. Resolving a ticket may graduate that fog into new tickets. Work beyond the named destination is **Out of scope**, never fog; if an existing ticket proves to be beyond the destination, close it and add a linked one-line reason under Out of scope rather than treating it as a decision on the route.

## Invocation

Either way, never hand-resolve more than one ticket per session; the exception is the parallel wave of research tickets.

### Chart the map

1. **Name the destination.** Call the Skill tool twice, for "grilling" and "domain-modeling", to pin down the spec, decision, or change this map is finding its way to.
2. **Map the frontier breadth-first.** Grill across the whole decision space rather than going deep on one thread. If this surfaces no fog and the journey fits one session, do not create a map; stop and ask how the user wants to proceed.
3. **Create the map** with the `wayfinder:map` label. Fill Destination, Notes, Workspace baseline, Logical contexts, Source authority, and Not yet specified; leave Decisions so far empty.
4. **Create every ticket that is precise now** as a child issue with its `wayfinder:<type>` label. Create first, then wire blocking edges in a second pass so real ticket identities can be referenced. Leave questions that are not yet precise in the fog.
5. **Fire the research Subagents.** Freeze the `N` research tickets just created and verify that all `N` Subagents can start concurrently. Launch exactly one Subagent per research ticket in parallel; each Subagent calls the Skill tool with "research". Do not batch them, reduce the wave, run them in the parent, or substitute sequential work. Each worker captures its findings on a throwaway `research/<name>` branch and leaves a context pointer from the ticket.
6. Stop. Charting is one session's work and resolves no HITL ticket by hand.

### Work through the map

1. Load the map, open children, native dependency/claim state, and recorded repository baselines. Compare them with current revisions before relying on old evidence.
2. Use a ticket named by the user, or choose the first open, unblocked, unclaimed frontier ticket. Claim it through the configured tracker before work.
3. Resolve that one ticket with its type-specific Skill. Fetch the full body of related or closed tickets on demand, and call the Skill tool for every skill named in the map's Notes. If no skill is named and the route is unclear, call the Skill tool twice, for "grilling" and "domain-modeling". Never answer the human's side of a HITL ticket yourself. For verification, use only applicable static, host, simulator, emulator, target, or HIL environments and state the limitations of anything unavailable.
4. Post the answer as a resolution comment, including evidence links, inspected repository revisions, environment, result, limitations, and follow-on decisions. Close the ticket and append a linked gist to Decisions so far.
5. Create then wire newly visible tickets, graduate newly precise fog, and remove the graduated text from Not yet specified. If a ticket is beyond the destination, close it and record it under Out of scope. Update or delete tickets invalidated by the decision.

Expect other sessions to edit the tracker concurrently. Never push research branches.

## Finish

Finish when the destination is clear, all decision tickets are resolved, in-scope fog is empty, revision drift is reconciled, and every decision links its evidence. Hand the map to `to-spec`, `to-tickets`, or the explicitly chosen next step; do not start implementation implicitly.
