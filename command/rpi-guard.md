---
description: Guard phase readiness before work or closure
---

# Guard a Phase

Audit a phase to determine whether it is ready for planning, ready for implementation, or ready to close. Use phase files as the source of truth and turn the workflow into an executable checklist.

## Artifact Rules

- Audit plan execution against `plans/` first.
- Use `research/` only for supporting context and unresolved investigation.
- Do not treat docs as the active source of truth.

## Phase Input
$ARGUMENTS

## Workflow

1. Resolve the target phase.
   - If the user gave a phase slug or file path, use it.
   - If they gave a parent plan, read `plans/<slug>.md` and choose the relevant child phase.
   - If nothing was given, check `plans/index.md` for active plan streams, then identify the most relevant phase.

2. Read the phase context.
   - Read the target phase file fully.
   - Read the parent plan file if the target is a child phase.
   - Read linked research files from `research/` when they exist.
   - Read the Implementation and Notes sections for planning details or verification logs.

3. Query `cm` when it can sharpen the audit.
   - Look for prior lessons, recurring failure modes, or constraints relevant to readiness.
   - Use `cm` to spot missing context or known pitfalls, not to override the phase file or current codebase.
   - If `cm` materially changes the audit, mention that briefly in the report or phase file notes.

4. Decide which audit mode applies.
   - Planning readiness: can this phase be turned into an implementation-ready plan?
   - Implementation readiness: can this phase be safely worked now?
   - Close readiness: can this phase be closed now?
   - If the user does not specify, check all three and report the highest unlocked state.

5. Audit planning readiness.
   - Confirm the phase has a clear problem statement or expected outcome.
   - Confirm linked research is resolved or identify that research is missing.
   - Confirm there is enough codebase context to define acceptance criteria.
   - Flag unresolved ambiguity around scope, security, UX, sequencing, or dependencies.

6. Audit implementation readiness.
   - Confirm the phase has actionable Description, Implementation, and Acceptance Criteria content.
   - Confirm the phase Implementation section includes concrete code snippets, test sketches, or patch fragments.
   - Confirm acceptance criteria include TDD-red-green-refactor.
   - Confirm acceptance criteria include committed-work-before-closure.
   - Confirm verification commands or observable checks exist.
   - Confirm blockers and dependencies are either resolved or explicitly called out.
   - Confirm linked research or parent context is sufficient for implementation.
   - Confirm the current branch already matches the plan stream, or that `rpi-implement` can safely rename, create, or switch to the correct branch before coding begins.
   - Flag mixed-stream committed work on the current branch as an implementation-readiness risk.

7. Audit close readiness.
   - Confirm the scoped work is implemented.
   - Confirm TDD-red-green-refactor was actually followed and recorded.
   - Confirm automated checks were run and results are recorded in the phase file.
   - Confirm any truly manual follow-up is identified.
   - Confirm the work is committed before closure.

8. Report the outcome.
   - Classify the phase as `not_ready`, `ready_to_plan`, `ready_to_implement`, or `ready_to_close`.
   - List failures first.
   - Then list missing fields, missing evidence, and suggested shortest fixes.
   - If useful, update the phase acceptance criteria so the phase becomes executable.

## Guard Report Shape

Use a concise structure like this:

```markdown
## Guard Report

### Status
- ready_to_implement

### Passes
- Parent plan exists and design is present
- Linked research file answers the open API question

### Fails
- Missing committed-work-before-closure acceptance criterion
- No executable verification command for the phase

### Shortest Fixes
- Add `- [ ] The work for this phase is committed before closure`
- Add `- [ ] just test-api`
```

## Default Guard Checks for Implementation Phases

Every implementation phase should satisfy these defaults:

- `Description` is actionable and scoped
- `Implementation` contains enough concrete code, test, or patch detail to execute safely
- `Acceptance Criteria` includes TDD-red-green-refactor
- `Acceptance Criteria` includes committed-work-before-closure
- at least one executable verification command exists
- parent or research context is sufficient to implement safely
- branch state is aligned with the current plan stream, or can be corrected before coding without mixing review stories

## Default Guard Checks for Closing Phases

Never consider an implementation phase close-ready unless all are true:

- the scoped change is complete
- TDD-red-green-refactor is recorded
- verification results are recorded
- a commit exists for the work
- any remaining manual follow-up is clearly called out

## Rules

- Guard against phase files, not docs outside `plans/`.
- Query `cm` when prior lessons or constraints can improve the audit, but treat phase files and current evidence as authoritative.
- Treat branch mismatch as a workflow risk to surface early, even when the phase fields themselves are otherwise ready.
- When the phase is almost ready, prefer the shortest corrective change that makes it executable.
- Do not silently ignore missing TDD-red-green-refactor or committed-work requirements.
