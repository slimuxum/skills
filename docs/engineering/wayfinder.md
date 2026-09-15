## What it does

`wayfinder` turns a destination that is too large or foggy for one session into a tracker-backed map of decision tickets. Apart from its parallel research wave, each session resolves at most one ticket. It records decisions with per-repository revisions and evidence until the remaining route is clear.

The map is an index. Each full decision lives in exactly one ticket; the map keeps a linked gist. Workspace, Repository, and Logical Context are mapped separately.

- **Default:** Uncertainty affecting execution decisions, high risk, or departure from your goal, scope, constraints, or expected outcome pauses work and related workers. You receive evidence, impact, recommendations, and reasons; execution and adjustments await your explicit confirmation.
- **Explicit delegation:** “Do not ask; decide yourself” or equivalent allows the original workflow's judgment, iteration, and fallbacks within your authorized task and scope without this additional pause. Existing permission limits and required confirmations remain. Silence, no reply, or ordinary “continue” grants no exception. Workers and handoffs carry your authorization wording, scope, unresolved issues, and confirmation status. The default returns when authorization is withdrawn or does not cover the work. Withdrawal or narrowing reaches active workers, with affected work paused until their mode and scope are updated.

## When to reach for it

You invoke this by typing `/wayfinder` — the agent will not reach for it on its own.

Use it for a broad migration, greenfield system, multi-repository change, or other effort whose important questions cannot yet be held in one session.

Use [grill-with-docs](https://aihero.dev/skills-grill-with-docs) for a design that can be settled in one sustained interview. Wayfinder is decision work, not a long implementation backlog.

## Ticket types

- **Research:** a primary-source fact-finding unit run by a required Subagent.
- **Prototype:** a disposable artifact used to make a choice concrete.
- **Grilling:** a live human interview using both grilling and domain modeling.
- **Task:** an enabling action needed before a decision. The agent drives an AFK task directly or gives the human a precise HITL checklist, and resolves it only when the work is complete.

Tickets use native child, blocker, claim, and close operations when the configured tracker supports them. A documented fallback is used only when the tracker does not support those relationships. If a ticket is outside the intended scope, Wayfinder explains the issue and recommends a response for your confirmation by default. Explicit delegation can cover closing, reclassifying, updating, or deleting it only within the authorized task, scope, and existing tracker permissions; it does not bring excluded work into scope.

## Common questions

**Does it default to GitHub or local Markdown?**

No. It reads the configured tracker mapping or asks for one. The map may cover several repositories or logical contexts, and every tracker operation uses the configured explicit target.

**What happens to research tickets?**

After naming the destination, breadth-first mapping, map creation, ticket creation, and second-pass dependency wiring, Wayfinder freezes the `N` research tickets it just created and launches exactly one Subagent per ticket in parallel. Every worker calls the Skill tool with "research". There are no sequential batches, reduced waves, or parent-agent substitutes.

**Does each research worker call `research`?**

Yes. Each research-ticket Subagent calls the Skill tool with "research" and follows that Skill's required background-agent method.

**Does it create research branches or commits?**

Yes. Each research ticket captures its findings on a throwaway `research/<name>` branch and leaves a context pointer from the ticket. Those branches are never pushed by Wayfinder.

**What if repository revisions move while the map is open?**

The next session compares the recorded baseline with current revisions. Drift is recorded and reconciled before old evidence is treated as current.

**How is verification represented?**

Each decision names the applicable static, host, simulator, emulator, target, or HIL evidence. An unavailable environment is a limitation, not a pass.

## It's working if

- The destination, repositories, revisions, contexts, source authority, and exclusions are visible on the map.
- Every ticket asks one precise decision question and owns its evidence.
- The frontier can be identified from real dependency and claim state.
- Research waves show an all-at-once one-worker-per-unit trace.
- Closed decisions record evidence paths or links, inspected revisions, verification results, and limitations.
- The map finishes with no unresolved in-scope fog and does not start implementation implicitly.

## Where it fits

A typical large-effort flow is:

```txt
setup-matt-pocock-skills → wayfinder → to-spec → to-tickets → implement
```

Research, prototypes, and interviews operate inside the map. [to-spec](https://aihero.dev/skills-to-spec) collapses the resolved decisions into one buildable specification. [ask-matt](https://aihero.dev/skills-ask-matt) decides whether Wayfinder's cost is warranted.
