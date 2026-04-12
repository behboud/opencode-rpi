---
description: Implement planned phases with verification, commits, and plan updates
---

# Implement a Phase

Implement one planned phase at a time. Phase files are the plan source of truth.

## Artifact Rules

- Use `plans/<slug>.md` and `plans/<slug>/phase-*.md` as the executable source of truth.
- Use `research/<topic>.md` for supporting investigation only.
- Do not move active execution details into docs.
- If the plan is too thin to execute safely, strengthen the plan file itself with concrete snippets and tests before coding.

Start by reading `AGENTS.md` and `README.md` carefully so you understand the operating model and TDD requirements before making changes.

## Phase Input
$ARGUMENTS

## Workflow

1. Resolve the target phase.
   - If the user gave a phase slug or file path, use it.
   - If they gave a parent plan, read `plans/<slug>.md` and choose the next ready phase from the phase links.
   - If nothing was given, check `plans/index.md` for active plan streams, then find the next ready phase.
   - If there are multiple plausible choices, ask which one.

2. Read the repo operating context before changing code.
   - Read `AGENTS.md` fully.
   - Read `README.md` fully.

3. Read the phase file before changing code.
   - Read the full phase file from `plans/<slug>/phase-NN-*.md`.
   - If the phase has a parent plan, read that too.
   - Read related research files from `research/` when they inform the task.
   - Investigate the codebase enough to understand the local architecture and invariants before editing.
   - Identify the tests, contracts, and existing patterns that define correct behavior.

4. Query `cm` before coding.
   - Look for prior implementation notes, debugging lessons, architecture constraints, and reusable fixes relevant to this phase.
   - Treat `cm` as supporting context; the current phase file, tests, and codebase remain authoritative.
   - If a memory meaningfully shapes the implementation, note that briefly in the phase file notes.

5. Align the branch with the implementation stream before coding.
   - The branch should describe the current plan stream using the plan slug.
   - Check the current branch with `git branch --show-current`.
   - Check working tree cleanliness with `git status --short`.
   - If the current branch already matches the stream, continue.
   - If the branch is mismatched but clean, rename it once or create and switch to the correct branch.
   - If the current branch contains committed work for a different stream that is not yet pushed or PR'd, surface that early and ask the user to push or open that PR first.
   - Prefer `git switch -c <branch>` for a fresh branch or `git branch -m <branch>` when a simple rename preserves the intended stream.
   - Do this before creating new commits so separate streams do not get mixed into one review story.

6. Mark the phase in-progress.
   - Update the Status field in the phase file to `in-progress`.
   - Create a todo list for the implementation steps.
   - Keep the phase file updated as you go.

7. Guard the phase before implementation.
   - Run `rpi-guard` mentally or operationally against the phase file before coding.
   - Confirm the phase is actually `ready_to_implement`.
   - Fix missing acceptance criteria or missing context before writing code.

8. Implement with TDD.
   - TDD-red-green-refactor.
   - No production logic without automated tests.
   - Use up to 3 focused subagents for targeted implementation, debugging, or review.
   - Keep work scoped to this phase only.
   - Think about how to improve the code as you go within the phase scope, especially where stronger tests or small refactors make the change safer.
   - Avoid communication purgatory; once the phase is understood, ship the work.

9. Keep the phase file current.
   - Update the phase file as understanding evolves.
   - If review uncovers missing checks, update the acceptance criteria in the phase file.
   - If implementation reveals the plan needs a better code sketch, patch shape, or test case, add that directly to the phase file.
   - If the plan no longer fits reality, record the mismatch in the phase file before asking the user.
   - If implementation produces reusable investigation or follow-up analysis, record it in a related file under `research/` instead of letting it live only in chat.
   - If you are using an agent mailbox or coordination channel, respond to overlapping work or coordination messages promptly.

10. Verify before closing.
    - Confirm the phase acceptance criteria includes TDD-red-green-refactor and a committed-work requirement.
    - Run the phase's automated checks first.
    - Prefer executable verification over manual steps.
    - Record commands and results in the phase file notes.
    - If a manual check is truly required, stop after automation and ask the user for confirmation.

11. Guard the phase before closure.
    - Run `rpi-guard` mentally or operationally against the phase file before marking it done.
    - Confirm the phase is actually `ready_to_close`.
    - If the phase is not close-ready, record the missing evidence and fix it before closure.

12. Finish the phase.
    - Make sure the work is committed before marking the phase done.
    - If the work produced durable reusable knowledge, store the distilled lesson in `cm` before ending the session.
    - Do not dump the whole implementation log into `cm`; store only reusable takeaways.
    - Update the phase file Status to `done`.
    - Update the parent plan file to reflect the completed phase.
    - Commit the plan file changes alongside the code changes.

## Implementation Mindset

- Use phase files as the source of truth for what to do next.
- Expect the needed code snippets, tests, and design fragments to already be in the plan files; improve them there if they are too thin.
- Investigate architecture and invariants before editing so you do not violate existing contracts.
- Use `cm` to retrieve prior lessons quickly, but verify against the present code and tests.
- Keep each implementation stream on a branch named after the plan slug, not on an unrelated carry-over branch.
- Keep momentum; do not stall in status chatter when the next useful change is clear.
- Improve the work through TDD-red-green-refactor cycles as you go, not as an afterthought.

## Required Completion Note

Update the phase file with a concise completion note:

```markdown
## Implementation Result
- Completed: [what changed]
- TDD: [how TDD-red-green-refactor was satisfied]
- Tests: `just test`, `just lint`
- Commit: `[sha]` — [message]
- Result: [pass/fail details]
- Follow-ups: [only if any remain]
```

## Rules

- Never track implementation progress outside `plans/`.
- Query `cm` before substantial implementation and store distilled reusable lessons after substantial implementation.
- Align the branch to the current plan stream before writing code.
- Never close an implementation phase before its work is committed.
- If blocked, document the mismatch in the phase file and ask one targeted question only when needed.
- Stop after one phase unless the user explicitly asks for more.
