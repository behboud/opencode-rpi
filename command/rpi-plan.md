---
description: Create a single implementation-ready plan file with phases, acceptance criteria, and TDD
---

# Plan Work

Create one implementation-ready markdown plan file. Research belongs in `thoughts/research/`.

## User Input
$ARGUMENTS

## Artifact Rules

- Write plan files only in `thoughts/plans/`.
- Write research only in `thoughts/research/`.
- Write supporting docs only in `thoughts/docs/`.
- Treat one plan file as the workstream source of truth.

## Workflow

1. Resolve the target.
   - Use the provided slug or path when present.
   - Otherwise derive a slug from the work description.
   - Check `thoughts/plans/` for an existing matching plan first.

2. Build context.
   - Query `cm` for relevant lessons, constraints, and patterns.
   - Read all mentioned files fully.
   - Read the existing plan if refining one.
   - Read related files in `thoughts/research/` and `thoughts/docs/` when relevant.

3. Research in parallel.
   - Use `@codebase-locator` for relevant files and tests.
   - Use `@codebase-analyzer` for current behavior and constraints.
   - Use `@codebase-pattern-finder` for existing patterns.
   - Create `thoughts/research/<topic>.md` when a durable investigation is needed.

4. Stay interactive.
   - If major ambiguity remains, summarize your understanding and ask one targeted question.
   - Recommend the default you would take.
   - Do not finalize a plan with unresolved critical ambiguity.

5. Write one plan file.
   - Create or update `thoughts/plans/<slug>.md`.
   - Link any relevant `thoughts/research/<topic>.md` or `thoughts/docs/<topic>.md` files.
   - Keep the full workstream in this file, including phase sections and implementation detail.

6. Make it executable.
   - Phases must be atomic and testable.
   - Every implementation phase must include TDD-red-green-refactor.
   - Every implementation phase must include committed-work-before-closure.
   - Prefer automated verification and explicit commands.
   - Include explicit non-goals.

## Planning Style

Use a pattern like this before locking the plan:

```text
Based on my research, I understand we need to [accurate summary].

I found:
- [current implementation detail with file:line reference]
- [pattern or constraint to follow]
- [relevant research file]

What I still need to confirm:
- [single unresolved question]
```

Then propose the phase structure:

```text
1. [Phase name] - [what it accomplishes]
2. [Phase name] - [what it accomplishes]
3. [Phase name] - [what it accomplishes]
```

## Plan Shape

Include these top-level sections:

- `Status`
- `Problem & Outcome`
- `Current State Analysis`
- `Desired End State`
- `What We're Not Doing`
- `Implementation Approach`
- one section per phase
- `Acceptance Criteria`
- `Notes`

Each phase section should include:

- `Status`
- `Description`
- `Acceptance Criteria`
- `Implementation`
- `Notes`
- `Implementation Result`

Minimum expectation:

- The plan contains enough concrete code or test sketching to remove ambiguity.
- Each implementation phase contains at least one concrete snippet or patch fragment and one concrete test sketch.

## Rules

- Never write active plans outside `thoughts/plans/`.
- Never keep research in the plan when it belongs in `thoughts/research/`.
- Keep `thoughts/docs/` supportive, never authoritative.
- Query `cm` before substantial planning and note any material influence.
- Every final plan must be one markdown file with no parent file or index dependency.
