
# Agents Guide (Condensed Constitution)

## Non-Negotiables

- Test-driven delivery is mandatory: TDD-red-green-refactor.
- Do not change/relax tests to "make CI pass". If behavior changes: update spec -> update tests (failing) -> update implementation.
- No production logic without automated tests (unit/contract/integration/e2e as appropriate). Tests must be deterministic; flaky tests block merge.
- Treat the test suite and verifier as the exact specification; never rely on assumptions.

## Root Cause Discipline (5 Whys)

- For any non-trivial bug/perf/workflow issue: write a Why1..WhyN chain until a first-principle/actionable root cause.
- Fix the deepest cause, not symptoms.
- Add/strengthen a systemic guard in the same change (test, lint rule, contract, runtime check).

## Duplication Prevention / Codebase Awareness

- Prefer extension/refactor over parallel abstractions.
- If duplication is intentional, think of a convergence/de-dup plan, and add it to tasks list.
- PRs must include evidence of the search (tools/queries or delegated agent output).

## Clarify Ambiguity Explicitly

- If requirements/contracts/targets/constraints are unclear: stop and ask.
- Do not finalize tests/implementation with unresolved critical ambiguities.
- Use tests, logs, and harness output to decide your next move.
- Keep context clean. Expect only minimal stdout. Read log files, which will contain machine-parsable entries like ERROR: <reason> and pre-computed summaries.
- Use time efficiently. Use incremental progress signals and the --fast sampling mode to detect regressions instead of running full test suites unnecessarily.

## Agent Harness tools

### Memory System: cass-memory

The Cass Memory System (cm) is a tool for giving agents an effective memory based on the ability to quickly search across previous coding agent sessions across an array of different coding agent tools (e.g., OpenCode, Codex, etc) and projects (and even across multiple machines, optionally) and then reflect on what they find and learn in new sessions to draw out useful lessons and takeaways; these lessons are then stored and can be queried and retrieved later, much like how human memory works.

The `cm onboard` command guides you through analyzing historical sessions and extracting valuable rules.

#### Quick Start

```bash
# 1. Check status and see recommendations
cm onboard status

# 2. Get sessions to analyze (filtered by gaps in your playbook)
cm onboard sample --fill-gaps

# 3. Read a session with rich context
cm onboard read /path/to/session.jsonl --template

# 4. Add extracted rules (one at a time or batch)
cm playbook add "Your rule content" --category "debugging"
# Or batch add:
cm playbook add --file rules.json

# 5. Mark session as processed
cm onboard mark-done /path/to/session.jsonl
```

---

### Beads workflow: `br`

This project uses `beads_rust` for issue tracking. Issues live in `.beads/` and are tracked in git.

Use `br` for work selection and issue lifecycle:

```bash
br ready
br show <id>
br update <id> --status=in_progress
br close <id> --reason="Completed"
br sync --flush-only
```

Default workflow:
1. Find ready work with `br ready`
2. Mark the selected issue `in_progress`
3. Implement the task
4. Close the issue when done
5. Run `br sync --flush-only` before ending the session

Guidance:
- Prefer `br` over manually reading `.beads` files.
- For any request about beads, triage with `bv --robot-*` and inspect/update with `br` before using raw file search or reading `.beads` directly.
- Keep issue state current as work progresses.
- Sync beads changes before finishing the session.

### RPI on beads

For `research`, `plan`, and `implement` workflows, beads are the system of record.

Rules:
- Do not write RPI artifacts to docs.
- Use bead fields for durable structure and comments on the bead for detailed working notes.
- Keep commands short and standard: `bv --robot-next`, `bv --robot-search`, `br show`, `br update`, `br create --parent`, `br comments add`, `br close`.

Recommended mapping:
- Research -> resolve or create a research bead, then store the question in fields and the full findings in comments on the bead.
- Plan -> store overview/design/acceptance in the parent bead, then create child beads for phases with default TDD and committed-work acceptance criteria.
- Guard -> audit a bead after planning and again before closure, then fix missing workflow requirements.
- Implement -> work one ready bead at a time, record progress/tests in comments, commit the work, then close the bead.

