# Opencode RPI Workflow Pack

Custom Opencode commands and agents for beads-first RPI workflows.

`RPI` in this project refers to the workflow style (`Research -> Plan -> Implement`), not Raspberry Pi.

## Overview

This repository provides a command and subagent pack for Opencode with a consistent operating model:
- Human-in-the-loop checkpoints at key transitions
- Phase-based implementation with explicit verification gates
- Automated validation first, manual testing only when automation is genuinely not possible
- Research, plans, and implementation notes stored in beads via `br`/`bv`
- Reusable cross-task lessons and prior-session resources retrieved and stored with cass-memory via `cm`

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
    ├── beads-analyzer.md
    ├── beads-locator.md
    ├── codebase-analyzer.md
    ├── codebase-locator.md
    ├── codebase-pattern-finder.md
    └── web-search-researcher.md
```

## Command Catalog

- `rpi-research`: Researches the current codebase and stores findings in comments on the bead
- `rpi-plan`: Writes parent/child bead plans with acceptance criteria and phase breakdowns
- `rpi-guard`: Audits whether a bead is ready to plan, implement, or close
- `rpi-implement`: Implements one planned bead at a time and records progress back into beads
- `rpi-validate`: Verifies implementation against bead criteria and reports alignment/deviations
- `rpi-describe-pr`: Generates PR descriptions from the repo PR template plus bead context

## Agent Catalog

- `codebase-locator`: Finds where features and components live
- `codebase-analyzer`: Explains implementation details and code flow
- `codebase-pattern-finder`: Finds existing implementation patterns and concrete examples
- `beads-locator`: Finds relevant beads, related work, and docs when needed
- `beads-analyzer`: Extracts decisions, constraints, and execution details from beads
- `web-search-researcher`: Performs web-backed technical research with cited sources

## Installation via `curl`

### Prereqs

Install the required CLIs before using this pack:

```bash
# GitHub CLI
# https://github.com/cli/cli

# beads_rust (`br`)
# https://github.com/Dicklesworthstone/beads_rust

# beads_viewer (`bv`)
# https://github.com/Dicklesworthstone/beads_viewer
```

From the current project root, download only `command/` and `agents/` into `.opencode/`:

```bash
mkdir -p .opencode && \
curl -L https://github.com/behboud/opencode-rpi/archive/refs/heads/main.tar.gz \
  | tar -xz -C .opencode --strip-components=1 \
    opencode-rpi-main/command \
    opencode-rpi-main/agents
```

This installs only the files you need and skips repo docs and git setup.

### Clean Refresh

If you want updates to replace the existing files cleanly, remove the current directories first:

```bash
rm -rf .opencode/command .opencode/agents && \
mkdir -p .opencode && \
curl -L https://github.com/behboud/opencode-rpi/archive/refs/heads/main.tar.gz \
  | tar -xz -C .opencode --strip-components=1 \
    opencode-rpi-main/command \
    opencode-rpi-main/agents
```

### Result

After extraction, your project will have:

```text
.opencode/
├── command/
└── agents/
```

## Updating

Use the clean refresh command to replace the files from `main`:

```bash
rm -rf .opencode/command .opencode/agents && \
mkdir -p .opencode && \
curl -L https://github.com/behboud/opencode-rpi/archive/refs/heads/main.tar.gz \
  | tar -xz -C .opencode --strip-components=1 \
    opencode-rpi-main/command \
    opencode-rpi-main/agents
```

## Removal

```bash
rm -rf .opencode/command .opencode/agents
```

## Workflow Model

### Research -> Plan -> Implement -> Validate

1. Research current behavior and constraints with concrete file references
2. Write the plan into a parent bead plus child phase beads
3. Guard the bead so readiness gaps are explicit before execution
4. Implement one bead/phase at a time, verify, and guard again before closure
5. Validate the implementation against the plan and verification checks

### Verification Rules

- Prefer executable checks (`just`, `make`, test/lint/typecheck/build commands)
- Use tool-based inspection for UI/API/output whenever possible
- Treat manual checks as exceptions for sudo/install/hardware-only scenarios

### Beads-First Artifacts

- Research, planning, and implementation artifacts live in beads
- Research should usually live in dedicated research beads so it stays in the bead graph as reference
- Name research beads with the convention `research: <topic>`
- Parent beads hold the overview, design, and top-level acceptance criteria
- Child beads hold phase-level scope and verification
- Code snippets, pseudo-code, migration notes, and research updates belong in comments on the bead

### Cass Memory Role

- `cm` is a supporting memory layer, not a replacement for beads
- Query `cm` before substantial research, planning, or implementation to find prior relevant sessions, rules, and resources
- Store only distilled reusable knowledge in `cm`, such as debugging lessons, architecture constraints, and durable research takeaways
- Keep the active workstream truth in beads, including scope, decisions, acceptance criteria, verification history, and detailed notes

### PR Support

- PR descriptions should be generated from the repo PR template when one exists
- PR summaries should pull scope, rationale, and verification context from beads when available
- PR descriptions should update the PR body directly with `gh pr edit`

## Requirements

- Existing Opencode installation with `.opencode` directory
- `br` from `beads_rust`: `https://github.com/Dicklesworthstone/beads_rust`
- `bv` from `beads_viewer`: `https://github.com/Dicklesworthstone/beads_viewer`
- `gh` CLI for PR-oriented workflows

## License

MIT License
