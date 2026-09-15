## What it does

`wizard` generates a host-side interactive bash script that walks a human through a controlled manual procedure. Its default engineering shape identifies the tool, target and bench, verifies a documented safe state, gates the approved action, captures the expected observation, and provides a stop-and-recover path. Credential, provisioning, migration and cutover procedures remain supported when they are the actual task.

The [agent](https://www.aihero.dev/ai-coding-dictionary/agent) writes and statically checks the script; it never runs the complete procedure. You do, on an authorized host with the required equipment and access. The first human execution is separate evidence and must not be claimed from static inspection.

- **Default:** Uncertainty affecting execution decisions, high risk, or departure from your goal, scope, constraints, or expected outcome pauses work and related workers. You receive evidence, impact, recommendations, and reasons; execution and adjustments await your explicit confirmation.
- **Explicit delegation:** “Do not ask; decide yourself” or equivalent allows the original workflow's judgment, iteration, and fallbacks within your authorized task and scope without this additional pause. Existing permission limits and required confirmations remain. Silence, no reply, or ordinary “continue” grants no exception. Workers and handoffs carry your authorization wording, scope, unresolved issues, and confirmation status. The default returns when authorization is withdrawn or does not cover the work. Withdrawal or narrowing reaches active workers, with affected work paused until their mode and scope are updated.

## When to reach for it

You can type `/wizard`, and the agent can also reach for it on its own. When it hits a step that requires human access, judgment, or physical control, it builds a repeatable gated procedure instead of leaving transient instructions in chat.

Reach for it when the next thing blocking you needs ordered human actions and recoverable checkpoints:

| Situation | What the wizard does |
| --- | --- |
| A target or bench must be identified and placed in a safe state | Names the tool, equipment and interlocks before any operation |
| A manual command, UI action or physical step has an expected observation | Gates the action, captures the result and gives a recovery path |
| A one-off migration needs switches flipped in a specific order | Sequences the irreversible steps behind confirmation gates |
| A project has to move from state A to state B once | Walks the transition and reports what it could not do |
| You are about to write those steps into a README | Writes an executable version instead, which can't rot as quietly |
| A target must be connected, placed in a safe state, flashed, observed, or recovered | Names the exact equipment, gates the dangerous action, captures the post-condition, and provides stop/recovery steps |
| A simulator, emulator, or HIL bench requires manual setup | Sequences tool, configuration, exclusive-use, and environment checks before execution |
| An approved credential or service setup needs human access | Optionally opens the scoped UI and stores values only in approved destinations |

Don't reach for it to *decide* what to build; for that, [grill-with-docs](https://aihero.dev/skills-grill-with-docs) and [to-spec](https://aihero.dev/skills-to-spec) are the tools.

## Prerequisites

Confirm the writable repository/path before generating one. For lab, target, or HIL work, identify the exact tool and version, equipment, connection and power state, permissions, exclusive-use rules, safe-state checks, expected observations, stop conditions, and recovery/rollback. For migrations, identify current state, target state, backups, irreversible actions, and post-conditions. The generated script runs on bash. Browser, `.env`, GitHub and CI operations are optional and require a scoped need, configured destination and matching authorization.

## Stages

A **stage** is one focused task on one screen. The script clears the terminal between stages, so a stage that overflows the screen loses the part that scrolled away. You author stages in dependency order and set `TOTAL_STAGES`, which drives the progress display.

Scoping happens before a line is written. The [skill](https://www.aihero.dev/ai-coding-dictionary/skill) reads relevant procedures, tool configuration, lab, target, safe-state and recovery sources rather than asking cold. For credential or CI setup, it also inspects `.env`, `.env.example`, `.env.*`, the README, `docker-compose*`, framework configuration, and every `secrets.*` or `vars.*` reference under `.github/workflows/`. It then shows the ordered stage list and every captured value or observation for confirmation. Each stage defines prerequisites, action, expected observation, destination, verification, failure handling, and recovery. Where the current tool, UI, command, or hardware state is unknown, it asks or consults authoritative documentation rather than inventing a procedure.

For each captured value, scoping settles where it lands:

| Destination | When |
| --- | --- |
| Terminal summary or approved evidence record | A tool/target/bench identity, observation, measurement, post-condition, or recovery result must be retained without exposing secrets |
| Nowhere | The stage is a pure confirmed action and no value should persist |
| Approved configuration file | The procedure explicitly requires a non-secret value there |
| Approved secret store | The procedure explicitly requires a credential there and the write is authorized |
| `.env`, GitHub secret or GitHub variable | Only when that exact destination is configured and approved; never by default |

## The template already solves the UX

The [template](https://github.com/mattpocock/skills/blob/main/skills/engineering/wizard/template.sh) ships the core experience: progress, focused instructions, confirmation gates and a closing summary. Its Stripe stage is only a replace-me example and is removed from every generated wizard. For a target or bench procedure, the authored stages must identify the equipment and safe state, gate the approved action, and verify the observation. They must stop on a failed post-condition and provide documented recovery. Recovery follows the shared default or your explicit delegation within its authorized scope; the procedure's required action confirmations remain. These checks and gates must be authored for the specific procedure. The fixed library retains optional URL, `.env`, and GitHub helpers; use them only for explicitly approved destinations. The `.env` helper needs an approved `ENV_FILE`, and the GitHub helpers are unsuitable when the current repository is ambiguous—use an explicit `--repo` command instead.

The agent that writes a wizard never runs it end to end. It verifies statically with `bash -n`, `shellcheck` where available, and a trace that every value and observation lands where scoped, every secret stays hidden, every dangerous step has a just-in-time confirmation and recovery path, and every `set_secret` or `set_var` name exactly matches the corresponding CI reference found during scoping. It states which checks ran, their results and limitations. The first human run remains separate evidence.

## Ephemeral by default

| What you have | What to do with the script |
| --- | --- |
| A one-off migration, a personal setup, a transition you'll never repeat | Put it in a scratch or `scripts/` path and delete it when the job is done |
| A controlled path the next person will also need | Select a repeatable repository procedure during scoping; the Skill commits it and links it from the README, but never pushes |

## Common questions

**Do my API keys end up in the model's context?**

Not when the credential is entered at runtime. The agent writes the script but does not run it; `ask_secret` accepts hidden terminal input and an optional persistence helper writes only to the destination approved during scoping. The wizard does not assume `.env` or GitHub. If you paste a key into chat while scoping, however, it enters the model [context](https://www.aihero.dev/ai-coding-dictionary/context) like any other pasted text.

**Can I go back and fix a value I mistyped?**

Not mid-run. There is no back button—the stages run forward. A wrong answer means stopping the run. Recovery or retry follows the shared default or your explicit delegation within its authorized scope. Equipment procedures retain their documented safe-state instructions, recovery steps, and required action confirmations. Values deliberately persisted through the optional configuration helpers can be offered as defaults; target, bench and observation values are otherwise entered again. This came up in the launch week and hasn't been closed since: "loved it! One thing though — is there a way to go back and correct what you've entered?"

There's a related open bug. Arrow keys in an `ask` prompt insert `^[[D` / `^[[C` instead of moving the cursor, because the prompt uses `read -r` rather than Readline ([issue #741](https://github.com/mattpocock/skills/issues/741)). Backspace works; arrow keys don't. Delete back to the mistake rather than moving the cursor into it.

**What does the default target or bench procedure cover?**

Authored stages must start with the approved tool and version, exact target and bench identity, connection/power/interlock checks, and the documented safe state. They must gate the manual command, UI action or physical step, name the expected observation, and record what actually happened. A failed post-condition must stop the procedure. Its recovery handling follows the shared default or your explicit delegation within its authorized scope, while keeping the procedure's required action confirmations. The agent does not invent a flashing command, safe state, expected signal or recovery procedure when the authoritative instructions are missing.

**Where does it sit in the workflow — after grilling and the spec?**

Nowhere in particular. It's a standalone, not a chain step. The common guess is `/grill-with-docs → /to-spec → /wizard`, and that sequence is fine, but the trigger is a manual procedure showing up, which can happen at any point: before you start, mid-build, or long after ship. It also works as a discovery tool—scoping exposes a missing tool version, bench reservation, safe-state instruction, expected observation or recovery step before anyone operates the equipment.

**Does it work outside Claude Code?**

The artifact does, unconditionally: it's a plain bash script and it doesn't care what [harness](https://www.aihero.dev/ai-coding-dictionary/harness) generated it. The skill itself is model-invoked, so it's listed everywhere — type `/wizard` in Claude Code or `$wizard` in Codex, or just describe the setup you're stuck on. Being model-invoked also keeps it clear of [#693](https://github.com/mattpocock/skills/issues/693), where Claude's desktop and web surfaces drop *user-invoked* skills from the [model](https://www.aihero.dev/ai-coding-dictionary/model)'s listing and report them as not installed.

**Didn't this used to be user-invoked?**

It did. It's now model-invoked, so the agent reaches for it unprompted when it hits a step you have to take. Nothing you could do before stopped working — model-invocation *adds* the agent's reach, it never removes yours, so `/wizard` behaves exactly as it did. What changed is the failure mode it retires: the agent hitting a credentials wall mid-build and dumping six numbered steps into the chat for you to follow by hand.

**It used to be in `in-progress/` — where is it now?**

`engineering/`, as of v1.2. It graduated out of the beta bucket and now ships in the plugin, so it arrives with the rest of the promoted set rather than needing an individual install. Its behaviour didn't change on graduation.

## It's working if

- You're shown an ordered list of stages, and the values each one produces, and asked to confirm — before any script exists.
- Tool, target, bench, safe state, expected observation and recovery are explicit whenever they apply.
- Optional URLs and credentials appear only when the scoped procedure needs them; secrets are typed blind and approved destinations are named.
- Each stage fits one screen. Nothing you still need has scrolled away.
- Ctrl-C has a safe stop path; recovery or a retry follows the shared default or your explicit delegation, keeps the procedure's required action confirmations, and starts from documented prerequisites rather than assuming the previous attempt completed.
- The final screen lists what it wrote, and separately lists what it couldn't do and you have to finish by hand.
- Every target, HIL, shared-environment, security-sensitive, or irreversible action has an immediate confirmation, expected post-condition, stop condition, and recovery.
- The script, repository path, captured observations, and ephemeral-or-repeatable choice are explicit; a repeatable wizard is committed and linked from the README without being pushed.
- Static inspection and the first human execution are reported as separate evidence.

## Where it fits

`wizard` is a reach-for-it-anytime standalone at the boundary where automation needs human access, judgment, or physical control. Its nearest neighbour is [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) for repository setup, and it pairs with [implement](https://aihero.dev/skills-implement) when a change needs credentials, a manual cutover, or target/HIL execution. It makes the authorized human procedure explicit and recoverable; when the user selects a repeatable repository procedure, it also commits and links the wizard, while deployment, release and target actions remain separate. [ask-matt](https://aihero.dev/skills-ask-matt) routes you when the fit is unclear.
