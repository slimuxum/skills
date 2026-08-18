# Engineering

Skills I use daily for code work.

## User-invoked

Reachable only when you type them (Claude Code: `disable-model-invocation: true`; Codex: `policy.allow_implicit_invocation: false` in `agents/openai.yaml`).

- **[ask-matt](./ask-matt/SKILL.md)** — Route a workspace-grounded situation to the right skill or flow.
- **[grill-with-docs](./grill-with-docs/SKILL.md)** — Pressure-test a workspace-grounded design while maintaining its canonical glossary and durable decisions inline.
- **[triage](./triage/SKILL.md)** — Triage incoming work across repositories, verify it in an appropriate environment, and draft a durable brief without treating tracker state as project completion.
- **[improve-codebase-architecture](./improve-codebase-architecture/SKILL.md)** — Scan a single- or multi-repository workspace for deepening opportunities and architectural friction, then grill through a selected candidate.
- **[setup-matt-pocock-skills](./setup-matt-pocock-skills/SKILL.md)** — Configure tracker, triage-label, and domain-document conventions once per workspace before the engineering flows.
- **[to-spec](./to-spec/SKILL.md)** — Synthesize settled decisions into a revision-aware engineering specification without interviewing again.
- **[to-tickets](./to-tickets/SKILL.md)** — Split a plan, spec, or settled conversation into dependency-ordered, independently verifiable tickets.
- **[implement](./implement/SKILL.md)** — Implement approved work across one or more repositories with risk-based verification and the original parallel review, then commit the reviewed slice to each current branch without pushing.
- **[wayfinder](./wayfinder/SKILL.md)** — Map a large, foggy effort as revision-aware decision tickets, using required parallel research workers when the frontier calls for them.

## Model-invoked

Model- or user-reachable (rich trigger phrasing so the model can reach for them).

- **[prototype](./prototype/SKILL.md)** — Build a disposable prototype for logic, timing, integration, target behaviour, or UI/HMI in its faithful runtime.

- **[diagnosing-bugs](./diagnosing-bugs/SKILL.md)** — Diagnose hard, intermittent, unsafe, or slow behaviour across static, host, simulator, emulator, target, and HIL environments.
- **[research](./research/SKILL.md)** — Investigate one bounded question in exactly one required background Subagent and write one revision-aware findings file.
- **[tdd](./tdd/SKILL.md)** — Use red-green development when the work has an executable, deterministic seam; choose the verification environment according to risk.
- **[domain-modeling](./domain-modeling/SKILL.md)** — Sharpen a logical context's terminology and decisions, updating its canonical glossary inline and offering durable ADRs.
- **[codebase-design](./codebase-design/SKILL.md)** — Reference deep-module and interface vocabulary for runtime, ownership, hardware, and verification constraints.
- **[code-review](./code-review/SKILL.md)** — Review a complete logical change on Standards and optional Spec axes using exactly one or two isolated Subagents.
- **[resolving-merge-conflicts](./resolving-merge-conflicts/SKILL.md)** — Resolve merge or rebase conflicts by intent across one or more repositories, then continue and finish the active Git operation without aborting or pushing.
- **[wizard](./wizard/SKILL.md)** — Generate a host-side wizard for controlled tool, target, bench/HIL, recovery, credential, migration, or cutover procedures.
