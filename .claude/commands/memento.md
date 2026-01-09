---
description: Extract session memories into CLAUDE.md - because Claude forgets, but your notes don't
---

# Memento

Usage: `/memento [--project PATH] [--user PATH]`

Analyze this conversation to extract **actionable** memory updates for CLAUDE.md files.

## Step 0: Resolve Paths

**Default paths:**
- Project Memory: `./CLAUDE.md`
- User Memory: `~/.claude/CLAUDE.md`

**Override via arguments:**
- `--project PATH` - custom project memory location
- `--user PATH` - custom user memory location

**Override via config (persistent):**
Check `~/.claude/CLAUDE.md` for a `## Memento Config` section:
```
## Memento Config
- PROJECT_PATH: ./docs/CLAUDE.md
- USER_PATH: ~/custom/CLAUDE.md
```

**Priority:** Arguments > Config > Defaults

Store resolved paths as `$PROJECT_PATH` and `$USER_PATH` for use in subsequent steps.

## Step 1: Analyze the Session

Review the conversation for:

- **Mistakes I made** - wrong assumptions, incorrect commands, things that failed
- **Corrections the user made** - "no", "actually", "I meant", "not that"
- **Preferences expressed** - how the user likes things done (explicit or implicit)
- **Project conventions discovered** - file structure, tooling, patterns in THIS codebase
- **Reusable learnings** - insights that apply across all projects

## Step 2: Categorize Suggestions

### What counts as ACTIONABLE:
- Changes how Claude behaves in future sessions
- Specific enough to apply ("use pnpm not npm" vs "check package manager")
- Would have prevented a mistake made in this session

### What is NOT actionable (skip these):
- Observations or reflections ("this was a good session")
- Meta-advice ("run retro after meaningful sessions")
- Vague principles ("be more careful")
- Things already known or obvious

### Where suggestions go:

**Project Memory (`$PROJECT_PATH`)** - PREFER THIS when:
- In a specific project folder (not home/general directory)
- Convention is specific to this codebase
- Tool/file structure is project-specific

**User Memory (`$USER_PATH`)** - Use when:
- Preference applies across ALL projects
- It's about how the user likes to work generally
- It's a Claude Code tip that's universally useful

## Step 3: Check for Duplicates

Read both CLAUDE.md files before suggesting. Never suggest something already present.

## Step 4: Present Suggestions (or say "nothing meaningful")

If you have ZERO actionable suggestions, just say:
> "No actionable suggestions from this session. The conversation was straightforward or learnings were already captured."

If you DO have suggestions, use `AskUserQuestion` with multiSelect.

**IMPORTANT: Always show BOTH categories in the same AskUserQuestion call when you have suggestions for each.**

The `questions` array should include:
1. Project Memory question (if any project suggestions)
2. User Memory question (if any user suggestions)

This shows them as separate tabs so the user can review and select from both.

Example structure:
```json
{
  "questions": [
    {
      "header": "Project Memory",
      "question": "Which insights should I add to $PROJECT_PATH?",
      "multiSelect": true,
      "options": [...]
    },
    {
      "header": "User Memory",
      "question": "Which insights should I add to $USER_PATH?",
      "multiSelect": true,
      "options": [...]
    }
  ]
}
```

Rules:
- Max 4 suggestions per category
- Each suggestion must be 1-2 lines, specific, actionable
- User can select none, some, or all from each category

## Step 5: Apply Selected Suggestions

For each selected suggestion:
1. Read the target CLAUDE.md (create if needed)
2. Append under appropriate section (create section if needed)
3. Use Edit tool

## Output Format

```
✅ Added to $PROJECT_PATH:
- [suggestion]

✅ Added to $USER_PATH:
- [suggestion]
```

Or if nothing was added:
```
No changes made.
```
