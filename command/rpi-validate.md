---
description: Validate implementation against plan, verify success criteria, identify issues
---

# Validate Plan

Validate that an implementation plan was executed correctly: success criteria met, diffs match intent, and deviations are documented.

## Workflow (Short Snippets + Pseudocode)

1. Locate and read the plan:
   - If you have a path, use it; otherwise ask for it.

2. Collect evidence (snippets):
   - `git log --oneline -n <N>`
   - `git diff <base>...HEAD`
   - `make check test` (or the repo's equivalent)

3. Validate phase-by-phase:
   - Pseudocode:
     - for each phase:
       - compare expected file changes vs actual diffs
       - run each automated check in the plan
       - list any manual checks only if automation is impossible (sudo/hardware/install)

4. Produce a report (keep it scannable):
   - what matches
   - what deviates (with file refs)
   - what failed (command + error summary)
   - what requires manual verification (and why)

## Rules

- Prefer automation; manual steps only when truly unavoidable.
- If a check fails, stop and report the failure before claiming completion.
