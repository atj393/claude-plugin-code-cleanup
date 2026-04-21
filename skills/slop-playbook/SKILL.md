---
name: slop-playbook
description: Internal cleanup playbook for removing clearly obsolete code paths, placeholders, and low-value AI artifacts.
user-invocable: false
---

Track: Deprecated code and AI slop removal

Task:
Find obsolete fallback paths, legacy compatibility branches, deprecated utilities, stale feature flags, placeholder implementations, TODO-style scaffolding, AI-generated stubs, and comments that narrate edit history instead of explaining intent.

Goal:
Remove clearly obsolete code and clean out low-value AI artifacts while preserving anything still needed for compatibility or active usage.

Rules specific to this track:
- Remove deprecated paths only when they are clearly no longer needed.
- Verify compatibility assumptions before deleting fallbacks.
- Delete placeholder logic, dead stubs, and narrative comments that do not help a new engineer understand the code.
- If a comment is worth keeping, rewrite it to explain why the code exists, not what changed in some past edit.
- Prefer fewer, better comments.
- Preserve comments that explain non-obvious constraints, integration quirks, or domain rules.

High-confidence examples:
- obviously unused TODO scaffolding
- placeholder branches that can never execute meaningfully
- comments like AI added this or temporary fix from earlier attempt
- legacy fallback paths superseded everywhere and no longer referenced

Avoid:
- deleting compatibility code without proving obsolescence
- removing important historical context that still explains a live constraint
- replacing precise comments with generic fluff

Output structure:
1. Critical assessment
2. High-confidence / low-risk candidates
3. Medium-confidence items not implemented
4. Low-confidence items untouched
5. Files likely affected
6. Validation plan
7. If editing was requested by the lead, implemented changes and validation results
