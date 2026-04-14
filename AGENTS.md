# Agent Rules

## Non-Negotiables

- TDD mandatory: red-green-refactor. Never relax tests to pass CI.
- No production logic without automated tests. Flaky tests block merge.

## Tools

### cass-memory (`cm`)

Cross-task memory for reusable lessons. Query before research/planning/implementation.

- Write only distilled knowledge (lessons, constraints, patterns), never transient state.
- Use `cm onboard` for cross-task memory. Use plan files and research files for active-workstream truth. Use `cm context <query> --json` to search.

### cass (`cass`)

Session history search. Use `cass search "<query>"` to find relevant past sessions before repeating research or debugging.

## Artifact Locations

- Plans live in `thoughts/plans/`.
- Research lives in `thoughts/research/`.
- Supporting docs live in `thoughts/docs/`.
- Do not write active plan or research artifacts outside `thoughts/`.

## Plan Rules

- One markdown plan file is the source of truth for an active workstream.
- Put phases, code snippets, test cases, patches, design sketches, and implementation results directly in that one plan file.
- Keep detailed workflow instructions in the command files, not here.

## Branch Workflow

- `main` is integration branch. One short-lived branch per plan stream: `<slug>`.
- Draft PRs from working branch to `main`.

## MCP Agent Mail

- `project_key` = repo absolute path. Reserve narrow file patterns, not whole repo.
- Coordinate early if another agent may touch same files or branch.
