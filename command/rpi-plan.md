---
description: Plan work in beads using parent and child beads
---

# Plan a Bead Stream

Create a beads-first implementation plan. Do not write plan docs.

## User Input
$ARGUMENTS

## Workflow

1. Resolve the planning bead.
   - If the user gave a bead ID, use it as the parent.
   - Otherwise find a candidate with `bv --robot-search`, `bv --robot-next`, or `bv --robot-triage`.
   - If no bead exists, create a parent bead with a short title using `br create`.
   - Before designing phases, resolve linked context with `bv --robot-related <id>` and identify any linked research beads.

2. Read all mentioned files and relevant bead context before deciding anything.
   - Read files fully.
   - Inspect the bead with `br show`.
   - Read linked research beads before shaping the plan.
   - Use `br show <research-id>` on each linked research bead you plan to rely on.
   - Use related-bead context when helpful.

3. Research in parallel.
   - `@codebase-locator` to find relevant files and tests.
   - `@codebase-analyzer` to understand the current implementation.
   - `@codebase-pattern-finder` to find similar patterns.
   - `@beads-locator` to find related beads and any explicitly relevant docs.
   - `@beads-analyzer` when prior bead history needs extracting.
   - If planning reveals a missing investigation that should be preserved, create a child research bead named `research: <topic>` and put the findings there before continuing.

4. Ask a question only if you are truly blocked after research.
   - Ask exactly one targeted question.
   - Recommend the default you would take.
   - Do not finalize a plan with unresolved scope, security, or UX ambiguity.

5. Write the plan into beads.
   - Parent bead:
     - `br update <id> --description` for the problem and target outcome.
     - `br update <id> --design` for architecture, sequencing, and rationale.
     - `br update <id> --acceptance-criteria` for the overall success checks.
     - `br update <id> --notes` for non-goals, risks, and dependency notes.
     - Reference related research bead IDs in `design` or `notes` when they matter to execution.
    - Child beads:
      - Create one bead per phase with `br create --parent <parent>`.
      - Keep titles short and phase-scoped.
      - Put phase-specific acceptance checks on the child bead.
      - Add default acceptance criteria that require TDD-red-green-refactor for the phase.
      - Add default acceptance criteria that require the phase work to be committed before the bead can close.
      - Put code snippets, example patches, migration notes, and test examples in comments on the bead with `br comments add`.
   - Use `bv --robot-plan` or `bv --robot-related <id>` if you need a dependency sanity check.

6. Keep the plan implementation-ready.
   - Phases must be atomic and testable.
   - Every implementation bead must require TDD-red-green-refactor.
   - Every implementation bead must require a commit before the bead is considered complete.
   - Prefer automated verification and short commands like `just test`.
   - Separate automated checks from truly manual checks.
   - Include explicit non-goals to prevent scope creep.

## Planning Interaction Pattern

Be interactive and skeptical.

- Do not jump straight to a full plan if major ambiguity remains.
- Present your current understanding before locking in the final structure.
- Ask only questions that code and bead research cannot answer.
- If the user corrects your understanding, verify it in code or beads before finalizing the plan.

Use patterns like these during planning:

```text
Based on my research, I understand we need to [accurate summary].

I found:
- [current implementation detail with file:line reference]
- [pattern or constraint to follow]
- [relevant bead or research bead]

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

## How to Structure the Parent Bead

The parent bead should read like the main plan document used to, but split across bead fields.

- `description`
  - brief problem statement
  - why the work matters
  - desired end state
- `design`
  - current state analysis
  - key discoveries with file references
  - implementation approach
  - ordered phase list
  - related research bead IDs
- `acceptance_criteria`
  - include a TDD-red-green-refactor requirement
  - include a committed-work requirement
  - top-level automated checks
  - top-level manual checks only if truly needed
- `notes`
  - non-goals
  - risks
  - rollout or migration notes
  - dependency reminders

Example parent bead content:

```text
description
Improve API error handling so validation failures return consistent structured responses and can be verified automatically.

design
Current state:
- `server/errors.ts:42` formats API errors
- `server/routes/user.ts:88` returns ad hoc validation payloads

Approach:
- introduce one shared response helper
- update route handlers to use it
- add regression tests before refactoring callers

Phases:
1. Add shared error response helper
2. Migrate user routes to the helper
3. Add coverage for validation and unknown errors

Related research:
- `repo-123` research: api error response patterns

acceptance_criteria
- [ ] Red: a failing test or check exists first and captures the intended change
- [ ] Green: the implementation makes the new or updated automated checks pass
- [ ] Refactor: the code is cleaned up with tests still passing
- [ ] `just test-api`
- [ ] `just lint`
- [ ] invalid payloads return the agreed response shape

