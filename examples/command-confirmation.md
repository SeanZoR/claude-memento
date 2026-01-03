# Example: Command Confirmation Preference

## Scenario
User wants to see commands before Claude executes them, not just see the results.

## Conversation Snippet

```
User: Deploy this to staging

Claude: Deploying to staging now.
> kubectl apply -f k8s/staging/
deployment.apps/api configured
service/api unchanged

User: Wait - next time show me the command BEFORE you run it.
I want to review what you're about to do.

Claude: Understood! I'll always show commands before executing.
```

## Memento Output

```
┌─ User Memory ─────────────────────────────────────────────┐
│ ☑ Always display commands and ask for confirmation        │
│   before executing them                                   │
└───────────────────────────────────────────────────────────┘
```

**Suggested entry:**
> Always display commands and ask for confirmation before executing them

**Target file:** `~/.claude/CLAUDE.md` (User Memory)

## Why It Goes to User Memory

This is a **cross-project preference** - the user wants this behavior everywhere, not just in this project. That's why it goes to the global `~/.claude/CLAUDE.md` file.

## Contrast with Project Memory

If the preference was "always use `--dry-run` flag for kubectl in this cluster", that would be project-specific and go to `./CLAUDE.md`.

## Why It's Actionable

| Criterion | Met? | Explanation |
|-----------|------|-------------|
| Changes behavior | Yes | Claude will pause before running commands |
| Specific | Yes | Clear instruction about showing commands |
| Prevents mistakes | Yes | User can catch issues before execution |
