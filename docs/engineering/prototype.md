## What it does

`prototype` writes **throwaway code or a bounded procedure that answers one question**: logic, state, an interface, timing, integration, target behaviour, or UI/HMI. The question and required fidelity decide whether the artifact is a host executable, harness, model, simulator/emulator scenario, target/HIL procedure, or UI variation in the product's real rendering runtime.

Throwaway is a constraint on scope, not an exemption from safety. The prototype carries only the assertions, instrumentation, error handling and recovery needed to answer safely. It never flashes a target or mutates shared data implicitly, and it captures the finished prototype on a throwaway branch rather than main.

- **Default:** Uncertainty affecting execution decisions, high risk, or departure from your goal, scope, constraints, or expected outcome pauses work and related workers. You receive evidence, impact, recommendations, and reasons; execution and adjustments await your explicit confirmation.
- **Explicit delegation:** “Do not ask; decide yourself” or equivalent allows the original workflow's judgment, iteration, and fallbacks within your authorized task and scope without this additional pause. Existing permission limits and required confirmations remain. Silence, no reply, or ordinary “continue” grants no exception. Workers and handoffs carry your authorization wording, scope, unresolved issues, and confirmation status. The default returns when authorization is withdrawn or does not cover the work. Withdrawal or narrowing reaches active workers, with affected work paused until their mode and scope are updated.

## When to reach for it

Type `/prototype`, or the [agent](https://www.aihero.dev/ai-coding-dictionary/agent) reaches for it automatically when a task fits.

Reach for it when running a small experiment will settle a question that discussion cannot. Prefer static or host execution; escalate to simulator, emulator, target or HIL only for behaviour cheaper environments cannot preserve. If something already built is broken, use [diagnosing-bugs](https://aihero.dev/skills-diagnosing-bugs).

You will also arrive here without choosing to. [wayfinder](https://aihero.dev/skills-wayfinder) files `prototype` decision [tickets](https://www.aihero.dev/ai-coding-dictionary/ticket) on its map, and working one is this skill.

## Two branches

The question picks the branch, and the branches produce very different artifacts:

- **Logic / behaviour / feasibility** — choose the smallest faithful static, host, simulator, emulator, target or HIL artifact. State what it preserves and what it cannot prove. HTML is appropriate only when a human-driven state demo is the actual experiment.
- **"What should this look like?"** — first identify the UI runtime, display geometry, inputs, existing host, and faithful renderer. Then build several **radically different** variants in Qt/QML, LVGL, a native or instrument HMI, Web, or the actual project environment. Compare them with a development-only selector appropriate to that runtime; a Web route and floating switcher are one option, not the universal shape.

Both start with explicit writable scope, environment, inputs, expected observation, stop conditions and cleanup. Target/HIL work also requires equipment identity, authorization and recovery.

## Capture the answer

A finished prototype reports the question, verdict, command or procedure, environment, inputs, output, source revisions, and limitations, including what ran and what did not. It folds the validated decision into the real code under normal production verification, while keeping the throwaway harness, losing variants, and development selectors on the prototype branch. It then captures the prototype and answer using the skill's throwaway-branch workflow.

## Common questions

**Does it always keep the prototype on a branch?**
Yes. The prototype is a primary source for the decision, so the Skill commits it to a throwaway branch outside main and leaves a context pointer on the implementation issue. It never pushes the branch.

**Why didn't it build an HTML demo?**
HTML is no longer the default for non-UI questions. A C/C++ host harness, simulator scenario, trace replay or controlled target/HIL procedure is usually a more faithful answer to an embedded engineering question.

**How do I compare UI variants if there is no browser or route?**
Use the runtime's own development seam. Qt/QML can use a launch property, `Loader`, `StackLayout` or debug control; LVGL can use a debug menu, physical-input gesture, build option or simulator target; a native or instrument HMI can use an external harness, replay configuration, diagnostic-only control or separate development images. All variants use the same geometry, inputs and representative states, and the real renderer is exercised before drawing a conclusion.

**Can it flash a target or operate a HIL bench to judge the UI?**
Only with explicit authorization for the exact target or bench, operation, safe state, stop conditions and recovery. Host or simulator rendering is preferred when it answers the visual question. Target or HIL is reserved for load-bearing properties such as real display integration, physical controls, luminance, timing, memory or startup.

**An agent told me to `/prototype` when I should have been implementing.**
Known, and it is a naming problem. `prototype` is a generic, appealing word that reads to a flow-unaware agent as "the obvious next step" once tickets exist, so it gets recommended by name even where the design was fully settled in conversation. If you already know what to build, the next step is `/implement`, per ticket. Reach for a prototype only when a specific design question is genuinely unresolved and talking won't resolve it.

**Should I prototype the whole application before building any of its production features — say, to demo it to prospects?**
That is a different artifact wearing this skill's name. A prototype here is scoped to one question, and "what is the whole app?" isn't one. A full-app prototype has no natural stopping point, so it becomes the production app by momentum: the cleanup pass never happens, and code written under prototype rules — no tests, no error handling — ends up in front of users. If you need a sales demo, build it deliberately as a demo and be explicit that none of it is production. If you need to settle a design question, cut it down to that question.

**How do I run it in its own session?**
A prototype lives in its own directory and generates a lot of [context](https://www.aihero.dev/ai-coding-dictionary/context) you don't want in the thread that asked the question, so run it somewhere else and bring back only the answer. [handoff](https://aihero.dev/skills-handoff) is the bridge in both directions.

**Isn't this the fastest possible way to burn tokens?**
It can be, if you prototype questions you could have answered by talking, or let one prototype sprawl across a whole feature. The comparison that matters isn't tokens against zero; it's [tokens](https://www.aihero.dev/ai-coding-dictionary/token) against building the wrong state model and finding out after it has production callers. Keep the question narrow and the run short, and the spend stays proportionate.

## It's working if

- You can say in one sentence what question the prototype exists to answer — and it's written at the top of the demo, not just in your head.
- The selected environment is the cheapest one that preserves the property being evaluated, and its limitations are explicit.
- Someone says "wait, that shouldn't be possible" or "huh, I assumed X". That's a bug in the *idea*, which is the entire point.
- The UI/HMI runtime, display and input constraints are explicit; variants disagree about layout and information hierarchy, and are rendered with a faithful renderer under comparable states.
- It is answered in one sitting. If you're still building it a day later, the question was too big; split it.
- No repository, target, lab or tracker state changed beyond the explicitly approved scope.

## Where it fits

`prototype` is a **reach-for-it-anytime standalone** — you drop into it to settle one design question, then drop back out — and it is also machinery another skill runs on.

Its largest consumer is [wayfinder](https://aihero.dev/skills-wayfinder). A wayfinder map is made of **decision tickets**, and `prototype` is one of the four types a ticket can be: the one used when the blocking question is "how should this look" or "how should it behave", which no amount of discussion resolves. Wayfinder raises the fidelity of a foggy discussion by making something concrete to react to, and this skill is how that concrete thing gets built. A prototype ticket is resolved by the answer, and the prototype is linked from the map as an asset.

The other neighbours are upstream and downstream of that. [grill-me](https://aihero.dev/skills-grill-me) and [grill-with-docs](https://aihero.dev/skills-grill-with-docs) answer grillable questions; the ungrillable ones come here instead, and the one-line answer goes back into the interview. Downstream, a validated state model or UI direction becomes settled input for [to-spec](https://aihero.dev/skills-to-spec), which can inline the decision-rich snippet the prototype produced rather than describing it in prose. For anything else, [ask-matt](https://aihero.dev/skills-ask-matt) routes you over the whole set.
