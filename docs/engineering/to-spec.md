## What it does

`to-spec` converts settled decisions into a canonical engineering specification. It does not repeat the design interview; it synthesizes the conversation, map, research, and codebase evidence already available. Its planned writing checkpoint is to sketch the highest practical public verification seams and confirm them with you before writing.

It records the workspace baseline, per-repository revisions, logical contexts, governing sources, required behavior, constraints, change map, verification environments, acceptance evidence, risks, and delivery constraints.

A specification is not restricted to User Stories. It uses state, interface, timing, resource, failure, compatibility, migration, generated-artifact, safety, security, or other requirement forms only when the problem needs them.

- **Default:** Uncertainty affecting execution decisions, high risk, or departure from your goal, scope, constraints, or expected outcome pauses work and related workers. You receive evidence, impact, recommendations, and reasons; execution and adjustments await your explicit confirmation.
- **Explicit delegation:** “Do not ask; decide yourself” or equivalent allows the original workflow's judgment, iteration, and fallbacks within your authorized task and scope without this additional pause. Existing permission limits and required confirmations remain. Silence, no reply, or ordinary “continue” grants no exception. Workers and handoffs carry your authorization wording, scope, unresolved issues, and confirmation status. The default returns when authorization is withdrawn or does not cover the work. Withdrawal or narrowing reaches active workers, with affected work paused until their mode and scope are updated.

## When to reach for it

You invoke this by typing `/to-spec` — the agent will not reach for it on its own.

Use it after [grill-with-docs](https://aihero.dev/skills-grill-with-docs) or a resolved [wayfinder](https://aihero.dev/skills-wayfinder) map when implementation spans sessions or needs a durable reviewable contract.

For a genuinely small, settled change that fits one session, [implement](https://aihero.dev/skills-implement) may be enough.

## Prerequisites

Run [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) once for the workspace so the project tracker/spec location, triage labels, and domain-document conventions are configured. The conversation or Wayfinder map must already contain settled decisions; this Skill does not start a second interview.

## Common questions

**Does the spec assume one repository or one context?**

No. It lists each repository and baseline revision, then maps logical contexts independently. A context may cross repositories and a repository may contain several contexts.

**What replaces the old User Story template?**

An outcome- and evidence-oriented structure. User Stories remain optional for human workflows, while embedded or systems work can specify modes, transitions, interfaces, units, timing, faults, resources, diagnostics, generated artifacts, and delivery constraints directly.

**What is the acceptance seam?**

The highest practical public owned boundary where the changed behaviour can be observed without reaching into implementation details. Existing seams are preferred, fewer is better, and one is ideal when it faithfully covers the change. It might be a published interface, host test surface, simulator/emulator boundary, protocol, target diagnostic interface, HIL observation point, or generated artifact. The skill sketches these seams and confirms them with you before writing.

**Does every requirement need target or HIL evidence?**

No. The spec selects applicable static, host, simulator, emulator, target, and HIL environments and states prerequisites and limitations. It never turns an unavailable environment into an implicit pass.

**Will it publish and label the spec automatically?**

Yes. Once you have confirmed the verification seams and the specification is complete, it publishes to the configured project tracker or spec location and applies the configured `ready-for-agent` label. It does not branch, commit, push, or edit implementation.

## It's working if

- Outcome and current baseline can be distinguished by an observable check.
- Repository revisions, logical contexts, source authority, and exclusions are explicit.
- Requirements use structures appropriate to the system instead of a forced product template.
- Every acceptance item names an environment, procedure, expected evidence, and limitation.
- Source conflicts and unknowns remain visible.
- Publication reports the canonical location and any verification that has not run.

## Where it fits

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review
wayfinder ────────────┘
```

[to-tickets](https://aihero.dev/skills-to-tickets) slices the approved spec into independently verifiable outcomes. [ask-matt](https://aihero.dev/skills-ask-matt) routes small work around the spec only when its cost is unnecessary.
