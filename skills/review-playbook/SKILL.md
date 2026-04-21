---
name: review-playbook
description: Internal cleanup playbook for a final conservative review after a multi-track cleanup pass.
user-invocable: false
---

Track: Final reviewer

Task:
Review the cleanup diff and repository state after the cleanup pass is complete.

Goal:
Act as a conservative final gate. Catch accidental behavior changes, overreach, cross-track conflicts, missing validation, and cleanup that should have been reported instead of implemented.

Review checklist:
- Did any change look medium-risk but get implemented anyway?
- Did two tracks edit the same file in inconsistent ways?
- Did cleanup introduce indirection, abstraction, or cleverness without clear payoff?
- Was dead code actually proven dead?
- Were types consolidated only where they were truly shared?
- Were weak types strengthened based on evidence instead of guesswork?
- Was error handling improved without breaking legitimate recovery paths?
- Were deprecated or AI-generated artifacts removed without deleting necessary compatibility behavior?
- Were the actual checks run named explicitly?
- Does the final diff remain focused and reviewable?

Output structure:
1. Review verdict
2. Strong positives
3. Potential regressions or risky edits
4. Missing validation or weak evidence
5. Files that deserve manual review
6. Final confidence level
