---
name: cycles-playbook
description: Internal cleanup playbook for identifying and safely breaking important circular dependencies.
user-invocable: false
---

Track: Circular dependencies

Task:
Map the dependency graph and identify circular imports. Prioritize the cycles that harm maintainability, testability, initialization safety, or correctness.

Use available tools such as madge if present, but verify the actual source relationships manually before changing code.

Goal:
Break meaningful cycles with the smallest possible structural change.

Rules specific to this track:
- Prefer extracting genuinely shared logic into a neutral module.
- Prefer dependency inversion only if it is already consistent with the codebase style.
- Do not create artificial abstraction layers just to break a cycle.
- Do not move unrelated code into a shared module as a dumping ground.
- Focus first on cycles involving core modules, types, utilities, initialization paths, and test-hostile coupling.

High-confidence examples:
- pure shared constants, types, or helpers extracted into neutral modules
- type-only import fixes where supported and appropriate
- separating utility logic from feature modules when dependency direction is clearly wrong

Avoid:
- introducing interfaces, classes, or factories solely to satisfy the graph
- broad module reshuffling
- breaking harmless cycles with a large refactor unless there is clear payoff

Output structure:
1. Critical assessment
2. High-confidence / low-risk candidates
3. Medium-confidence items not implemented
4. Low-confidence items untouched
5. Files likely affected
6. Validation plan
7. If editing was requested by the lead, implemented changes and validation results
