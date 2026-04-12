# Agent Rules

## Non-Negotiables

- TDD mandatory: red-green-refactor. Never relax tests to pass CI.
- No production logic without automated tests. Flaky tests block merge.

## Tools

### cass-memory (`cm`)

Cross-task memory for reusable lessons. Query before research/planning/implementation.

- Write only distilled knowledge (lessons, constraints, patterns) — never transient state.
- Use `cm onboard` for cross-task memory; use plan files and research files for active-workstream truth. `cm context <query> --json` to search.

### cass (`cass`)

Session history search. Use `cass search "<query>"` to find relevant past sessions before repeating research or debugging.

## Artifact Locations

- Plans live in `plans/`.
- Research lives in `research/`.
- Do not write plan or research artifacts to docs.

## Plan Rules

- Parent plans and phase files are the source of truth for active work.
- Put the actual code snippets, test cases, patches, and design sketches directly in the plan files.
- Keep detailed workflow instructions in the command files, not here.

## Branch Workflow

- `main` is integration branch. One short-lived branch per plan stream: `<slug>`.
- Draft PRs from working branch to `main`.

## MCP Agent Mail

- `project_key` = repo absolute path. Reserve narrow file patterns, not whole repo.
- Coordinate early if another agent may touch same files or branch.
