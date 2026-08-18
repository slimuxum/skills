---
name: wizard
description: Generate an interactive host-side bash wizard for manual procedures only a human can perform. Use for tool, target, bench, safe-state, observation, and recovery workflows for lab or HIL work, or for credentials, provisioning, CI secrets, migrations, and cutovers. Do not invoke it for steps the agent can perform itself.
---

# Wizard

A **wizard** is a host-side bash script that walks a human through a controlled manual procedure. The default engineering shape identifies the tool, exact target and bench, establishes a documented safe state, gates the approved action, captures the expected observation, and gives a stop-and-recover path. A stage may also open a URL or capture a credential when the scoped procedure actually needs it.

The core UX is already solved by [template.sh](template.sh): stage-by-stage progress, instructions, confirmation gates, and a closing summary. Its shipped Stripe stage is only a replace-me example; remove it from every generated wizard. The library retains optional helpers for URLs, hidden credential entry, `.env` upserts, and GitHub secret or variable writes. **Use those credential helpers only when the approved procedure requires those exact destinations.** They are not the default wizard flow. Your job is to scope the procedure and author its stages without hand-editing the library above the `STAGES` marker.

A wizard is ephemeral by default: save it to a scratch or `scripts/` path and delete it when the job is done. Commit it only when the user explicitly wants a repeatable controlled procedure in the repository.

## Process

### 1. Scope the procedure

Work out every manual step the human must take and every value or observation captured along the way. Read the applicable repository procedures and tool configuration first—don't ask cold:

- For lab, target, or HIL work: exact equipment/target identity, connection and power state, required tools, permissions, exclusive-use rules, safe-state checks, stop conditions, and recovery/rollback.
- For a manual tool procedure: tool and version, working directory or project, inputs, command or UI path, expected observation, failure signal, and recovery.
- For a migration or transition: current state, target state, checkpoints, irreversible actions, backup, rollback, and post-condition verification.
- For credentials or software setup when applicable: inspect `.env`, `.env.example`, `.env.*`, the README, `docker-compose*`, framework configuration, and `.github/workflows/*`. Enumerate every `secrets.*` and `vars.*` reference, then identify the authoritative source, destination, sensitivity, and capture journey for each value.

Then show the user the ordered list of stages and the values each produces, ask whether the wizard is ephemeral or a repeatable repository procedure, and confirm — they may add, drop, or reorder.

**Done when:** every stage has prerequisites, action, confirmation, captured output/observation, destination, verification, failure handling, and recovery where applicable. Mark secrets for hidden entry and never echo them into evidence.

### 2. Map each stage's journey

For each stage, write the precise path a human follows: URL/UI path, host command, physical action, target/HIL control, expected observation, and recovery. Where the current UI, tool, hardware state, or command is unknown, stop and ask or consult authoritative docs; never invent a procedure.

**Done when:** every stage traces to concrete instructions a stranger could follow.

### 3. Author the wizard

Copy `template.sh` to the target path. Replace the example stages with one `stage` per step, in dependency order, and set `TOTAL_STAGES` correctly.

- Core helpers: `stage`, `say`/`step`, `ask`, `pause`, and `confirm`.
- Optional URL/credential helpers: `open_url`, `ask_secret`, `write_env`, `set_secret`, and `set_var`. Use only the helpers and destinations established during scope; do not introduce `.env`, GitHub, CI, or browser work by convention. `write_env` requires an explicitly approved `ENV_FILE`. The shipped GitHub helpers target the current repository, so do not call them in a multi-repository or ambiguous working directory; author an explicit approved `gh ... --repo <owner/repo>` stage instead.

Hold the bar the template sets: name the tool, target and bench where applicable; state the safe state and expected observation; use `ask_secret` for secrets; persist only approved values; and `confirm` immediately before every irreversible, target-affecting, shared-environment, or security-sensitive action. Add explicit stop-and-recover instructions for failed post-conditions. Keep each stage to one focused task. Don't touch the library above the marker.

### 4. Verify and hand off

- `bash -n <script>`; run `shellcheck` if available.
- `chmod +x <script>`.
- Don't run it end-to-end yourself. Trace it statically: every input and observation lands where scoped, secrets remain hidden, every dangerous step has a just-in-time confirmation and recovery path, every final claim has a verification step, and every `set_secret`/`set_var` name exactly matches the corresponding `secrets.*`/`vars.*` reference discovered during scoping.
- Tell the user how to run it. If the user selected a repeatable repository procedure during scoping, commit it and link it from the README so the next person runs the script instead of asking an AI. Never push.
- Report which static checks ran, their results and limitations. Keep the first human execution as separate evidence; never claim it ran when only the script was inspected.
