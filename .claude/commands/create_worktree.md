---
description: Create a worktree and launch an implementation session from a plan
model: opus
---

# Create Worktree

You are tasked with creating a dedicated worktree and launching an implementation session for an approved plan.

## Initial Response

When this command is invoked:

1. **Check if parameters were provided**:
   - If a ticket/branch hint and plan path were provided, skip the default prompt
   - Parse what you can from the input immediately
   - Validate the plan path format and branch naming expectations

2. **If no parameters were provided**, respond with:
```
I'll help you create a worktree and launch an implementation session.

Please provide:
1. The Linear ticket key (e.g. ABC-1234)
2. The plan file path (relative path, usually under docs/plans/)
3. Optional branch slug (if omitted, I'll derive one from the plan name)

Example:
/create_worktree ABC-1234 docs/plans/2026-03-04-ABC-1234-improve-foo.md
```

Then wait for user input.

## Required Inputs

Collect and confirm:
- `ticket_key` (e.g. `ABC-1234`)
- `plan_path` (relative path only, typically `docs/plans/...`)
- `branch_name` (default: `<ticket_key>-<plan-slug>`, kebab-case)
- `worktree_path` (default: `~/wt/humanlayer/<ticket_key>`)

## Process Steps

### Step 1: Validate Plan Path and Inputs

1. **Validate plan path usage**:
   - Use only a relative path from repo root
   - Prefer plans created by `/create_plan` in `docs/plans/...`
   - Do not use absolute paths

2. **Validate ticket and branch format**:
   - Ticket key should look like `ABC-1234`
   - Branch should be lowercase kebab-case and include the ticket key

3. **Read `hack/create_worktree.sh` fully** before running it:
   - Confirm expected arguments and behavior
   - Ensure command invocation matches script usage

### Step 2: Build Launch Prompt and Command

Use this launch prompt template:

`/implement_plan at <plan_path> and when you are done implementing and all tests pass, read ./.claude/commands/commit.md and create a commit, then read ./.claude/commands/describe_pr.md and create a PR, then add a comment to the Linear ticket with the PR link`

Use this launch command template:

`humanlayer launch --model opus -w <worktree_path> "<launch_prompt>"`

### Step 3: Confirm With User Before Execution

Before creating the worktree or launching, send:

```
Based on the input, I plan to create a worktree with the following details:

worktree path: <worktree_path>
branch name: <branch_name>
path to plan file: <plan_path>
launch prompt:

    /implement_plan at <plan_path> and when you are done implementing and all tests pass, read ./.claude/commands/commit.md and create a commit, then read ./.claude/commands/describe_pr.md and create a PR, then add a comment to the Linear ticket with the PR link

commands to run:

    ./hack/create_worktree.sh <ticket_key> <branch_name>
    humanlayer launch --model opus -w <worktree_path> "/implement_plan at <plan_path> and when you are done implementing and all tests pass, read ./.claude/commands/commit.md and create a commit, then read ./.claude/commands/describe_pr.md and create a PR, then add a comment to the Linear ticket with the PR link"
```

Incorporate any user feedback before proceeding.

### Step 4: Execute

After explicit user confirmation:

1. Run:
   - `./hack/create_worktree.sh <ticket_key> <branch_name>`
2. Then launch:
   - `humanlayer launch --model opus -w <worktree_path> "<launch_prompt>"`
3. Share the resulting session details/output with the user.

## Important Guidelines

1. **Path consistency with `/create_plan`**:
   - Plans are expected under `docs/plans/...`
   - Keep plan paths relative so they work in the worktree context

2. **Safety and clarity**:
   - Never launch before user confirmation
   - Show exact commands before execution
   - If plan file does not exist, stop and ask for correction

3. **Command references**:
   - Use `./.claude/commands/commit.md`
   - Use `./.claude/commands/describe_pr.md`
   - Keep command names and locations exact
