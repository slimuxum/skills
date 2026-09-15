## What it does

`wait-what` is what you type when a message didn't land. The [agent](https://www.aihero.dev/ai-coding-dictionary/agent) then re-pitches what it just said. It adds the context you were missing, writes in ASD-STE100 Simplified Technical English, and uses the vocabulary from the active Logical Context sources.

The core re-pitch instruction stays short and precise. It gives the [model](https://www.aihero.dev/ai-coding-dictionary/model) a clear action: add the missing context and use a register you can follow.

- **Default:** Uncertainty affecting execution decisions, high risk, or departure from your goal, scope, constraints, or expected outcome pauses work and related workers. You receive evidence, impact, recommendations, and reasons; execution and adjustments await your explicit confirmation.
- **Explicit delegation:** “Do not ask; decide yourself” or equivalent allows the original workflow's judgment, iteration, and fallbacks within your authorized task and scope without this additional pause. Existing permission limits and required confirmations remain. Silence, no reply, or ordinary “continue” grants no exception. Workers and handoffs carry your authorization wording, scope, unresolved issues, and confirmation status. The default returns when authorization is withdrawn or does not cover the work. Withdrawal or narrowing reaches active workers, with affected work paused until their mode and scope are updated.

## When to reach for it

You invoke it by typing `/wait-what`. The agent will not reach for it on its own, and it shouldn't. Only you know when you stopped following.

Use it the second you notice you're skimming. The agent has drifted into jargon it invented, stacked five acronyms, or explained a decision whose premise you never saw. It fixes the conversation you're already in. To stop the jargon arriving at all, use [grill-with-docs](https://aihero.dev/skills-grill-with-docs), which builds the shared language upfront.

## Common questions

**Why is the skill called `wait-what` instead of `tldr`?**

The leading word is **wait**. "Be concise" is an instruction about the agent's output, and the model obeys it by clipping words and losing you further. **Wait** is about *your* state. It says comprehension failed here. An agent that hears "be brief" writes telegrams. An agent that hears "wait, you lost me" backs up and explains.

That difference is the whole skill. Every popular fix for verbosity names the *output*: `/tldr`, `/no-fluff`, `/talk-normal`. The model over-corrects into a caveman register that is shorter and no clearer. Naming the *listener* asks for both halves at once: fewer words **and** the context you were missing.

The skill says re-pitch **that**, not "that last message". What lost you is usually bigger than one paragraph, so the agent decides how far back to go.

**Where does its project vocabulary come from?**

The body reuses the leading words already in your agent instructions and active context artifacts. ASD-STE100 Simplified Technical English sets the register. The ubiquitous language supplies the nouns. A Logical Context may span repositories or select subtrees, so the skill does not infer the vocabulary source from the current directory or one root file.

If you have no context artifact, the skill still works from the vocabulary established in the conversation. You lose only the persistent domain-vocabulary half.

## It's working if

- The re-pitch is **shorter and clearer**, not shorter and blunter.
- It adds the premise you were missing, instead of only deleting words.
- Project nouns replace invented ones. The terms from the active Logical Context come back, even when it spans repositories.
- You can use it twice in a row, and it does not degrade into terseness.

## Where it fits

You can use `wait-what` at any point, in any conversation, inside any other skill. It repairs one message after the fact. The real cure is a shared language agreed upfront, and that is [grill-with-docs](https://aihero.dev/skills-grill-with-docs): a [grilling](https://www.aihero.dev/ai-coding-dictionary/grilling) session that runs [domain-modeling](https://aihero.dev/skills-domain-modeling) as it goes, so the words you both use land in the active context artifacts. If you're unsure which skill fits the moment, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
