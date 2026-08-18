# CONTEXT.md Format

## Structure

```md
# {Context Name}

{One or two sentence description of what this context is and why it exists.}

## Language

**Order**:
{A one or two sentence description of the term}
_Avoid_: Purchase, transaction

**Invoice**:
A request for payment sent to a customer after delivery.
_Avoid_: Bill, payment request

**Customer**:
A person or organization that places orders.
_Avoid_: Client, buyer, account
```

## Rules

- **Be opinionated.** When multiple words exist for the same concept, pick the best one and list the others under `_Avoid_`.
- **Keep definitions tight.** One or two sentences max. Define what it IS, not what it does.
- **Only include terms specific to this project's context.** General programming concepts (timeouts, error types, utility patterns) don't belong even if the project uses them extensively. Before adding a term, ask: is this a concept unique to this context, or a general programming concept? Only the former belongs.
- **Group terms under subheadings** when natural clusters emerge. If all terms belong to a single cohesive area, a flat list is fine.

## Workspace and context mapping

The glossary location is configured, not inferred. A workspace may contain several repositories; a Logical Context may span repositories or select subtrees; one repository may participate in several contexts.

When a map is needed, list stable repository identifiers, subtree membership, canonical glossary/ADR locations, and relationships explicitly:

```md
# Context Map

## Repositories

- `control-fw` — target firmware
- `shared-if` — owned interface definitions
- `verification` — host, simulator, and HIL assets

## Contexts

### Motion Control

- Members: `control-fw:src/motion`, `shared-if:motion`, `verification:motion`
- Glossary: `shared-if:docs/contexts/motion.md`
- ADRs: `shared-if:docs/adr/motion/`

## Relationships

- **Motion Control → Diagnostics**: publishes bounded status and fault information through the owned interface
```

Use the repository's existing format when one exists. If no map or canonical glossary exists, propose the smallest suitable location and obtain approval before creating it. Never default to a root `CONTEXT.md`, duplicate one glossary across repositories, or infer the active context from the current directory. If membership is unclear, ask.
