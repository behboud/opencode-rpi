# Opencode RPI Workflow Pack

Custom Opencode commands and agents for a single-plan-file RPI workflow.

`RPI` here means `Research -> Plan -> Implement`, not Raspberry Pi.

## Overview

This repository provides an Opencode command and subagent pack with a consistent operating model:
- Human-in-the-loop checkpoints at key transitions
- Phase-based execution with explicit verification gates
- Automated validation first, manual testing only when automation is genuinely not possible
- Plans in `thoughts/plans/`, research in `thoughts/research/`, and supporting docs in `thoughts/docs/`
- Reusable cross-task lessons retrieved and stored with cass-memory via `cm`

## Repository Structure

```text
opencode-rpi/
├── command/
│   ├── rpi-describe-pr.md
│   ├── rpi-guard.md
│   ├── rpi-implement.md
│   ├── rpi-plan.md
│   ├── rpi-research.md
│   └── rpi-validate.md
└── agents/
    ├── codebase-analyzer.md
    ├── codebase-locator.md
    ├── codebase-pattern-finder.md
    ├── plan-analyzer.md
    ├── plan-locator.md
    └── web-search-researcher.md
```

## Command Catalog

- `rpi-research`: Research the current codebase and store findings in `thoughts/research/<topic>.md`
- `rpi-plan`: Create one implementation-ready plan file in `thoughts/plans/<slug>.md`
- `rpi-guard`: Audit whether a plan phase is ready to plan, implement, or close
- `rpi-implement`: Implement one planned phase at a time and record progress back into the same plan file
- `rpi-validate`: Verify implementation against phase criteria and report alignment or deviations
- `rpi-describe-pr`: Generate PR descriptions from the repo PR template plus plan context

## Agent Catalog

- `codebase-locator`: Finds where features and components live
- `codebase-analyzer`: Explains implementation details and code flow
- `codebase-pattern-finder`: Finds existing implementation patterns and concrete examples
- `plan-locator`: Finds relevant plan files in `thoughts/plans/` and research in `thoughts/research/`
- `plan-analyzer`: Extracts decisions, constraints, and execution details from plan files
- `web-search-researcher`: Performs web-backed technical research with cited sources

## Installation via `curl`

From the current project root, download only `command/` and `agents/` into `.opencode/`:

```bash
mkdir -p .opencode && \
curl -L https://github.com/behboud/opencode-rpi/archive/refs/heads/main.tar.gz \
  | tar -xz -C .opencode --strip-components=1 \
    opencode-rpi-main/command \
    opencode-rpi-main/agents
```

### Clean Refresh

```bash
rm -rf .opencode/command .opencode/agents && \
mkdir -p .opencode && \
curl -L https://github.com/behboud/opencode-rpi/archive/refs/heads/main.tar.gz \
  | tar -xz -C .opencode --strip-components=1 \
    opencode-rpi-main/command \
    opencode-rpi-main/agents
```

## Workflow Model

### Research -> Plan -> Implement -> Validate

1. Research current behavior and constraints with concrete file references
2. Write one implementation-ready plan file in `thoughts/plans/`
3. Store durable research in `thoughts/research/`
4. Keep optional supporting docs in `thoughts/docs/`
5. Guard the phase so readiness gaps are explicit before execution
6. Implement one phase at a time, updating the same plan file as the work progresses
7. Validate the implementation against the plan and verification checks

### Plan File Structure

```text
thoughts/
├── plans/
│   └── <slug>.md
├── research/
│   └── <topic>.md
└── docs/
    └── <topic>.md
```

Each plan file holds the full stream: overview, current-state analysis, desired end state, non-goals, implementation approach, phase sections, acceptance criteria, notes, and implementation results.

### Branch Workflow

- Use one short-lived branch per plan stream, named after the plan slug
- Draft PRs from the working branch to `main`
- Check branch alignment at the start of implementation

### Verification Rules

- Prefer executable checks (`just`, `make`, test/lint/typecheck/build commands)
- Use tool-based inspection for UI/API/output whenever possible
- Treat manual checks as exceptions for sudo/install/hardware-only scenarios

### Cass Memory Role

- `cm` is a supporting memory layer, not a replacement for plan or research files
- Query `cm` before substantial research, planning, or implementation
- Store only distilled reusable knowledge in `cm`
- Keep the active workstream truth in `thoughts/plans/` and `thoughts/research/`

### PR Support

- PR descriptions should be generated from the repo PR template when one exists
- PR summaries should pull scope, rationale, and verification context from plan files when available
- PR descriptions should update the PR body directly with `gh pr edit`

## Requirements

- Existing Opencode installation with `.opencode` directory
- `gh` CLI for PR-oriented workflows
- `cm` from cass-memory for cross-task reusable lessons (optional but recommended)
- `cass` for session history search (optional but recommended)

## License

MIT License
