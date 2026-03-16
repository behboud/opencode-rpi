---
description: Audit bead readiness and close-readiness before work or closure
---

# Guard a Bead

Audit a bead to determine whether it is ready for planning, ready for implementation, or ready to close. Use beads as the source of truth and turn the workflow into an executable checklist.

## Bead Input
$ARGUMENTS

## Workflow

1. Resolve the target bead.
   - If the user gave a bead ID, use it.
   - If they gave a parent bead, use `bv --robot-plan`, `bv --robot-related`, or `br ready` to pick the relevant child.
   - If nothing was given, use `bv --robot-next`.

2. Read the bead context.
   - Read the target bead with `br show <id>`.
   - Read the parent bead if the target is a child bead.
   - Resolve related context with `bv --robot-related <id>`.
   - Read linked research beads when they exist.
   - Read comments on the bead when they contain planning details, implementation notes, or verification logs.

3. Decide which audit mode applies.
   - Planning readiness: can this bead be turned into an implementation-ready plan?
   - Implementation readiness: can this bead be safely worked now?
   - Close readiness: can this bead be closed now?
   - If the user does not specify, check all three and report the highest unlocked state.

4. Audit planning readiness.
   - Confirm the bead has a clear problem statement or expected outcome.
   - Confirm linked research beads are resolved or identify that research is missing.
   - Confirm there is enough codebase context to define phases or acceptance criteria.
   - Flag unresolved ambiguity around scope, security, UX, sequencing, or dependencies.

5. Audit implementation readiness.
   - Confirm the bead has actionable `description`, `design`, and `acceptance-criteria` content.
   - Confirm acceptance criteria include TDD-red-green-refactor.
   - Confirm acceptance criteria include committed-work-before-closure.
   - Confirm verification commands or observable checks exist.
   - Confirm blockers and dependencies are either resolved or explicitly called out.
   - Confirm linked research beads or parent context are sufficient for implementation.

6. Audit close readiness.
   - Confirm the scoped work is implemented.
   - Confirm TDD-red-green-refactor was actually followed and recorded.
   - Confirm automated checks were run and results are recorded on the bead.
   - Confirm any truly manual follow-up is identified.
   - Confirm the work is committed before closure.
   - Confirm the bead comments tell enough of the implementation story for future reference.

7. Report the outcome.
   - Classify the bead as `not_ready`, `ready_to_plan`, `ready_to_implement`, `ready_to_close`, or `closed_cleanly`.
   - List failures first.
   - Then list missing fields, missing evidence, and suggested shortest fixes.
   - If useful, add or update bead acceptance criteria with `br update` so the bead becomes executable.

## Guard Report Shape

Use a concise structure like this:

```markdown
## Guard Report

### Status
- ready_to_implement

### Passes
- Parent bead exists and design is present
- Linked research bead `repo-123` answers the open API question

### Fails
- Missing committed-work-before-closure acceptance criterion
- No executable verification command for the phase

### Shortest Fixes
- Add `- [ ] The work for this bead is committed before closure`
- Add `- [ ] just test-api`
```

## Default Guard Checks for Implementation Beads

Every implementation bead should satisfy these defaults:

- `description` is actionable and scoped
- `acceptance-criteria` includes TDD-red-green-refactor
- `acceptance-criteria` includes committed-work-before-closure
- at least one executable verification command exists
- parent or research context is sufficient to implement safely

## Default Guard Checks for Closing Beads

Never consider an implementation bead close-ready unless all are true:

- the scoped change is complete
- TDD-red-green-refactor is recorded
- verification results are recorded
- a commit exists for the work
- any remaining manual follow-up is clearly called out

## Rules

- Guard against beads, not docs.
- Prefer short commands: `br show`, `br update`, `br comments add`, `bv --robot-next`, `bv --robot-related`, `bv --robot-plan`.
- When the bead is almost ready, prefer the shortest corrective change that makes it executable.
- Do not silently ignore missing TDD-red-green-refactor or committed-work requirements.
