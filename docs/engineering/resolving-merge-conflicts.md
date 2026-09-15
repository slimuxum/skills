## What it does

`resolving-merge-conflicts` works through in-progress merges or rebases by intent across one or more repositories. It maps every operation and unrelated local change, resolves only the confirmed scope, verifies the integrated logical change, then stages and continues until the operation finishes. It never pushes.

It refuses to treat a conflict as a text problem. Before touching a hunk it traces each side back to its **[primary source](https://www.aihero.dev/ai-coding-dictionary/primary-source)** — commits, PRs, requirements, interface owners, generated-source inputs, and compatibility rules. It preserves both intents where compatible and names the trade-off where they are not. It never aborts or pushes.

- **Default:** Uncertainty affecting execution decisions, high risk, or departure from your goal, scope, constraints, or expected outcome pauses work and related workers. You receive evidence, impact, recommendations, and reasons; execution and adjustments await your explicit confirmation.
- **Explicit delegation:** “Do not ask; decide yourself” or equivalent allows the original workflow's judgment, iteration, and fallbacks within your authorized task and scope without this additional pause. Existing permission limits and required confirmations remain. Silence, no reply, or ordinary “continue” grants no exception. Workers and handoffs carry your authorization wording, scope, unresolved issues, and confirmation status. The default returns when authorization is withdrawn or does not cover the work. Withdrawal or narrowing reaches active workers, with affected work paused until their mode and scope are updated.

## When to reach for it

Type `/resolving-merge-conflicts`, or the [agent](https://www.aihero.dev/ai-coding-dictionary/agent) reaches for it automatically when a task fits.

Reach for it when git has already stopped on conflicts it could not resolve itself. It is scoped to the conflict in front of you, not to anything either side of it:

| Your situation | Skill |
| --- | --- |
| Mid-merge or mid-rebase, conflict markers in the tree | This one |
| Merge finished, something now misbehaves for reasons you can't see | [diagnosing-bugs](https://aihero.dev/skills-diagnosing-bugs) |
| Planning how to slice work so branches collide less | Neither — see the parallel-work question below |

## Primary sources over `ours` and `theirs`

The failure mode this exists to kill is resolving by flag: `--ours`, `--theirs`, or hand-deleting whichever block looks less important, so the markers go away and the build compiles. That resolution can be syntactically perfect and still silently drop a change somebody made on purpose.

You cannot preserve an intent you have not read. Work starts in every affected repository's history and owned interfaces, then moves to the diff. Verification covers the combined result at the environments relevant to the change — static, host, simulator, emulator, target, or HIL — rather than assuming the checks from each side prove the integration.

## Common questions

**Claude Code already resolves conflicts pretty well on its own. Why does this need a skill?**

The added value is the "find the primary sources" and "run feedback loops" steps, which otherwise have to be prompted by hand every time. An unprompted agent will usually produce a plausible resolution from the diff alone and stop there. The skill's value is the two steps it will not let the agent skip — reading why each side exists, and running the checks afterwards. That is a thin margin over a good [model](https://www.aihero.dev/ai-coding-dictionary/model), and it is meant to be: at least one reader has predicted this is a whole skill that becomes a no-op as models improve.

**Should I keep parallel agents off the same files to avoid conflicts in the first place?**

Mostly no. Zoning files off between parallel tasks costs more than it saves, because agents are good enough at merge conflicts that the tradeoff is not as harsh as it looks. The one piece of discipline worth keeping is to do large refactors first. A large rename landing after ten branches have forked off it is the case that stays expensive.

One caveat from a user report on parallel worktrees: when sibling [sessions](https://www.aihero.dev/ai-coding-dictionary/session) each build a ticket in their own tree, the merge back is best done by the session that wrote the change, because it is the one that already knows the intent. Batching everybody's conflicts onto one agent at the end throws away exactly the [context](https://www.aihero.dev/ai-coding-dictionary/context) step 2 of this skill has to go and reconstruct.

**Will it abort, continue, stage, or commit for me?**

It will not abort. Once the conflicts are resolved and the integrated result is verified, it stages the resolved files and continues the merge or rebase until completion, including the commit Git requires. It never pushes.

## It's working if

- The agent quotes commit messages, PRs or issues at you while resolving, not just diff hunks.
- Every hunk ends up with both sides' behaviour, or with an explicit note naming what was dropped and why.
- Nothing appears in the result that was on neither branch.
- Each relevant interface, generated source, and cross-repository compatibility edge is checked against its owner or primary source.
- Appropriate static, host, simulator, emulator, target, or HIL evidence states what ran, its result and limitations; unavailable or unrun evidence is not implied green.
- The resolved scope and verification evidence are shown before staging and continuing.
- The operation finishes without aborting, and nothing is pushed.

## Where it fits

A reach-for-it-anytime standalone with no dependencies on another skill: it starts when Git stalls and ends when conflicts are resolved, evidence is reported, and the merge or rebase has been continued until Git finishes it. Its nearest neighbour is [diagnosing-bugs](https://aihero.dev/skills-diagnosing-bugs), which takes over when an integrated result misbehaves. It sits off the main idea-to-ship flow, so [ask-matt](https://aihero.dev/skills-ask-matt) is the map for what runs before and after it.
