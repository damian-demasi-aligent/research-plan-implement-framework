---
description: Generate comprehensive PR descriptions following repository templates
model: opus
---

# Generate PR Description

You are tasked with generating a comprehensive pull request description following the repository's local template and conventions.

## Initial Response

When this command is invoked:

1. **Check if parameters were provided**:
   - If a PR number/URL was provided, skip the default prompt
   - Start discovery immediately

2. **If no parameters were provided**, respond with:
```
I'll help you generate a complete PR description.

Please provide one of:
1. A PR number (for example `123`)
2. A PR URL
3. Confirmation to use the PR associated with the current branch

If relevant, also share the related ticket key (for example `ABC-123`) and plan path (for example `docs/plans/2026-03-04-ABC-123-improve-foo.md`).
```

Then wait for user input.

## Path and Naming Conventions

Use the same conventions as `/create_plan`:
- Plans live in `docs/plans/...`
- Tickets follow `ABC-123` key style
- Ticket documents use `docs/tickets/abc-123.md`
- Related research docs live in `docs/research/...`

PR description files should be stored under:
- Template: `docs/prs/pr_description.md`
- Generated description: `docs/prs/{number}_description.md`

## Process Steps

### Step 1: Read Template and Context

1. **Read the local PR description template first**:
   - Check if `docs/prs/pr_description.md` exists
   - If missing, inform the user to create it at `docs/prs/pr_description.md`
   - Read it fully and extract all required sections/checklists

2. **Read related context files fully when available**:
   - Related implementation plan from `docs/plans/...`
   - Related ticket from `docs/tickets/...`
   - Related research notes from `docs/research/...`

### Step 2: Identify the Target PR

1. Check current branch PR:
   - `gh pr view --json url,number,title,state,headRefName,baseRefName 2>/dev/null`
2. If needed, list open PRs:
   - `gh pr list --limit 10 --json number,title,headRefName,author`
3. If still ambiguous, ask the user which PR to describe.

### Step 3: Check for Existing Draft

1. Check if `docs/prs/{number}_description.md` already exists
2. If it exists:
   - Read it fully
   - Treat this run as an update, not a fresh draft

### Step 4: Gather Complete PR Data

1. Gather metadata:
   - `gh pr view {number} --json url,title,number,state,baseRefName,headRefName,commits`
2. Gather full code changes:
   - `gh pr diff {number}`
3. If `gh` reports no default repo:
   - Instruct user to run `gh repo set-default` and select the correct repository

### Step 5: Analyze and Synthesize

Analyze deeply before writing:
- Purpose and user impact of each change
- Internal refactors vs user-facing behavior changes
- Risks, migration concerns, and breaking changes
- How implementation aligns with plan/ticket intent (if provided)

Use focused research agents when needed for complex diffs:
- `codebase-locator` - find referenced code locations quickly
- `codebase-analyzer` - explain behavior and architectural impact
- `docs-locator` - find related plan/research docs
- `docs-analyzer` - extract decisions/constraints from docs

### Step 6: Handle Verification Checklist

For each checklist item under "How to verify it" (or equivalent):
- If it is runnable in CLI, run it and mark:
  - pass: `- [x]`
  - fail: `- [ ]` with short failure note
- If manual/external, keep unchecked and call it out for user follow-up
- Document anything you could not verify directly

### Step 7: Write and Save Description

1. Fill every template section completely:
   - problem/context
   - approach and rationale
   - user impact
   - risks/migrations/breaking changes
   - concise changelog summary
2. Save to:
   - `docs/prs/{number}_description.md`

### Step 8: Update PR

1. Update PR body:
   - `gh pr edit {number} --body-file docs/prs/{number}_description.md`
2. Confirm update success
3. If any checklist items remain unchecked, remind user before merge

## Important Notes

- Always read local templates and referenced docs fully
- Keep output scannable, specific, and evidence-based
- Focus on "why" and impact, not only "what changed"
- Call out manual verification clearly
- Keep naming, paths, and ticket examples consistent with `/create_plan`
