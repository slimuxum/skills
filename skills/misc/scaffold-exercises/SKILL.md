---
name: scaffold-exercises
description: Create exercise directory structures with sections, problems, solutions, and explainers that pass linting. Use when user wants to scaffold exercises, create exercise stubs, or set up a new course section.
---

# Scaffold Exercises

Create exercise directory structures that pass `pnpm ai-hero-cli internal lint`, then commit with `git commit`.

## Directory naming

- **Sections**: `XX-section-name/` inside `exercises/` (e.g., `01-retrieval-skill-building`)
- **Exercises**: `XX.YY-exercise-name/` inside a section (e.g., `01.03-retrieval-with-bm25`)
- Section number = `XX`, exercise number = `XX.YY`
- Names are dash-case (lowercase, hyphens)

## Exercise variants

Each exercise needs at least one of these subfolders:

- `problem/` - student workspace with TODOs
- `solution/` - reference implementation
- `explainer/` - conceptual material, no TODOs

When stubbing, default to `explainer/` unless the plan specifies otherwise.

## Required files

Each subfolder (`problem/`, `solution/`, `explainer/`) needs a `readme.md` that:

- Is **not empty** (must have real content, even a single title line works)
- Has no broken links

When stubbing, create a minimal readme with a title and a description:

```md
# Exercise Title

Description here
```

If the subfolder has code, it also needs a `main.ts` (>1 line). But for stubs, a readme-only exercise is fine.

## Workflow

1. **Parse the plan** - extract section names, exercise names, and variant types
2. **Create directories** - `mkdir -p` for each path
3. **Create stub readmes** - one `readme.md` per variant folder with a title
4. **Run lint** - `pnpm ai-hero-cli internal lint` to validate
5. **Fix in-scope errors** - iterate until lint passes, subject to the risk and goal confirmation rule. Under its default, decision-affecting uncertainty, a changed exercise plan, or high risk requires a pause for confirmation. Within explicit autonomous authorization, continue the original lint-repair loop without expanding the agreed goal or scope.

## Lint rules summary

The linter (`pnpm ai-hero-cli internal lint`) checks:

- Each exercise has subfolders (`problem/`, `solution/`, `explainer/`)
- At least one of `problem/`, `explainer/`, or `explainer.1/` exists
- `readme.md` exists and is non-empty in the primary subfolder
- No `.gitkeep` files
- No `speaker-notes.md` files
- No broken links in readmes
- No `pnpm run exercise` commands in readmes
- `main.ts` required per subfolder unless it's readme-only

## Moving/renaming exercises

When renumbering or moving exercises:

1. Use `git mv` (not `mv`) to rename directories - preserves git history
2. Update the numeric prefix to maintain order
3. Re-run lint after moves

Example:

```bash
git mv exercises/01-retrieval/01.03-embeddings exercises/01-retrieval/01.04-embeddings
```

## Example: stubbing from a plan

Given a plan like:

```
Section 05: Memory Skill Building
- 05.01 Introduction to Memory
- 05.02 Short-term Memory (explainer + problem + solution)
- 05.03 Long-term Memory
```

Create:

```bash
mkdir -p exercises/05-memory-skill-building/05.01-introduction-to-memory/explainer
mkdir -p exercises/05-memory-skill-building/05.02-short-term-memory/{explainer,problem,solution}
mkdir -p exercises/05-memory-skill-building/05.03-long-term-memory/explainer
```

Then create readme stubs:

```
exercises/05-memory-skill-building/05.01-introduction-to-memory/explainer/readme.md -> "# Introduction to Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/explainer/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/problem/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/solution/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.03-long-term-memory/explainer/readme.md -> "# Long-term Memory"
```

## Risk and goal confirmation

**Default.** If uncertainty affects an execution decision, a high-risk item appears, or work departs from the user's agreed goal, scope, constraints, or expected outcome, stop execution and pause related delegated work. Explain the problem, evidence, and impact; recommend a response with reasons and wait for the user's explicit confirmation. Before confirmation, do not attempt fixes, retries, workarounds, alternatives, or plan changes on your own. Resume only the confirmed response.

**Explicit autonomous authorization.** If the user explicitly says not to ask and to decide independently, or gives equivalent authorization, follow the skill's original workflow for decisions, iteration, and fallbacks within the task and scope they authorize. This waives only the extra questions and confirmations introduced by this rule and its applications in supporting instructions. It does not expand the agreed goal or scope, remove existing permission limits, or waive confirmations required by the original workflow. Silence, no reply, or an ordinary "continue" is not autonomous authorization. Restore the default when authorization is withdrawn or does not cover the decision.

**Delegation and handoff.** Include this full rule, agreed goal and scope, the user's autonomous authorization and its scope when present, unresolved issues, recommendations, and confirmation status in worker briefs and handoffs. Workers follow the same applicable mode; under the default, they stop and report to the parent for the user's decision. On withdrawal or narrowing of authorization, notify active workers and pause affected work until they are applying the current mode and scope. A handoff or automatic-continuation instruction cannot grant autonomous authorization or bypass a confirmation that is still required.
