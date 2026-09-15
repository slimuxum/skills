---
name: loop-me
description: Grill me about specs for the workflows I want to build, within this workspace.
disable-model-invocation: true
argument-hint: "A workflow to design, or nothing to go find one"
---

Run a stateful `/grilling` session whose only output is **workflow** specs. Use the grilling discipline — relentless, a round of questions at a time, a recommended answer attached to each — aimed at the vocabulary and goal below. Create, edit, and delete specs as the grilling resolves things.

## The loop lens

A **loop** is a recurring pattern in the user's life: their career, their week, their morning, a single repeated activity. Picturing a life as loops within loops reveals how predictable its activities really are — which is what makes them worth **delegating**. Use the lens to find loops worth specifying, and propose ones the user hasn't noticed.

A **workflow** is the spec of one loop, made real. You run a workflow on a loop — the loop is its running instantiation. Workflows live in `workflows/*.md` and are the source of truth.

## Vocabulary

A shared language, reached for only when a workflow calls for it — never a checklist. **Mandate nothing structural**: a workflow needs no AI, no planned checkpoint, and no schedule unless the grilling shows it does. Apply the risk and goal confirmation rule, including any explicit autonomous authorization, throughout the work and include it in generated workflow specs. Under its default, decision-affecting uncertainty, high risk, or goal deviation requires an immediate pause. A workflow's autonomous schedule or **Push right** advice does not itself grant permission to skip that pause.

- **Trigger** — what fires each run: an **event** (a new email, a new issue) or a **schedule** (every morning). Event-triggering is usually the more efficient.
- **Checkpoint** — a human-in-the-loop point where the user is asked to verify or decide. Some workflows have none and run autonomously; some use no AI at all.
- **Push right** — defer the checkpoint as far as it will go. Do maximal work before involving the human, so they are asked once, late, with everything prepared.
- **Brief** — what a checkpoint presents: a tight, decision-ready summary — what was produced, why, and a link down to the asset itself — never the raw output. The user reads a brief, not a draft. Speed of review is imperative.

## Definition of done

A workflow spec is done when an implementer agent could build it without asking a single question. Grill until then; nothing is done while a question remains.

## The workspace

- `workflows/*.md` — one spec per workflow.
- `NOTES.md` — raw notes on the user's world: the tools they use, the channels they process, and their own terminology for both. When it is empty or thin, interview them about their world before specifying anything. Sharpen fuzzy terms into canonical ones as they surface, and record them here.

## Risk and goal confirmation

**Default.** If uncertainty affects an execution decision, a high-risk item appears, or work departs from the user's agreed goal, scope, constraints, or expected outcome, stop execution and pause related delegated work. Explain the problem, evidence, and impact; recommend a response with reasons and wait for the user's explicit confirmation. Before confirmation, do not attempt fixes, retries, workarounds, alternatives, or plan changes on your own. Resume only the confirmed response.

**Explicit autonomous authorization.** If the user explicitly says not to ask and to decide independently, or gives equivalent authorization, follow the skill's original workflow for decisions, iteration, and fallbacks within the task and scope they authorize. This waives only the extra questions and confirmations introduced by this rule and its applications in supporting instructions. It does not expand the agreed goal or scope, remove existing permission limits, or waive confirmations required by the original workflow. Silence, no reply, or an ordinary "continue" is not autonomous authorization. Restore the default when authorization is withdrawn or does not cover the decision.

**Delegation and handoff.** Include this full rule, agreed goal and scope, the user's autonomous authorization and its scope when present, unresolved issues, recommendations, and confirmation status in worker briefs and handoffs. Workers follow the same applicable mode; under the default, they stop and report to the parent for the user's decision. On withdrawal or narrowing of authorization, notify active workers and pause affected work until they are applying the current mode and scope. A handoff or automatic-continuation instruction cannot grant autonomous authorization or bypass a confirmation that is still required.
