---
name: research
description: Investigate a bounded question against high-trust primary sources in a required background Subagent and capture revision-aware findings in one Markdown file. Use for documentation, standards, APIs, or source-reading legwork.
---

# Research

Spin up exactly one **background Subagent** to do the research, so you keep working while it reads. Do not replace the Subagent with synchronous research.

Its job:

1. Investigate one bounded question against **primary sources** — authoritative specifications, official documentation, first-party APIs, and source code at identified revisions — not secondary summaries. Follow every claim back to the source that owns it; use secondary sources only to locate primary evidence and label any unavoidable exception.
2. Keep Workspace, Repository, and Logical Context distinct. For every inspected repository, record its revision and paths in scope. Do not assume Node, Web, one repository, or one context.
3. Record each conclusion with a source link or repository path, revision when applicable, retrieval date for external material, and a limitation where the source cannot settle the claim. Surface conflicting authorities instead of silently choosing one.
4. Write the findings to a single Markdown file, citing each claim's source. Include the question and scope, repositories and revisions, findings, implications, conflicts, unknowns, and uninspected scope.
5. Save it where the owning repository or Logical Context already keeps research notes. Match the existing convention; if there is none, choose one sensible location and report it.
6. Return the file path and a concise summary.

When Wayfinder delegates a research ticket, follow that ticket's original capture rule: place the findings on its throwaway `research/<name>` branch and leave a context pointer from the ticket. Otherwise, research writes only the findings file and does not mutate tracker state, implementation, or generated artifacts.
