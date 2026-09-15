## What it does

`grill-with-docs` combines the required [grilling](https://aihero.dev/skills-grilling) interview with [domain-modeling](https://aihero.dev/skills-domain-modeling). The interview pressure-tests the design while resolved terms are written inline to the canonical glossary and durable trade-offs are offered as ADRs.

Both skill invocations are required. If either capability is missing, the run stops as `BLOCKED`; it does not pretend the missing discipline ran.

- **Default:** Uncertainty affecting execution decisions, high risk, or departure from your goal, scope, constraints, or expected outcome pauses work and related workers. You receive evidence, impact, recommendations, and reasons; execution and adjustments await your explicit confirmation.
- **Explicit delegation:** “Do not ask; decide yourself” or equivalent allows the original workflow's judgment, iteration, and fallbacks within your authorized task and scope without this additional pause. Existing permission limits and required confirmations remain. Silence, no reply, or ordinary “continue” grants no exception. Workers and handoffs carry your authorization wording, scope, unresolved issues, and confirmation status. The default returns when authorization is withdrawn or does not cover the work. Withdrawal or narrowing reaches active workers, with affected work paused until their mode and scope are updated.

## When to reach for it

You invoke this by typing `/grill-with-docs` — the agent will not reach for it on its own.

Use it when an idea or design fits one sustained session and there is a real workspace whose language and decisions should remain available afterward.

Use [grill-me](https://aihero.dev/skills-grill-me) when no workspace documentation should be changed. Use [wayfinder](https://aihero.dev/skills-wayfinder) when the decision space is too large or foggy for one session.

## Prerequisites

Run [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) once for the workspace so the canonical domain-document locations are known. The invocation must be able to call both `grilling` and `domain-modeling`, and the active repositories and Logical Context must be identifiable.

## Common questions

**Does it assume one repository?**

No. It establishes the repositories and revisions in scope, then maps the logical context separately.

**Is it only for Web designs?**

No. Questions cover behavior, interfaces, modes, failures, recovery, timing, concurrency, resources, build or generated artifacts, and applicable verification. C, C++, embedded, services, tools, and other domains use the same discipline.

**What verification can it discuss?**

Static analysis, host, simulator, emulator, target, and HIL. It records constraints and unknowns without claiming a run occurred.

**What can it write?**

Only glossary and ADR changes through `domain-modeling`. It does not implement, mutate tracker state, create branches, or commit.

**What does it hand downstream?**

Settled decisions plus a visible list of unresolved questions, source conflicts, repository baselines, verification constraints, and out-of-scope areas.

## It's working if

- Both required skills appear in the execution trace.
- Questions expose assumptions instead of merely restating the idea.
- Repository and logical-context boundaries remain distinct.
- Resolved domain terms are captured inline in the canonical glossary rather than deferred.
- The result is detailed enough for `to-spec` without hiding unknowns.

## Where it fits

The common flow is:

```txt
setup-matt-pocock-skills → grill-with-docs → to-spec → to-tickets
```

Research may feed the interview, and a prototype may answer a question that discussion cannot. [ask-matt](https://aihero.dev/skills-ask-matt) decides when to use those branches.
