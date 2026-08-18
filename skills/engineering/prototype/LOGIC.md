# Logic or Behaviour Prototype

Build the smallest disposable artifact that can answer a question about logic, state, interfaces, timing, resources, integration, or target behaviour. HTML is one possible presentation, not the default verification environment.

## When this is the right shape

- "I'm not sure if this state machine handles the edge case where X then Y."
- "Does this data model actually let me represent the case where..."
- "I want to feel out what the API should look like before writing it."
- "Can this algorithm meet the memory or timing bound?"
- "Does the driver/service interface survive these failures?"
- Anything where running a bounded experiment will settle the question.

If the question is "what should this look like" — wrong branch. Use [UI.md](UI.md).

## Process

### 1. State the question

Before writing code, write down what state model and what question you're prototyping. One paragraph, at the top of the demo (in a visible intro, not just a comment). A logic prototype that answers the wrong question is pure waste — make the question explicit so it can be checked later, whether the user is watching now or returning to it AFK.

### 2. Pick the minimum faithful environment

Choose among static analysis, host, simulator, emulator, target, or HIL. State what the chosen environment preserves and what it cannot prove. Prefer host or simulation for rapid iteration; require target or HIL only for hardware, timing, concurrency, resource, electrical, or integration behaviour that cheaper environments cannot represent.

For target or HIL work, identify the exact target, permitted operations, recovery procedure, lab ownership, and stop conditions before running anything. If authorization or safe recovery is missing, report `BLOCKED`.

### 3. Isolate the question behind a portable interface

Keep the experiment driver separate from the logic or interface being evaluated. Reuse production code only when doing so does not mutate it or smuggle prototype shortcuts into it.

The right shape depends on the question:

- **A pure reducer** — `(state, action) => state`. Good when actions are discrete events and state is a single value.
- **A state machine** — explicit states and transitions. Good when "which actions are even legal right now" is part of the question.
- **A small set of pure functions** over a plain data type. Good when there's no implicit current state — just transformations.
- **A class or module with a clear method surface** when the logic genuinely owns ongoing internal state.

Pick whichever shape fits the question, not whichever tool is familiar. Make dependencies such as clocks, allocators, buses, files, services, and hardware explicit enough to substitute or observe.

### 4. Build the experiment

Use one command or bounded procedure. Examples include a small C/C++ host executable, a unit or property harness, a trace replay, a simulator/emulator scenario, a target diagnostic image, an HIL procedure, or a self-contained HTML page for human-driven state exploration.

Use domain language. Make inputs, observable state, expected result, environment, and stop condition explicit.

Cover the smallest set of scenarios that distinguishes the competing answers: nominal behaviour, a critical edge or failure, and any relevant timing/resource boundary. Do not expand into product completeness.

### 5. Run or hand over

Run only within the authorized environment. For manual, target, or HIL execution, hand over the exact procedure and capture the observation without exposing secrets or uncontrolled machine state.

### 6. Capture the answer

Report the question, verdict, command or procedure, environment, inputs, output, source revisions, and limitations. Fold the validated reducer, state machine, function set, interface decision, or other answer into the real production module and run its normal verification; do not carry the throwaway harness into production. Commit the prototype and its answer to a throwaway branch outside main, leave a context pointer to that branch on the implementation issue, and never push.

## Anti-patterns

- **Don't confuse evidence with production hardening.** Add the smallest check needed to answer the question; do not build a permanent suite around throwaway code.
- **Don't touch real data, devices, or calibration by default.** Use controlled substitutes unless the question requires the real environment and the user authorizes it.
- **Don't generalise.** No "what if we wanted to support X later." The prototype answers one question.
- **Don't blur the subject and the harness together.** Keep experiment-only drivers, stubs, instrumentation, and build options visibly separate.
- **Don't claim target properties from host-only evidence.** State what remains unverified.
- **Don't ship the prototype directly.** Reimplement validated decisions under production constraints.
