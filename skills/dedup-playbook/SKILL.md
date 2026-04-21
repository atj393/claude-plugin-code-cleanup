---
name: dedup-playbook
description: Internal cleanup playbook for deduplication. Use when assessing or implementing low-risk DRY improvements in a codebase.
user-invocable: false
---

Track: Deduplication

Task:
Scan the codebase for repeated logic, copy-pasted functions, duplicated condition trees, repeated transformation code, and redundant abstractions.

Goal:
Reduce unnecessary duplication only where doing so clearly improves maintainability without obscuring intent.

Rules specific to this track:
- Do not merge code that only looks similar but serves different domain purposes.
- Do not introduce generic helpers that make the code harder to read.
- Consolidate only when the shared behavior is genuinely the same.
- Prefer local duplication over bad abstraction.
- Be especially cautious with UI code, validation logic, business rules, and error flows that may appear similar but diverge subtly.
- If shared logic is extracted, choose names that explain intent, not mechanics.

High-confidence examples:
- truly identical helper logic repeated across files
- repeated constant maps or literal unions that have drift risk
- duplicated utility functions with identical semantics
- repeated small transformation blocks that are clearly the same operation

Avoid:
- merging similar-looking branches with slightly different business meaning
- extracting one-off helpers used in only two places if readability gets worse
- refactors whose main benefit is cleanliness rather than reduced maintenance risk

Output structure:
1. Critical assessment
2. High-confidence / low-risk candidates
3. Medium-confidence items not implemented
4. Low-confidence items untouched
5. Files likely affected
6. Validation plan
7. If editing was requested by the lead, implemented changes and validation results
