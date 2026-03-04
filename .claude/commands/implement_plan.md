---
description: Implement technical plans from docs/plans with verification
model: opus
---

# Implement Plan

You are tasked with implementing an approved technical plan from `docs/plans/`. These plans contain phased changes and explicit automated/manual success criteria.

## Initial Response

When this command is invoked:

1. **Check if parameters were provided**:
   - If a plan path is provided, start immediately
   - Validate that it is a relative path, typically under `docs/plans/...`
   - Read the plan fully before any implementation

2. **If no parameters were provided**, respond with:
```
I'll help implement your approved plan.

Please provide the plan path (relative path), usually in:
- `docs/plans/YYYY-MM-DD-ABC-123-description.md`

Example:
/implement_plan docs/plans/2026-03-04-ABC-123-improve-error-handling.md
```

## Getting Started

When given a plan path:
- Read the plan completely and check for any existing checkmarks (- [x])
- Read the original ticket and all files mentioned in the plan
- **Read files fully** - never use limit/offset parameters, you need complete context
- Think deeply about how the pieces fit together
- Create a todo list to track your progress
- Start implementing if you understand what needs to be done

If no plan path provided, ask for one.

## Path and Naming Conventions

Use the same conventions as `/create_plan`:
- Plan files: `docs/plans/...`
- Ticket key format: `ABC-123`
- Ticket files: `docs/tickets/abc-123.md`
- Research/context docs: `docs/research/...`

Always use relative paths in prompts and commands.

## Implementation Philosophy

Plans are carefully designed, but reality can be messy. Your job is to:
- Follow the plan's intent while adapting to what you find
- Implement each phase fully before moving to the next
- Verify your work makes sense in the broader codebase context
- Update checkboxes in the plan as you complete sections

When things don't match the plan exactly, think about why and communicate clearly. The plan is your guide, but your judgment matters too.

## Process Steps

### Step 1: Read and Prepare

1. Read the plan file fully and identify:
   - current phase and unchecked items
   - automated verification commands
   - manual verification checklist
2. Read all referenced files fully:
   - original ticket (usually `docs/tickets/abc-123.md`)
   - files explicitly mentioned in the plan
   - related docs from `docs/research/...` if referenced
3. Create a todo list to track phase progress.

### Step 2: Implement the Current Phase

1. Implement all required code changes for the active phase.
2. Keep scope constrained to the plan's phase objectives.
3. If needed for targeted investigation, use specialized agents:
   - `codebase-locator` for finding relevant files
   - `codebase-analyzer` for understanding implementation details
   - `codebase-pattern-finder` for similar patterns to follow
   - `docs-locator` and `docs-analyzer` for related documentation context

If you encounter a mismatch with the plan, stop and report:
```
Issue in Phase [N]:
Expected: [what the plan says]
Found: [actual situation]
Why this matters: [explanation]

How should I proceed?
```

### Step 3: Verify the Phase

After implementing a phase:
- Run all automated verification commands listed in the plan
- Fix any issues before proceeding
- Update your progress in both the plan and your todos
- Check off completed implementation items in the plan file itself
- Do **not** check off manual verification items until user confirmation

After automated checks pass, pause for human verification and use:
  ```
  Phase [N] Complete - Ready for Manual Verification

  Automated verification passed:
  - [List automated checks that passed]

  Please perform the manual verification steps listed in the plan:
  - [List manual verification items from the plan]

  Let me know when manual testing is complete so I can proceed to Phase [N+1].
  ```

If instructed to execute multiple phases consecutively, skip the pause until the last phase. Otherwise, assume you are just doing one phase.

do not check off items in the manual testing steps until confirmed by the user.

### Step 4: Continue or Stop

- If manual verification is confirmed, continue to the next phase.
- If manual verification fails, debug the specific failure and report findings clearly.
- If the user asks for debugging help, use `/debug` and then return to `/implement_plan`.

## If You Get Stuck

When something isn't working as expected:
- First, make sure you've read and understood all the relevant code
- Consider if the codebase has evolved since the plan was written
- Present the mismatch clearly and ask for guidance

Use sub-tasks sparingly - mainly for targeted debugging or exploring unfamiliar territory.

## Resuming Work

If the plan has existing checkmarks:
- Trust that completed work is done
- Pick up from the first unchecked item
- Verify previous work only if something seems off

## Important Guidelines

- Read all referenced context files fully (no partial reads)
- Keep paths, ticket naming, and plan locations consistent with `/create_plan`
- Favor incremental, verifiable changes over broad refactors
- Keep the user informed at phase boundaries and mismatch points

Remember: you're implementing a solution, not just checking boxes. Keep forward momentum while preserving correctness.
