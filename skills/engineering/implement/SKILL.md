---
name: implement
description: "Implement scoped work from a spec, ticket, or agreed plan across one or more repositories, with risk-based verification, review, and a final commit."
disable-model-invocation: true
---

# Implement

Implement only the work the user authorized. Do not redesign the plan, advance workflow state, close tickets, or push. Finish the implemented slice by committing it to the current branch, as this Skill has always done.

## 1. Preflight

Before writing:

- Resolve the full workspace scope: repositories, logical contexts, relevant subtrees, source revisions, and cross-repository interfaces.
- Restate acceptance criteria and identify missing or contradictory inputs. Stop with `BLOCKED` when safe implementation would require guessing.
- Confirm writable paths, generated/vendor exclusions, permitted tools, authoritative compiler/linker/build/toolchain configuration, target/lab access, and the intended current branch in each repository.
- Record the initial status of every in-scope repository. Preserve unrelated user changes.
- Select verification environments by risk: static, host, simulator, emulator, target, and HIL. State what each can prove and what remains limited.

## 2. Execute the logical change

Order edits by interface compatibility and integration dependency, not by the current working directory. Keep repositories independently buildable when possible; when an integration point cannot be green independently, state the temporary condition and the final integration check.

Call the Skill tool with "tdd" only where a fast, deterministic red-green loop is appropriate. For configuration, generated code, hardware-only behaviour, timing/resource constraints, or expensive target/HIL checks, use the risk-appropriate static analysis, review, procedure, or staged verification instead of manufacturing a low-value test.

Make the smallest coherent change. After each slice, run the cheapest relevant check. Do not flash targets, mutate shared infrastructure, run destructive migrations, or change calibration without explicit authorization and recovery steps.

## 3. Verify and review

Run the agreed verification work. For each check, report the command or procedure, environment, source revision, result, output summary, and limitations. Keep agreed checks that did not run explicit.

Call the Skill tool with "code-review" against the complete logical change across all affected repositories, including committed, staged, unstaged, and untracked files. Address findings only within the approved scope.

## 4. Finish

Summarize changed repositories/files, acceptance coverage, evidence, and remaining risks. Commit the coherent reviewed slice to the current branch in each affected repository. Never push, merge, close a ticket, or mark product completion.
