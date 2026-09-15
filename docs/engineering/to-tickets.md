## What it does

`to-tickets` splits a plan, engineering specification, or settled current conversation into dependency-ordered, agent-sized outcomes. Each ticket carries logical-context scope, repository baselines, owned change surfaces, acceptance evidence, verification environment, dependencies, and scope boundaries.

It produces an execution map and stops. It does not implement, assign, close, branch, commit, or push.

- **Default:** Uncertainty affecting execution decisions, high risk, or departure from your goal, scope, constraints, or expected outcome pauses work and related workers. You receive evidence, impact, recommendations, and reasons; execution and adjustments await your explicit confirmation.
- **Explicit delegation:** “Do not ask; decide yourself” or equivalent allows the original workflow's judgment, iteration, and fallbacks within your authorized task and scope without this additional pause. Existing permission limits and required confirmations remain. Silence, no reply, or ordinary “continue” grants no exception. Workers and handoffs carry your authorization wording, scope, unresolved issues, and confirmation status. The default returns when authorization is withdrawn or does not cover the work. Withdrawal or narrowing reaches active workers, with affected work paused until their mode and scope are updated.

## When to reach for it

You invoke this by typing `/to-tickets` — the agent will not reach for it on its own.

Use it when the source requires more than one implementation session, when real dependency edges matter, or when several repositories must advance against recorded baselines. The source can be a plan, spec, or the settled current conversation.

Skip it for a single bounded change that one [implement](https://aihero.dev/skills-implement) session can complete safely.

## Prerequisites

Run [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) once for the workspace so the tracker and `ready-for-agent` label mapping are configured. The source may be a plan, specification, or settled current conversation, but it must have no unresolved decision that changes the decomposition.

## Tracer bullets without a Web default

A tracer bullet is the smallest coherent outcome that can be observed independently. It may cross several real change surfaces, but it is not automatically schema → API → UI.

Depending on the system, a slice may involve C or C++ code, build configuration, linker or startup behavior, drivers, operating-system integration, middleware, protocols, services, tools, generated code, documentation, tests, or hardware interaction.

Prefer one repository per ticket when the outcome remains coherent. A cross-repository ticket must justify the coupling, list every baseline revision, and keep commit permission separate per repository.

## Common questions

**How are tickets verified?**

Each ticket selects applicable static, host, simulator, emulator, target, or HIL checks. It names the procedure, expected evidence, prerequisites, and limitations. Later gates may consume earlier evidence, but unavailable hardware is never called green.

**What if the work is a wide mechanical refactor?**

Use expand–migrate–contract when no thin slice can remain valid alone. Migration batches own observable checks, and an explicit integration gate carries any validity that cannot exist per batch.

**Does the skill invent file paths?**

No. It uses stable identifiers and paths only when the source evidence supports them. Tickets describe owned change surfaces, not speculative line-by-line implementation plans.

**When does it create tracker tickets?**

After presenting the blocker-first list, repository ownership, verification, dependencies, exceptions, and every proposed mutation. The user can merge, split, reorder, or change ownership before approving publication.

**Are tickets automatically agent-ready?**

Yes. Once you approve the breakdown, real tracker tickets receive the configured `ready-for-agent` label unless you instruct otherwise, and local ticket files carry `Status: ready-for-agent`. The skill does not assign or close them, create branches, commit, or push, and it never closes or modifies a source parent issue.

## It's working if

- Every ticket answers “what observable outcome exists when this is done?”
- Acceptance can fail at the recorded baseline or protects an explicit invariant.
- Dependencies are real edges rather than list order.
- Each repository and revision is visible, including cross-repository exceptions.
- A fresh agent can work from the spec and ticket without hidden conversation context.
- The initial frontier and all tracker mutations are reported after publication.

## Where it fits

```txt
to-spec → to-tickets → implement (uses tdd where applicable, then code-review)
```

[to-spec](https://aihero.dev/skills-to-spec) owns the engineering requirements. [implement](https://aihero.dev/skills-implement) owns one approved frontier ticket. [ask-matt](https://aihero.dev/skills-ask-matt) decides whether ticket decomposition is needed.
