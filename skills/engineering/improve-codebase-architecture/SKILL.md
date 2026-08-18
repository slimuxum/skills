---
name: improve-codebase-architecture
description: Scan a single- or multi-repository workspace for deepening opportunities and cross-repository architectural friction, present a visual HTML report, then grill through whichever candidate the user picks.
disable-model-invocation: true
---

# Improve Codebase Architecture

Surface architectural friction and propose **deepening opportunities** — refactors that turn shallow modules into deep ones without losing interface, runtime, or verification constraints. The aim is locality, verifiability, and safe evolution across the whole logical change.

This command is _informed_ by the project's domain model and built on a shared design vocabulary:

- Call the Skill tool with "codebase-design" for the architecture vocabulary (**module**, **interface**, **depth**, **seam**, **adapter**, **leverage**, **locality**) and its principles: the deletion test, interface-as-verification-surface, and seams justified by real variation, ownership, isolation, or control. Use these terms exactly in every suggestion — don't drift into "component," "service," "API," or "boundary."
- The canonical glossary for each active Logical Context gives names to good seams; the applicable ADRs record decisions this command should not re-litigate.

## Process

### 1. Explore

Map the workspace before scanning: repositories, source revisions, logical contexts, relevant subtrees, and cross-repository interfaces. Confirm the requested read scope. Do not infer workspace boundaries from `package.json`, a monorepo layout, or the current working directory alone.

**Scope before you scan — YAGNI.** Deepening a module pays off by making future changes to it easier, so put extra weight on the parts of the codebase that have recently changed. Decide *where* to look before you look:

- If the user named a direction — a module, a subsystem, a pain point — take it, and skip the inference below.
- Otherwise, inspect history in every in-scope repository to find hot spots and cross-repository changes that repeatedly land together. If the changes are scattered with no clear hot spot, widen the net.

Read the configured glossary and applicable ADRs for every active Logical Context first. Do not infer them from the current directory or a repository-root filename.

Before scanning, verify that the harness can start an isolated exploration sub-agent. If it cannot, stop with `BLOCKED`; do not scan sequentially in the parent agent and do not write a report.

Spawn one sub-agent to walk the full in-scope workspace. Tell it not to invoke this Skill or spawn another agent. Don't follow rigid heuristics — explore organically and note where you experience friction:

- Where does understanding one concept require bouncing between many small modules?
- Where are modules **shallow** — interface nearly as complex as the implementation?
- Where have pure functions been extracted just for testability, but the real bugs hide in how they're called (no **locality**)?
- Where do tightly-coupled modules leak across their seams?
- Which parts of the codebase are untested, or hard to test through their current interface?
- Which contracts, generated artifacts, duplicated definitions, or integration sequences couple repositories?
- Where do ABI, timing, memory, concurrency, startup, shutdown, fault-containment, or hardware assumptions leak?

Apply the **deletion test** to anything you suspect is shallow: would deleting it concentrate complexity, or just move it? A "yes, concentrates" is the signal you want.

### 2. Present candidates as an HTML report

Write a self-contained HTML file to the OS temp directory so nothing lands in any repository. Resolve the temp directory from `$TMPDIR`, falling back to `/tmp` (or `%TEMP%` on Windows), and write `<tmpdir>/architecture-review-<timestamp>.html`. Use [HTML-REPORT.md](HTML-REPORT.md), open the report for the user with the platform-appropriate browser command, and tell them the absolute path. Each candidate must include a before/after visualisation. Use inline CSS and hand-built SVG so the report works in locked-down and offline engineering environments without CDN access.

For each candidate, render a card with:

- **Files** — which files/modules are involved
- **Workspace scope** — repositories, logical contexts, source revisions, and cross-repository interfaces involved
- **Problem** — why the current architecture is causing friction
- **Solution** — plain English description of what would change
- **Benefits** — explained in terms of locality and leverage, and how tests would improve
- **Runtime constraints** — applicable ABI, timing, memory, concurrency, lifecycle, fault-containment, and hardware concerns
- **Verification impact** — checks required in static, host, simulator, emulator, target, or HIL environments, plus limitations
- **Before / After diagram** — side-by-side, custom-drawn, illustrating the shallowness and the deepening
- **Recommendation strength** — one of `Strong`, `Worth exploring`, `Speculative`, rendered as a badge

End the report with a **Top recommendation** section: which candidate you'd tackle first and why.

**Use the active Logical Context's canonical vocabulary for the domain, and the `/codebase-design` vocabulary for the architecture.** If the glossary defines "Order," talk about "the Order intake module" — not "the FooBarHandler," and not "the Order service."

**ADR conflicts**: if a candidate contradicts an existing ADR, only surface it when the friction is real enough to warrant revisiting the ADR. Mark it clearly in the card (e.g. a warning callout: _"contradicts ADR-0007 — but worth reopening because…"_). Don't list every theoretical refactor an ADR forbids.

Do NOT propose interfaces yet. After the report, ask the user: "Which of these would you like to explore?"

### 3. Grilling loop

Once the user picks a candidate, call the Skill tool with "grilling" to walk the decision tree with them — constraints, dependencies, the shape of the deepened module, what sits behind the seam, what tests survive.

Do not edit code during the survey. As decisions crystallize in the grilling loop, call the Skill tool with "domain-modeling" so glossary and decision records stay current:

- **Naming a deepened module after a concept absent from the canonical glossary?** Add it to the canonical glossary for that Logical Context.
- **Sharpening a fuzzy term during the conversation?** Update the canonical glossary inline.
- **User rejects the candidate with a load-bearing reason?** Offer an ADR, framed as: _"Want me to record this as an ADR so future architecture reviews don't re-suggest it?"_ Only offer when the reason would actually be needed by a future explorer to avoid re-suggesting the same thing — skip ephemeral reasons ("not worth it right now") and self-evident ones.
- **Want to explore alternative interfaces for the deepened module?** Call the Skill tool with "codebase-design" and use its design-it-twice parallel sub-agent pattern.
