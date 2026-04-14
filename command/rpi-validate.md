---
description: Validate implementation against phase criteria and verification checks
---

# Validate a Phase

Validate implementation against the plan file and the phase's recorded acceptance criteria.

## User Input
$ARGUMENTS

## Artifact Rules

- Validate against `thoughts/plans/` first.
- Use `thoughts/research/` and `thoughts/docs/` only as supporting context.

## Workflow

1. Resolve the target.
   - Use the provided phase name, plan slug, or file path when present.
   - Otherwise identify the most relevant phase from `thoughts/plans/`.

2. Build context.
   - Read the full plan file and the relevant phase section.
   - Read implementation notes, code snippets, and verification logs.
   - Query `cm` when prior lessons may affect validation.

3. Gather evidence.
   - Use `rpi-guard` mentally or operationally first.
   - Check recent code changes with git.
   - Read the files the phase says should have changed.
   - Use `@codebase-analyzer` and `@codebase-pattern-finder` when useful.

4. Validate.
   - Compare actual code to the phase Description, Implementation, Acceptance Criteria, and Notes.
   - Run every automated verification command listed on the phase.
   - Treat manual verification as a last resort.

5. Record the result.
   - Add a validation section to the relevant phase notes.
   - Update acceptance criteria if validation exposes missing or incorrect checks.
   - Store only distilled reusable lessons in `cm`.

## Validation Shape

```markdown
## Validation Report

### Status
- Pass / Partial / Fail

### Automated Checks
- `just test` - pass

### Matches
- [what aligns with the phase]

### Deviations
- [what differs, with `path:line` refs]

### Manual Follow-up
- [only if automation is impossible]
```

## Rules

- Validate against plan files, not loose docs.
- Query `cm` only to sharpen validation, never to override current evidence.
- Run real checks when possible.
- Identify ambiguity precisely before concluding validation.
