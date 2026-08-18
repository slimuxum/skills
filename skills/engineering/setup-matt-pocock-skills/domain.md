# Domain Docs

How the engineering skills should consume the domain documentation this project already uses. Adapt these pointers to the approved repositories and paths; do not use this file to invent a new cross-repository registry.

## Before exploring, read these

- the project instruction files that apply to the repository and subtree;
- the existing glossary or context document for the active Logical Context, wherever the project keeps it;
- existing ADRs, specifications, interface definitions, generated documentation, and platform or verification instructions that govern the work.

If optional domain artifacts do not exist, proceed without manufacturing them. Flag an absence only when it prevents an authoritative interpretation. The `/domain-modeling` skill may propose a canonical location when a term or decision actually needs to be recorded.

## Repository and context rules

- Treat Repository and Logical Context as separate boundaries.
- A Logical Context may span repositories or select subtrees; one repository may participate in several contexts.
- Follow existing links and project conventions to the relevant documents. Do not derive locations from `package.json`, a monorepo layout, `src/<context>`, or the current directory.
- When a feature spans interface, firmware, verification, platform, or calibration repositories, read each repository's applicable documents without copying them into a central summary.
- Keep mutable terms and decisions in their existing canonical location; use pointers rather than duplicated copies.
- Qualify cross-repository references by repository and path or subtree.

## Use the glossary's vocabulary

When your output names a domain concept, use the term from the active context's canonical glossary. Do not drift to synonyms it explicitly avoids.

If the concept is missing, either reconsider language the project does not use or note a real gap for `/domain-modeling`; do not silently add it.

## Flag ADR conflicts

If your output contradicts an existing ADR, specification, interface definition, or other governing project source, identify its repository, path, and affected Logical Context rather than silently overriding it:

> _Contradicts ADR-0007 (event-sourced orders) — but worth reopening because…_
