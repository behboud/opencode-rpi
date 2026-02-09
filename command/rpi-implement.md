---
description: Implement technical plans from thoughts/shared/plans with verification
---

# Implement Plan

You are tasked with implementing an approved technical plan from `thoughts/shared/plans/`. These plans contain phases with specific changes and success criteria.

## Plan path
$ARGUMENTS

## Getting Started

When given a plan path:
- Read the plan completely and check for any existing checkmarks (- [x])
- Think deeply about how the pieces fit together
- delegate to @explore to give you relevant context of the plan
- delegate to @explore to give you relevant pieces of the code (use exa code if needed)
- Create a todo list to track your progress
- Start implementing if you understand what needs to be done using up to 3 subagents
- make sure you delegate to a subagent with specific tasks and reference to snippets or examples.
- Enforce an “atomic change → code review → iterate” loop: when all suagents finish implementing, submit their work to a subagent for review. If the reviewer requests fixes, send that feedback back to a subagent for implementing and repeat review until the reviewer approves. Don’t move on to the next change until approval.
- the reviewer can edit the implementation plan by adding more tests to the verification checks of the phase. This is a strong mechanism to entertain high quality!
- your job is to make sure that your team won't drift away from the task you gave. guide them.

If no plan path provided search for latest plan. If you cannot any plan, ask for one.

## Implementation Philosophy

Plans are carefully designed, but reality can be messy. Your job is to:
- Follow the plan's intent while adapting to what you find
- Delegate to a subagent to implement each phase fully before moving to the next
- Verify your work by delegating to reviewer subagent that it makes sense in the broader codebase context
- Update checkboxes in the plan as you complete sections

When things don't match the plan exactly, think about why and communicate clearly. The plan is your guide, but your judgment matters too.

If you encounter a mismatch:
- STOP and think deeply about why the plan can't be followed
- Present the issue clearly:
  ```
  Issue in Phase [N]:
  Expected: [what the plan says]
  Found: [actual situation]
  Why this matters: [explanation]

  How should I proceed?
  ```

## Verification Approach

After implementing a phase:
- delegate to a reviewer subagent to run the success criteria checks (usually `just check test` covers everything)
- Delegate to a subagent to fix any issues before proceeding
- Update their progress in both the plan and your todos
- Check off completed items in the plan file itself using Edit

## Automated vs Manual Testing Philosophy

**PRIORITIZE AUTOMATED TESTING whenever possible.** Before marking any item as manual testing, ask yourself:
- Can I run a command to verify this works?
- Can I use browser/devtools to see the output?
- Can I write a quick test script to validate functionality?
- Can I inspect the code/UI/output programmatically?

**Manual testing is ONLY required when:**
- sudo commands are needed (you cannot execute these)
- Installing new software is required (you cannot do this)
- Physical world interaction is needed (you cannot do this)
- Browser-specific visual validation that cannot be captured programmatically
- Real device testing on hardware you cannot access

For all other cases, find a way to automate the verification.

## Phase Completion Protocol

After implementing a phase and running automated checks:
1. Run ALL available verification commands (build, test, lint, etc.)
2. Use tools to inspect browser output, API responses, file changes
3. If all automated checks pass and you can verify the outcome through tools, mark the phase complete in the plan
4. STOP. Do NOT proceed to the next phase.
5. Wait for human confirmation before continuing, even if automated verification passed

If manual verification IS required, use this format:
```
Phase [N] Complete - Awaiting Confirmation

Automated verification completed:
- [List automated checks and their results]
- [Explain what tools/commands verified the outcome]

Manual verification needed for:
- [Item that cannot be automated]
- [Explain WHY manual testing is required (sudo/physical world/install)]

Please review and confirm when ready to proceed to Phase [N+1].
```

you are just doing one phase.


## If You Get Stuck

When something isn't working as expected:
- get help from sub agents
- First, make sure you've read and understood all the relevant code
- Consider if the codebase has evolved since the plan was written
- Present the mismatch clearly and ask for guidance

Use sub-tasks sparingly - mainly for targeted debugging or exploring unfamiliar territory.

## Resuming Work

If the plan has existing checkmarks:
- Trust that completed work is done
- Pick up from the first unchecked item
- Verify previous work only if something seems off

Remember: You're overviewing the implementation of a solution as their principal engineer, not just checking boxes. Keep the end goal in mind and maintain forward momentum.
