---
description: Implement planned beads with verification, commits, and bead updates
---

# Implement a Bead

Implement one planned bead at a time. Beads are the plan source of truth; do not edit plan docs.

Start by reading `AGENTS.md` and `README.md` carefully so you understand the operating model, TDD requirements, and beads workflow before making changes.

## Bead Input
$ARGUMENTS

## Workflow

1. Resolve the target bead.
   - If the user gave a bead ID, use it.
   - If they gave a parent bead, use `bv --robot-plan` or `br ready` to choose the next ready child.
   - If nothing was given, use `bv --robot-next`.
   - If there are multiple plausible choices, prefer `bv --robot-triage` and pick the most actionable impactful bead.

2. Read the repo operating context before changing code.
   - Read `AGENTS.md` fully.
   - Read `README.md` fully.

3. Read the bead before changing code.
   - Use `br show <id>`.
   - Read comments on the bead if they contain snippets or planning details.
   - If the bead has a parent, read the parent too.
   - Read related research beads before coding when they inform the task.
   - Investigate the codebase enough to understand the local architecture and invariants before editing.
   - Identify the tests, contracts, and existing patterns that define correct behavior.

4. Query `cm` before coding.
   - Look for prior implementation notes, debugging lessons, architecture constraints, and reusable fixes relevant to this bead.
   - Treat `cm` as supporting context; the current bead, tests, and codebase remain authoritative.
   - If a memory meaningfully shapes the implementation, note that briefly in a bead comment so later readers can follow the reasoning.

5. Align the branch with the implementation stream before claiming or coding.
   - Derive the expected branch from the parent epic bead when one exists; use the target bead only when there is no parent.
   - The branch should describe the current epic stream using `bead-<parent-epic-id>-<goal-slug>`.
   - Check the current branch before proceeding with a command like `git branch --show-current`.
   - Check working tree cleanliness with `git status --short` so you know whether a rename or branch switch is safe.
   - Check whether the branch already has a PR with `gh pr view --json number,url,state`.
   - Check whether there are unpushed commits with `git status -sb` or by comparing against the upstream branch.
   - If the current branch already matches the stream, continue.
   - If the branch is mismatched but does not contain conflicting stream work, rename it once or create and switch to the correct branch before proceeding.
   - If the current branch contains committed work for a different epic that is not yet pushed or does not yet have a PR, give the user early feedback to push or open that PR first, then continue the current implementation on a fresh branch for this epic.
   - Prefer `git switch -c <expected-branch>` for a fresh branch or `git branch -m <expected-branch>` when a simple rename preserves the intended stream.
   - Do this before creating new commits so separate bead streams do not get mixed into one review story.

6. Claim and track the bead.
   - Move it to `in_progress` with `br update <id> --claim` or `br update <id> --status=in_progress`.
   - Create a todo list.
   - Keep the bead updated as you go so the graph stays accurate.

7. Guard the bead before implementation.
   - Run `rpi-guard` mentally or operationally against the bead before coding.
   - Confirm the bead is actually `ready_to_implement`.
   - Fix missing acceptance criteria or missing context before writing code.

8. Implement with TDD.
   - TDD-red-green-refactor.
   - No production logic without automated tests.
   - Use up to 3 focused subagents for targeted implementation, debugging, or review. You have to delegate to those agents.
   - Keep work scoped to this bead only.
   - Think about how to improve the code as you go within the bead scope, especially where stronger tests or small refactors make the change safer.
   - Avoid communication purgatory; once the bead is understood, ship the work.

9. Keep bead state current.
   - Add concise progress notes with `br comments add <id>`.
   - If review uncovers missing checks, update the bead acceptance criteria instead of editing a markdown plan.
   - If the plan no longer fits reality, record the mismatch on the bead before asking the user.
   - If implementation produces reusable investigation or follow-up analysis, record it in a related research bead instead of letting it live only in chat.
   - If you are using an agent mailbox or coordination channel, respond to overlapping work or coordination messages promptly.

10. Verify before closing.
   - Confirm the bead acceptance criteria includes TDD-red-green-refactor and a committed-work requirement.
   - Run the bead's automated checks first.
   - Prefer executable verification over manual steps.
   - Record commands and results in a comment on the bead.
   - If a manual check is truly required, stop after automation and ask the user for that confirmation.

11. Guard the bead before closure.
   - Run `rpi-guard` mentally or operationally against the bead before closing it.
   - Confirm the bead is actually `ready_to_close`.
   - If the bead is not close-ready, record the missing evidence and fix it before closure.

12. Finish the bead.
   - Make sure the work is committed before closing the bead.
   - If the work produced durable reusable knowledge, store the distilled lesson in `cm` before ending the session.
   - Do not dump the whole implementation log into `cm`; store only reusable takeaways such as root causes, pitfalls, constraints, or winning patterns.
   - Close it with `br close <id> --reason "Completed"` only after verification is done and the work is committed.
   - If useful, add a short comment on the parent bead noting what completed.
   - Run `br sync --flush-only` before ending the session.

## Implementation Mindset

- Use beads as the source of truth for what to do next.
- Prefer `bv --robot-next` and `bv --robot-triage` over ad hoc guessing when choosing work.
- Investigate architecture and invariants before editing so you do not violate existing contracts.
- Use `cm` to retrieve prior lessons quickly, but verify against the present code and tests.
- Keep each implementation stream on a branch named `bead-<parent-epic-id>-<goal-slug>`, not on an unrelated carry-over branch.
- Keep momentum; do not stall in status chatter when the next useful change is clear.
- Improve the work through TDD-red-green-refactor cycles as you go, not as an afterthought.

## Required Completion Comment

Use a concise comment like this:

```markdown
## Implementation Update
- Completed: [what changed]
- TDD: [how TDD-red-green-refactor was satisfied]
- Tests: `just test`, `just lint`
- Commit: `[sha]` - [message]
- Result: [pass/fail details]
- Follow-ups: [only if any remain]
```

## Rules

- Never track implementation progress in docs.
- Prefer short beads commands: `br show`, `br update --claim`, `br comments add`, `br close`, `bv --robot-next`.
- Query `cm` before substantial implementation and store distilled reusable lessons after substantial implementation.
- Align the branch to the current epic stream before claiming the bead or writing code.
- Never close an implementation bead before its work is committed.
- If blocked, document the mismatch on the bead and ask one targeted question only when needed.
- Stop after one bead/phase unless the user explicitly asks for more.
