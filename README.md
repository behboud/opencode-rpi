# Opencode RPI Workflow Pack

Custom Opencode commands and agents for research-driven implementation workflows.

`RPI` in this project refers to the workflow style (`Research -> Plan -> Implement`), not Raspberry Pi.

## Overview

This repository provides a command and subagent pack for Opencode with a consistent operating model:
- Human-in-the-loop checkpoints at key transitions
- Phase-based implementation with explicit verification gates
- Automated validation first, manual testing only when automation is genuinely not possible
- Structured documentation in `thoughts/` for research, plans, and PR descriptions

## Repository Structure

```
opencode-rpi/
├── command/
│   ├── rpi-describe-pr.md
│   ├── rpi-implement.md
│   ├── rpi-log-error.md
│   ├── rpi-log-success.md
│   ├── rpi-plan.md
│   ├── rpi-research.md
│   └── rpi-validate.md
└── agent/
    ├── codebase-analyzer.md
    ├── codebase-locator.md
    ├── codebase-pattern-finder.md
    ├── thoughts-analyzer.md
    ├── thoughts-locator.md
    └── web-search-researcher.md
```

## Command Catalog

- `rpi-research`: Documents how the codebase currently works using parallel exploration and concrete references
- `rpi-plan`: Produces phased implementation plans with success criteria and explicit scope boundaries
- `rpi-implement`: Executes one approved plan phase at a time with subagent delegation and review loops
- `rpi-validate`: Verifies implementation against plan criteria and reports alignment/deviations
- `rpi-describe-pr`: Generates and syncs comprehensive PR descriptions from the repository template
- `rpi-log-success`: Captures repeatable patterns behind successful agentic coding sessions
- `rpi-log-error`: Captures failure patterns with focus on prompt, context, and harness quality

## Agent Catalog

- `codebase-locator`: Finds where features and components live
- `codebase-analyzer`: Explains implementation details and code flow
- `codebase-pattern-finder`: Finds existing implementation patterns and concrete examples
- `thoughts-locator`: Finds relevant documents in `thoughts/`
- `thoughts-analyzer`: Extracts high-value decisions and constraints from `thoughts/`
- `web-search-researcher`: Performs web-backed technical research with cited sources

## Installation via Git Subtree

Add this repository as a subtree to your existing Opencode configuration directory (`.opencode`).

### 1) Navigate to your Opencode config directory

```bash
cd /path/to/your/project/.opencode
```

### 2) Add the remote

```bash
git remote add opencode-rpi git@github.com:behboud/opencode-rpi.git
```

### 3) Fetch the remote

```bash
git fetch opencode-rpi
```

### 4) Create the subtree

```bash
git subtree add --prefix=opencode-rpi opencode-rpi main
```

### 5) Reference commands and agents from the subtree

Use paths under `opencode-rpi/` in your Opencode command/agent configuration.

Examples:
- `opencode-rpi/command/rpi-plan.md`
- `opencode-rpi/command/rpi-implement.md`
- `opencode-rpi/agent/codebase-analyzer.md`

## Updating from Upstream

```bash
git fetch opencode-rpi
git subtree pull --prefix=opencode-rpi opencode-rpi main
```

## Removing the Subtree

```bash
git rm -r opencode-rpi
git commit -m "Remove opencode-rpi subtree"
git remote remove opencode-rpi
```

## Workflow Model

### Research -> Plan -> Implement -> Validate

1. Research current behavior and constraints with concrete file references
2. Build a phased plan with explicit success criteria
3. Implement one phase at a time, verify, and pause for human confirmation
4. Validate the implementation against the plan and verification checks

### Verification Rules

- Prefer executable checks (`just`, `make`, test/lint/typecheck/build commands)
- Use tool-based inspection for UI/API/output whenever possible
- Treat manual checks as exceptions for sudo/install/hardware-only scenarios

### Documentation and PR Support

- Plan/research artifacts are written under `thoughts/shared/`
- PR descriptions are generated from `thoughts/shared/pr_description.md`
- PR descriptions are stored as `thoughts/shared/prs/{number}_description.md` and can be synced with `gh pr edit`

## Requirements

- Existing Opencode installation with `.opencode` directory
- Git with subtree support
- SSH access to GitHub for the remote URI
- `gh` CLI for PR-oriented workflows
- Repository-level `thoughts/` setup when using planning/research/PR description workflows

## License

MIT License
