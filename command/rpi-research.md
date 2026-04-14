---
description: Research a codebase and store findings in thoughts/research files
---

# Research a Topic

Document the codebase as it exists today. Store research in `thoughts/research/`.

## User Input
$ARGUMENTS

## Artifact Rules

- Write research only in `thoughts/research/`.
- Link relevant research from `thoughts/plans/` when a plan exists.
- Use `thoughts/docs/` only for stable supporting docs, not active research state.

## Workflow

1. Resolve the target.
   - Use the provided slug or path when present.
   - Otherwise check `thoughts/plans/` for a matching active plan.
   - Create or update `thoughts/research/<topic>.md`.

2. Build context.
   - Query `cm` first.
   - Read any directly mentioned files fully.
   - Treat live code as the source of truth.

3. Research in parallel.
   - Use `@codebase-locator` to find relevant files.
   - Use `@codebase-analyzer` to explain the current implementation.
   - Use `@codebase-pattern-finder` to find existing patterns and tests.
   - Do web research only if the user explicitly asks.

4. Synthesize findings.
   - Document only what exists now.
   - Include exact file paths and line numbers.
   - Distinguish fresh codebase findings from useful `cm` context.

5. Persist the result.
   - Write `thoughts/research/<topic>.md`.
   - Link it from the plan file when relevant.
   - Store only distilled reusable lessons in `cm`.

## Research Shape

```markdown
# Research: <topic>

## Question
[Original request]

## Findings
- [What exists]

## Code References
- `path/to/file.ts:42` - [What is here]

## Related Plans
- `thoughts/plans/<slug>.md` - [Why it matters]

## Relevant Memory
- [Useful `cm` hit]

## Open Questions
- [Only if more investigation is needed]
```

## Rules

- Never write research outside `thoughts/research/`.
- Query `cm` before substantial research and store only distilled reusable lessons after.
- Do fresh codebase research every time.
- Do not recommend changes unless the user asked.
