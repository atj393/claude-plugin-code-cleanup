---
name: errors-playbook
description: Internal cleanup playbook for removing silent failures and preserving only real boundary error handling.
user-invocable: false
---

Track: Error handling cleanup

Task:
Inspect try/catch blocks, Promise catch handlers, fallback branches, ignored return values, and defensive patterns that may be hiding failures.

Goal:
Remove error handling that silently swallows problems or masks real failures, while preserving error handling that provides real boundary value such as cleanup, logging, user messaging, retries, or recovery.

Rules specific to this track:
- Keep error handling that serves a real purpose.
- Remove or improve catch blocks that silently ignore errors.
- Be cautious with user-facing flows, background jobs, cleanup paths, and best-effort telemetry where suppression may be intentional.
- If an error is intentionally downgraded, it should still be visible through logging, reporting, or explicit documented handling where appropriate.
- Do not convert recoverable boundaries into hard crashes without strong justification.
- Prefer explicit handling over silent fallback.

High-confidence examples:
- empty catch blocks with no justification
- catch handlers returning misleading defaults that mask bugs
- promise chains that discard actionable failures
- swallowed errors in internal utility code where propagation is clearly correct

Avoid:
- removing intentional recovery logic
- changing UX behavior without understanding the flow
- breaking best-effort cleanup behavior
- turning every handled failure into thrown exceptions indiscriminately

Output structure:
1. Critical assessment
2. High-confidence / low-risk candidates
3. Medium-confidence items not implemented
4. Low-confidence items untouched
5. Files likely affected
6. Validation plan
7. If editing was requested by the lead, implemented changes and validation results
