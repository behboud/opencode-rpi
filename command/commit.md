---
description: Create git commits with user approval and no Claude attribution
---

# Commit Changes

Create git commits for the changes made during this session.

## Workflow (Snippets, Not Full Scripts)

1. Inspect what changed:
   - `git status`
   - `git diff`

2. Decide commit grouping:
   - Pseudocode:
     - group files by intent (feature/fix/refactor/docs)
     - pick 1 commit per cohesive change

3. Propose the commit plan to the user:
   - files per commit + commit message(s)
   - ask for confirmation before committing

4. Commit (after confirmation):
   - `git add <file1> <file2>`
   - `git commit -m "<imperative message explaining why>"`
   - `git log --oneline -n <N>`

## Rules

- Never use `git add -A` or `git add .`
- Never add co-authors or tool attribution (no "Generated with ...", no "Co-Authored-By")
- Write messages as if the user wrote them (imperative, focused on why)
