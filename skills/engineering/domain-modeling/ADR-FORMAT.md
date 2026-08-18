# ADR Format

ADRs live in the configured canonical ADR location and follow that repository's naming and numbering convention.

If no ADR convention exists, propose the target repository, path, naming scheme, and numbering scheme, then obtain approval before creating anything. Do not default to `docs/adr/` or duplicate a mutable ADR across repositories.

## Template

```md
# {Short title of the decision}

{1-3 sentences: what's the context, what did we decide, and why.}
```

That's it. An ADR can be a single paragraph. The value is in recording *that* a decision was made and *why* — not in filling out sections.

## Optional sections

Only include these when they add genuine value. Most ADRs won't need them.

- **Status** frontmatter (`proposed | accepted | deprecated | superseded by ADR-NNNN`) — useful when decisions are revisited
- **Considered Options** — only when the rejected alternatives are worth remembering
- **Consequences** — only when non-obvious downstream effects need to be called out

## Numbering

Follow the configured canonical location's existing convention. If it uses sequential numbers, scan that location for the highest existing number and increment it. If no convention exists, include the proposed number in the write approval.

## When to offer an ADR

All three of these must be true:

1. **Hard to reverse** — the cost of changing your mind later is meaningful
2. **Surprising without context** — a future reader will look at the code and wonder "why on earth did they do it this way?"
3. **The result of a real trade-off** — there were genuine alternatives and you picked one for specific reasons

If a decision is easy to reverse, skip it — you'll just reverse it. If it's not surprising, nobody will wonder why. If there was no real alternative, there's nothing to record beyond "we did the obvious thing."

### What qualifies

- **Architectural shape.** "The safety monitor is isolated from the application partition." "The interface model is owned in a separate repository and generated into each consumer."
- **Integration patterns between contexts.** "The body-control and diagnostics contexts exchange versioned signals through the platform interface, not direct internal calls."
- **Technology choices that carry lock-in.** MCU family, RTOS, communication stack, serialization format, persistent storage, toolchain, or deployment target. Not every library — only choices that are expensive to replace.
- **Boundary and scope decisions.** "Signal validity is owned by the interface context; each consumer owns its fallback behavior." The explicit no-s are as valuable as the yes-s.
- **Deliberate deviations from the obvious path.** "This value is copied at the boundary to preserve timing isolation." Anything where a reasonable reader would assume the opposite. These stop the next engineer from "fixing" something that was deliberate.
- **Constraints not visible in the code.** "This path must complete within the allocated execution budget." "Dynamic allocation is unavailable after initialization on this target."
- **Rejected alternatives when the rejection is non-obvious.** Record why a plausible bus, scheduling, partitioning, or representation alternative was rejected so the same trade-off is not reopened without new evidence.
