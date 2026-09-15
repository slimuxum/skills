---
name: code-review
description: "Review a complete logical change across one or more repositories on two axes: Standards and, when an authoritative specification exists, Spec. With a spec, run exactly two isolated sub-agents in parallel; without one, run only Standards. Use for branches, PRs, work in progress, or changes since fixed points."
---

# Code Review

Review the complete logical change without editing it. Keep Standards and Spec separate so one cannot mask the other.

## Process

### 1. Summarize the review scope in session

Map every repository and logical context in scope. In the conversation and reviewer briefs, summarize for each repository:

- root, source revision, branch, and user-supplied fixed point; ask rather than guess when it is missing
- committed change from merge-base to `HEAD`
- staged, unstaged, and untracked files
- generated/vendor files and cross-repository contract or integration edges

Use repository-specific commands. A single `git diff` is not a complete multi-repository change. Confirm every fixed point resolves and the combined change is non-empty before dispatching reviewers.

Keep this scope summary ephemeral in the conversation and reviewer briefs. Do not persist or synchronize it anywhere.

### 2. Identify authoritative sources

Find the originating specification or requirement in this order: issue references in commit messages through the configured tracker workflow; a path passed by the user; a matching spec under `docs/`, `specs/`, or `.scratch/`; then ask the user where it is. Only when the user confirms there is no specification may the Spec axis be skipped. Do not silently treat a failed search as proof that no spec exists, and do not invent requirements.

Collect coding standards, contribution rules, build/toolchain configuration, architecture docs, ADRs, owned headers/IDL/schemas, compatibility policy, verification plan/results, and applicable runtime or safety constraints. Repository-specific authority wins over generic heuristics.

The Standards child must receive this judgement-call baseline in full, not only the smell names:

- **Mysterious Name** — a function, variable, or type whose name does not reveal what it does or holds. Rename it; if no honest name comes, the design is murky.
- **Duplicated Code** — the same logic shape appears in more than one changed hunk or file. Extract the shared shape and call it from both.
- **Feature Envy** — a method reaches into another object's data more than its own. Move the method onto the data it envies.
- **Data Clumps** — the same fields or parameters keep travelling together. Bundle them into one type.
- **Primitive Obsession** — a primitive or string stands in for a domain concept. Give the concept its own small type.
- **Repeated Switches** — the same `switch` or `if` cascade on the same type recurs. Replace it with polymorphism or one shared map.
- **Shotgun Surgery** — one logical change forces scattered edits across many files. Gather what changes together into one module.
- **Divergent Change** — one file or module is edited for several unrelated reasons. Split it so each module changes for one reason.
- **Speculative Generality** — abstractions, parameters, or hooks exist for needs the specification does not have. Delete or inline them until a real need appears.
- **Message Chains** — long `a.b().c().d()` navigation exposes a walk the caller should not know. Hide it behind one method on the first object.
- **Middle Man** — a class or function mostly delegates onward. Remove it and call the real target directly.
- **Refused Bequest** — a subclass or implementer ignores or overrides most inherited behavior. Drop the inheritance and use composition.

Repository rules override this baseline. Each smell remains a labelled heuristic, never a hard violation, and anything tooling already enforces is skipped.

### 3. Fix the two-axis topology

- **Standards** — always run. Ask whether the change is built right under repository-authoritative engineering rules.
- **Spec** — run only when an authoritative requirement or specification exists. Ask whether the change implements the right thing without omissions or scope creep.

Interface, architecture, verification, runtime, and safety are checks inside these axes, never additional axes or sub-agents.

### 4. Preflight and dispatch

Determine required capacity before review: exactly two isolated sub-agents when Spec exists, otherwise exactly one Standards sub-agent. When two are required, they must start concurrently. If the harness cannot supply the required isolation or capacity, stop with `BLOCKED`; run nothing in the parent and never substitute sequential review.

With Spec, spawn exactly Standards and Spec in parallel. Without Spec, spawn only Standards and report Spec as `NOT_APPLICABLE`; do not create any other review sub-agent. Give each child the in-session scope summary, agreed review objective, relevant change content, only its authoritative sources, the full risk and goal confirmation rule with any explicit autonomous authorization, its scope, and pending confirmations, and this guard. Paste the full smell baseline above into the Standards child brief; the child must not depend on implicit access to this Skill.

> Review only the assigned axis. Do not invoke `code-review` or any other Skill. Do not spawn sub-agents. Do not edit files, run destructive commands, or broaden scope. Cite repository, path, line/hunk, authority, impact, and a concrete remedy for every finding. Distinguish confirmed defects from questions and limitations.

Axis-specific checks:

- **Standards:** check documented rules and the full judgement-call baseline supplied above. Repository rules override heuristics; skip tooling-enforced diagnostics. Also check repository-authoritative interface/architecture constraints, source/binary/data/protocol compatibility, ownership, generated-source consistency, dependency direction, cross-repository integration, build/toolchain rules, bounded resources, concurrency/interrupt context, timing, lifecycle, error handling, fault containment, security, hardware effects, and recovery. Check that claimed static/host/simulator/emulator/target/HIL evidence is independent, revision-bound, environment-faithful, and explicit about what ran, its result, and its limitations. Keep the axis report under 400 words.
- **Spec:** check missing or partial requirements, scope creep, and incorrect implementation. Include specified interface/architecture contracts, compatibility and integration order, required verification environments and acceptance evidence, plus runtime, timing, memory, concurrency, lifecycle, fault, security, hardware, target, and HIL requirements. Keep the axis report under 400 words.

### 5. Aggregate without masking

Present the two reports under `## Standards` and `## Spec` verbatim or lightly cleaned. When the user confirmed that no Spec exists, make `## Spec` say no specification is available. Add coverage and limitations inside the relevant axis; do not create another review axis. Preserve each axis's severity and citations; do not blend or rerank them into one score.

End with finding counts and the worst finding within each axis. A clean review means no supported findings in the reviewed scope, not that the product is complete. Make no code, tracker, commit, or workflow-state changes.

## Risk and goal confirmation

**Default.** If uncertainty affects an execution decision, a high-risk item appears, or work departs from the user's agreed goal, scope, constraints, or expected outcome, stop execution and pause related delegated work. Explain the problem, evidence, and impact; recommend a response with reasons and wait for the user's explicit confirmation. Before confirmation, do not attempt fixes, retries, workarounds, alternatives, or plan changes on your own. Resume only the confirmed response.

**Explicit autonomous authorization.** If the user explicitly says not to ask and to decide independently, or gives equivalent authorization, follow the skill's original workflow for decisions, iteration, and fallbacks within the task and scope they authorize. This waives only the extra questions and confirmations introduced by this rule and its applications in supporting instructions. It does not expand the agreed goal or scope, remove existing permission limits, or waive confirmations required by the original workflow. Silence, no reply, or an ordinary "continue" is not autonomous authorization. Restore the default when authorization is withdrawn or does not cover the decision.

**Delegation and handoff.** Include this full rule, agreed goal and scope, the user's autonomous authorization and its scope when present, unresolved issues, recommendations, and confirmation status in worker briefs and handoffs. Workers follow the same applicable mode; under the default, they stop and report to the parent for the user's decision. On withdrawal or narrowing of authorization, notify active workers and pause affected work until they are applying the current mode and scope. A handoff or automatic-continuation instruction cannot grant autonomous authorization or bypass a confirmation that is still required.
