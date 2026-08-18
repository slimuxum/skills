## What it does

`triage` evaluates incoming issues and external changes across one or more repositories. It recommends a category and state role, checks the claim at the cheapest faithful environment, and proposes a durable brief, reporter question, or closure reason. The tracker is a communication surface, not the sole source of project, verification, release, or target state.

It is only for issues **you didn't create**. Raw bug reports, incoming feature requests, an external pull request that arrived unannounced — work that landed in the tracker from outside, in whatever shape the reporter left it. [Tickets](https://www.aihero.dev/ai-coding-dictionary/ticket) that [to-tickets](https://aihero.dev/skills-to-tickets) produced are already agent-ready by construction, and running `triage` over them is wasted work at best. The rule is flat: `/triage` is only for incoming issues, not for issues you created yourself.

The second thing that separates it from labelling by hand: it recommends and waits for your direction before applying the outcome. `ready-for-agent` means an AFK agent can take the next step; it does not by itself say that implementation or verification is complete.

## When to reach for it

You invoke this by typing `/triage` and then describing what you want in plain language — the [agent](https://www.aihero.dev/ai-coding-dictionary/agent) won't reach for it on its own. "Show me anything that needs my attention", "let's look at #42", "move #42 to ready-for-agent".

| What you have | Where to go |
| --- | --- |
| A tracker full of raw reports from other people | `/triage` |
| A rough idea of your own, nothing written down | [grill-with-docs](https://aihero.dev/skills-grill-with-docs) |
| A settled conversation to turn into a [spec](https://www.aihero.dev/ai-coding-dictionary/spec) | [to-spec](https://aihero.dev/skills-to-spec) |
| A spec to split into agent-ready tickets | [to-tickets](https://aihero.dev/skills-to-tickets) |
| A confirmed bug that needs a root cause, not a label | [diagnosing-bugs](https://aihero.dev/skills-diagnosing-bugs) |

## Prerequisites

`triage` needs the tracker mapping configured by [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills). The role names below are **canonical**; actual label strings may differ. Tracker configuration does not define the full code scope: the run still maps every implicated repository, logical context, source revision, interface owner, and target environment.

The tracker config also decides whether external pull requests count as a request surface, and who counts as external. That flag defaults to off and is no longer a setup question — flip it in `docs/agents/issue-tracker.md` if you want PRs in scope.

## The state machine

Every triaged item ends up carrying exactly one category role and one state role. Two categories: `bug` (something is broken) and `enhancement` (new feature or improvement). Five states:

| State | Means |
| --- | --- |
| `needs-triage` | You need to evaluate it. Where an unlabelled issue normally lands first. |
| `needs-info` | Waiting on the reporter. Returns to `needs-triage` when they reply. |
| `ready-for-agent` | Fully specified, with an agent brief attached. An [AFK](https://www.aihero.dev/ai-coding-dictionary/afk) agent can take it. |
| `ready-for-human` | The same brief, plus why this can't be delegated — judgment, external access, manual testing. |
| `wontfix` | Closed, with the reason recorded. |

That is the tracker vocabulary, and the "exactly one state role" invariant keeps queries simple. Verification evidence is separate: state what ran, its result and limitations, and keep unavailable or unrun checks explicit. Do not overload tracker labels to hide missing evidence.

`wontfix` splits three ways, and the difference matters because only one of them writes to the knowledge base:

| Why you're closing it | What happens |
| --- | --- |
| Already implemented | A comment pointing at where it already lives. Nothing is written to `.out-of-scope/` — it's a built feature, not a rejected one, and filing it there would poison the dedup checks. |
| Rejected bug | Polite explanation, then close. |
| Rejected enhancement | A file in `.out-of-scope/`, linked from the closing comment, then close. |

`.out-of-scope/` is one markdown file per rejected **concept**, not per issue, written as a short design document rather than a database row: what was rejected, why, and every issue that has asked for it. `triage` reads the whole directory before it evaluates anything, and matches by concept rather than keyword — "night theme" matches `dark-mode.md`. When it hits a match it surfaces the old decision and asks whether you still feel the same way, instead of re-litigating the request from scratch.

## Verify before you brief

Before any [grilling](https://www.aihero.dev/ai-coding-dictionary/grilling), `triage` checks the claim at the cheapest faithful environment: static, host, simulator, emulator, target, or HIL. For a PR it does not check out over unrelated work; for target or lab work it does not operate equipment or mutate shared state without authorization. It records the command or procedure, environment, repository revisions, code/interface path, limitations, and evidence status.

It runs two more checks against the codebase in the same pass — **redundancy** (is this already implemented, searched by domain concept rather than by the reporter's wording?) and **prior rejection** (does `.out-of-scope/` already say no?). Both are cheap, and both produce a `wontfix` when they hit.

All of it exists to make one artifact good: the **agent brief**, the structured comment posted when an issue moves to `ready-for-agent`. Once it's posted, the brief is the contract and the original report is only context. Briefs are written to be **durable** rather than precise, because an issue can sit in `ready-for-agent` for weeks while the code moves underneath it. So they name types, signatures and behavioural contracts, and never file paths or line numbers. A confirmed reproduction makes a far stronger brief than a guess does.

## A PR is an issue with attached code

Where the tracker treats external pull requests as a request surface, they run through the same machine — same categories, same states, same transitions. The states read against the change: `ready-for-agent` means a brief describes the next step for an agent; `ready-for-human` means the change is ready for a human to merge. A brief on a PR describes what remains in the existing logical change and records any verification or release limitation separately.

Discovery surfaces only *external* PRs, because a collaborator's in-flight branch is not triage work. That filter is discovery-only — name a PR explicitly and it gets triaged whoever wrote it. The GitHub tracker template uses the REST `author_association` field through an explicit `gh api repos/<owner>/<repo>/...` path because `gh pr list --json` does not expose that field.

## Common questions

**I ran `/to-spec` and `/to-tickets`, and now those tickets are sitting there untriaged. Do I run `/triage` over them?**
No. Tickets produced by `to-tickets` are already structured for an agent and should not pass through inbound triage. Once the breakdown is approved, the publisher applies the configured `ready-for-agent` label unless instructed otherwise. `triage` remains the on-ramp for raw work that arrives from outside.

**Is `triage` still relevant now that there's a `to-spec` → `to-tickets` → `implement` flow?**
Only if you have inbound work. `triage` predates that spine and does a different job: it is the lane for reports other people filed. If everything in your tracker came out of your own planning, you will rarely open it. If you maintain anything public, or your team files bugs at you, it is the front door. The main use is open-source repos taking issues from external contributors.

**The agent tried to apply `ready-for-agent` and `gh` said the label doesn't exist.**
Known open bug ([#616](https://github.com/mattpocock/skills/issues/616)). `setup-matt-pocock-skills` writes the label vocabulary into `docs/agents/triage-labels.md`, but does not create the labels in your tracker. Create the five state labels and two category labels yourself, once, with `gh label create --repo <owner/repo>` or the tracker's UI, and it stops. There is a community fix branch linked from the issue that hasn't been merged.

**Five tracker states aren't enough — what about blocked, deferred, or implemented?**
Keep workflow roles separate from evidence and release state. A missing or postponed check does not require inventing another tracker role; record the limitation in the brief. Likewise, `ready-for-agent` does not mean verified, released, or completed. If the repository defines extra roles, treat them as local policy rather than inventing transitions.

**How is this different from `/diagnosing-bugs`?**
Verification here is bounded: enough to classify the claim, scope the affected repositories/interfaces, and attach honest evidence. Root-cause work belongs to [diagnosing-bugs](https://aihero.dev/skills-diagnosing-bugs). A target/HIL check may still be necessary for triage when the symptom cannot exist faithfully elsewhere, but the run stops at classification rather than a fix.

**Can I point it at my whole backlog and let it run?**
You can ask, but watch what it reads. The "show what needs attention" pass is a cheap listing meant for *selection* — you pick one, and then it gathers full [context](https://www.aihero.dev/ai-coding-dictionary/context) on the one you picked. Run it across twenty issues at once and an agent can quietly fall back to that cheap listing as its evidence base, which returns issue bodies but not comments. A user hit exactly this: three issues already carried a comment saying "already fixed, recommend closing", and all three got fresh agent briefs instead. If you want a bulk pass, say explicitly that comments must be read per issue.

**Does it work with Linear, or anything other than GitHub Issues?**
Yes — the tracker is config, not a hard-coded assumption, and people run it against Linear (via the `linear` CLI), GitLab, and plain markdown files under `.scratch/`. A common split is Linear for issues and planning, GitHub for code and PRs: skills that say "issue tracker" map to Linear, skills that say "PR" map to GitHub. On the local-markdown tracker there is an open template bug where the generated file can carry the acceptance criteria twice, once at the top level and once inside the agent brief ([#200](https://github.com/mattpocock/skills/issues/200)).

## It's working if

- Every item it touches ends with exactly one category role and one state role — never zero, never two states in conflict.
- It gives you a recommendation with reasoning and stops, rather than relabelling and moving on.
- The affected repositories, logical contexts, revisions, interfaces, and target environments are explicit rather than inferred from the tracker repository.
- Verification uses the cheapest faithful environment and states what ran, its result, environment, and limitations; unavailable target/HIL evidence is not implied green.
- The briefs it writes name types and behaviours, and contain no file paths and no line numbers.
- A request that was rejected six months ago comes back, and it says so and quotes the old reason instead of triaging it fresh.
- Every comment it posts opens with `> *This was generated by AI during triage.*`
- It recommends the category and state with reasoning, waits for direction, and then applies the selected outcome.

## Where it fits

`triage` is an **on-ramp**, not a step in the main chain. The main flow runs from an idea you had — grill, spec, tickets, implement, review — and `triage` is the parallel lane for inbound work. It can produce an actionable brief for [implement](https://aihero.dev/skills-implement). When a request needs sharpening, `triage` invokes [grilling](https://aihero.dev/skills-grilling) and [domain-modeling](https://aihero.dev/skills-domain-modeling) together so resolved terms and decisions land in the active Logical Context's canonical glossary and ADRs as they are made. [ask-matt](https://aihero.dev/skills-ask-matt) routes between the lanes.
