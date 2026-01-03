# Example: Git Workflow Preference

## Scenario
User has a specific git workflow they prefer, revealed through corrections during the session.

## Conversation Snippet

```
User: Commit these changes

Claude: Creating commit...
> git commit -m "Update authentication logic"

User: Wait - I always want to see the diff before committing,
and use conventional commits format

Claude: Understood! Let me show the diff first:
> git diff --staged
[shows changes]

Ready to commit with: "feat: update authentication logic"
Proceed?
```

## Memento Output

```
┌─ User Memory ─────────────────────────────────────────────┐
│ ☑ Before git commits: show diff and use conventional      │
│   commit format (feat:, fix:, docs:, etc.)                │
└───────────────────────────────────────────────────────────┘
```

**Suggested entry:**
> Before git commits: show `git diff --staged` and use conventional commit format (feat:, fix:, docs:, etc.)

**Target file:** `~/.claude/CLAUDE.md` (User Memory)

## Why User Memory?

This is a workflow preference that applies everywhere:
- Not specific to any project
- Reflects how the user likes to work with git
- Should apply to all future sessions

## Compound Preferences

This example shows a **compound preference** (two things learned at once):
1. Show diff before committing
2. Use conventional commits format

Both are captured in a single, actionable suggestion.

## Why It's Actionable

| Criterion | Met? | Explanation |
|-----------|------|-------------|
| Changes behavior | Yes | Claude will show diff and format commits |
| Specific | Yes | Clear format mentioned |
| Prevents mistakes | Yes | No more surprise commits |
