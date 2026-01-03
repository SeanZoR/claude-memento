<h1 align="center">Memento</h1>

<p align="center">
  <strong>Extract session memories into CLAUDE.md</strong><br>
  <em>Because Claude forgets, but your notes don't.</em>
</p>

<p align="center">
  <a href="https://github.com/SeanZoR/memento/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License"></a>
  <a href="https://github.com/SeanZoR/memento/stargazers"><img src="https://img.shields.io/github/stars/SeanZoR/memento?style=social" alt="GitHub stars"></a>
  <a href="https://claude.ai"><img src="https://img.shields.io/badge/Claude-Code-blueviolet" alt="Claude Code"></a>
</p>

<p align="center">
  <a href="#-quick-start">Quick Start</a> •
  <a href="#-how-it-works">How It Works</a> •
  <a href="#-examples">Examples</a> •
  <a href="#-roadmap">Roadmap</a> •
  <a href="#-contributing">Contributing</a>
</p>

---

## The Problem

**Claude has no memory between sessions.** Every time you start a new conversation, it's a blank slate. You've probably noticed yourself repeating the same instructions:

- *"Use pnpm, not npm"*
- *"Tests go in `__tests__/` folders"*
- *"I prefer TypeScript over JavaScript"*

Sound familiar?

## The Solution

**Memento** is a Claude Code command that analyzes your coding sessions and extracts actionable insights into your `CLAUDE.md` files. Future Claude sessions automatically read these files, giving Claude "memory" of your preferences and project conventions.

> *Like Leonard in the film [Memento](https://en.wikipedia.org/wiki/Memento_(film)), Claude has no memory. This tool helps you leave notes for future Claude.*

---

## Quick Start

### Installation

```bash
# Clone the repository
git clone https://github.com/SeanZoR/memento.git

# Install globally (recommended)
cp memento/.claude/commands/memento.md ~/.claude/commands/

# Or install for a specific project
cp memento/.claude/commands/memento.md /path/to/your/project/.claude/commands/
```

### Usage

At the end of any Claude Code session, simply run:

```
/memento
```

That's it! Memento will analyze your conversation and present actionable suggestions.

---

## How It Works

### 1. Analyze
Memento reviews your conversation looking for:
- Mistakes Claude made and corrections you provided
- Preferences you expressed (explicit or implicit)
- Project conventions discovered
- Reusable learnings

### 2. Categorize
Suggestions are filtered for **actionability** and sorted into:

| Category | Location | Purpose |
|----------|----------|---------|
| **Project Memory** | `./CLAUDE.md` | Conventions specific to this codebase |
| **User Memory** | `~/.claude/CLAUDE.md` | Preferences across all projects |

### 3. Select & Apply
You choose which suggestions to keep via an interactive multi-select prompt:

```
┌─ Project Memory ──────────────────────────────────────────┐
│ ☑ Use pnpm instead of npm for package management          │
│ ☑ Run tests with `pnpm test:unit` not `npm test`          │
│ ☐ API endpoints are in src/routes/                        │
└───────────────────────────────────────────────────────────┘

┌─ User Memory ─────────────────────────────────────────────┐
│ ☑ Always show the command before executing it             │
│ ☐ Prefer functional components over class components      │
└───────────────────────────────────────────────────────────┘
```

Selected suggestions are appended to the appropriate `CLAUDE.md` file.

---

## What Makes a Good Suggestion

Memento is designed to surface **actionable** insights - things that will actually change Claude's behavior in future sessions:

| ✅ Actionable | ❌ Not Actionable |
|--------------|-------------------|
| "Use pnpm, not npm in this project" | "Check package manager" |
| "Tests are in `__tests__/` folders" | "This was a good session" |
| "Always show command before running" | "Be more careful" |
| "This project uses Tailwind CSS" | "Consider using CSS frameworks" |
| "Database migrations are in `prisma/`" | "Database stuff is important" |

### The Actionability Test

A suggestion is actionable if it:
1. **Changes behavior** - Would make Claude do something differently
2. **Is specific** - Clear enough to apply without interpretation
3. **Prevents mistakes** - Would have avoided an error in this session

---

## Examples

### Example 1: Package Manager Correction

**Session snippet:**
```
User: Install the dependencies
Claude: npm install
User: No, use pnpm in this project
Claude: pnpm install
```

**Memento suggestion:**
> ✅ Project Memory: "Use pnpm instead of npm for package management"

### Example 2: Testing Convention

**Session snippet:**
```
User: Where are the tests?
Claude: Looking in /tests...
User: They're in __tests__ folders next to the source files
```

**Memento suggestion:**
> ✅ Project Memory: "Tests are colocated in `__tests__/` folders, not a root `/tests` directory"

### Example 3: User Preference

**Session snippet:**
```
User: Wait, show me the command before you run it
Claude: Sure! Here's what I'll run: git push origin main
```

**Memento suggestion:**
> ✅ User Memory: "Always display commands before executing them"

See more examples in the [examples/](examples/) directory.

---

## Architecture

```
memento/
├── .claude/
│   └── commands/
│       └── memento.md      # The command implementation
├── docs/
│   ├── architecture.md     # Technical deep-dive
│   └── assets/             # Images and diagrams
├── examples/               # Example scenarios
└── README.md
```

### How CLAUDE.md Works

Claude Code automatically reads `CLAUDE.md` files at the start of each session:

1. **Project-level** (`./CLAUDE.md`) - Instructions for the current codebase
2. **User-level** (`~/.claude/CLAUDE.md`) - Your personal preferences across all projects

Memento intelligently categorizes suggestions to the appropriate file based on whether the insight is project-specific or universal.

---

## Roadmap

We're building Memento into a comprehensive memory toolkit for Claude Code:

### Current
- [x] `/memento` command for end-of-session memory extraction

### Planned
- [ ] **Memento MCP Server** - Retrospective analysis across all past sessions
- [ ] **Memento Dashboard** - Web UI to browse and manage memories
- [ ] **Memento Sync** - Backup and restore your Claude memories across machines
- [ ] **Memento Skills** - Pre-built memory extraction for specific workflows

See the [roadmap discussion](https://github.com/SeanZoR/memento/discussions) for more details and to contribute ideas.

---

## Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Ways to Contribute
- Report bugs and suggest features
- Improve documentation
- Add example scenarios
- Help build the MCP server and dashboard

---

## FAQ

<details>
<summary><strong>Why not just edit CLAUDE.md manually?</strong></summary>

You can! Memento is designed to make it easier by:
- Surfacing insights you might forget to document
- Formatting suggestions consistently
- Categorizing into project vs. user memory automatically
</details>

<details>
<summary><strong>Does this send my data anywhere?</strong></summary>

No. Memento runs entirely locally within your Claude Code session. It analyzes your conversation and writes to local files. No data is sent to external servers.
</details>

<details>
<summary><strong>Can I customize what Memento looks for?</strong></summary>

The current version uses a fixed analysis prompt. Customization options are planned for future releases.
</details>

<details>
<summary><strong>What if I accidentally add something wrong?</strong></summary>

Simply edit the `CLAUDE.md` file directly to remove or modify any entry. It's just a markdown file.
</details>

---

## Acknowledgments

- Inspired by the film [Memento](https://en.wikipedia.org/wiki/Memento_(film)) (2000)
- Built for [Claude Code](https://claude.ai/code) by Anthropic
- Thanks to all contributors and early adopters

---

## License

MIT License - see [LICENSE](LICENSE) for details.

---

<p align="center">
  <sub>Built with 🧠 by <a href="https://github.com/SeanZoR">Sean</a></sub>
</p>
