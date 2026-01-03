# Memento Architecture

This document explains how Memento works and the design decisions behind it.

## Overview

Memento is a Claude Code command that runs at the end of coding sessions to extract actionable memories. It bridges Claude's stateless nature by persisting learnings to CLAUDE.md files.

```
┌─────────────────────────────────────────────────────────────┐
│                    Claude Code Session                       │
│  User <──> Claude conversation with corrections/preferences │
└─────────────────────────────────────────────────────────────┘
                              │
                              │ User runs /memento
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    Memento Command                           │
│  1. Analyze conversation for actionable insights            │
│  2. Categorize into project vs user memory                  │
│  3. Check for duplicates in existing files                  │
│  4. Present suggestions via multi-select UI                 │
│  5. Append selected items to CLAUDE.md files                │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    CLAUDE.md Files                           │
│  ./CLAUDE.md          - Project-specific conventions        │
│  ~/.claude/CLAUDE.md  - User's global preferences           │
└─────────────────────────────────────────────────────────────┘
                              │
                              │ Next session
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    New Claude Session                        │
│  Claude reads CLAUDE.md files automatically                 │
│  Has context about preferences and conventions              │
└─────────────────────────────────────────────────────────────┘
```

## Core Components

### 1. Session Analysis

The command reviews the conversation looking for:

| Signal | Example | Why It Matters |
|--------|---------|----------------|
| Mistakes | "Wrong file path" | Prevents repeat errors |
| Corrections | "No, use pnpm" | Captures preferences |
| Preferences | "Show me first" | Improves workflow |
| Conventions | "Tests go here" | Project knowledge |

### 2. Actionability Filter

Not everything is worth remembering. The filter ensures suggestions:

```
┌──────────────────────────────────────────────────────────┐
│                  Actionability Test                       │
│                                                          │
│  1. Changes behavior?  ─── Does it make Claude act       │
│                            differently next time?        │
│                                                          │
│  2. Specific enough?   ─── Can it be applied without     │
│                            interpretation?               │
│                                                          │
│  3. Prevents mistakes? ─── Would it have avoided an      │
│                            error in this session?        │
│                                                          │
│  All three = Actionable                                  │
│  Missing any = Skip                                      │
└──────────────────────────────────────────────────────────┘
```

### 3. Memory Categorization

```
                    Is this insight...
                          │
          ┌───────────────┴───────────────┐
          │                               │
   Specific to this              Applies to all
   codebase/project?             my projects?
          │                               │
          ▼                               ▼
   ┌─────────────┐               ┌─────────────┐
   │ ./CLAUDE.md │               │ ~/.claude/  │
   │   Project   │               │  CLAUDE.md  │
   │   Memory    │               │ User Memory │
   └─────────────┘               └─────────────┘
```

**Project Memory examples:**
- "Use pnpm in this project"
- "API routes are in src/routes/"
- "Database is PostgreSQL"

**User Memory examples:**
- "Always show commands before running"
- "Prefer TypeScript over JavaScript"
- "Use conventional commits"

### 4. Duplicate Detection

Before presenting suggestions, Memento reads existing CLAUDE.md files to avoid:
- Exact duplicates
- Semantically similar entries
- Contradictory instructions

### 5. User Selection UI

Uses Claude Code's `AskUserQuestion` with `multiSelect: true`:

```json
{
  "questions": [
    {
      "header": "Project Memory",
      "question": "Which insights should I add to ./CLAUDE.md?",
      "multiSelect": true,
      "options": [...]
    },
    {
      "header": "User Memory",
      "question": "Which insights should I add to ~/.claude/CLAUDE.md?",
      "multiSelect": true,
      "options": [...]
    }
  ]
}
```

This displays as tabbed interface where users can select from both categories.

## Design Decisions

### Why Commands, Not Skills?

Commands are simpler for single-action tools. Skills are better for complex, multi-step workflows. Memento is intentionally a command because:
- Single clear purpose
- Runs once at session end
- No ongoing state needed

### Why Append-Only?

Memento only appends to CLAUDE.md files, never edits or removes:
- Safer - won't accidentally delete important instructions
- Traceable - easy to see what was added when
- User control - manual editing for removals

### Why Max 4 Suggestions Per Category?

- Respects user's time
- Forces prioritization of most valuable insights
- Prevents decision fatigue
- Keeps CLAUDE.md files focused

### Why Interactive Selection?

Rather than auto-appending:
- User validates relevance
- Catches false positives
- Builds trust in the tool
- Educational - shows what was learned

## File Locations

| File | Location | Purpose |
|------|----------|---------|
| Command | `.claude/commands/memento.md` | The implementation |
| Project Memory | `./CLAUDE.md` | Codebase conventions |
| User Memory | `~/.claude/CLAUDE.md` | Global preferences |
| Session Logs | `~/.claude/projects/*/` | Historical data |

## Future Architecture

### Memento MCP Server

Will add retrospective analysis:

```
┌─────────────────────────────────────────────────────────────┐
│                    Memento MCP Server                        │
│                                                             │
│  Tools:                                                     │
│  - memento_search     Search past sessions                  │
│  - memento_patterns   Find recurring corrections            │
│  - memento_suggest    Propose memories from history         │
│  - memento_analytics  Token usage, session stats            │
│                                                             │
│  Data Source: ~/.claude/projects/*/*.jsonl                  │
└─────────────────────────────────────────────────────────────┘
```

### Memento Sync

Will enable backup and cross-machine sync:

```
┌─────────────────────────────────────────────────────────────┐
│                    Memento Sync                              │
│                                                             │
│  Commands:                                                  │
│  - memento-sync push    Backup to remote                    │
│  - memento-sync pull    Restore from remote                 │
│  - memento-sync diff    Preview changes                     │
│                                                             │
│  Features:                                                  │
│  - Git-based storage                                        │
│  - Smart CLAUDE.md merging                                  │
│  - Selective sync (commands, skills, memories)              │
└─────────────────────────────────────────────────────────────┘
```

## Security Considerations

- Memento only reads conversation context (already in memory)
- Writes only to local CLAUDE.md files
- No external network calls
- No secrets or credentials handling
- User explicitly approves all additions
