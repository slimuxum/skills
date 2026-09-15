## What it does

`ask-matt` routes a workspace-grounded engineering situation to the skill or short flow that fits it. It distinguishes workspace, repository, and logical-context boundaries; checks the current phase and artifact; and names verification prerequisites.

It reads a target skill's `SKILL.md` before making load-bearing claims about that skill. It recommends the next step and stops; it is a router, not the executor of the selected skill.

- **Default:** Uncertainty affecting execution decisions, high risk, or departure from your goal, scope, constraints, or expected outcome pauses work and related workers. You receive evidence, impact, recommendations, and reasons; execution and adjustments await your explicit confirmation.
- **Explicit delegation:** “Do not ask; decide yourself” or equivalent allows the original workflow's judgment, iteration, and fallbacks within your authorized task and scope without this additional pause. Existing permission limits and required confirmations remain. Silence, no reply, or ordinary “continue” grants no exception. Workers and handoffs carry your authorization wording, scope, unresolved issues, and confirmation status. The default returns when authorization is withdrawn or does not cover the work. Withdrawal or narrowing reaches active workers, with affected work paused until their mode and scope are updated.

## When to reach for it

You invoke this by typing `/ask-matt` — the [agent](https://www.aihero.dev/ai-coding-dictionary/agent) will not reach for it on its own. Use it when you know the situation but not the next skill:

| Situation | Typical route |
| --- | --- |
| A bounded idea in a real workspace | `grill-with-docs → to-spec? → to-tickets? → implement` |
| A huge effort whose route is unclear | `wayfinder → to-spec → to-tickets → implement` |
| A bounded external fact is missing | `research` |
| Raw bugs or requests arrived | `triage → implement` |
| Broken behaviour, an intermittent flake, a regression, or another hard failure | `diagnosing-bugs` |
| One concrete behavior should be built test-first | `tdd` |
| A branch or change needs review | `code-review` |
| The first engineering flow is starting in this workspace | `setup-matt-pocock-skills`, then resume the chosen flow |
| A phase just ended | Continue, clear, handoff, Subagent, or compact |

Question marks mean the router decides whether the durable artifact is worth its cost.

## The main boundaries

Use [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) once per workspace before the first engineering flow, then again when tracker, triage-label, or domain-document conventions change.

Use [grill-with-docs](https://aihero.dev/skills-grill-with-docs) when a design fits one sustained interview. Use [wayfinder](https://aihero.dev/skills-wayfinder) when the decision space exceeds one session. Use [to-spec](https://aihero.dev/skills-to-spec) for a durable engineering contract and [to-tickets](https://aihero.dev/skills-to-tickets) when implementation spans sessions.

The router never assumes the system is Node, Web, single-repository, or single-context. It keeps applicable static, host, simulator, emulator, target, and HIL verification visible. The selected Skill retains its original side effects: for example, `implement` commits its reviewed slice, while no route pushes implicitly.

## Phase boundaries and Subagents

At a phase boundary, consider:

1. continue;
2. clear when nothing must survive;
3. handoff when context must become portable;
4. Subagent for one bounded independent unit;
5. compact when none of the earlier choices fits.

When this tree selects Subagent, the router recommends a real Subagent rather than a parent-agent or sequential substitute. The selected skill retains its own worker count, parallelism, isolation, and failure rules.

## Common questions

**Does it execute every skill it recommends?**

No. It routes and stops. The human or calling workflow invokes the recommended skill or Subagent.

**Does every first engineering flow start with setup?**

Yes, once per workspace. Repeat setup only when the tracker, triage-label, or domain-document conventions change.

**Why did it ask about repositories and logical contexts?**

They answer different questions. A repository controls revision and writes; a logical context controls meaning and design boundaries. Correct routing, evidence, and downstream artifacts need both.

**Can it route to an unavailable user-invoked skill?**

It verifies from the repository or plugin authority rather than assuming the injected model-invocation list is complete. If a required capability truly is unavailable, it reports the missing prerequisite.

**Where do research and prototypes fit?**

Research establishes primary-source facts. A prototype makes one design choice concrete. The prototype detour always uses `handoff` in both directions: hand off to an isolated workspace and fresh session, run the prototype there, then hand the answer back to the original design thread. Neither research nor a prototype silently becomes production implementation.

**When is a commit part of the flow?**

`implement` commits each reviewed slice to the current branch; `prototype` captures its answer on a throwaway branch; and a wizard selected as repeatable is committed and linked from the README. These methods never push. Other routes do not invent a commit step.

## It's working if

- The route names the deciding boundary, not just a matching keyword.
- It identifies expected artifacts, repository baselines, and verification level.
- Claims about another skill follow an actual read of that skill.
- Small work avoids unnecessary durable artifacts, while large work keeps decisions and dependencies.
- A selected Subagent branch points to a real worker path and never recommends a parent-agent or sequential substitute.

## Where it fits

`ask-matt` sits above the engineering skills rather than inside one flow. It most often routes to [grill-with-docs](https://aihero.dev/skills-grill-with-docs), [wayfinder](https://aihero.dev/skills-wayfinder), [research](https://aihero.dev/skills-research), or [triage](https://aihero.dev/skills-triage), then identifies the point where the main flow resumes.
