## What it does

`setup-matt-pocock-skills` configures the three project conventions the engineering skills assume: where tracker work goes, how triage roles map to labels, and where domain documents live or will live. Run it once per workspace before the first engineering flow. It supports one repository or several repositories with different roles and tracker targets.

It reads the project's current conventions first, previews every proposed edit, and writes only after approval. It does not create new project-wide inventories, domain infrastructure, tracker objects, or labels.

- **Default:** Uncertainty affecting execution decisions, high risk, or departure from your goal, scope, constraints, or expected outcome pauses work and related workers. You receive evidence, impact, recommendations, and reasons; execution and adjustments await your explicit confirmation.
- **Explicit delegation:** “Do not ask; decide yourself” or equivalent allows the original workflow's judgment, iteration, and fallbacks within your authorized task and scope without this additional pause. Existing permission limits and required confirmations remain. Silence, no reply, or ordinary “continue” grants no exception. Workers and handoffs carry your authorization wording, scope, unresolved issues, and confirmation status. The default returns when authorization is withdrawn or does not cover the work. Withdrawal or narrowing reaches active workers, with affected work paused until their mode and scope are updated.

## When to reach for it

You invoke this by typing `/setup-matt-pocock-skills` — the [agent](https://www.aihero.dev/ai-coding-dictionary/agent) will not reach for it on its own.

Reach for it before the first engineering flow in a workspace, and again when tracker, triage-label, or domain-document conventions change.

| Situation | What to do |
| --- | --- |
| First engineering flow in this workspace | Run setup |
| A skill needs to create or update tracker state, but the tracker target is unclear | Run setup |
| Triage needs labels, but their project-specific names are unknown | Run setup |
| A domain-aware flow needs canonical document locations that project guidance does not identify | Run setup |
| Repository or verification facts changed, but tracker and doc conventions did not | Skip setup and ground the task facts directly |

## Prerequisites

Name the repository or repositories setup may inspect. Before writing, approve every target repository and path. Setup will reuse the existing instruction-file authority and project layout; it will not scan sibling repositories or introduce a new top-level convention without permission.

## The three conventions

| Convention | What setup records | What it does not do |
| --- | --- | --- |
| Issue tracker | The exact GitHub `<owner/repo>`, GitLab `<namespace/project>`, local `<tracker-root>`, or existing custom workflow and commands | Create issues or other tracker state during setup |
| Triage labels | A mapping from the five canonical roles to labels already used by each configured tracker target | Create or rename labels |
| Domain docs | Pointers and consumer rules for existing or selected canonical locations for glossaries, ADRs, specifications, interface definitions, and related project documents | Create those documents or consolidate them into a new central artifact |

An automotive feature may span an interface repository, firmware repository, verification repository, and target-platform documentation. Setup can point each consumer at the existing locations and can record separate tracker targets where the project already uses them. It does not persist a master workspace table or duplicate mutable documents between repositories.

## Common questions

**Must I run it before the first engineering flow?**

Yes, once per workspace. Repeat it only when the configured conventions change.

**Will it scan or configure sibling repositories automatically?**

No. You name every repository in scope, and each write requires approval. Multi-repository support does not grant workspace-wide access.

**Does it require GitHub Issues?**

No. GitHub, GitLab, local Markdown, and existing custom trackers are supported. Every GitHub or GitLab command names its tracker project explicitly; local Markdown uses the root you choose rather than a fixed default directory.

**Does it create issues or labels?**

No. Setup records tracker targets, workflows, commands, and label mappings; it does not create tracker objects or labels itself.

**Does it configure source authority or verification?**

No. Downstream skills ground governing sources and verification procedures from the approved task scope. Setup may preserve links that already exist in project guidance, but it does not create a new project-wide inventory for them.

**What will it write?**

Only the approved `AGENTS.md` or `CLAUDE.md` pointer block and applicable `docs/agents/issue-tracker.md`, `docs/agents/triage-labels.md`, or `docs/agents/domain.md` files. It previews their complete contents first and never commits them.

## It's working if

- Every configured tracker command names its exact remote project, or every local path starts at the approved `<tracker-root>`.
- Triage mappings are complete whenever `triage` is installed and reuse existing labels when the tracker already has them.
- Domain-document pointers follow the project's existing locations without creating a parallel registry.
- Tracker commands retain their explicit remote project or configured local root.
- Every write was previewed and approved, and setup is not reported complete while a required tracker or domain-document convention remains unresolved.

## Where it fits

Setup is the run-once workspace configuration step before the engineering flow. Its closest neighbours are [triage](https://aihero.dev/skills-triage), [to-spec](https://aihero.dev/skills-to-spec), [to-tickets](https://aihero.dev/skills-to-tickets), and [wayfinder](https://aihero.dev/skills-wayfinder), which consume the tracker and document conventions it records. [ask-matt](https://aihero.dev/skills-ask-matt) routes here before the first engineering flow in a workspace.
