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
- delegate to @thoughts-locator to give you relevant context of the plan
- delegate to @codebase-locator to give you relevant pieces of the code
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
- delegate to a reviewer subagent to run the success criteria checks (usually `make check test` covers everything)
- Delegate to a subagent ot fix any issues before proceeding
- Update their progress in both the plan and your todos
- Check off completed items in the plan file itself using Edit
- **Pause for human verification**: After completing all automated verification for a phase, pause and inform the human that the phase is ready for manual testing. Only add manual tests that make sense. Means if you can test the cases, then they are not required as manual tests but should be automated. Use this format for manual tests:
  ```
  Phase [N] Complete - Ready for Manual Verification

  Automated verification passed:
  - [List automated checks that passed]

  Please perform the manual verification steps listed in the plan:
  - [List manual verification items from the plan]

  Let me know when manual testing is complete so I can proceed to Phase [N+1].
  ```

you are just doing one phase.

do not check off items in the manual testing steps until confirmed by the user.


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
