# Memento Roadmap

This document outlines the planned evolution of Memento from a simple command into a comprehensive memory toolkit for Claude Code.

## Current State (v1.0)

### `/memento` Command
- Analyzes current session for actionable insights
- Categorizes into project vs user memory
- Interactive multi-select for user approval
- Appends to CLAUDE.md files

## Phase 1: Polish & Examples

### Goals
- Professional documentation
- Rich example library
- Community contribution guidelines

### Deliverables
- [x] Comprehensive README with badges
- [x] Example scenarios covering common patterns
- [x] CONTRIBUTING.md guide
- [x] Architecture documentation
- [ ] Demo GIF/video

## Phase 2: Memento MCP Server

### Vision
Retrospective analysis of all past Claude Code sessions, not just the current one.

### Core Features

**Session Search**
```
"Find all sessions where I corrected Claude about testing"
"Show me errors from last week's debugging sessions"
```

**Pattern Detection**
```
"What corrections do I make most often?"
"Which projects have the most memory entries?"
```

**Bulk Memory Extraction**
```
"Suggest memories from my last 10 sessions"
"Find preferences I expressed but never saved"
```

### Technical Approach
- TypeScript MCP server using `@modelcontextprotocol/sdk`
- Reads JSONL session logs from `~/.claude/projects/`
- Exposes tools callable from Claude Code
- Optional HTTP transport for dashboard

### Tools to Implement
| Tool | Purpose |
|------|---------|
| `memento_search` | Full-text search across sessions |
| `memento_session` | Get details of specific session |
| `memento_patterns` | Identify recurring themes |
| `memento_suggest` | Propose new memory entries |
| `memento_stats` | Usage analytics |

## Phase 3: Memento Dashboard

### Vision
Web UI for browsing and managing Claude memories across all sessions.

### Features
- Session timeline with searchable history
- Memory management (view, edit, delete entries)
- Analytics (token usage, session frequency)
- One-click memory extraction from past sessions

### Technical Approach
- React/Vue frontend with Tailwind
- Served by MCP server via HTTP
- Local-first, optional cloud sync

### Mockup
```
┌─────────────────────────────────────────────────────────────┐
│  Memento Dashboard                              [Settings]  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Sessions (Last 30 days)           Memories                 │
│  ────────────────────────          ─────────                │
│  ▸ Today                           Project: my-app          │
│    └ my-app: Refactored auth       • Use pnpm not npm       │
│    └ website: Fixed CSS bug        • Tests in __tests__/    │
│  ▸ Yesterday                                                │
│    └ api-server: Added endpoint    Global                   │
│  ▸ Last Week                       • Show commands first    │
│    └ ...                           • Use TypeScript         │
│                                                             │
│  ─────────────────────────────────────────────────────────  │
│  Suggested Memories (from history analysis)                 │
│  ☐ "Always run linting before commit" (appeared 5x)        │
│  ☐ "Database migrations in prisma/" (appeared 3x)          │
│                    [Add Selected]                           │
└─────────────────────────────────────────────────────────────┘
```

## Phase 4: Memento Sync

### Vision
Backup and sync Claude memories across machines.

### Features
- Git-based remote storage
- Smart CLAUDE.md merging (not just overwrite)
- Selective sync (choose what to include)
- Conflict resolution

### Commands
```bash
memento-sync init           # Connect to Git repo
memento-sync push           # Backup to remote
memento-sync pull           # Restore from remote
memento-sync status         # Show sync state
memento-sync diff           # Preview changes
```

### What Gets Synced
| Item | Default | Notes |
|------|---------|-------|
| `~/.claude/CLAUDE.md` | Yes | Core memories |
| `~/.claude/commands/` | Yes | Custom commands |
| `~/.claude/skills/` | Yes | Custom skills |
| `~/.claude/settings.json` | Yes | Preferences |
| `~/.claude/mcp-config.json` | Selective | May have local paths |
| `~/.claude/projects/` | No | Too large, contains logs |
| Credentials | Never | Security |

## Phase 5: Advanced Features

### Memento Teams (Concept)
Shared project memories for teams:
- Team-level CLAUDE.md files
- Sync conventions across developers
- Onboarding acceleration

### Memento Learn (Concept)
Pattern learning across the community:
- Anonymized, aggregated patterns
- "Users who work with React often save..."
- Opt-in only

### IDE Integration (Concept)
- VS Code extension showing current memories
- Quick-add memories from editor
- Memory search without opening Claude Code

## Contributing

We welcome contributions at any phase! See [CONTRIBUTING.md](../CONTRIBUTING.md) for guidelines.

Priority areas:
- Examples for Phase 1
- MCP server development for Phase 2
- UI/UX design for Phase 3
- Sync logic for Phase 4
