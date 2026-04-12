---
description: Generate a PR description from the repo PR template and plan files
---

# Generate PR Description

Generate a PR description from the repo PR template when one exists, and pull implementation context from plan files whenever possible.

## Artifact Rules

- Pull execution context from `plans/` first.
- Pull supporting investigation from `research/` when relevant.
- Do not rely on docs as the active workstream source of truth.

## User Input
$ARGUMENTS

## Workflow

1. Read the repo PR template.
   - Check conventional template locations such as `.github/pull_request_template.md` and `.github/PULL_REQUEST_TEMPLATE.md`.
   - If no template exists, write a clear PR body with `## Summary` and `## Testing` sections.
   - Use the template for final formatting only.

2. Resolve the PR.
   - First check the current branch with `gh pr view --json url,number,title,state`.
   - If needed, list candidates with `gh pr list --limit 10 --json number,title,headRefName,author`.

3. Read the current PR body.
   - Use `gh pr view {number} --json body`.
   - If a body already exists, update it instead of starting blind.

4. Gather PR data.
   - Read PR metadata with `gh pr view {number} --json url,title,number,state,baseRefName,commits`.
   - Read the full diff with `gh pr diff {number}`.
   - If GitHub is not configured, tell the user to run `gh repo set-default`.

5. Gather plan context for the PR.
   - Look for plan slugs in the branch name, commit messages, and diff context.
    - Read the parent plan file and relevant phase files from `plans/`.
    - Read relevant research files from `research/` when they explain rationale or tradeoffs.
    - Treat plan files as the best source for why the work exists, planned scope, design intent, code sketch context, non-goals, and validation notes.

6. Analyze and verify.
   - Use the diff and plan context together.
   - Distinguish user-facing changes from internal work.
   - Surface breaking changes, migrations, follow-ups, and explicit non-goals.
   - For template verification steps, run every command you can and mark the results accurately.

7. Write the PR description.
   - Fill the template with concrete details from the diff and plan files.
   - Prefer the plan rationale for the "why" and the diff for the "what".
   - Mention relevant plan file paths when useful for reviewer context.

8. Update the PR.
   - Update the PR body directly with `gh pr edit {number} --body-file <temp-file>` or an equivalent heredoc flow.

## Rules

- Use the repo PR template only as the formatting template, not as the source of truth.
- Prefer plan files for scope, rationale, phase summaries, and verification history.
- Be specific, scannable, and honest about any unchecked verification.
