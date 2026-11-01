---
name: ub-mode
description: >
  Apply disciplined, risk-tiered software engineering principles when writing or
  changing code. Classify the task, then apply First Make it Work, Then Make it
  Right, and Practice at a proportional level. Use when the user runs /ub-mode or asks for these principles.
---

# ub-mode

Every time you are asked to make a change:

1. Classify the task using **Task Tiers**. Choose the lowest sufficient tier, considering risk and blast radius, not just edit size.
2. Apply **First Make it Work**, **Then Make it Right**, and **Practice** at the depth required by that tier.
3. Verify the result and report what changed, what was checked, and what remains unverified.
4. **Do not stage, commit, push, merge, rebase, or otherwise publish Git changes unless the user explicitly requests that Git operation.** A request to implement or finish a task is not authorization to commit or push.

# Task Tiers

## Classification

Classify before editing; reassess whenever discoveries increase the scope or risk. When in doubt, choose the higher tier. A small diff can still be critical. Do not downgrade merely to avoid tests or gates. Project-specific instructions may require stricter checks.

| Tier                | Typical tasks                                                                                                                            | Minimum verification                                                                     | Additional discipline                                                                                                                                                                 |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **L0 — Trivial**    | Copy, comments, formatting, isolated styling, nonfunctional configuration changes                                                        | Inspect diff; check formatting or syntax if applicable                                   | No mandatory TDD or full quality-gate suite; confirm there is no behavioral change                                                                                                    |
| **L1 — Standard**   | Localized bug fixes, ordinary business features, small refactors                                                                         | Reproduce when fixing a bug; relevant tests; applicable typecheck/lint                   | Prefer Red → Green for behavior changes; add meaningful regression tests; keep edits localized                                                                                        |
| **L2 — Structural** | Domain/data model changes, cross-module refactors, shared API contracts, substantial editor/rendering logic                              | L1 checks plus relevant integration/contract tests and broader regression checks         | Explicitly inspect design and boundaries; use TDD for testable behavior; run applicable Crapper/Dryer gates on affected code; use Mutator selectively on important logic              |
| **L3 — Critical**   | Security/auth/authorization, payments, destructive migrations, concurrency, transactions, irreversible writes, critical production paths | L2 checks plus failure/retry/rollback scenarios and appropriately broad regression tests | Validate invariants, idempotency, data integrity and concurrency; run all applicable quality gates, including mutation testing for critical testable logic; identify unresolved risks |

### Tier Selection Rules

- Classify by the **highest applicable risk**, not by the number of files or lines changed.
- Escalate to **L2** for changes to persistent data shapes, shared interfaces, or behavior spanning multiple modules.
- Escalate to **L3** when failure could compromise security, lose or corrupt data, cause duplicate financial or irreversible effects, or trigger serious concurrency failures.
- If a change spans tiers, apply the highest tier to the risky portion, while keeping checks proportional elsewhere.
- If a required check cannot run because tooling or environment is unavailable, **do not claim it passed**. Use a reasonable alternative when possible and report the limitation.
- Avoid ceremonial tests and unrelated refactors; verification must demonstrate useful behavior or enforce a meaningful invariant.

# First Make it Work

## Laziness Protocol

- One decision lives in one place.
- Ship the smallest edit that solves the problem.

## Foundational Thinking

- Get the data shape right before you write the logic.
- Do first whatever helps every later phase.
- If a concurrent writer would matter, do not share state.

## TDD

- For L1 behavior changes, prefer writing the failing test before you change the code; for testable L2/L3 behavior changes, require it.
- See it fail for the reason you expect.
- Make the smallest change that makes it pass.
- L0 does not require new tests when behavior is unchanged.
- If an honest test is impractical, do not write a bad one; explain the substitute verification.

## Redesign From First Principles

- Do not bolt a new requirement onto the old design.
- Ask what you would have built if it had been there from the start, think that through, and deliver it in pieces.
- Apply explicit architecture reconsideration for L2/L3; for L0/L1, do not redesign unrelated code.

## Attack the Premise

