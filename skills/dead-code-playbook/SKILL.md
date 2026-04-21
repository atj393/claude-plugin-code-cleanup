---
name: dead-code-playbook
description: Internal cleanup playbook for removing truly dead code after manual verification.
user-invocable: false
---

Track: Dead code removal

Task:
Find unused exports, unreferenced functions, dead utilities, orphaned files, obsolete constants, and unreachable code.

Use repository tools if available such as knip, ts-prune, eslint unused rules, build references, and framework diagnostics, but never trust static analysis blindly.
Manual verification is mandatory before deletion.

Rules specific to this track:
- Check for dynamic imports.
- Check framework conventions and file-based routing.
- Check config references, CLI entry points, generated code, registration patterns, plugin hooks, test discovery, and string-based lookups.
- Check package exports and public API surfaces.
- Remove only what is confirmed dead.
- If a file is suspicious but not fully proven dead, report it and do not remove it.

High-confidence examples:
- unused local helpers confirmed by references and search
- exports proven unused internally and not exposed publicly
- dead branches behind permanently false conditions
- files replaced by newer implementations and no longer referenced anywhere

Avoid:
- deleting code only because a tool says unused
- deleting convention-driven files
- deleting compatibility shims without proving they are obsolete
- deleting anything referenced indirectly unless fully verified

Output structure:
1. Critical assessment
2. High-confidence / low-risk candidates
3. Medium-confidence items not implemented
4. Low-confidence items untouched
5. Files likely affected
6. Validation plan
7. If editing was requested by the lead, implemented changes and validation results
