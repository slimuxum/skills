---
name: setup-matt-pocock-skills
description: "Configure issue-tracker, triage-label, and domain-document conventions for the engineering skills across one or more repositories. Run once before first use."
disable-model-invocation: true
---

# Setup Matt Pocock's Skills

Run once per workspace before the first engineering flow, and again when tracker, triage-label, or domain-document conventions change. Scaffold the three configurations the downstream engineering skills assume; do not turn setup into a software-verification or implementation phase.

The scope may contain one repository or several, including separate interface, firmware, verification, platform, or calibration repositories. Do not create a new persistent cross-repository inventory or other project-wide infrastructure.

Use this sequence: inspect read-only, present findings, resolve choices, preview exact writes, obtain approval, then edit only approved files. Do not create tracker objects, labels, underlying domain documents, branches, commits, or remote state.

## 1. Inspect existing conventions

Confirm the repositories the user wants to configure. Do not scan parent or sibling directories without permission. In each approved repository, read what already exists:

- root `AGENTS.md` or `CLAUDE.md` and any existing `## Agent skills` block;
- `docs/agents/` output from a previous setup;
- configured issue-tracker targets, local tracker paths, and label vocabulary;
- existing glossaries, context documents, ADR locations, specifications, interface documents, and links to them;
- Git remotes and available tracker CLIs as clues, not as authority;
- whether the `triage` skill is installed.

Reuse project conventions rather than replacing them. Do not assume Node, Web, a monorepo, one repository, or one Logical Context per repository. Do not create a new cross-repository registry or other infrastructure.

## 2. Configure issue-tracker access

Configure where downstream specs, tickets, and triage work are published. A tracker may be shared across repositories, or different repositories or Logical Contexts may use different tracker projects. Recommend an explicit GitHub or GitLab target when project conventions make it clear; otherwise ask the user to choose GitHub, GitLab, local Markdown, or an existing custom workflow.

Record the exact target for every configured route:

- GitHub: explicit `<owner/repo>`;
- GitLab: explicit `<namespace/project>`;
- local Markdown: a user-approved `<tracker-root>` and the repository or directory that owns it;
- another tracker: the existing project identity and the operations downstream skills should use.

Record the tracker conventions and commands downstream skills need, always with the explicit target. Do not infer the tracker target from the current directory or select GitHub solely because a remote points there.

Use the applicable seed file in this skill folder. Its commands must retain the explicit tracker target so they remain safe when run from another repository.

## 3. Configure triage labels

Skip this section when `triage` is not installed. Otherwise, read the labels already used by each configured tracker target and map the five canonical roles to existing label strings:

- `needs-triage`;
- `needs-info`;
- `ready-for-agent`;
- `ready-for-human`;
- `wontfix`.

Recommend the identity mapping by default. If the tracker already uses different labels, collect the overrides so downstream skills reuse them rather than creating a second vocabulary. Setup records the mapping; it never creates labels.

Write `docs/agents/triage-labels.md` whenever `triage` is installed.

## 4. Configure domain-document layout

Read and reuse the project's existing locations for glossaries, context documents, ADRs, specifications, interface definitions, and other agent-facing domain documents. For an automotive or other multi-repository system, these locations may sit in different repositories and one Logical Context may span several of them.

Write the pointers and consumer rules downstream skills need in `docs/agents/domain.md`. Do not create the referenced glossary, ADR, context map, specification, interface document, or a new canonical summary as a side effect. If no convention exists, propose a single-context layout for a simple repository or a user-confirmed mapping for multiple Logical Contexts, then record the selected intended locations; do not leave this configuration absent.

## 5. Preview and write

Show the proposed `## Agent skills` block and the complete contents of every file to be changed. State the repository and path for each write. Ask the user to approve or revise the draft before the first edit.

For each configured repository, update the instruction file the project already treats as authoritative. If both `CLAUDE.md` and `AGENTS.md` exist and the authority is unclear, ask. If neither exists, ask whether and where to create one. Do not duplicate an existing `## Agent skills` block.

The block should point to the applicable configuration:

```markdown
## Agent skills

### Issue tracker

[tracker mapping and mutation policy]. See `docs/agents/issue-tracker.md`.

### Triage labels

[label vocabulary]. See `docs/agents/triage-labels.md`.

### Domain docs

[existing domain-document locations and consumer rules]. See `docs/agents/domain.md`.
```

Include Issue tracker and Domain docs. Include Triage labels whenever `triage` is installed. Use the seed files in this skill folder as starting points, but adapt them to the approved project conventions; they are examples, not authority:

- [domain.md](./domain.md)
- [issue-tracker-github.md](./issue-tracker-github.md)
- [issue-tracker-gitlab.md](./issue-tracker-gitlab.md)
- [issue-tracker-local.md](./issue-tracker-local.md)
- [triage-labels.md](./triage-labels.md)

Do not duplicate a mutable convention across repositories when the project already has one canonical copy. Add a pointer only where the project's existing instruction layout needs one.

## 6. Report

Report:

- configured repositories and exact files changed;
- tracker targets and configured workflows;
- triage-label mappings, when configured;
- existing domain-document locations that were referenced;
- any convention the user declined to resolve, in which case setup remains incomplete.

Do not claim that setup verified software behavior, target behavior, or compliance. Do not commit the setup changes. State that a commit requires separate, explicit authorization for each affected repository.
