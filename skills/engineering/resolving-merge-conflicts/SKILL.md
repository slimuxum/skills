---
name: resolving-merge-conflicts
description: "Use when you need to resolve an in-progress merge or rebase by intent across one or more repositories, verify the integration, and finish the operation."
---

# Resolving Merge Conflicts

1. **Map the operations.** For every affected repository, record root, source revision, operation type, current step, conflicts, unrelated local changes, and the user's intended integration result. Do not assume one repository's state describes the whole logical change.

2. **Confirm scope.** Ask before editing when writable scope, generated files, submodules, target configuration, or the intended winner is unclear. Never abort; this Skill resolves and finishes the in-progress operation.

3. **Find primary sources.** Read commits, PRs, issues/specs, interface owners, generated-source inputs, and cross-repository compatibility rules for both sides.

4. **Resolve each hunk by intent.** Preserve both intents where compatible. Where incompatible, follow the confirmed integration goal and record the trade-off. Do not invent new behaviour, rewrite unrelated work, or resolve generated output without checking its source.

5. **Verify the integrated change.** Run the applicable static, host, simulator, emulator, target, and HIL checks for the combined result, not just each side independently. State what ran, the result, its environment, and limitations; keep agreed checks that did not run explicit.

6. **Finish the operation.** Show the resolved scope and evidence, stage the resolved files, and continue the merge or rebase until it completes. Create the merge commit when Git requires it. Never push.
