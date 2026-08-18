## What it does

`improve-codebase-architecture` surveys a single- or multi-repository workspace for **deepening opportunities** and cross-repository friction, writes a visual HTML report with runtime and verification constraints, and then [grills](https://www.aihero.dev/ai-coding-dictionary/grilling) you through whichever one you pick.

It never changes code. The run produces one HTML file in your OS temp directory and a conversation. During the grilling loop it keeps the canonical glossary current as terms settle and offers to record durable rejected decisions as ADRs; the refactor itself happens later through the normal build flow.

Two filters keep the report from becoming generic cleanup advice. Every candidate has to pass the **deletion test** — would removing this module concentrate complexity behind a smaller interface, or just spread it across callers? Only the "concentrates" cases earn a card. And unless you point it at a specific area, it reads recent commit history first and biases the scan toward paths that are actively changing, on the grounds that a deepening in code nobody touches is a refactor you will never cash in.

## When to reach for it

You invoke this by typing `/improve-codebase-architecture` — the [agent](https://www.aihero.dev/ai-coding-dictionary/agent) will not reach for it on its own.

It sits outside the build loop — it is not a step in the main loop but something you run periodically to queue up more work to improve the codebase. The four situations it gets used in:

| Situation | How it is used |
| --- | --- |
| Routine upkeep | Run it every few days, or whenever a spare moment appears, to stop structure rotting between features. |
| Before a big build | Point it at the [spec](https://www.aihero.dev/ai-coding-dictionary/spec): "how can we make this change easy?" This is the most effective prompt for it. |
| Brownfield audit | Run it on a large, unstructured or [vibe-coded](https://www.aihero.dev/ai-coding-dictionary/vibe-coding) repo to find out what shape it is actually in. |
| Legacy test work | Use it to find the missing seams first, before writing tests against untestable code. |

Where it is confusable with siblings:

- For designing one module you have already chosen, use [codebase-design](https://aihero.dev/skills-codebase-design) — that is the bench, this is the survey that finds what to put on it.
- For a whole effort too big to hold in one session, use [wayfinder](https://aihero.dev/skills-wayfinder).
- For "this specific thing is broken," use [diagnosing-bugs](https://aihero.dev/skills-diagnosing-bugs). It hands back here when the real finding is that there is no good seam to lock the bug down.

## Prerequisites

It requires an isolated exploration sub-agent. The skill checks before scanning; without one it reports `BLOCKED` and does not let the parent agent perform a sequential substitute. It also needs the in-scope repository roots and reads applicable context/ADR material from each logical context.

The report goes to `<tmpdir>/architecture-review-<timestamp>.html`, outside every repository. During the grilling loop, resolved domain terms are maintained inline in the canonical glossary, and rejected candidates can be offered as ADRs.

## Depth, and the report that hunts for it

The skill turns on one idea: **depth**. A deep module puts a lot of behaviour behind a small, stable interface. A shallow one leaks its implementation through an interface nearly as wide as the code beneath it. The report is a hunt for shallowness — pure functions extracted only for testability while the real bugs live in how they are called (no **locality**), modules leaking across their **seams**, a concept you cannot understand without opening five files — and a proposal for the deepening that fixes it.

Each candidate names repositories, logical contexts, source revisions, cross-repository interfaces, files, friction, a plain-English solution, runtime constraints, verification impact, limitations, a before/after visual, and a strength badge.

| Badge | What it means for you |
| --- | --- |
| `Strong` | The deletion test passes clearly and the friction is real. Take these seriously. |
| `Worth exploring` | Plausible deepening, but the payoff depends on where the code is going next. |
| `Speculative` | Surfaced for completeness. Most of these are safe to ignore. |

The report ends with a **Top recommendation** — the one it would tackle first — and then the skill stops and asks which candidate you want to explore. Nothing has been decided at that point, and no code has moved.

## What happens after you pick one

Picking a candidate starts a [grilling](https://aihero.dev/skills-grilling) session over it: constraints, what sits behind the seam, which tests survive, what the deepened interface should look like. The output of that session is a decision, not a diff. From there the normal flow applies — take the decision into [to-spec](https://aihero.dev/skills-to-spec), then [to-tickets](https://aihero.dev/skills-to-tickets), then [implement](https://aihero.dev/skills-implement).

## Common questions

**It grilled me for an hour about one idea instead of showing me options. Can I turn that off?**

Yes — say so when you invoke it ("don't grill me, just show the report"). This is the loudest complaint the skill has. One user put it bluntly: they liked it as "a convenient way to get a thorough analysis of improvements," and after the grilling loop was added found it "borderline unusable," reporting sessions where it proposed a single solution and then asked "10's or 100's of questions." The design intent is that the report comes first and the grill only starts on a candidate you chose, but weaker [models](https://www.aihero.dev/ai-coding-dictionary/model) skip straight to interviewing you about the first idea they had. Reports in that thread vary sharply by model, and it is an open issue — the skill does not yet have a documented no-grill mode.

**The report opened as unstyled raw HTML with no diagrams. What happened?**

It should not. The report is self-contained, with inline CSS and hand-built SVG, so it renders in offline and locked-down environments. An unstyled page or missing diagram means the generated report did not follow the format reference.

**It gave me twelve candidates. Do I work through them in the same session or start a new one?**

One candidate per session. Working through several in one conversation fills the [context window](https://www.aihero.dev/ai-coding-dictionary/context-window) with the report, the grilling, the domain-model edits and the code changes all at once. The report only lives in a temp file, so carry the candidate itself rather than the file: pick one, grill it, take the decision into `/to-spec`, and turn the rest into [tickets](https://www.aihero.dev/ai-coding-dictionary/ticket) you can pick up independently later. Put the chosen improvement into a spec rather than going straight to implementation. This is a recurring question with no documented workflow in the skill itself.

**How should I prompt it?**

With the next thing you are building in mind. Where a big build is coming up, point it at the spec and ask "how can we make this change easy?" An unprompted run scans for hot spots on its own, which is fine for routine upkeep, but naming a direction is what makes the report actionable.

**Does it work on a large legacy codebase?**

Partly. It is strong on big existing codebases lacking consistent structure, and it is the recommended upkeep mechanism after any one-time structural setup. The honest counterweight: users with genuinely out-of-control projects report it "helped a little but still doesn't seem to cut it," and one developer with an eight-year legacy codebase reported the model going in circles where the same skill produces a clean graph on a tidy repo. There is no dedicated `/refactor` skill for that case yet. If the codebase has no shared vocabulary at all, [grill-with-docs](https://aihero.dev/skills-grill-with-docs) to establish one first tends to make this skill's output much better.

**How is this different from `/codebase-design`?**

`/codebase-design` is a reference, not a session driver. It supplies the vocabulary — module, interface, depth, seam, adapter, leverage, locality — and this skill borrows it. Pointing a fresh agent at `/codebase-design` as the thing to "do" is a known failure: with no process of its own to follow, the agent invents one, re-explores code and runs for a very long time before asking you anything. Drive with this skill; consume that one.

**Will it ever tell me the codebase is fine?**

Rarely, and you should know that going in. The skill is built to output findings, so the framing pushes it toward producing candidates rather than concluding that nothing is wrong. The strength badges are the defence — a report where everything is `Speculative` is the skill telling you it found nothing, in the only way it knows how.

**Does it work in a harness without sub-agents?**

No. The scan must run in an isolated sub-agent. Capability names are harness-neutral, but the requirement is hard: without a worker the skill stops `BLOCKED` rather than silently producing a weaker parent-agent scan.

**How do I implement the chosen deepening in an embedded C/C++ codebase?**

Treat the candidate as a design input, not a folder recipe. Define the owned interface, invariants, error and timing behaviour, then enforce the seam with the project's public/private headers, component libraries, include and link visibility, generated interfaces, and dependency checks. If the seam crosses repositories or generated-code ownership, record compatibility and integration order before implementation. Take that approved design through the normal specification and implementation flow rather than editing it inside this survey skill.

## It's working if

- The candidates name your domain's concepts, not invented class names — "the Order intake module," not "the FooBarHandler."
- The candidates cluster in files you have edited recently, not in dormant corners of the repo.
- No code changed during the run. The HTML report exists only in the OS temp directory; any glossary or ADR update came from the grilling/domain-modeling step.
- The scan covers every repository in the selected logical context and names cross-repository interfaces.
- Every candidate states runtime constraints, required verification environments and uncovered limitations.
- It stops after the report and asks which candidate you want, rather than continuing on its own.
- Each card explains the payoff as locality or leverage, and says which tests get simpler — not just "this is cleaner."
- Rejecting a candidate for a durable reason gets you an offer to record an ADR, so the next run does not re-suggest it.

## Where it fits

`improve-codebase-architecture` is **periodic maintenance** — run it outside any chain to queue up work rather than to do it. Its neighbours are [codebase-design](https://aihero.dev/skills-codebase-design), which owns the depth-and-seam vocabulary every candidate is written in, [grilling](https://aihero.dev/skills-grilling), which walks the decision tree once you have chosen a candidate, and [domain-modeling](https://aihero.dev/skills-domain-modeling), which keeps the canonical context glossaries and ADRs current as the decision settles. What it produces is an idea, which re-enters the main build flow at [grill-with-docs](https://aihero.dev/skills-grill-with-docs) or [to-spec](https://aihero.dev/skills-to-spec). For which skill fits a situation, [ask-matt](https://aihero.dev/skills-ask-matt) is the router over the whole set.
