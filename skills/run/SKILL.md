---
name: run
description: Run a careful, low-risk code quality cleanup pass over a repository or selected paths. Use when you want a coordinated cleanup using seven focused tracks with conservative implementation and validation.
when_to_use: Use for codebase cleanup, quality hardening, low-risk refactors, dead code checks, deduplication, type cleanup, circular dependency review, and cleanup of deprecated or AI-generated artifacts. Prefer this over ad hoc cleanup prompts when you want a structured multi-track pass.
argument-hint: [optional paths and notes]
disable-model-invocation: true
context: fork
agent: code-cleanup-lead
---

# Cleanup scope

Use this scope exactly as given by the user:

`$ARGUMENTS`

If the user provided no arguments, treat the scope as the whole repository.

# Your task

Run a careful, low-risk cleanup pass with seven tracks:
1. deduplication
2. type consolidation
3. dead code removal
4. circular dependencies
5. type strengthening
6. error handling cleanup
7. deprecated code and AI slop removal

Then run a final reviewer pass.

# Non-negotiable behavior

- Be conservative.
- Preserve behavior.
- Keep diffs small and reviewable.
- Prefer local fixes over architectural rewrites.
- Do not do broad naming churn, formatting churn, or speculative refactors.
- Only implement high-confidence, low-risk changes.
- Medium-risk items must be reported, not implemented.
- Low-confidence items must be left untouched.

# Required orchestration strategy

## Phase 1 - repo understanding
- Identify the package manager, workspace layout, frameworks, and conventions.
- Find the best available validation commands for lint, typecheck, tests, and build.
- Find analysis tools already present such as knip, ts-prune, madge, eslint, tsconfig references, or custom scripts.

## Phase 2 - parallel assessment only
Launch the seven specialized subagents to inspect their own track and return:
- critical assessment
- high-confidence candidates
- medium-confidence candidates
- low-confidence candidates
- likely file touch set
- recommended validation for that track

During this phase, prefer concurrent background work when safe.
No worker should start broad edits before the lead has reviewed overlap risk.

## Phase 3 - conflict-aware implementation
- Compare the workers' proposed file touch sets.
- If two or more tracks touch the same files or same module boundary, sequence them instead of running edits in parallel.
- Prefer this implementation order unless the repo suggests a safer one:
  1. dead code removal
  2. deprecated code and AI slop removal
  3. type consolidation
  4. deduplication
  5. circular dependencies
  6. type strengthening
  7. error handling cleanup
- After each implemented batch, run the smallest relevant validation.
- If a change introduces failures, revert or reduce scope rather than pushing through.

## Phase 4 - final review
Run the reviewer subagent after all edits are complete.
The reviewer must check for:
- accidental behavior changes
- same-file conflict damage
- hidden risk from over-aggressive cleanup
- unresolved follow-up items
- missing validation

## Phase 5 - final output
Return one consolidated summary with:
1. repo understanding
2. per-track assessment
3. implemented changes
4. skipped medium-risk items
5. validation run and results
6. final risk summary
7. recommended next pass

# Important implementation policy

Use the specialized subagents by name:
- code-cleanup-dedup
- code-cleanup-types
- code-cleanup-dead-code
- code-cleanup-cycles
- code-cleanup-strong-types
- code-cleanup-errors
- code-cleanup-slop
- code-cleanup-review

Workers should own different files whenever possible.
Parallelize assessment aggressively, but parallelize editing only when file ownership is clearly separated.
