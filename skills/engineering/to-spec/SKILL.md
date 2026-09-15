---
name: to-spec
description: Synthesize a settled engineering conversation or Wayfinder map into a revision-aware specification without interviewing the user again.
disable-model-invocation: true
---

# To Spec

Convert agreed decisions into an implementation-facing engineering specification. Do **not** interview the user again; synthesize what the conversation, map, research, and codebase already establish. The original checkpoint remains confirmation of the proposed verification seams. Apply the additional risk and goal confirmation rule in its default or explicitly authorized autonomous mode. Do not invent missing decisions or implement the change. If another load-bearing decision is absent, list it as open and stop before publication rather than starting a new interview.

## Ground the specification

Read the workspace configuration or establish the same facts inline:

- workspace root;
- repositories in scope and each current revision;
- logical contexts and their repository or subtree membership;
- authoritative code, specifications, interface definitions, generated artifacts, the active Logical Context's glossary and ADRs, research, and user decisions;
- applicable verification environments;
- configured output path or project tracker target.

Workspace, Repository, and Logical Context are distinct. Do not assume one repository, one context, Node, Web, a UI, or a cloud service. When an input claim conflicts with an authoritative source, surface the conflict and resolve or list it as an open decision.

Use the active glossary's domain vocabulary throughout the specification and respect applicable ADRs.

If the workspace has not been configured, tell the user to run `setup-matt-pocock-skills`. Do not invoke that user-only skill or create configuration as a side effect.

## Agree the verification seams

Sketch the public owned seams at which the change will be tested or otherwise observed. Prefer existing seams to new ones and use the highest seam possible. If a new seam is needed, propose it at the highest stable interface; the fewer seams across the codebase, the better, with one being ideal when it faithfully covers the change.

For automotive software, a seam may be a published source or binary interface, process, repository, protocol, generated interface, simulator/emulator boundary, target diagnostic interface, or HIL observation point. Name the environment and what each seam cannot prove. Do not replace a public seam with an internal implementation detail just because it is easier to test.

Check with the user that these seams match their expectations. Do not write or publish the spec until they confirm them.

## Define the observable change

State the outcome and the smallest end-to-end observation that distinguishes the changed system from the baseline. The observation may be a static-analysis result, host behavior, simulator or emulator behavior, target behavior, HIL evidence, generated artifact, protocol trace, timing measurement, or another source-backed check.

Acceptance must be capable of failing at the recorded baseline unless it verifies an invariant that must remain true. A confirmed seam may require more than one environment when no single environment preserves every relevant property.

## Write the right kind of requirements

Use the structures the engineering problem needs. User stories are optional and belong only where a human workflow benefits from them.

Include applicable requirements for:

- functional behavior, modes, states, transitions, and failure or recovery behavior;
- interfaces, data formats, units, ranges, timing, ordering, compatibility, and configuration;
- resource, concurrency, persistence, startup, shutdown, diagnostics, serviceability, safety, or security constraints;
- generated artifacts, build integration, migration, rollout, and rollback;
- quality attributes and explicitly excluded behavior.

Do not claim compliance, safety integrity, performance, or hardware behavior without an authoritative source and planned evidence.

If a prototype produced a small decision-rich snippet that communicates a validated state machine, reducer, schema, interface, or type shape more precisely than prose, inline only that essential snippet and identify its prototype source. Do not paste a working demo.

## Specification format

Use this structure, omitting sections that are genuinely inapplicable rather than filling them with guesses:

```markdown
## Outcome

<observable end state and why it matters>

## Provenance and baseline

- Workspace: <root or stable identity>
- Inputs: <map, decisions, research, or conversation>
- Prepared against: <date>
- Source authority: <ordered sources and conflict policy>

## Scope

### Logical contexts
<contexts and boundaries>

### Repositories
| Repository | Baseline revision | In-scope subtrees or artifacts | Change ownership |
| --- | --- | --- | --- |

### Out of scope
<explicit exclusions>

## Current behavior and constraints

<source-backed baseline, interfaces, invariants, and limitations>

## Required behavior

<numbered functional and engineering requirements; optional user stories only where useful>

## Interface and state detail

<states, transitions, data, units, ranges, timing, errors, compatibility, or N/A with reason>

## Repository change map

<which outcomes touch which repositories or generated artifacts; no invented file list>

## Verification and evidence

| Requirement | Observable acceptance | Environment | Procedure or command | Expected evidence | Limitation |
| --- | --- | --- | --- | --- | --- |

## Delivery constraints

<ordering, migration, rollback, tooling, target or lab access, safe-state, and recovery constraints>

## Open decisions and risks

<unknowns, source conflicts, revision drift, and unavailable verification>
```

Applicable verification environments are static analysis, host, simulator, emulator, target, and HIL. Name the repository, working directory, prerequisites, inputs, expected artifact or observation, and limitations for each planned check. Make anything not executed explicit.

## Publish

After the verification seams are confirmed and the specification is complete, publish it to the configured project tracker or configured spec location. Apply the configured `ready-for-agent` triage label; no additional triage is needed. Do not create branches, commit, push, or edit implementation.

Report the canonical spec location, affected repositories and revisions, unresolved decisions, and verification that has not run.

## Risk and goal confirmation

**Default.** If uncertainty affects an execution decision, a high-risk item appears, or work departs from the user's agreed goal, scope, constraints, or expected outcome, stop execution and pause related delegated work. Explain the problem, evidence, and impact; recommend a response with reasons and wait for the user's explicit confirmation. Before confirmation, do not attempt fixes, retries, workarounds, alternatives, or plan changes on your own. Resume only the confirmed response.

**Explicit autonomous authorization.** If the user explicitly says not to ask and to decide independently, or gives equivalent authorization, follow the skill's original workflow for decisions, iteration, and fallbacks within the task and scope they authorize. This waives only the extra questions and confirmations introduced by this rule and its applications in supporting instructions. It does not expand the agreed goal or scope, remove existing permission limits, or waive confirmations required by the original workflow. Silence, no reply, or an ordinary "continue" is not autonomous authorization. Restore the default when authorization is withdrawn or does not cover the decision.

**Delegation and handoff.** Include this full rule, agreed goal and scope, the user's autonomous authorization and its scope when present, unresolved issues, recommendations, and confirmation status in worker briefs and handoffs. Workers follow the same applicable mode; under the default, they stop and report to the parent for the user's decision. On withdrawal or narrowing of authorization, notify active workers and pause affected work until they are applying the current mode and scope. A handoff or automatic-continuation instruction cannot grant autonomous authorization or bypass a confirmation that is still required.
