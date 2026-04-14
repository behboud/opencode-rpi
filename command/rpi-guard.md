---
description: Guard phase readiness before work or closure
---

# Guard a Phase

Audit a phase for planning readiness, implementation readiness, or close readiness. Use the plan file as the source of truth.

## User Input
$ARGUMENTS

## Artifact Rules

- Audit against `thoughts/plans/` first.
- Use `thoughts/research/` and `thoughts/docs/` only as supporting context.

## Workflow

1. Resolve the target.
   - Use the provided phase name, plan slug, or file path when present.
   - Otherwise find the most relevant phase in `thoughts/plans/`.

2. Build context.
   - Read the full plan file and the relevant phase section.
   - Read linked research or supporting docs when relevant.
   - Query `cm` if prior lessons can sharpen the audit.

3. Audit readiness.
   - Planning: clear outcome, enough context, no critical ambiguity.
   - Implementation: actionable description, concrete implementation detail, TDD criterion, committed-work criterion, verification commands, resolved blockers.
   - Close: scoped work complete, TDD recorded, checks recorded, commit exists, manual follow-up clearly called out.

4. Report the result.
   - Classify as `not_ready`, `ready_to_plan`, `ready_to_implement`, or `ready_to_close`.
   - List failures first.
   - Then list the shortest fixes.

## Guard Report Shape

```markdown
## Guard Report

### Status
- ready_to_implement

### Passes
- Plan file exists and design is present

### Fails
- Missing committed-work-before-closure acceptance criterion

### Shortest Fixes
- Add `- [ ] The work for this phase is committed before closure`
```

## Rules

- Guard against plan files, not loose docs.
- Query `cm` only to sharpen the audit, never to override current evidence.
- Surface branch mismatch as a workflow risk.
- Do not ignore missing TDD or committed-work requirements.
