---
description: Validate bead execution against bead criteria and verification checks
---

# Validate a Bead

Validate implementation against the bead plan and the bead's recorded acceptance criteria. Beads are the source of truth.

## Bead Input
$ARGUMENTS

## Workflow

1. Resolve the validation target.
   - If the user gave a bead ID, use it.
   - If they gave a parent bead, choose the relevant child with `bv --robot-plan`, `bv --robot-related`, or `br ready`.
   - If nothing was given, use `bv --robot-next` or ask only if multiple materially different beads fit.

2. Read the bead context first.
   - Read the target bead with `br show <id>`.
   - Read its parent bead if the target is a phase bead.
   - Read comments on the bead for implementation notes, code snippets, and verification logs.

3. Query `cm` before deeper validation when useful.
   - Look for prior constraints, related lessons, or recurring failure patterns relevant to the bead.
   - Treat `cm` as supporting context only; validate against the current bead, code, and executed checks.
   - If a `cm` lesson materially affects the validation outcome, mention that briefly in the validation comment.

4. Guard the bead before validating closure.
   - Run `rpi-guard` mentally or operationally against the bead first.
   - Use it to identify missing readiness or close-readiness evidence before the deeper validation pass.

5. Discover implementation evidence.
   - Check recent code changes with git.
   - Read the files that the bead says should have changed.
   - Spawn parallel research tasks when needed:
     - `@explore` to compare intended vs actual file changes.
     - `@explore` to verify tests and verification commands.
     - `@beads-analyzer` to extract the exact planned expectations from bead fields/comments.

6. Validate systematically.
   - Compare actual code and behavior to the bead `description`, `design`, `acceptance-criteria`, and `notes`.
   - Run every automated verification command listed on the bead.
   - Treat manual verification as a last resort only when it truly cannot be automated.
   - Call out mismatches between the plan bead and comments on the implementation bead.

7. Record the result in beads.
   - Add a validation comment with `br comments add <id>`.
   - If validation reveals missing or incorrect acceptance checks, update the bead with `br update`.
   - If validation uncovers a reusable systemic lesson, store the distilled takeaway in `cm`.
   - Do not write validation reports to docs.

## Validation Comment Shape

```markdown
## Validation Report

### Status
- Pass / Partial / Fail

### Automated Checks
- `just test` - pass
- `just lint` - fail: [brief reason]

### Matches
- [what aligns with the bead plan]

### Deviations
- [what differs, with `path:line` refs]

### Manual Follow-up
- [only if automation is impossible, with reason]
```

## Rules

- Validate against beads, not docs.
- Prefer short commands: `br show`, `br comments add`, `br update`, `bv --robot-plan`, `bv --robot-related`.
- Query `cm` when it helps surface prior constraints or failure patterns, but validate against present evidence.
- Use guard before deciding a bead is ready to close.
- Run real checks when possible; do not ask the user to do work you can automate.
- If the bead is ambiguous, identify the ambiguity precisely before concluding validation.