- When two fixes that share one premise fail the same gate, write that premise in one sentence and count which actors hold the imbalance.
- Remove the assignment.
- Do not start another fix until you have done that.

## Subtract Before You Add

- Take complexity out before you add anything.

## Outcome-Oriented Execution

- Aim at the end state you can check.
- A break in the middle is allowed when you planned it and can undo it.

## Build the Lever

- If the work is not obvious at a glance, build the tool that does it when the tool will reduce risk or repeated effort.
- Do one unit by hand, then make the tool match that unit.
- Do not build infrastructure for a trivial one-off edit.

## Architecture

### Model the Domain

- Put the domain in a structure that makes the illegal states impossible.
- Maintain a glossary of domain terms when the domain warrants it.
- If the code is already clear and local, leave it.

### Boundary Discipline

- Check the data where it enters.
- Inside, trust it.
- Keep the business logic in a pure function where practical.

### Type System Discipline

- Make the illegal value impossible to write.
- Parse outside data once, where it comes in.
- Do not silence the compiler.

### Make Operations Idempotent

- Running it twice, or running it after a crash halfway through, ends in the same state wherever the operation requires retry safety.
- For L3, explicitly verify retries, partial failures, and repeated requests when relevant.

### Separate Before Serializing Shared State

- If they do not need the same mutable thing, give each its own.
- If they do, allow one writer, and enforce that in the structure.

# Then Make it Right

## Fix Root Causes

- Reproduce it.
- Fix the cause.
- A guard that only hides the crash is not a fix.

## Sequence Work into Verifiable Units

- Finish one small unit and check it.
- Then start the next.

## Test Behavior, Not Implementation

- The test calls the code the way its user does and checks the result that user would see.
- If the test would still pass when every function returned nothing, throw it out.

## Crapper

- For L2, run Crapper on affected testable code when supported; for L3, run it on relevant critical code as a quality gate when supported.
- Target CRAP < 10 per function by splitting it, by testing it, or both.
- Consider whether case statements are worth reducing.
- For L0/L1, do not require Crapper by default unless the project already enforces it.
- If unsupported or unavailable, report that rather than inventing a score.

## Dryer

- For L2/L3, use Dryer when supported to inspect structural duplication in affected code; eliminate harmful duplication ranked 0.8 or above without creating misleading abstractions.
- For L0/L1, use normal review rather than requiring a whole-project duplication scan.
- If unsupported or unavailable, report that rather than claiming a pass.

## Mutator

- For L2, use mutation testing selectively on behavior whose failure would be consequential; for L3, run it on critical testable logic when tooling permits.
- Cover the uncovered.
- Kill the survivors.
- Delete dead code.
- Do not require mutation testing for L0 or ordinary L1 changes.
- Report unrun mutation tests or surviving mutants that remain relevant.

## Quality Tool Execution

- Select quality tools according to the task tier and actual risk.
- Tools are available on PATH.
- Do not install missing tools automatically without user approval.
- Run quality tools only against relevant changed modules when practical.
- For TypeScript monorepos, respect package boundaries and existing test commands.
- Prefer existing coverage reports over triggering redundant test runs.
- Treat duplication findings as refactoring candidates, not mandatory abstractions.
- Do not suppress or manipulate metrics merely to satisfy thresholds.
- If a tool is unavailable, report the limitation and use available equivalent checks.

## Refactoring

- Pin what the code does.
- Change the structure.
- Prove the behavior did not change.

# Practice

## Keep a Log

- Keep a concise record of significant actions, decisions, verification, and failures; be proportional to task tier.
- Report the outcome, checks actually run, failures or limitations, and remaining risks upon completion.

## Guard the Context Window

- Keep the main thread small.
- Send the bulk elsewhere when the environment supports it, and keep the summary.

## Worktree Cleanup

- Find what is safe to remove, confirm nothing still in use needs it, then reclaim the disk.
- Uncommitted work waits for a decision; do not discard, reset, or overwrite the user's changes without explicit authorization.
- Do not perform unrelated worktree cleanup for L0/L1 tasks.
