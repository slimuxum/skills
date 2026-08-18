# Deepening

How to deepen a cluster of shallow modules safely, given its dependencies. Assumes the vocabulary in [SKILL.md](SKILL.md) — **module**, **interface**, **seam**, **adapter**.

## Dependency categories

When assessing a candidate for deepening, classify its dependencies. The category determines how the deepened module is tested across its seam.

### 1. In-process

Pure computation, in-memory state, no I/O. Always deepenable — merge the modules and test through the new interface directly. No adapter needed.

### 2. Platform-substitutable

Dependencies with a faithful host, simulator, emulator, fake device, or in-memory stand-in. Deepen when the substitute preserves the contract being verified. Keep target or HIL verification for timing, memory, concurrency, peripheral, and integration risks the substitute cannot preserve.

### 3. Cross-process or cross-repository but owned

Owned processes, libraries, firmware partitions, generated interfaces, or services across repository and deployment boundaries. Define which repository owns the contract, its compatibility policy, and the integration order. Use an adapter where transport, OS, hardware, or deployment varies; do not assume an in-memory adapter proves binary, timing, or target behaviour.

Recommendation shape: *"Define the owned contract at this seam, keep policy in one deep module, isolate platform transport behind adapters, and verify logic on host plus the remaining integration risks on simulator, emulator, target, or HIL."*

### 4. External or hardware-controlled

Third-party services, vendor stacks, devices, buses, and hardware behaviour you do not control. Isolate the dependency behind an owned contract. Use mocks only for interactions they can represent; use conformance, replay, simulator, emulator, target, or HIL evidence for the rest.

## Seam discipline

- **Require a real reason for a seam.** Multiple adapters are strong evidence, but platform ownership, fault containment, binary compatibility, hardware isolation, or independent verification can also justify a seam.
- **Internal seams vs external seams.** A deep module can have internal seams (private to its implementation, used by its own tests) as well as the external seam at its interface. Don't expose internal seams through the interface just because tests use them.

## Verification strategy: consolidate without erasing evidence

- Map each affected requirement and risk to static, host, simulator, emulator, target, or HIL verification.
- Add verification at the deepened interface before removing existing tests or checks.
- Remove an old test only when its signal is demonstrably redundant; preserve tests that cover distinct faults, timing, ABI, resource, or target behaviour.
- Assert observable outcomes and documented runtime properties, not private implementation state.
- Record limitations when an environment cannot exercise the production contract.
