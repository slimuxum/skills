# HTML Report Format

The architectural review is rendered as a single self-contained HTML file in the OS temp directory. Put all CSS in the file and draw diagrams with HTML and inline SVG. Do not require a CDN, package install, network connection, or external asset; automotive development environments are often offline or locked down.

## Scaffold

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Architecture review — {{repo name}}</title>
    <style>
      :root { color-scheme: light; font-family: ui-sans-serif, system-ui, sans-serif; }
      body { margin: 0; background: #fafaf9; color: #0f172a; }
      main { max-width: 64rem; margin: 0 auto; padding: 3rem 1.5rem; }
      section { margin-top: 3rem; }
      article { margin-top: 2rem; padding: 1.25rem; border: 1px solid #e2e8f0; border-radius: .75rem; background: white; }
      .diagram { border: 1px solid #e2e8f0; border-radius: .5rem; padding: 1rem; background: white; }
      .before-after { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 1rem; }
      .module { fill: #fff; stroke: #64748b; stroke-width: 1.5; }
      .seam { stroke-dasharray: 4 4; }
      .leak { stroke: #dc2626; }
      .deep { fill: #1e293b; color: white; }
      @media (max-width: 720px) { .before-after { grid-template-columns: 1fr; } }
    </style>
  </head>
  <body>
    <main>
      <header>...</header>
      <section id="candidates">...</section>
      <section id="top-recommendation">...</section>
    </main>
  </body>
</html>
```

## Header

Repo name, date, and a compact legend: solid box = module, dashed line = seam, red arrow = leakage, thick dark box = deep module. No introduction paragraph — straight into the candidates.

## Candidate card

The diagrams carry the weight. Prose is sparse, plain, and uses the glossary terms (from the `/codebase-design` skill) without ceremony.

Each candidate is one `<article>`:

- **Title** — short, names the deepening (e.g. "Collapse the signal-conditioning pipeline").
- **Badge row** — recommendation strength (`Strong` = emerald, `Worth exploring` = amber, `Speculative` = slate), plus a tag for the dependency category (`in-process`, `platform-substitutable`, `cross-process or cross-repository but owned`, `external or hardware-controlled`).
- **Files** — a compact monospaced list.
- **Before / After diagram** — the centrepiece. Two columns, side by side. See patterns below.
- **Problem** — one sentence. What hurts.
- **Solution** — one sentence. What changes.
- **Wins** — bullets, ≤6 words each. e.g. "Tests hit one interface", "State logic stops leaking", "Delete 4 shallow wrappers".
- **ADR callout** (if applicable) — one line in an amber-tinted box.

No paragraphs of explanation. If the diagram needs a paragraph to be understood, redraw the diagram.

## Diagram patterns

Pick the pattern that fits the candidate. Mix them. Don't make every diagram look the same — variety is part of the point.

### Inline SVG graph (the workhorse for dependencies / call flow)

Use an inline `<svg>` when the point is "X calls Y calls Z, and look at the mess." Give every node and edge explicit coordinates, labels, markers, and accessible text. Colour leakage edges red and the deep module dark. A compact hand-drawn sequence works well for "before: 6 round-trips; after: 1."

```html
<div class="diagram">
  <svg viewBox="0 0 640 180" role="img" aria-label="Signal handling call graph with calibration leakage">
    <defs><marker id="arrow" markerWidth="8" markerHeight="8" refX="7" refY="4" orient="auto"><path d="M0,0 L8,4 L0,8 Z" fill="currentColor" /></marker></defs>
    <rect class="module" x="20" y="60" width="130" height="48" rx="6" />
    <text x="85" y="89" text-anchor="middle">SignalHandler</text>
    <path d="M150 84 H245" stroke="#64748b" marker-end="url(#arrow)" />
    <!-- Continue with explicit nodes and edges; use class="leak seam" for leakage. -->
  </svg>
</div>
```

### Hand-built boxes-and-arrows (when a freeform layout communicates better)

Modules can also be `<div>` elements with borders and labels, with arrows drawn as inline SVG `<line>` or `<path>` elements. Reach for this when the "after" diagram should feel like one thick-bordered deep module with greyed-out internals.

### Cross-section (good for layered shallowness)

Stack styled horizontal bands to show layers a call passes through. Before: 6 thin layers each doing nothing. After: 1 thick band labelled with the consolidated responsibility.

### Mass diagram (good for "interface as wide as implementation")

Two rectangles per module — one for interface surface area, one for implementation. Before: interface rectangle is nearly as tall as the implementation rectangle (shallow). After: interface rectangle is short, implementation rectangle is tall (deep).

### Call-graph collapse

Before: a tree of function calls rendered as nested boxes. After: the same tree collapsed into one box, with the now-internal calls shown faded inside it.

## Style guidance

- Lean editorial, not corporate-dashboard. Generous whitespace. Serif is optional for headings; use local system fonts only.
- Colour sparingly: one accent (emerald or indigo) plus red for leakage and amber for warnings.
- Keep diagrams ~320px tall so before/after sits comfortably side by side without scrolling.
- Use small uppercase labels with wider letter spacing inside diagrams — they should read as schematic, not as UI.
- The report is static and needs no scripts. If the user explicitly requests Mermaid and network access is known available, it may be an opt-in alternative, never the default or only rendering.

## Top recommendation section

One larger card. Candidate name, one sentence on why, anchor link to its card. That's it.

## Tone

Plain English, concise — but the architectural nouns and verbs come straight from the `/codebase-design` skill. Concision is not an excuse to drift.

**Use exactly:** module, interface, implementation, depth, deep, shallow, seam, adapter, leverage, locality.

**Never substitute:** component, service, unit (for module) · API, signature (for interface) · boundary (for seam) · layer, wrapper (for module, when you mean module).

**Phrasings that fit the style:**

- "Signal-processing module is shallow — interface nearly matches the implementation."
- "Calibration policy leaks across the seam."
- "Deepen: one interface, one place to test."
- "Platform variation justifies the seam: target bus in production, simulator adapter on host."

**Wins bullets** name the gain in glossary terms: *"locality: bugs concentrate in one module"*, *"leverage: one interface, N call sites"*, *"interface shrinks; implementation absorbs the wrappers"*. Don't write *"easier to maintain"* or *"cleaner code"* — those terms aren't in the glossary and don't earn their place.

No hedging, no throat-clearing, no "it's worth noting that…". If a sentence could be a bullet, make it a bullet. If a bullet could be cut, cut it. If a term isn't in the `/codebase-design` glossary, reach for one that is before inventing a new one.
