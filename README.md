# Opencode RPI Configuration

Custom Opencode command and agent configurations optimized for Raspberry Pi development workflows.

## Overview

This repository contains the **HumanLayer AI** research, plan, and implement command prompts adapted for Opencode. The commands follow the HumanLayer philosophy of:
- Human-in-the-loop workflows with explicit checkpoints
- Phase-based implementation with verification gates
- Prioritizing automated testing over manual intervention

These commands were originally developed by HumanLayer for their agentic AI workflows and have been adapted for use with Opencode.

## Repository Structure

```
opencode-rpi/
├── command/          # Custom command prompts
│   ├── hl-commit.md
│   ├── hl-create_plan.md
│   ├── hl-describe_pr.md
│   ├── hl-implement_plan.md
│   ├── hl-research_codebase.md
│   └── hl-validate_plan.md
└── agent/            # Custom agent configurations
    ├── codebase-analyzer.md
    ├── codebase-locator.md
    ├── codebase-pattern-finder.md
    ├── thoughts-analyzer.md
    ├── thoughts-locator.md
    └── web-search-researcher.md
```

## Installation via Git Subtree

Add this repository as a subtree to your existing Opencode configuration folder (`.opencode`).

### Step 1: Navigate to your Opencode config directory

```bash
cd /path/to/your/project/.opencode
```

### Step 2: Add the remote

```bash
git remote add opencode-rpi git@github.com:behboud/opencode-rpi.git
```

### Step 3: Fetch the remote

```bash
git fetch opencode-rpi
```

### Step 4: Create the subtree

```bash
git subtree add --prefix=opencode-rpi opencode-rpi main
```

### Step 5: Update commands to use the subtree

After adding the subtree, update your command references to point to the new prefix:

```bash
# For example, instead of using:
/command/hl-commit.md

# Use:
/opencode-rpi/command/hl-commit.md
```

Or configure your Opencode to automatically include the `opencode-rpi/command/` and `opencode-rpi/agent/` directories.

## Updating from Upstream

To pull the latest changes from this repository:

```bash
git fetch opencode-rpi
git subtree pull --prefix=opencode-rpi opencode-rpi main
```

## Removing the Subtree

If you want to remove the subtree later:

```bash
git rm -r opencode-rpi
git commit -m "Remove opencode-rpi subtree"
```

Then optionally remove the remote:

```bash
git remote remove opencode-rpi
```

## Key Features

### HumanLayer AI Origin

These commands are based on the HumanLayer AI project, which pioneered the concept of:
- **Research**: Deep codebase exploration before acting
- **Plan**: Structured implementation plans with phases and success criteria
- **Implement**: Systematic execution with verification gates
- **Validate**: Comprehensive validation against plan specifications

The HumanLayer approach emphasizes that agents should:
1. Research thoroughly before making changes
2. Create detailed plans with success criteria
3. Implement one phase at a time with verification
4. Always wait for human confirmation before proceeding

### Automated Testing First

All command prompts prioritize automated verification:
- Agents run all available test commands (make check, make test, etc.)
- Agents use tools to inspect browser output, API responses, file changes
- Manual testing is only requested when automation is genuinely impossible (sudo, physical hardware, new software installation)

### Phase-Based Implementation

The `hl-implement_plan.md` command follows a systematic approach:
1. Agents implement one phase at a time
2. Agents verify each phase with automated checks
3. Agents STOP and wait for human confirmation before proceeding
4. Agents do not auto-continue to subsequent phases

### Git Commit Workflow

The `hl-commit.md` command ensures:
- Explicit user confirmation before any git operations
- No Claude attribution in commits
- Clean, atomic commit messages
- User-centric authorship

## Requirements

- An existing Opencode installation with `.opencode` directory
- Git with subtree support
- SSH access to GitHub (for the remote URI)
- Compatible with Opencode's command and agent system

## Related Projects

- [HumanLayer AI](https://humanlayer.dev) - The original project that developed these command patterns

## License

MIT License
