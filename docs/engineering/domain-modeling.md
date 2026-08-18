## What it does

`domain-modeling` sharpens the vocabulary and hard-to-reverse decisions of a logical context. It challenges overloaded terms, tests them against concrete scenarios, compares them with authoritative sources, and records resolved glossary terms inline in their canonical locations.

It treats Workspace, Repository, and Logical Context as separate boundaries. A glossary may therefore describe a context that spans repositories, without being copied into each repository.

## When to reach for it

Type `/domain-modeling`, or the agent reaches for it automatically when terminology, context boundaries, or durable design decisions need attention.

Use it when:

- the same term means different things in code, specifications, or conversation;
- a context boundary is unclear;
- an existing glossary contradicts current behavior;
- a costly architectural choice needs an ADR;
- a design conversation needs a maintained language underneath it.

Simply reading a glossary for grounding does not require this skill.

## Common questions

**Where does it create `CONTEXT.md`?**

It follows the configured canonical location. If none exists, it chooses one location for the active Logical Context and creates files lazily when the first term or decision is resolved. It does not duplicate a mutable glossary across repositories.

**Can one context span repositories?**

Yes. The skill records source paths and revisions for each repository while maintaining one canonical glossary where practical.

**What happens when code and a specification disagree?**

The conflict is surfaced with source, repository, revision, and affected context. The skill does not silently promote one source to authority.

**What belongs in the glossary?**

Canonical terms and relationships. Requirements, implementation plans, issue lists, and verification evidence belong elsewhere.

**When is an ADR justified?**

Only when a decision is costly to reverse, surprising without context, and the result of a real trade-off. It offers the ADR first and writes it when you accept.

## It's working if

- Vague terms are replaced by explicit alternatives and decisions.
- Boundary, failure, recovery, timing, and resource scenarios expose ambiguity.
- Every source conflict is visible and revision-aware.
- Resolved terms are written inline to the canonical glossary rather than batched for later.
- The report identifies affected contexts, repositories, files, unknowns, and `NOT_RUN` scope.

## Where it fits

[grill-with-docs](https://aihero.dev/skills-grill-with-docs) invokes domain modeling while interviewing a design. [to-spec](https://aihero.dev/skills-to-spec) consumes the resulting language and decisions. [ask-matt](https://aihero.dev/skills-ask-matt) routes questions about terminology here.
