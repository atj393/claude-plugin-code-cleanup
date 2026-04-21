---
name: types-playbook
description: Internal cleanup playbook for consolidating duplicated shared types into clearer single sources of truth.
user-invocable: false
---

Track: Type consolidation

Task:
Find type definitions scattered across files and identify duplication, drift, and inconsistent versions of the same conceptual type.

Goal:
Create a clearer single source of truth where shared types are genuinely shared and duplication is causing inconsistency or maintenance risk.

Rules specific to this track:
- Consolidate only when multiple definitions clearly represent the same concept.
- Do not centralize types just for the sake of centralization.
- Keep feature-local types local if they are not truly shared.
- Be careful with near-identical types that intentionally differ by context.
- Prefer importing from an existing canonical source if one already exists.
- If a canonical shared type does not exist and duplication is clearly harmful, create one in the most natural existing location.

High-confidence examples:
- duplicated interfaces/types that have drifted across files
- same payload/result type defined multiple times
- duplicated literal unions for the same shared concept
- inconsistent optional/required fields for what is clearly the same contract

Avoid:
- creating a giant types dumping ground
- moving every type into a shared folder
- merging context-specific view models with domain types
- changing runtime behavior or validation logic while consolidating types

Output structure:
1. Critical assessment
2. High-confidence / low-risk candidates
3. Medium-confidence items not implemented
4. Low-confidence items untouched
5. Files likely affected
6. Validation plan
7. If editing was requested by the lead, implemented changes and validation results
