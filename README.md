# Opencode RPI Workflow Pack

Custom Opencode commands and agents for plan-file-first RPI workflows.

`RPI` in this project refers to the workflow style (`Research -> Plan -> Implement`), not Raspberry Pi.

## Overview

This repository provides a command and subagent pack for Opencode with a consistent operating model:
- Human-in-the-loop checkpoints at key transitions
- Phase-based implementation with explicit verification gates
- Automated validation first, manual testing only when automation is genuinely not possible
- Plans stored in `plans/` and research stored in `research/`, both tracked in git
- Reusable cross-task lessons retrieved and stored with cass-memory via `cm`

## Repository Structure

```
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

- `rpi-research`: Researches the current codebase and stores findings in `research/<topic>.md`
- `rpi-plan`: Writes parent plans and phase files with acceptance criteria and implementation detail
- `rpi-guard`: Audits whether a phase is ready to plan, implement, or close
- `rpi-implement`: Implements one planned phase at a time and records progress back into phase files
- `rpi-validate`: Verifies implementation against phase criteria and reports alignment/deviations
- `rpi-describe-pr`: Generates PR descriptions from the repo PR template plus plan context

## Agent Catalog

- `codebase-locator`: Finds where features and components live
- `codebase-analyzer`: Explains implementation details and code flow
- `codebase-pattern-finder`: Finds existing implementation patterns and concrete examples
- `plan-locator`: Finds relevant plan files in `plans/` and research in `research/`
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
2. Write the plan into a parent plan file plus child phase files in `plans/`
3. Store durable research in `research/`
4. Guard the phase so readiness gaps are explicit before execution
5. Implement one phase at a time, verifying and guarding again before closure
6. Validate the implementation against the plan and verification checks

### Plan File Structure

```
plans/
├── index.md              # Lists all plan streams and their status
├── <slug>.md             # Parent plan (overview, design, acceptance)
└── <slug>/               # Phase files for this plan stream
    ├── phase-01-*.md
    └── phase-02-*.md
```

```
research/
├── index.md              # Optional list of research topics
└── <topic>.md            # Durable investigation notes
```

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
- Keep the active workstream truth in plan files and research files

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
