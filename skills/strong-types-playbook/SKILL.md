---
name: strong-types-playbook
description: Internal cleanup playbook for replacing weak types with strong, correct types where the evidence is clear.
user-invocable: false
---

Track: Type strengthening

Task:
Find weak typing such as any, broad unknown usage, unsafe casts, placeholder generics, loose object shapes, and AI-generated type shortcuts.

Goal:
Replace weak types with stronger, correct types where the real type can be confidently derived from the codebase and usage.

Rules specific to this track:
- Investigate actual call sites, returned values, library APIs, schema definitions, and runtime usage before changing a type.
- Preserve legitimate boundary use of unknown at untrusted input boundaries.
- Replace any only when the real shape or constraint can be established confidently.
- Prefer narrower types that reflect actual behavior.
- Avoid false precision when runtime guarantees do not exist.
- Run typechecks after each batch.

High-confidence examples:
- any used where a concrete existing type already exists
- repeated unsafe casts hiding a stable real shape
- loose callback signatures that can be derived from actual usage
- inferred unions that can be made explicit and safer

Avoid:
- inventing complex types without proof
- replacing unknown at parsing or external-input boundaries unless proper validation exists
- overengineering generics
- forcing strictness that breaks legitimate flexibility

Output structure:
1. Critical assessment
2. High-confidence / low-risk candidates
3. Medium-confidence items not implemented
4. Low-confidence items untouched
5. Files likely affected
6. Validation plan
7. If editing was requested by the lead, implemented changes and validation results
