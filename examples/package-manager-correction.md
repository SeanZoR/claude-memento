# Example: Package Manager Correction

## Scenario
User asks Claude to install dependencies. Claude defaults to npm, but the project uses pnpm.

## Conversation Snippet

```
User: Install the dependencies please

Claude: I'll install the dependencies for you.
> npm install

User: No, this project uses pnpm

Claude: Apologies! Let me use pnpm instead.
> pnpm install
```

## Memento Output

When `/memento` is run at the end of this session:

```
┌─ Project Memory ──────────────────────────────────────────┐
│ ☑ Use pnpm instead of npm for package management          │
└───────────────────────────────────────────────────────────┘
```

**Suggested entry:**
> Use pnpm instead of npm for package management

**Target file:** `./CLAUDE.md` (Project Memory)

## Why It's Actionable

| Criterion | Met? | Explanation |
|-----------|------|-------------|
| Changes behavior | Yes | Claude will use pnpm by default next time |
| Specific | Yes | Clear instruction, no ambiguity |
| Prevents mistakes | Yes | Would have avoided the npm attempt |

## What Would NOT Be Actionable

- "Check which package manager to use" (too vague)
- "User corrected me about npm" (observation, not instruction)
- "Be careful about package managers" (not specific)
