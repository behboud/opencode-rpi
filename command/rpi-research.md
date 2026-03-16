---
description: Research codebase and store findings in beads
---
# Research in Beads

Answer the user's question by documenting the codebase as it exists today. Store research in beads, not docs.

## User Input
$ARGUMENTS

## Workflow

1. Resolve the target bead first.
   - If the user gave a bead ID, use it.
   - Otherwise look for the active work bead with `bv --robot-search`, `bv --robot-next`, or `bv --robot-triage`.
   - If an active work bead exists, create a child research bead with a short title via `br create --parent <id>` and use that as the research container.
   - If nothing fits, create a standalone research bead with a short title via `br create`.
   - Research should always end with a bead that captures the output.

2. Read any directly mentioned files yourself before delegating.
   - Read full files, not slices.
   - Treat live code as the source of truth.

3. Break the question into research slices and track them with TodoWrite.

4. Research in parallel.
   - Delegate to `@explore` to find relevant files.
   - Delegate to `@explore` to explain how the current code works.
   - Delegate to `@explore` to find existing patterns and tests.
   - Delegate to `@beads-locator` to find related beads and explicitly relevant docs.
   - Delegate to `@beads-analyzer` only when bead history needs deeper extraction.
   - Do web research only if the user explicitly asks for it.

5. Synthesize findings.
   - Document only what exists now.
   - No recommendations, refactors, or root-cause analysis unless the user asked.
   - Include exact file paths and line numbers.
   - Prefer related bead IDs when citing prior work.

6. Persist the result back into beads.
   - Use `br update <id> --description` for the research question and scope.
   - Use `br update <id> --notes` for the short evergreen summary.
   - Use `br comments add <id>` for the full research note and supporting detail.
   - If follow-up research happens later, append another comment to the same bead.
   - Close the research bead when the findings are captured, unless the user wants it left open.

## Research Bead Pattern

- If research belongs to an active workstream, create a child research bead under that work bead.
- Use a short title such as `research: auth refresh flow`.
- Research beads are reference nodes in the graph by default; they should not block other beads unless the work truly depends on them.
- Put the distilled answer in bead fields and the full trail in comments on the bead so the work is not lost.

## Research Comment Shape

Use this structure inside a comment on the bead:

```markdown
## Research Question
[Original request]

## Findings
- [What exists]
- [How components connect]

## Code References
- `path/to/file.ts:42` - [What is here]

## Related Beads
- `repo-123` - [Why it matters]

## Open Questions
- [Only if something still needs investigation]
```

## Rules

- Never write research results to docs.
- Prefer short beads commands: `bv --robot-search`, `bv --robot-next`, `br show`, `br comments add`.
- Always do fresh codebase research; beads are context, not a substitute for reading code.
- Mention exact file references so the user can verify quickly.
