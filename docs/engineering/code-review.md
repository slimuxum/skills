## What it does

`code-review` reviews a complete logical change across one or more repositories. It inventories committed, staged, unstaged, and untracked work, generated/vendor artifacts, and cross-repository integration edges. The review retains exactly two possible axes: **Standards** always runs; **Spec** runs only when an authoritative specification exists.

With a Spec, exactly two isolated [sub-agents](https://www.aihero.dev/ai-coding-dictionary/subagent) run concurrently: Standards and Spec. If the normal lookup finds no Spec, the Skill asks you where it is; only after you confirm none exists does it run the single Standards sub-agent. Missing isolation or capacity stops the review; the parent agent may not review or run the axes sequentially.

## When to reach for it

Type `/code-review`, or the agent reaches for it automatically when you ask to review a branch, a PR, work in progress, or anything "since X".

| Your situation | Reach for |
| --- | --- |
| A branch, PR, working tree, or cross-repository change exists and you want to know whether it is correct and sufficiently verified | `code-review` |
| You want bugs hunted in the diff — null paths, races, off-by-one | Claude Code's own built-in review, not this one (see the name clash below) |
| Nothing is written yet and you want it written test-first | [tdd](https://aihero.dev/skills-tdd) |
| A whole spec needs building, review included | [implement](https://aihero.dev/skills-implement), which calls this skill itself |
| The whole codebase has drifted, not one diff | [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) |
| Something is broken and you do not know why | [diagnosing-bugs](https://aihero.dev/skills-diagnosing-bugs) |

Supply the fixed point for every repository. If one is missing, the skill asks rather than guessing. It checks every ref and prepares an in-session scope summary before spawning anything; a single `git diff` is not accepted as a complete multi-repository review scope. The summary stays in the conversation and reviewer briefs.

## Prerequisites

The Standards axis reads each repository's own rules and falls back on a labelled smell baseline. It also reads applicable build/toolchain configuration, architecture docs and ADRs, owned headers/IDL/schemas, compatibility policy, verification results, and runtime or safety constraints. The Spec axis reads the authoritative requirement or [specification](https://www.aihero.dev/ai-coding-dictionary/spec).

The Spec axis needs a spec to exist and be findable. It looks in this order:

1. Issue references in the commit messages (`#123`, `Closes #45`, a GitLab `!67`), fetched through `docs/agents/issue-tracker.md`.
2. A path you pass in as an argument.
3. A spec file under `docs/`, `specs/`, or `.scratch/` matching the branch or feature name.
4. Asking you.

Step 1 depends on `docs/agents/issue-tracker.md`, which [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) writes. Without it the axis still works if you hand it a path. A failed search never silently cancels the Spec axis: the Skill asks, and skips that worker only when you confirm there is no spec.

## The two review axes

| Axis | Main question | Typical authority |
| --- | --- | --- |
| Standards | Is the change built right under repository-authoritative standards, interfaces, architecture, toolchain, evidence-quality, runtime, and safety rules? | Coding and contribution rules, headers/IDL/schema, ADRs, build graph/toolchain, compatibility policy, verification results, runtime and safety constraints |
| Spec | Is it the right thing, including every specified interface/architecture contract, verification environment, runtime property, and safety requirement, without scope creep? | Originating issue, specification, requirement, acceptance evidence |

Interface/architecture, verification, runtime, and safety are checklists inside these two axes. They do not create additional axes or sub-agents.

A generic review skill that does not know your standards is the thing this design is trying to avoid — it flags what is deliberate in your codebase and misses the invariants your codebase actually depends on. So the repo's own documentation is the [primary source](https://www.aihero.dev/ai-coding-dictionary/primary-source) on the Standards axis, and **the repo always overrides**.

The **smell baseline** is the floor underneath Standards: twelve Fowler code smells from _Refactoring_ ch.3 — Mysterious Name, Duplicated Code, Feature Envy, Data Clumps, Primitive Obsession, Repeated Switches, Shotgun Surgery, Divergent Change, Speculative Generality, Message Chains, Middle Man, Refused Bequest. Each is a labelled heuristic, never a hard violation. Anything tooling already enforces is skipped by that axis.

## Common questions

**It collides with Claude Code's own `/code-review`. What do I do?**

This is the most reported problem with the skill, and it is not fixed. Claude Code ships its own `/code-review`, which does something different — it hunts bugs in the diff, where this one checks spec compliance and repo standards. Installing this library means one of them wins, and which one wins depends on how you installed. Via the plugin marketplace, everything is aliased under a `mattpocock-skills:` prefix and the built-in becomes hard to reach at the unqualified name; via a plain skills install, the local file wins and this skill shadows the built-in. One clean answer is to remove Claude Code's built-in skills entirely: a large [context](https://www.aihero.dev/ai-coding-dictionary/context) saving, and the collision stops mattering. The shadowing itself is arguably a Claude Code [harness](https://www.aihero.dev/ai-coding-dictionary/harness) bug — a skill author should be free to name a skill anything — so the other answer is to rename the local copy. Editing the frontmatter or renaming the directory gets undone by `npx skills update`; the durable workaround reported by users is to fork the skill to a new name and drop `code-review` from the managed set, keeping a note of the commit you forked from so you can re-sync by hand.

**Can its sub-agents invoke `/code-review` again or spawn more agents?**

No. Every child brief explicitly forbids invoking `code-review` or any other Skill, spawning sub-agents, editing files, running destructive commands, or broadening scope. With Spec there are exactly two children; without Spec there is exactly one. If the required isolation or capacity is unavailable, the whole review stops as `BLOCKED`.

**Should I run it in the same [session](https://www.aihero.dev/ai-coding-dictionary/session) that wrote the code?**

Prefer a fresh one. As one reader put it: "Same context reviewing itself isn't review, it's confirmation bias with a slash command." The reviewing agent in the authoring session holds every assumption that shaped the code, which is exactly the context an independent reviewer would not have. This is also why people ask for [implement](https://aihero.dev/skills-implement) without its built-in review step — it runs the review inside the session that just wrote the diff. Invoking `/code-review` yourself from a clean session is the honest version.

**After every ticket, or once at the end?**

Both work, and the skill does not decide for you. Per-ticket keeps each diff small enough that the Spec axis has one clear spec to check against, which is the mode `implement` uses. Batching to the end of a branch catches interactions between tickets that the per-ticket passes each miss. If you are unsure, review per ticket and run one final pass against the branch point.

**Can I trust the findings?**

Not without checking. Sub-agent output is a hypothesis, not evidence. The skill preserves the independent axis reports, so read each cited repository, path, line/hunk, authority, impact, and remedy before acting. The citations and separate `Coverage and limitations` section are what make the review auditable.

**Why does it find new problems every single time I run it?**

Because fixes create new surface, and because the judgement-call half of the Standards axis is not deterministic between runs. One reader described the loop plainly: "/code-review and /improve-code-architecture always find new stuff every time. I implement fixes, rerun these skills, and again and again." There is no convergence guarantee. Treat a pass as a list of leads, act on the ones with a cited rule behind them, and stop — do not run it in a loop until it comes back clean, because it will not.

**Does it review my uncommitted or cross-repository work?**

Yes, when it is part of the declared logical change. The in-session scope summary covers each repository's committed range, index, working tree, untracked files, generated/vendor artifacts, and integration edges. It is not persisted. Do not create an interim commit just to make work visible to review.

## It's working if

- Every affected repository, source revision, fixed point, working-tree state, and cross-repository edge appears in the in-session scope summary, which is not persisted.
- With Spec, exactly Standards and Spec run as two isolated concurrent sub-agents. Only after you confirm no Spec exists does Standards run alone.
- No interface, architecture, verification, runtime, or safety sub-agent is created; those checks stay inside Standards and Spec.
- Anti-recursion guards are present, and missing required capacity produces `BLOCKED` before review starts with no parent/sequential fallback.
- Every finding cites the repository, path and line/hunk, authority, impact, and concrete remedy.
- Each axis report stays under 400 words and is presented verbatim or lightly cleaned.
- Static, host, simulator, emulator, target, and HIL claims remain scoped to the environments actually evidenced.
- Every claimed check states what ran, its result, and limitations; agreed checks that did not run remain explicit.
- The closing summary preserves a worst issue per axis and never invents one overall score.

## Where it fits

`code-review` is the review step at the tail of the build chain — `grill-with-docs → to-spec → to-tickets → implement → code-review` — and also stands alone on any branch or PR you point it at.

- [implement](https://aihero.dev/skills-implement) is the closest neighbour: it drives the build and sends the complete logical change here before the final commit.
- [to-spec](https://aihero.dev/skills-to-spec) and [to-tickets](https://aihero.dev/skills-to-tickets) produce the document the Spec axis checks against; a vague spec makes that axis vague.
- [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) is the whole-codebase counterpart; this skill reviews one declared logical change, which may span repositories.

[ask-matt](https://aihero.dev/skills-ask-matt) routes across the whole set when you are unsure which skill the situation wants.
