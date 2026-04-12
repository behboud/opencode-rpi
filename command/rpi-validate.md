---
description: Validate implementation against phase criteria and verification checks
---

# Validate a Phase

Validate implementation against the phase plan and the phase's recorded acceptance criteria. Phase files are the source of truth.

## Artifact Rules

- Validate against `plans/` first.
- Use `research/` for supporting context when the phase references it.
- Do not treat docs as the active source of truth.

## Phase Input
$ARGUMENTS

## Workflow

1. Resolve the validation target.
   - If the user gave a phase slug or file path, use it.
   - If they gave a parent plan, read `plans/<slug>.md` and choose the relevant child phase.
   - If nothing was given, check `plans/index.md` for active plan streams, then identify the most relevant phase.

2. Read the phase context first.
   - Read the target phase file fully.
   - Read its parent plan if the target is a child phase.
   - Read the Implementation and Notes sections for code snippets, patches, test cases, and verification logs.

3. Query `cm` before deeper validation when useful.
   - Look for prior constraints, related lessons, or recurring failure patterns relevant to the phase.
   - Treat `cm` as supporting context only; validate against the current phase file, code, and executed checks.
   - If a `cm` lesson materially affects the validation outcome, mention that briefly in the validation note.

4. Guard the phase before validating closure.
   - Run `rpi-guard` mentally or operationally against the phase first.
   - Use it to identify missing readiness or close-readiness evidence before the deeper validation pass.

5. Discover implementation evidence.
   - Check recent code changes with git.
   - Read the files that the phase says should have changed.
   - Spawn parallel research tasks when needed:
     - `@explore` to compare intended vs actual file changes.
     - `@explore` to verify tests and verification commands.

6. Validate systematically.
   - Compare actual code and behavior to the phase Description, Implementation, Acceptance Criteria, and Notes.
   - Run every automated verification command listed on the phase.
   - Treat manual verification as a last resort only when it truly cannot be automated.
   - Call out mismatches between the parent plan and the phase implementation.

7. Record the result in the phase file.
   - Add a validation section to the phase file notes.
   - If validation reveals missing or incorrect acceptance checks, update the phase file acceptance criteria.
   - If validation uncovers a reusable systemic lesson, store the distilled takeaway in `cm`.

## Validation Note Shape

```markdown
## Validation Report

### Status
- Pass / Partial / Fail

### Automated Checks
- `just test` — pass
- `just lint` — fail: [brief reason]

### Matches
- [what aligns with the phase plan]

### Deviations
- [what differs, with `path:line` refs]

### Manual Follow-up
- [only if automation is impossible, with reason]
```

## Rules

- Validate against phase files in `plans/`, not loose docs.
- Query `cm` when it helps surface prior constraints or failure patterns, but validate against present evidence.
- Use guard before deciding a phase is ready to close.
- Run real checks when possible; do not ask the user to do work you can automate.
- If the phase is ambiguous, identify the ambiguity precisely before concluding validation.
