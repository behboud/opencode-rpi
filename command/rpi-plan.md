---
description: Plan work in phase files with acceptance criteria and TDD
---

# Plan Work

Create a plans-first implementation plan using markdown phase files. Research belongs in `research/`.

## Artifact Rules

- Write parent plans and phase files only in `plans/`.
- Write durable investigation notes only in `research/`.
- Do not write active planning or research artifacts to docs.
- Treat the parent plan and phase files as the source of truth for the workstream.

## User Input
$ARGUMENTS

## Workflow

1. Resolve the planning target.
   - If the user gave a slug or path, use it.
   - Otherwise propose a slug derived from the work description (e.g., `api-error-handling`).
   - Check `plans/index.md` for existing plan streams that match.

2. Query `cm` before shaping the plan.
   - Look for related prior plans, architecture constraints, debugging lessons, and reusable implementation patterns.
   - Treat `cm` as supporting memory, not as a substitute for reading the current codebase.
   - If a `cm` resource materially affects the plan, reference that influence in the plan notes.

3. Read all mentioned files and relevant context before deciding anything.
   - Read files fully.
   - Read existing plan phase files if this is a refinement.
   - Read related research files in `research/` before shaping phases.

4. Research in parallel.
   - `@codebase-locator` to find relevant files and tests.
   - `@codebase-analyzer` to understand the current implementation.
   - `@codebase-pattern-finder` to find similar patterns.
   - If planning reveals a missing investigation, create a `research/<topic>.md` file and put findings there.

5. Ask a question only if you are truly blocked after research.
   - Ask exactly one targeted question.
   - Recommend the default you would take.
   - Do not finalize a plan with unresolved critical ambiguity.

6. Write the plan into files.
   - Create `plans/<slug>.md` for the parent plan.
   - Create `plans/<slug>/` directory with one phase file per phase.
   - Update `plans/index.md` to list the new plan stream.
   - If the plan depends on durable investigation, create or update `research/<topic>.md` and link it from the parent plan.
   - Parent plan file contains: Status, Problem & Outcome, Design with phase links, Acceptance Criteria, Notes.
   - Parent plan Design must include concrete implementation material such as interfaces, pseudo-code, example patches, or test sketches that express the intended change.
   - Each phase file contains: Status, Description, Acceptance Criteria, Implementation, Notes.
   - Each phase file Implementation section must include actual code snippets, test cases, or patch fragments for that phase.
   - Every implementation phase must include TDD-red-green-refactor in acceptance criteria.
   - Every implementation phase must include committed-work-before-closure in acceptance criteria.

7. Keep the plan implementation-ready.
   - Phases must be atomic and testable.
   - Every implementation phase must require TDD-red-green-refactor.
   - Every implementation phase must require a commit before the phase is considered complete.
   - Prefer automated verification and short commands like `just test`.
   - Separate automated checks from truly manual checks.
   - Include explicit non-goals to prevent scope creep.
   - Surface reusable constraints or past pitfalls from `cm` when they reduce implementation risk.

## Planning Interaction Pattern

Be interactive and skeptical.

- Do not jump straight to a full plan if major ambiguity remains.
- Present your current understanding before locking in the final structure.
- Ask only questions that code and research cannot answer.
- If the user corrects your understanding, verify it in code before finalizing.

Use patterns like these during planning:

```text
Based on my research, I understand we need to [accurate summary].

I found:
- [current implementation detail with file:line reference]
- [pattern or constraint to follow]
- [relevant research file]

What I still need to confirm:
- [single unresolved question]
```

```text
Here is the proposed phase structure:

1. [Phase name] - [what it accomplishes]
2. [Phase name] - [what it accomplishes]
3. [Phase name] - [what it accomplishes]
```

Only ask the user to decide when the choice materially changes scope, security, UX, or delivery order.

## How to Structure the Parent Plan File

The parent plan file should contain everything a plan index needs plus phase links.

- `Status` — current state of the whole stream
- `Problem & Outcome` — brief problem statement, why the work matters, desired end state
- `Design` — current state analysis, key discoveries with file references, implementation approach, ordered phase list with links, related research files, relevant `cm` takeaways, and concrete code or test sketches that make the intended change executable
- `Acceptance Criteria` — top-level verification checklist including TDD and committed-work requirements
- `Notes` — non-goals, risks, rollout or migration notes, dependency reminders

## How to Structure Phase Files

Each phase file should be implementation-ready on its own.

- `Status` — ready, in-progress, blocked, or done
- `Description` — exact change for this phase, files or components involved
- `Acceptance Criteria` — require TDD-red-green-refactor, require a commit before closure, executable checks first, observable outcomes second
- `Implementation` — required section containing code snippets, example patches, pseudo-code, interfaces, test cases, or migration notes — all inline
- `Notes` — edge cases, constraints, handoff guidance

Minimum expectation:

- Parent plan: enough concrete code or test sketching to make the design unambiguous
- Each implementation phase: at least one concrete snippet or patch fragment and at least one concrete test case or test sketch

Use a shape like this inside each phase file:

```markdown
## Implementation

### Code Sketch
```text
// intended API, helper, or structural change
```

### Test Sketch
```text
// failing or expected test shape for this phase
```

### Patch Shape
```diff
- old behavior
+ new behavior
```
```

Use a shape like this for the parent plan file:

```markdown
# <Plan Title>

## Status: ready

## Problem & Outcome
[why this work exists and what done looks like]

## Design
Current state:
- `path/to/file.ext:42` - [relevant behavior]

Approach:
- [implementation approach]

Key interface or sketch:
```text
// shared type, helper, or contract that anchors the design
```

Phase order:
1. [Phase 1](./<slug>/phase-01-*.md)
2. [Phase 2](./<slug>/phase-02-*.md)

Related research:
- `research/<topic>.md`

## Acceptance Criteria
- [ ] top-level success check

## Notes
- [non-goals, risks, migration notes]
```

## Success Criteria Guidance

Always separate success criteria into automated verification and truly manual verification.

Every implementation phase should include these default criteria:

- TDD-red-green-refactor happened for the scoped change
- The work is committed before the phase is marked done

Automated verification:
- test commands
- lint/typecheck/build commands
- API checks
- file existence or generated output checks

Manual verification:
- only when automation is not possible
- UI review that cannot be captured programmatically
- hardware or physical-world interaction
- install/sudo-gated steps

## Common Planning Patterns

For database changes:
- schema or migration first
- data access next
- business logic after that
- API or UI consumers last

For new features:
- research existing patterns first
- define the data model
- implement backend logic
- expose the API
- add UI last

For refactors:
- document current behavior first
- add safety tests
- plan incremental changes
- preserve compatibility unless the user asked to break it

## Research Escalation Pattern

When you discover a question that deserves durable investigation:

- create `research/<topic>.md`
- put the research question and scope at the top
- put the distilled answer in a Findings section
- put detailed analysis in a dedicated section
- reference that research file from the plan notes

When relevant `cm` memory exists:

- use it to sharpen the plan, not to skip current-code analysis
- reference the `cm` insight briefly in the plan notes

## Clarification Rules

- Make informed defaults when the choice is low impact.
- Stop and ask when ambiguity materially changes scope, security, privacy, UX, or sequencing.
- Ask one question at a time.
- Do not finalize the plan with unresolved critical ambiguity.

## Rules

- Never write plans to docs. They live in `plans/`.
- Never store research in `plans/`; use `research/`.
- Query `cm` before substantial planning and reference any material memory influence in the plan.
- Every final plan must be in markdown phase files and be executable without a companion system.
- Plan files must carry the actual implementation ideas directly, not just summarize them.