Where information should live:
- `description`: problem statement, scope, expected outcome
- `design`: implementation approach, sequencing, architecture notes
- `acceptance-criteria`: executable checks and observable outcomes, including TDD-red-green-refactor and committed-work requirements for implementation beads
- `notes`: risks, non-goals, rollout notes, dependency reminders
- comments: research summaries, code snippets, pseudo-code, migration notes, verification results

Practical guidance:
- If a user gives a bead ID, start there with `br show <id>`.
- If no bead is given, use `bv --robot-next` or `bv --robot-search` to find the right starting point.
- If no suitable bead exists, create one with a short title via `br create`.
- If the task is research for an active workstream, create a child research bead with `br create --parent` and put the findings there.
- Name research beads with the convention `research: <topic>` so they are easy to spot in the graph.
- Research beads should be reference nodes by default, not blockers, unless the implementation truly depends on the answer.
- If planning produces phases, create child beads instead of docs.
- If you include code examples in a plan, put them in a comment on the relevant bead.
- For PR descriptions, use beads as the source for scope, rationale, and verification history, then update the PR body directly.

### Branch workflow

Keep the git workflow close to normal: `main` is the integration branch, and agents should usually work on one short-lived branch per coherent stream of beads.

Guidance:
- Before starting a new branch, look for a meaningful stream of related beads rather than treating every single bead as its own branch by default.
- A meaningful stream usually means the beads touch the same subsystem, the same files or tests, and belong to one reviewable story.
- Good candidates for one branch are a parent bead plus tightly related child beads, or a small cluster of beads that share the same contract or architecture boundary.
- If a branch name no longer matches the work, rename it once to something clearer rather than creating an extra personal `dev` branch.
- Prefer draft PRs from the working branch directly to `main` for visibility and review. Do not add an extra `dev` or personal integration branch unless humans already use that workflow in this repo.
- Start a fresh branch from updated `main` when a new bead stream moves into a different subsystem or would make the PR feel mixed or hard to review.
- Keep commits small and coherent, and merge branches back to `main` in reviewable slices rather than building a long-lived mega-branch.
- When multiple agents are active, coordinate branch intent in comments or agent mail if the bead stream or file ownership could overlap.

### Beads triage sidecar: `bv`

Use `bv` for deterministic triage, prioritization, and dependency-aware planning over `.beads/beads.jsonl`.

Critical rule:
- Never run bare `bv`
- Always use `--robot-*` flags

Default commands:

```bash
bv --robot-triage
bv --robot-next
```

Guidance:
- Start with `bv --robot-triage` when you need to know what to work on or what unblocks the most value.
- Use `bv --robot-next` when you only need the top recommended next step.
- Use `bv` for planning and triage, not for agent-to-agent coordination.

### MCP Agent Mail: coordination for multi-agent workflows

Use MCP Agent Mail when multiple agents may touch the same repo or related repos.

Default workflow:
1. Start a session for this repo and fetch inbox context.
2. Reserve files before editing.
3. Send a message or thread update if work overlaps another agent.
4. Check inbox during longer tasks and acknowledge messages that require it.
5. Release reservations when done.

Core tools:
- `macro_start_session` to register and fetch inbox.
- `file_reservation_paths` to reserve files or globs before editing.
- `send_message` for coordination updates.
- `fetch_inbox` to check for new messages.
- `acknowledge_message` for ack-required mail.
- `release_file_reservations` when finished.

Guidance:
- Use this repo's absolute path as the `project_key`.
- Reserve narrow file patterns, not the whole repo.
- Prefer one shared `thread_id` per bead, ticket, or feature stream.
- If another agent may touch the same files or branch, coordinate early.
- For cross-repo work, either share one project key or link agents and keep the same `thread_id`.

Common pitfalls:
- If you see `from_agent not registered`, start or register the agent first.
- If you hit a reservation conflict, narrow the pattern or coordinate with the holder.
