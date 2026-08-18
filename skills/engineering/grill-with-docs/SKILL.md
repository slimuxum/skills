---
name: grill-with-docs
description: Relentlessly interview a workspace-grounded plan or design while maintaining its glossary and architectural decisions inline.
disable-model-invocation: true
---

# Grill with Docs

Establish the workspace root, relevant repositories and revisions, logical context, source authority, and writable documentation paths.

Call the Skill tool twice, for "grilling" and "domain-modeling". Both disciplines stay active together throughout the interview.

Use `grilling` to pressure-test goals, boundaries, failure modes, timing, resources, interfaces, and verification assumptions. Use `domain-modeling` to resolve terminology and update the canonical glossary inline as decisions land, offering ADRs for durable trade-offs.

Treat Repository and Logical Context as separate boundaries. Do not assume Node, Web, a single repository, or a single context. Keep applicable verification environments explicit: static analysis, host, simulator, emulator, target, and HIL.

Maintain a short list of unresolved decisions, source conflicts, repository revisions, verification constraints, and out-of-scope areas for `to-spec`.

Do not implement, mutate tracker state, create branches, or commit. Do not widen the selected workspace or Logical Context.
