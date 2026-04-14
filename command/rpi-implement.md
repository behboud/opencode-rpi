---
description: Implement planned phases with verification, commits, and plan updates
---

# Implement a Phase

Implement one planned phase at a time. The plan file is the source of truth.

## User Input
$ARGUMENTS

## Artifact Rules

- Use `thoughts/plans/<slug>.md` as the executable source of truth.
- Use `thoughts/research/<topic>.md` for supporting investigation.
- Keep `thoughts/docs/` supportive, not authoritative.
- If the plan is too thin, strengthen the plan file before coding.

## Workflow

1. Resolve the target phase.
   - Use the provided phase name, plan slug, or file path when present.
   - Otherwise read the plan and pick the next ready phase.
   - Ask only if multiple choices are plausible.

2. Build context.
   - Read `AGENTS.md` and `README.md` fully.
   - Read the full plan file and the target phase section.
   - Read related research or supporting docs when relevant.
   - Query `cm` before coding.
   - Investigate the local architecture, tests, contracts, and patterns before editing.

3. Align the branch.
   - Match the branch to the plan slug before creating commits.
   - Surface mixed-stream committed work early.

4. Start the phase.
   - Mark the phase `in-progress` in the plan file.
   - Create a todo list.
   - Guard the phase before implementation.

5. Implement with TDD.
   - Follow red-green-refactor.
   - No production logic without automated tests.
   - Use up to 3 focused subagents when useful.
   - Keep the work scoped to the phase.

6. Keep the plan current.
   - Update phase status, notes, acceptance criteria, and implementation detail as understanding improves.
   - Record durable follow-up investigation in `thoughts/research/`.

7. Verify and close.
   - Run automated checks first and record results in the plan.
   - Ask the user only for truly manual verification.
   - Guard the phase before closure.
   - Commit the work before marking the phase `done`.
   - Store only distilled reusable lessons in `cm`.

## Completion Note

Add this inside the relevant phase section:

```markdown
## Implementation Result
- Completed: [what changed]
- TDD: [how red-green-refactor was satisfied]
- Tests: `just test`, `just lint`
- Commit: `[sha]` - [message]
- Result: [pass/fail details]
- Follow-ups: [only if any remain]
```

## Rules

- Never track implementation progress outside `thoughts/plans/`.
- Query `cm` before substantial implementation and store only distilled reusable lessons after.
- Never close a phase before its work is committed.
- If blocked, document the mismatch in the plan file and ask one targeted question only when needed.
- Stop after one phase unless the user explicitly asks for more.
