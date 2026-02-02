---
description: Implement technical plans from thoughts/shared/plans with verification
---

# Implement Plan

Implement one approved phase from a plan in `thoughts/shared/plans/`, then stop for review.

## Plan Path

$ARGUMENTS

## Workflow (Pseudocode + Snippets)

1. Load plan:
   - read the plan fully; find the first unchecked item

2. Implement one phase:
   - pseudocode:
     - make the smallest atomic change set for this phase
     - keep changes aligned to plan intent

3. Verify:
   - run the plan's checks (snippets): `make test`, `make check`, `npm test`
   - if a check fails: fix, rerun, and report

4. Mark progress:
   - update checkboxes in the plan for completed items

5. Stop:
   - do not continue to the next phase until a human confirms

## Manual Testing

- Only request manual verification when automation is impossible (sudo/hardware/install).
