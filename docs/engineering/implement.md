## What it does

`implement` builds work that has already been decided. Point it at a [ticket](https://www.aihero.dev/ai-coding-dictionary/ticket), a [spec](https://www.aihero.dev/ai-coding-dictionary/spec), or the plan agreed in the conversation. It maps every affected repository and logical context, implements the smallest complete vertical slice, chooses verification by risk and available environment, and sends the complete logical change through [code-review](https://aihero.dev/skills-code-review).

It does not silently widen the plan, writable scope, target operations, or release authority. Missing cross-repository dependencies, incompatible interfaces, unsafe target steps, or unavailable required evidence become explicit blockers. A successful run produces a reviewed change, an in-session evidence summary, and a commit on each affected repository's current branch; it never pushes.

- **Default:** Uncertainty affecting execution decisions, high risk, or departure from your goal, scope, constraints, or expected outcome pauses work and related workers. You receive evidence, impact, recommendations, and reasons; execution and adjustments await your explicit confirmation.
- **Explicit delegation:** “Do not ask; decide yourself” or equivalent allows the original workflow's judgment, iteration, and fallbacks within your authorized task and scope without this additional pause. Existing permission limits and required confirmations remain. Silence, no reply, or ordinary “continue” grants no exception. Workers and handoffs carry your authorization wording, scope, unresolved issues, and confirmation status. The default returns when authorization is withdrawn or does not cover the work. Withdrawal or narrowing reaches active workers, with affected work paused until their mode and scope are updated.

## When to reach for it

You invoke this by typing `/implement` — the agent won't reach for it on its own. It ships with `disable-model-invocation: true`, so no other skill can call it either. Wherever [ask-matt](https://aihero.dev/skills-ask-matt) or [to-tickets](https://aihero.dev/skills-to-tickets) says "then `/implement` per ticket", that is an instruction to you, not something the agent will do unprompted.

Where the work currently lives decides whether this is the right skill:

| The work is… | Reach for |
| --- | --- |
| A ticket on the tracker | `/implement #42`, one ticket per [session](https://www.aihero.dev/ai-coding-dictionary/session), [clearing](https://www.aihero.dev/ai-coding-dictionary/clearing) context between tickets |
| A spec, not yet split up, and the build spans sessions | [to-tickets](https://aihero.dev/skills-to-tickets) first, then `/implement` per ticket |
| A spec, and the build is small | `/implement` directly against the spec |
| Only in the conversation you just had, and it's still small | `/implement` right there, in the same window |
| Not written down anywhere yet | [grill-with-docs](https://aihero.dev/skills-grill-with-docs), or [grill-me](https://aihero.dev/skills-grill-me) if there's no codebase |
| One concrete behaviour you want test-first, with no spec | [tdd](https://aihero.dev/skills-tdd) directly |
| Already built, and you want it checked | [code-review](https://aihero.dev/skills-code-review) directly |

If the plan lives only in the thread, identify that conversation as the authority source when invoking the skill.

## Prerequisites

Before changing anything, identify every repository, source revision, current branch/worktree, logical context, generated or vendor-owned path, interface owner, authoritative compiler/linker/build/toolchain configuration, and allowed write scope. Confirm the current branch is the intended commit target in each repository, and separately confirm target/HIL, tracker, push, or release operations. Do not infer one repository from the current directory.

If the tickets came from [to-tickets](https://aihero.dev/skills-to-tickets), the tracker they live on was configured by [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills). `code-review` reads the same configuration to find the originating spec at close-out.

## What one run does

A run is six beats, in order:

1. Read the authority source and summarize the per-repository scope in the session.
2. Identify interfaces, integration order, runtime constraints, and a risk-based verification plan.
3. Implement one complete vertical slice across the affected repositories.
4. Use [tdd](https://aihero.dev/skills-tdd) only where a fast, deterministic red-green loop is faithful; use static, build, host, simulator, emulator, target, or HIL evidence elsewhere.
5. Run the required checks at the cheapest faithful environments and record each result with its environment, revision, and limitation.
6. Run [code-review](https://aihero.dev/skills-code-review) over committed, staged, unstaged, and untracked parts of the complete change, then commit the reviewed slice to the current branch. Do not push.

One run covers one coherent ticket or small specification, even when that logical slice spans repositories. Finish, commit, and report that slice before starting unrelated work.

## Verification seams and environments

The idea the skill runs on is the **seam**: a published source, binary, process, repository, protocol, or hardware boundary where behaviour can be observed without coupling the check to internals. A seam should follow real ownership or variation. Do not introduce an interface solely to make a test convenient.

Choose verification environments by the property at risk. Host tests can prove portable logic; simulation or emulation can prove modeled integration; target or HIL is needed when hardware, electrical behaviour, timing, memory layout, startup, concurrency, or peripheral integration is load-bearing. A lower-fidelity pass never implies a higher-fidelity pass.

## Common questions

**It finished, but my ticket is still open and the acceptance criteria are still unchecked.**

That can be correct. The implementation commit and tracker state are separate operations. The run reports what is complete and proposes any tracker or criteria update; it does not close or update the ticket automatically.

**Can I point it at all my tickets at once, or run several in parallel?**

No. One invocation, one ticket. Batch dispatch across a ticket queue and [subagent](https://www.aihero.dev/ai-coding-dictionary/subagent) fan-out are both requested repeatedly, and neither exists. Running several `/implement` sessions side by side in one checkout is worse than unsupported: one field report describes a `git commit --amend` in one session landing on another session's commit, a stash vanishing from `refs/stash`, and commits landing on the wrong branch, all in a single afternoon across three issues. The sessions share one working directory, one index, and one HEAD. Git worktrees are the community workaround, and note that `refs/stash` is shared across worktrees too, so worktrees alone do not fix the stash case. If you want parallelism today, you are assembling it yourself.

**Can it open a pull request instead of committing?**

The Skill commits the reviewed slice to the current branch. Pushing and opening a pull request are separate operations and require an explicit request.

**`code-review` says it cannot see my changes.**

The review receives an in-session per-repository scope summary covering the selected base, committed range, index, working tree, untracked files, generated artifacts, and cross-repository edges. An incomplete summary triggers the shared default: explain the gap and recommend a correction for your confirmation. Explicit delegation can cover correcting that summary within the authorized task and scope; it cannot broaden the review or write scope. Do not persist the summary or commit merely to make work visible.

With a Spec, exactly Standards and Spec run concurrently in isolated sub-agents. Without a Spec, only Standards runs. This is a hard capability requirement, not an invitation for the implementing agent to add axes or review sequentially in the parent context.

**One ticket burned 150k tokens. Am I using it wrong?**

Probably the ticket or workspace scope is too large. A run maps repositories and interfaces, implements a vertical slice, gathers risk-based evidence, and completes a parallel review. Right-size tickets in [to-tickets](https://aihero.dev/skills-to-tickets), but do not split an atomic cross-repository contract change into independently broken halves merely to reduce context.

**`/implement #2` in a fresh session worked on something completely unrelated.**

`#2` is resolved against whatever numbered list the agent can see, which in a fresh session may be a todo file, a checklist, or another work list rather than the configured tracker. The resolution is confident rather than fail-closed, so the mistake is not obvious until it has started. Pass the full reference, the issue URL or `owner/repo#2`, and ask it to confirm the title back before it begins.

## It's working if

- The session opens by reading the ticket or spec and restating what it will build, rather than asking you what to build.
- Every affected repository and logical context is named with its source revision and allowed write scope.
- Each important risk maps to a faithful static, host, simulator, emulator, target, or HIL check; unavailable evidence has an explicit status rather than an implied pass.
- The review covers the complete cross-repository logical change, including staged, unstaged, and untracked work.
- The reviewed slice is committed to the intended current branch; no target operation, tracker write, push, merge, or closure occurs without separate authority.
- The change is one coherent vertical slice through every affected interface, not unrelated work swept together.

## Where it fits

`implement` is the build step of the main chain, second from the end:

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review
```

Its neighbours are [to-tickets](https://aihero.dev/skills-to-tickets), which produces the tickets it consumes and declares blocking edges; [tdd](https://aihero.dev/skills-tdd), which supplies one verification technique where appropriate; and [code-review](https://aihero.dev/skills-code-review), which reviews the complete logical change before the final commit. It sits downstream of planning but still fails closed on missing scope, incompatible cross-repository contracts, unsafe operations, and unavailable required capabilities.

That trust is why [wayfinder](https://aihero.dev/skills-wayfinder) merges onto the chain at [to-spec](https://aihero.dev/skills-to-spec) rather than looping its map straight into `implement`. Go straight to `implement` from a map only when the effort turned out genuinely small.

[ask-matt](https://aihero.dev/skills-ask-matt) is the router over the whole set when you are not sure which flow you are in.
