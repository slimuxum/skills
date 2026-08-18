## What it does

`research` delegates one bounded question to exactly one required background Subagent. The worker follows claims to primary sources and writes one Markdown findings file with citations, repository revisions, authority conflicts, and limitations, while the main session keeps working on independent tasks.

## When to reach for it

Type `/research`, or the agent reaches for it automatically when a bounded fact-finding unit should run in the required background Subagent.

Use it for bounded reading work against official specifications, first-party documentation or APIs, source code, or other authoritative material. It is especially useful when the main session should continue while the worker reads.

Use one invocation per research unit. [wayfinder](https://aihero.dev/skills-wayfinder) can dispatch several independent units as a required parallel wave.

## Common questions

**Can the main agent just do the research if Subagents are unavailable?**

No. Background delegation is part of the skill. It reports the missing background-worker capability instead of using a synchronous substitute.

**Where does the note go in a multi-repository workspace?**

It follows the owning repository or Logical Context's existing research-note convention. If there is none, the worker chooses one sensible location and reports it.

**What counts as evidence?**

External claims carry a primary link and retrieval date. Repository claims carry a path and revision. Conflicting authorities and unavoidable secondary evidence are labeled.

**Does research update issues or commit its note?**

Normally no: it writes the findings file only. The exception is a Wayfinder research ticket, whose original method captures the findings on a throwaway `research/<name>` branch and leaves a context pointer from the ticket.

## It's working if

- The trace shows exactly one background worker for the research unit.
- One Markdown file contains the question, scope, findings, citations, implications, conflicts, unknowns, and uninspected areas.
- Every inspected repository is tied to a revision.
- The main session keeps working while the background Subagent reads.

## Where it fits

Research feeds evidence into [grill-with-docs](https://aihero.dev/skills-grill-with-docs), [wayfinder](https://aihero.dev/skills-wayfinder), or [to-spec](https://aihero.dev/skills-to-spec). It supplies facts; it does not replace the decisions those skills produce. [ask-matt](https://aihero.dev/skills-ask-matt) routes to it when one bounded external fact is the missing piece.
