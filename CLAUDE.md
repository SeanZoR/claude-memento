# Memento Project

## Overview
Memento is a Claude Code command that extracts session memories into CLAUDE.md files. This repo is the source of truth for the `/memento` command.

## Project Structure
```
memento/
├── .claude/commands/memento.md  # The main command implementation
├── docs/                        # Documentation and assets
├── examples/                    # Example scenarios
└── README.md                    # User-facing documentation
```

## Key Concepts

### Actionability
The core philosophy is that suggestions must be **actionable** - they should change Claude's future behavior. Vague observations like "be more careful" are filtered out.

### Memory Categories
- **Project Memory** (`./CLAUDE.md`) - Conventions specific to a codebase
- **User Memory** (`~/.claude/CLAUDE.md`) - Cross-project preferences

## Development Guidelines

### When Modifying the Command
- Test with diverse conversation types (debugging, feature building, refactoring)
- Ensure the AskUserQuestion format shows both tabs correctly
- Keep suggestions to max 4 per category
- Each suggestion should be 1-2 lines, specific, actionable

### When Adding Examples
- Include realistic conversation snippets
- Show both the trigger (what happened) and the suggestion (what's extracted)
- Cover different domains (frontend, backend, DevOps, etc.)

### Testing
- After changes, run `/memento` in a test session
- Verify suggestions are properly categorized
- Check that duplicates are detected correctly
- Ensure file writes work for both new and existing CLAUDE.md files

## Conventions

### File Naming
- Commands: lowercase with hyphens (`memento.md`)
- Docs: Title Case for main docs, lowercase for technical docs
- Examples: descriptive names (`package-manager-correction.md`)

### Commit Messages
- Use conventional commits: `feat:`, `fix:`, `docs:`, `refactor:`
- Keep first line under 72 characters
- Reference issues when applicable

## Roadmap Components

### Memento MCP (Planned)
- Will live in `memento-mcp/` directory
- TypeScript with `@modelcontextprotocol/sdk`
- Reads session history from `~/.claude/projects/`

### Memento Sync (Planned)
- Will live in `memento-sync/` directory
- Git-based backup with smart CLAUDE.md merging
- CLI tool for backup/restore operations

## Important Notes
- This command is often symlinked to `~/.claude/commands/` for global use
- Changes here propagate to all users who have symlinked
- Always update README.md when changing command behavior
- Session logs are in `~/.claude/projects/[encoded-path]/*.jsonl`
