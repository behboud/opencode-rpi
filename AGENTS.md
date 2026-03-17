
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

`cm` is the reusable memory layer. Use it to retrieve prior sessions, rules, patterns, and lessons across tasks and repos.

Rules:
- Query `cm` before meaningful research, planning, or implementation.
- Write back only distilled reusable knowledge: debugging lessons, architecture constraints, durable research takeaways, repeated failure modes, and winning patterns.
- Do not mirror whole bead comments, validation logs, or transient status into `cm`.
- Beads remain the system of record for active workstream state.

The `cm onboard` command guides you through analyzing historical sessions and extracting valuable rules.

Common commands: `cm onboard status`, `cm onboard sample --fill-gaps`, `cm onboard read <session> --template`, `cm playbook add ...`, `cm onboard mark-done <session>`

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

Beads are the system of record for `research`, `plan`, and `implement`. `cm` is the reusable memory layer around that flow.

Rules:
- Do not write RPI artifacts to docs.
- Use bead fields for durable structure and comments on the bead for detailed working notes.
- Use `cm` for reusable cross-task memory; use beads for active-workstream truth.
- Keep commands short and standard: `bv --robot-next`, `bv --robot-search`, `br show`, `br update`, `br create --parent`, `br comments add`, `br close`.

Recommended mapping:
- Research -> query `cm` for relevant prior work, resolve or create a research bead, store the question in fields and the full findings in comments on the bead, then add distilled reusable takeaways to `cm`.
- Plan -> query `cm` for prior constraints, patterns, and related lessons, store overview/design/acceptance in the parent bead, then create child beads for phases with default TDD and committed-work acceptance criteria.
- Guard -> audit a bead after planning and again before closure, then fix missing workflow requirements.
- Implement -> query `cm` for prior implementation lessons before coding, work one ready bead at a time, record progress/tests in comments, commit the work, then store durable new lessons in `cm` before closing the bead.
- Validate -> validate against current bead/code/checks, use `cm` only for supporting context, and store systemic lessons when validation reveals one.

Storage rubric:
- bead `description`: problem, scope, expected outcome
- bead `design`: approach, sequencing, architecture notes
- bead `acceptance-criteria`: executable checks and observable outcomes; implementation beads must include TDD and committed-work requirements
- bead `notes`: risks, non-goals, rollout, dependencies
- bead comments: research trail, snippets, pseudo-code, migration notes, verification results
- `cm`: reusable lessons, constraints, patterns, heuristics, references worth retrieving later

Practical guidance:
- If a user gives a bead ID, start there with `br show <id>`.
- If no bead is given, use `bv --robot-next` or `bv --robot-search` to find the right starting point.
- If no suitable bead exists, create one with a short title via `br create`.
- Before deep research, planning, or implementation, check `cm` for relevant prior work or stored lessons.
- If the task is research for an active workstream, create a child research bead with `br create --parent` and put the findings there.
- Name research beads with the convention `research: <topic>` so they are easy to spot in the graph.
- Research beads should be reference nodes by default, not blockers, unless the implementation truly depends on the answer.
- If planning produces phases, create child beads instead of docs.
- If you include code examples in a plan, put them in a comment on the relevant bead.
- When `cm` materially influences research, planning, or implementation, mention that briefly in the bead comment or notes so later readers can follow the reasoning trail.
- After meaningful work, add only distilled reusable knowledge to `cm`; do not mirror whole bead comments or dump transient status updates there.
- For PR descriptions, use beads as the source for scope, rationale, and verification history, then update the PR body directly.

### Branch workflow

Keep the git workflow close to normal: `main` is the integration branch, and agents should usually work on one short-lived branch per coherent stream of beads.

Guidance:
- Prefer one short-lived branch per coherent bead stream, not per bead.
- A good stream usually shares subsystem, files/tests, and review story.
- Rename a mismatched branch once instead of creating a separate personal `dev` branch.
- Prefer draft PRs from the working branch directly to `main`.
- Start a fresh branch when the next bead stream would make the PR mixed or hard to review.
- Keep commits small and coherent; coordinate overlapping branch/file work with agent mail.

### Beads triage sidecar: `bv`

Use `bv` for deterministic triage, prioritization, and dependency-aware planning over `.beads/beads.jsonl`.

Critical rule: never run bare `bv`; always use `--robot-*` flags.

Default commands: `bv --robot-triage`, `bv --robot-next`

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

Core tools: `macro_start_session`, `file_reservation_paths`, `send_message`, `fetch_inbox`, `acknowledge_message`, `release_file_reservations`

Guidance:
- Use this repo's absolute path as the `project_key`.
- Reserve narrow file patterns, not the whole repo.
- Prefer one shared `thread_id` per bead, ticket, or feature stream.
- If another agent may touch the same files or branch, coordinate early.
- For cross-repo work, either share one project key or link agents and keep the same `thread_id`.

Common pitfalls:
- If you see `from_agent not registered`, start or register the agent first.
- If you hit a reservation conflict, narrow the pattern or coordinate with the holder.