notes
- Do not redesign the entire error taxonomy
- Watch for frontend consumers expecting the old payload
```

## How to Structure Phase Beads

Each phase bead should be implementation-ready on its own.

- `title`
  - short and action-oriented
- `description`
  - exact change for that phase
  - files or components likely involved
- `acceptance_criteria`
  - require TDD-red-green-refactor
  - require a commit before closure
  - executable checks first
  - observable outcomes second
- `notes`
  - edge cases
  - constraints
  - handoff guidance
- comments on the bead
  - code snippets
  - example patches
  - pseudo-code
  - test cases
  - migration notes

Example phase bead:

```text
title
Add shared API error helper

description
Create a reusable helper for structured API error responses and wire it into the shared error layer used by route handlers.

acceptance_criteria
- [ ] Red: a failing automated test demonstrates the missing helper behavior
- [ ] Green: the implementation makes the new test pass
- [ ] Refactor: the helper and callers are cleaned up with tests still green
- [ ] `just test-api`
- [ ] `just typecheck`
- [ ] `server/errors.ts` exposes the shared helper
- [ ] The work for this bead is committed before closure

notes
- Preserve existing status codes
- Do not migrate all routes in this phase
```

Example comment on the bead:

```ts
function toErrorResponse(code: string, message: string, details?: unknown) {
  return { error: { code, message, details } }
}
```

Add more than one snippet when it helps the implementer understand the intended change.

Example snippets for a phase bead comment:

```ts
export function buildValidationError(field: string, reason: string) {
  return {
    error: {
      code: 'validation_error',
      message: 'Validation failed',
      details: [{ field, reason }],
    },
  }
}
```

```ts
it('returns a structured validation error', async () => {
  const response = await request(app)
    .post('/users')
    .send({ email: 'not-an-email' })

  expect(response.status).toBe(400)
  expect(response.body.error.code).toBe('validation_error')
})
```

```diff
- return res.status(400).json({ message: 'Bad input' })
+ return res.status(400).json(buildValidationError('email', 'invalid format'))
```

Use snippets to capture:

- a likely helper or interface shape
- a representative test case
- an example before/after patch
- a migration sketch when data or config changes are involved

## Success Criteria Guidance

Always separate success criteria into automated verification and truly manual verification.

Every implementation bead should also include these default criteria:

- TDD-red-green-refactor happened for the scoped change
- The work is committed before the bead closes

- Automated verification
  - test commands
  - lint/typecheck/build commands
  - API checks
  - file existence or generated output checks
- Manual verification
  - only when automation is not possible
  - UI review that cannot be captured programmatically
  - hardware or physical-world interaction
  - install/sudo-gated steps

Example acceptance criteria block:

```text
Automated:
- [ ] Red: a failing test or check was added first
- [ ] Green: the new or changed test now passes
- [ ] Refactor: cleanup completed with tests still passing
- [ ] `just test-api`
- [ ] `just lint`
- [ ] `curl localhost:3000/api/foo` returns 200

Manual:
- [ ] Error banner looks correct in the browser

Completion:
- [ ] The work for this bead is committed before closure
```

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

- create a child bead named `research: <topic>`
- put the research question in `description`
- put the distilled answer in `notes`
- put the detailed analysis in comments on the bead
- then reference that bead ID from the plan bead

When research beads already exist:

- resolve them first with `bv --robot-related <id>`
- read them before writing phases or acceptance criteria
- carry the relevant bead IDs into the plan bead so implementation can find them quickly

## Clarification Rules

- Make informed defaults when the choice is low impact.
- Stop and ask when ambiguity materially changes scope, security, privacy, UX, or sequencing.
- Ask one question at a time.
- Do not finalize the plan with unresolved critical ambiguity.
- If more than one detail is unclear, prioritize the highest-impact one first.

## Parent Bead Shape

- `description`: what we are changing and why
- `design`: current state, proposed approach, ordered phase list, related research beads
- `acceptance_criteria`: top-level verification checklist
- `acceptance_criteria`: top-level verification checklist, including TDD expectations
- `notes`: out-of-scope items, risks, dependencies, rollout notes

## Child Bead Shape

- `title`: short phase name
- `description`: exact goal and touched areas
- `acceptance_criteria`: commands and observable outcomes
- `acceptance_criteria`: commands, observable outcomes, TDD-red-green-refactor, and committed-work requirement
- `notes`: constraints and handoff notes
- comments: snippets, examples, pseudo-code, migration/test details

## Rules

- Never write plans to docs.
- Keep commands short: `br show`, `br update`, `br create --parent`, `br comments add`, `bv --robot-plan`.
- Every final plan must live in beads and be executable without a companion markdown file.
