---
description: Set up worktree for reviewing colleague's branch
model: opus
---

# Local Review

You are tasked with setting up a local review environment for a colleague's branch. This involves creating a worktree, setting up dependencies, and launching a new Claude Code session.

## Initial Response

When this command is invoked with a parameter like `gh_username:branchName`:

1. **Parse the input**:
   - Extract GitHub username and branch name from the format `username:branchname`
   - If no parameter provided, ask for it in the format: `gh_username:branchName`

2. **Extract ticket information**:
   - Look for ticket keys in the branch name (e.g., `abc-123`, `ABC-123`)
   - Use this to create a short worktree directory name
   - If no ticket found, use a sanitized version of the branch name

3. **Confirm path and naming conventions**:
   - Keep conventions consistent with `/create_plan`:
     - ticket key format: `ABC-123`
     - plan paths: `docs/plans/...`
     - ticket paths: `docs/tickets/abc-123.md`
   - Use relative docs paths in any follow-up instructions inside the worktree

Then proceed with setup.

## Process Steps

### Step 1: Set Up Remote and Worktree

1. **Set up the remote and worktree**:
   - Check if the remote already exists using `git remote -v`
   - If not, add it: `git remote add USERNAME git@github.com:USERNAME/<repo_name>`
   - Fetch from the remote: `git fetch USERNAME`
   - Create worktree: `git worktree add -b BRANCHNAME ~/wt/<repo_name>/SHORT_NAME USERNAME/BRANCHNAME`

### Step 2: Configure the Worktree

1. **Configure the worktree**:
   - Copy Claude settings: `cp .claude/settings.local.json WORKTREE/.claude/`
   - Run setup: `make -C WORKTREE setup`
   - If docs templates are required for downstream commands (e.g., `/describe_pr`), verify expected docs files exist under `docs/...`

### Step 3: Review Readiness

1. Confirm the worktree is ready and provide:
   - worktree path
   - checked-out review branch
   - next command the reviewer should run (for example `/debug`, `/implement_plan`, or `/describe_pr` as appropriate)

## Error Handling

- If worktree already exists, inform the user they need to remove it first
- If remote fetch fails, check if the username/repo exists
- If setup fails, provide the error but continue with the launch

## Important Notes

- Keep mocked ticket examples and extracted keys in `ABC-123` style
- Keep naming/paths aligned with `/create_plan` conventions (`docs/plans`, `docs/tickets`, `docs/research`)
- Do not rewrite branch names unnecessarily; preserve colleague branch intent
- If no ticket key exists in branch name, use a safe sanitized branch-derived directory name

## Example Usage

```
/local_review johndoe1234:sam/ABC-123-hotkey-for-yolo-mode
```

This will:
- Add 'johndoe1234' as a remote
- Create worktree at `~/wt/<repo_name>/abc-123`
- Set up the environment
