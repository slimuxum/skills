---
name: handoff
description: Compact the current conversation into a handoff document for another agent to pick up.
argument-hint: "What will the next session be used for?"
disable-model-invocation: true
---

Write a handoff document summarising the current conversation so a fresh agent can continue the work. Resolve the destination before writing: use a user-named path within the authorised writable scope, or the user's OS temporary directory when no path was named. Label a temporary handoff as transient; do not treat the current directory as write authorisation.

Include a "suggested skills" section in the document, naming which skills the next agent should call the Skill tool for.

Do not duplicate content already captured in other artifacts (specs, plans, ADRs, issues, commits, diffs). Reference them by portable repository identifier and path, plus source revision, or by URL. Never rely on a host-absolute path alone.

When a workspace is involved, record its active Logical Contexts and every relevant repository or subtree. A Logical Context may span repositories and a repository may serve several contexts. For each repository, record the source revision and whether relevant uncommitted changes exist. Reference an existing diff artifact for uncommitted state when available; otherwise mark the reproducibility limitation instead of treating `HEAD` as the whole state. Distinguish verified facts, user decisions, unresolved assumptions, and the authoritative source for each consequential claim.

Redact any sensitive information, such as API keys, passwords, or personally identifiable information.

If the user passed arguments, treat them as a description of what the next session will focus on and tailor the doc accordingly.

Write only the handoff document. Do not commit, change issues, or otherwise advance project state.
