---
description: Debug issues by investigating logs and git history
model: opus
---

# Debug

You are tasked with helping debug issues discovered during plan implementation or manual verification. This command investigates logs and git history without editing files.

## Initial Response

When this command is invoked:

1. **Check if parameters were provided**:
   - If a plan path or ticket path is provided, read it fully first
   - Use the same path conventions as `/create_plan`:
     - plans: `docs/plans/...`
     - tickets: `docs/tickets/...`
   - Then begin investigation immediately

2. **If no parameters were provided**, respond with:
```
I'll help debug your current issue.

What specific problem are you encountering?
- What were you trying to implement or manually verify?
- What happened instead?
- Any exact error message, timestamp, or failing command?
- Which plan/ticket is this tied to (for example `docs/plans/2026-03-04-ABC-123-fix-foo.md`)?

I can investigate logs and recent changes to help identify the issue.
```

## Environment Information

Use these key locations and tools:

**Git State**:
- Check current branch, recent commits, uncommitted changes
- Similar to how `commit` and `describe_pr` commands work

## Process Steps

### Step 1: Understand the Problem

1. **Read all provided context files fully**:
   - plan files (for example `docs/plans/2026-03-04-ABC-123-fix-foo.md`)
   - ticket files (for example `docs/tickets/abc-123.md`)
   - related docs or notes mentioned by the user
   - never read partially; always read complete files

2. **Establish expected vs actual behavior**:
   - what phase/step is being executed
   - what success criteria were expected
   - what actually happened
   - exact time window for failures (if known)

3. **Quick state check**:
   - Current git branch and recent commits
   - Any uncommitted changes
   - When the issue started occurring

### Step 2: Investigate the Issue

Before asking broad follow-up questions, investigate with focused parallel sub-tasks.

Use the project's specialized agents (same naming style as `/create_plan`):
- **codebase-locator** - Locate relevant implementation files for the failing area
- **codebase-analyzer** - Explain current behavior and likely failure points
- **docs-locator** - Find related plans/research/debug notes when needed
- **docs-analyzer** - Extract key constraints or prior decisions from those docs

Investigation checklist:

1. **Logs**:
   - identify most recent relevant daemon/WUI/MCP logs
   - extract errors/warnings near the failure timeframe
   - capture repeated patterns and first-occurrence timestamp

2. **Git/file state**:
   - inspect branch, commit history, and working tree state
   - verify files expected by the plan phase exist
   - compare current code behavior against the referenced plan/ticket

### Step 3: Present Findings

Present a focused debug report:

```markdown
## Debug Report

### What's Wrong
[Clear statement of the issue based on evidence]

### Evidence Found

**From Git/Files**:
- [Recent changes that might be related]
- [File state issues]

### Root Cause
[Most likely explanation based on evidence]

### Next Steps

1. **Try This First**:
   ```bash
   [Specific command or action]
   ```

2. **If That Doesn't Work**:
   - If this appears code-related, run `/create_plan` or update existing `docs/plans/...` with follow-up fixes

### Can't Access?
Some issues might be outside my reach:
- Browser console errors (F12 in browser)
- MCP server internal state
- System-level issues

Would you like me to investigate something specific further?
```

## Important Notes

- **Path conventions must match `/create_plan`**:
  - Use `docs/plans/...` for plans
  - Use `docs/tickets/abc-123.md` style for ticket paths
- **Mock ticket examples should follow the same pattern**:
  - ticket key: `ABC-123`
  - ticket file: `docs/tickets/abc-123.md`
- **Use specialized agent names consistently**:
  - `codebase-locator`, `codebase-analyzer`, `docs-locator`, `docs-analyzer`
- **Read context files completely** - no partial reads
- **No file editing** - this command is investigation only

## Quick Reference

**Git State**:
```bash
git status
git log --oneline -10
git diff
```

Remember: this command is for investigation during implementation/verification. Use it to gather evidence quickly, then either continue implementation (`/implement_plan`) or loop back to planning (`/create_plan`) as needed.
