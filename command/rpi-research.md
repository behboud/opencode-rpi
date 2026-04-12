---
description: Research a codebase and store findings in research files
---

# Research a Topic

Answer the user's question by documenting the codebase as it exists today. Store research in `research/`, not in plans or loose docs.

## Artifact Rules

- Write durable investigation notes only in `research/`.
- Link relevant research files from the parent plan in `plans/`.
- Do not write active research artifacts to docs.
- Research files support plans; they do not replace plan files as the execution source of truth.

## User Input
$ARGUMENTS

## Workflow

1. Resolve the target first.
   - If the user gave a slug or file path, use it.
   - Otherwise check `plans/index.md` for active plan streams that match.
   - If research belongs to an active workstream, create `research/<topic>.md` and link it from the parent plan.
   - If no plan stream exists, create a standalone `research/<topic>.md`.
   - Research should always end with a file that captures the output.

2. Query `cm` before doing fresh research.
   - Look for prior related sessions, playbook rules, debugging lessons, and reusable resources.
   - Treat `cm` as supporting context only; live code still wins when it disagrees.
   - If `cm` returns something useful, cite it in the research file.

3. Read any directly mentioned files yourself before delegating.
   - Read full files, not slices.
   - Treat live code as the source of truth.

4. Break the question into research slices and track them with a todo list.

5. Research in parallel.
   - Delegate to `@explore` to find relevant files.
   - Delegate to `@explore` to explain how the current code works.
   - Delegate to `@explore` to find existing patterns and tests.
   - Do web research only if the user explicitly asks for it.

6. Synthesize findings.
   - Document only what exists now.
   - No recommendations, refactors, or root-cause analysis unless the user asked.
   - Include exact file paths and line numbers.
   - Distinguish fresh codebase findings from prior reusable context retrieved from `cm`.

7. Persist the result in a research file.
   - Create or update `research/<topic>.md`.
   - Use the structure below.
   - If the research belongs to an active plan stream, add the research path to the parent plan file.
   - If the work produced reusable cross-task knowledge, store a distilled version in `cm` as well.
   - Do not copy the whole research file into `cm`; store only durable lessons or references worth retrieving later.

## Research File Shape

Use this structure:

```markdown
# Research: <topic>

## Question
[Original request]

## Findings
- [What exists]
- [How components connect]

## Code References
- `path/to/file.ts:42` — [What is here]

## Related Plans
- [plans/slug.md] — [Why it matters]

## Relevant Memory
- [Short note about any useful `cm` hit and why it helped]

## Open Questions
- [Only if something still needs investigation]
```

## Rules

- Never write research results outside `research/`.
- Query `cm` before substantial research and store distilled reusable lessons after substantial research.
- Always do fresh codebase research; plan files are context, not a substitute for reading code.
- `cm` is context, not authority; verify everything important against the current codebase.
- Mention exact file references so the user can verify quickly.
