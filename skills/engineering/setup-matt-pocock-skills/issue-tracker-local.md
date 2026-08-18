# Issue tracker: Local Markdown

Issues and specs for the configured repositories or Logical Context live as Markdown files under a configured `<tracker-root>`. Record the literal root and which repository or directory owns it.

## Conventions

- One feature per directory: `<tracker-root>/<feature-slug>/`
- The spec is `<tracker-root>/<feature-slug>/spec.md`
- Implementation issues are one file per ticket at `<tracker-root>/<feature-slug>/issues/<NN>-<slug>.md`, numbered from `01` — never a single combined tickets file
- Triage state is recorded as a `Status:` line near the top of each issue file (see `triage-labels.md` for the role strings)
- Comments and conversation history append to the bottom of the file under a `## Comments` heading

## When a skill says "publish to the issue tracker"

Create a new file under `<tracker-root>/<feature-slug>/` (creating the directory if needed).

## When a skill says "fetch the relevant ticket"

Read the file at the referenced path. The user will normally pass the path or the issue number directly.

## Wayfinding operations

Used by `/wayfinder`. The **map** is a file with one **child** file per ticket.

- **Map**: `<tracker-root>/<effort>/map.md` — the Notes / Decisions-so-far / Fog body.
- **Child ticket**: `<tracker-root>/<effort>/issues/NN-<slug>.md`, numbered from `01`, with the question in the body. A `Type:` line records the ticket type (`research`/`prototype`/`grilling`/`task`); a `Status:` line records `claimed`/`resolved`.
- **Blocking**: a `Blocked by: NN, NN` line near the top. A ticket is unblocked when every file it lists is `resolved`.
- **Frontier**: scan `<tracker-root>/<effort>/issues/` for files that are open, unblocked, and unclaimed; first by number wins.
- **Claim**: set `Status: claimed` and save before any work.
- **Resolve**: append the answer, set `Status: resolved`, and update the map pointer.
