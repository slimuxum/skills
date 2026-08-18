# Issue tracker: GitLab

Issues and specs for the configured repositories or Logical Context live as GitLab issues. Record the explicit `<namespace/project>` for every tracker target and pass it to every command.

## Conventions

- **Create an issue**: `glab issue create --repo <namespace/project> --title "..." --description "..."`. Use an approved description source for multi-line bodies.
- **Read an issue**: `glab issue view <number> --repo <namespace/project> --comments`. Use `--output json` for machine-readable output.
- **List issues**: `glab issue list --repo <namespace/project> --output json` with appropriate `--label` filters.
- **Comment on an issue**: `glab issue note <number> --repo <namespace/project> --message "..."`. GitLab calls comments "notes".
- **Apply / remove labels**: `glab issue update <number> --repo <namespace/project> --label "..."` / `glab issue update <number> --repo <namespace/project> --unlabel "..."`. Multiple labels can be comma-separated or added by repeating the flag.
- **List available labels**: `glab label list --repo <namespace/project>`.
- **Close**: `glab issue close <number> --repo <namespace/project>`. `glab issue close` does not accept a closing comment, so post the explanation first with `glab issue note <number> --repo <namespace/project> --message "..."`, then close.
- **Merge requests**: GitLab calls PRs "merge requests". Keep `--repo <namespace/project>` on every `glab mr` command as well.

Do not infer the tracker project from the current directory or a Git remote. In a multi-repository scope, code and tracker ownership may differ. Keep `--repo <namespace/project>` on every issue, label, and MR command, and use an explicit project in every API path.

## Merge requests as a triage surface

**MRs as a request surface: no.** _(Set to `yes` if this repo treats external merge requests as feature requests; `/triage` reads this flag.)_

When set to `yes`, MRs run through the same labels and states as issues, using the `glab mr` equivalents:

- **Read an MR**: `glab mr view <number> --repo <namespace/project> --comments` and `glab mr diff <number> --repo <namespace/project>` for the diff.
- **List external MRs for triage**: `glab mr list --repo <namespace/project> --output json`, then keep only MRs whose author is not a project member/owner (a contributor's MR, not a maintainer's in-flight work).
- **Comment / label / close**: `glab mr note <number> --repo <namespace/project> --message "..."`, `glab mr update <number> --repo <namespace/project> --label "..."` / `glab mr update <number> --repo <namespace/project> --unlabel "..."`, and `glab mr close <number> --repo <namespace/project>`.

Unlike GitHub, GitLab numbers issues and MRs separately, so `#42` is unambiguous once you know which surface the maintainer means.

## When a skill says "publish to the issue tracker"

Create a GitLab issue.

## When a skill says "fetch the relevant ticket"

Run `glab issue view <number> --repo <namespace/project> --comments`.

## Wayfinding operations

Used by `/wayfinder`. The **map** is a single issue with **child** issues as tickets.

- **Map**: a single issue labelled `wayfinder:map`, holding the Notes / Decisions-so-far / Fog body. `glab issue create --repo <namespace/project> --label wayfinder:map`. (On GitLab tiers with native epics, an epic may hold the map instead; a labelled issue works everywhere.)
- **Child ticket**: an issue carrying `Part of #<map>` at the top of its description and labels `wayfinder:<type>` (`research`/`prototype`/`grilling`/`task`). Once claimed, the ticket is assigned to the driving dev.
- **Blocking**: GitLab's **native blocking link** — the canonical, UI-visible representation. Add it with the `/blocked_by #<n>` quick action, posted as a note (`glab issue note <child> --repo <namespace/project> --message "/blocked_by #<blocker>"`). Native blocking links are a Premium/Ultimate feature; on the free tier (or where unavailable) fall back to a `Blocked by: #<n>, #<n>` line at the top of the description. A ticket is unblocked when every blocker is closed.
- **Frontier query**: `glab issue list --repo <namespace/project> --output json` scoped to the map's children, drop any with an open blocker — a native `blocked_by` link to an open issue (`glab api projects/<url-encoded-namespace-project>/issues/<iid>/links`), or an open issue in the `Blocked by` line — or an assignee; first in map order wins.
- **Claim**: `glab issue update <n> --repo <namespace/project> --assignee @me` — the session's first write.
- **Resolve**: run `glab issue note <n> --repo <namespace/project> --message "<answer>"`, then `glab issue close <n> --repo <namespace/project>`, and append the map pointer in that same project.
