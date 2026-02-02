---
description: Create detailed implementation plans through interactive research and integration
---

# Implementation Plan

Create a detailed implementation plan via an interactive, iterative loop: research, propose structure, ask one focused question, refine.

## User Input

$ARGUMENTS

## Workflow (Short Snippets + Pseudocode)

1. Gather context:
   - Read any files the user mentions (fully).
   - If needed, locate related code/docs via targeted searches.

2. Understand the current state:
   - Pseudocode:
     - identify entry points and data flow
     - note constraints (existing patterns, config, tests)
     - list unknowns that affect scope/security/UX

3. Ask at most one question at a time:
   - Ask only what you cannot answer from the code.

4. Propose plan structure first, then details:
   - Pseudocode outline:
     - Overview
     - Current State
     - Desired End State (with how to verify)
     - Phases (1..N), each with: changes + success criteria

5. Write plan files:
   - Path convention (snippet): `thoughts/shared/plans/YYYY-MM-DD-<topic>-phase-<N>.md`

## Success Criteria Conventions

- Separate `Automated Verification` vs `Manual Verification`.
- Commands shown should be minimal snippets, e.g. `make test`, `npm test`, `curl <endpoint>`.
- Manual verification only when automation is impossible (sudo/hardware/install).

## Clarifications

- If a decision materially changes scope/security/UX and no safe default exists, mark it as:
  - `[NEEDS CLARIFICATION: <one concrete question>]`
  - Limit to 3 total; prioritize scope > security/privacy > UX.
