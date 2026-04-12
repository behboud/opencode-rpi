# Agent Rules

## Non-Negotiables

- TDD mandatory: red-green-refactor. Never relax tests to pass CI.
- No production logic without automated tests. Flaky tests block merge.

## Tools

### cass-memory (`cm`)

Cross-task memory for reusable lessons. Query before research/planning/implementation.

- Write only distilled knowledge (lessons, constraints, patterns) — never transient state or bead comments.
- Beads are the system of record for active work; `cm` is for cross-task memory.

### cass (`cass`)

Session history search. Use `cass search "<query>"` to find relevant past sessions before repeating research or debugging.

### beads (`br`)

Issue tracking in `.beads/`, tracked in git.

- Lifecycle: `br ready` → `br update <id> --status=in_progress` → implement → `br close <id> --reason="Completed"` → `br sync --flush-only` before session end.
- Prefer `br show`/`br update` over raw `.beads/` file access.
- Name research beads: `research: <topic>`. Default to reference node (not blocker) unless implementation truly depends on the answer.

### beads triage (`bv`)

- **Never run bare `bv`** — always use `--robot-*` flags. `bv --robot-next` for top step, `bv --robot-triage` for full view.

### RPI on beads

- Do not write RPI artifacts to docs. Use bead fields for durable structure; bead comments for working notes.
- Planning produces child beads, not docs. Code examples go in bead comments.

### Branch workflow

- `main` is integration branch. One short-lived branch per bead stream: `bead-<epic-id>-<goal-slug>`.
- Draft PRs from working branch to `main`.

### MCP Agent Mail

- `project_key` = repo absolute path. Reserve narrow file patterns, not whole repo.
- Coordinate early if another agent may touch same files/branch.