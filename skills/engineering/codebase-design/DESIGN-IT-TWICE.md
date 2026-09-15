# Design It Twice

When the user wants to explore alternative interfaces for a chosen deepening candidate, use this parallel sub-agent pattern. Based on "Design It Twice" (Ousterhout) — your first idea is unlikely to be the best.

Uses the vocabulary in [SKILL.md](SKILL.md) — **module**, **interface**, **seam**, **adapter**, **leverage**.

## Process

### 1. Frame the problem space

Before spawning sub-agents, write a user-facing explanation of the problem space for the chosen candidate:

- The constraints any new interface would need to satisfy
- The dependencies it would rely on, and which category they fall into (see [DEEPENING.md](DEEPENING.md))
- The repositories and logical contexts involved, contract ownership, and compatibility constraints
- Applicable ABI, timing, memory, concurrency, startup/shutdown, failure-containment, and hardware constraints
- The verification environments available: static, host, simulator, emulator, target, and HIL
- A rough illustrative code sketch to ground the constraints — not a proposal, just a way to make the constraints concrete

Show this to the user, then proceed to Step 2 while they read and think when the [risk and goal confirmation rule](SKILL.md#risk-and-goal-confirmation) permits it. Under the default, pause for decision-affecting uncertainty, high risk, or departure from the agreed goal. Explicit autonomous authorization allows the original dispatch flow within its scope.

### 2. Spawn sub-agents

Before proposing an interface, freeze the actual set of design constraints and its worker count `N` (`N >= 3`, including any applicable fourth or later constraint). Verify that the harness can start all `N` isolated sub-agents concurrently. If it cannot, stop with `BLOCKED`, name the missing capacity, and produce no designs. Do not reduce the selected set or generate alternatives sequentially in the parent agent.

Spawn the frozen `N` sub-agents in parallel. Each must produce a **radically different** interface for the deepened module. Include the full risk and goal confirmation rule, agreed objective and scope, any explicit autonomous authorization with its scope, and pending confirmations in every brief; workers must not depend on implicit Skill access. Also say: **Do not invoke `codebase-design`, Design It Twice, or any other Skill; do not spawn sub-agents. Perform this assigned design directly.**

Prompt each sub-agent with a separate technical brief (file paths, coupling details, dependency category from [DEEPENING.md](DEEPENING.md), what sits behind the seam). The brief is independent of the user-facing problem-space explanation in Step 1. Give each agent a different design constraint:

- Agent 1: "Minimize the interface — aim for 1–3 entry points max. Maximise leverage per entry point."
- Agent 2: "Maximise flexibility — support many use cases and extension."
- Agent 3: "Optimise for the most common caller — make the default case trivial."
- Agent 4 (if applicable): "Design around ports & adapters for cross-seam dependencies."

Include both [SKILL.md](SKILL.md) vocabulary and the canonical vocabulary for every active Logical Context so each sub-agent names things consistently with the architecture and domain language.

Each sub-agent outputs:

1. Interface (types, methods, params — plus invariants, ordering, error modes)
2. Usage example showing how callers use it
3. What the implementation hides behind the seam
4. Dependency strategy and adapters (see [DEEPENING.md](DEEPENING.md))
5. Repositories changed, interface owner, compatibility and integration sequence
6. Verification strategy across applicable environments, with uncovered limitations
7. Runtime and safety trade-offs — where leverage is high, where it is thin

### 3. Present and compare

Present designs sequentially so the user can absorb each one, then compare them in prose. Contrast by **depth**, **locality**, **seam placement**, repository ownership, compatibility, runtime constraints, and verification coverage.

After comparing, give your own recommendation: which design you think is strongest and why. If elements from different designs would combine well, propose a hybrid. Be opinionated — the user wants a strong read, not a menu.
