<p align="center">
<pre align="center">
███╗   ███╗███████╗███╗   ███╗███████╗███╗   ██╗████████╗ ██████╗
████╗ ████║██╔════╝████╗ ████║██╔════╝████╗  ██║╚══██╔══╝██╔═══██╗
██╔████╔██║█████╗  ██╔████╔██║█████╗  ██╔██╗ ██║   ██║   ██║   ██║
██║╚██╔╝██║██╔══╝  ██║╚██╔╝██║██╔══╝  ██║╚██╗██║   ██║   ██║   ██║
██║ ╚═╝ ██║███████╗██║ ╚═╝ ██║███████╗██║ ╚████║   ██║   ╚██████╔╝
╚═╝     ╚═╝╚══════╝╚═╝     ╚═╝╚══════╝╚═╝  ╚═══╝   ╚═╝    ╚═════╝
</pre>
</p>

<p align="center">
  <strong>Memory tools for Claude Code</strong><br>
  <em>Because Claude forgets, but your notes don't.</em>
</p>

<p align="center">
  <a href="https://github.com/SeanZoR/claude-memento/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License"></a>
  <a href="https://github.com/SeanZoR/claude-memento/stargazers"><img src="https://img.shields.io/github/stars/SeanZoR/claude-memento?style=social" alt="GitHub stars"></a>
  <a href="https://claude.ai"><img src="https://img.shields.io/badge/Claude-Code-blueviolet" alt="Claude Code"></a>
</p>

---

## Install

```bash
mkdir -p ~/.claude/commands && \
curl -fsSL https://raw.githubusercontent.com/SeanZoR/claude-memento/main/.claude/commands/memento.md -o ~/.claude/commands/memento.md && \
curl -fsSL https://raw.githubusercontent.com/SeanZoR/claude-memento/main/.claude/commands/brag.md -o ~/.claude/commands/brag.md
```

---

## Commands

### `/memento` — Extract session memories

Run at the end of any Claude Code session to capture learnings into your CLAUDE.md files.

```
/memento
```

**What it does:**
- Analyzes your conversation for mistakes, corrections, and preferences
- Filters for actionable insights only
- Sorts into Project Memory (`./CLAUDE.md`) vs User Memory (`~/.claude/CLAUDE.md`)
- Lets you pick which suggestions to keep

**Example:**
```
You: Install the dependencies
Claude: npm install
You: No, use pnpm in this project
```
→ Extracts: *"Use pnpm instead of npm for package management"*

---

### `/brag` — Generate social content ideas

Scan your recent sessions for shareable moments and generate content ideas for LinkedIn and X/Twitter.

```
/brag [days] [--notion]
```

| Argument | Description |
|----------|-------------|
| `days` | Number of days to scan (default: 14) |
| `--notion` | Push selected moments to Notion as drafts |

**Examples:**
```
/brag           # Last 14 days
/brag 7         # Last 7 days
/brag --notion  # With Notion integration
```

**What it finds:**
- 🚀 **Shipped** — Deployments, launches, PRs merged
- 🐛 **Fixed** — Bugs squashed, issues resolved
- 💡 **Discovered** — TILs, new learnings
- 🔧 **Built** — New tools, features, commands
- 🤯 **Struggled** — Debugging war stories, rabbit holes

**Output:** Opens an interactive HTML page with content angles for each platform.

<details>
<summary>Notion Integration Setup</summary>

Add to `~/.claude/CLAUDE.md`:

```markdown
## Brag Config
- NOTION_TOKEN: your_notion_integration_token
- NOTION_DATABASE_ID: your_database_id
```

Then use `/brag --notion` to push moments as drafts.
</details>

---

## Why Memento?

**Claude has no memory between sessions.** Every new conversation starts from scratch. You've probably caught yourself repeating:

- *"Use pnpm, not npm"*
- *"Tests go in `__tests__/` folders"*
- *"Always show the command before running it"*

Memento fixes this by helping you build up CLAUDE.md files that Claude automatically reads at the start of each session.

> *Like Leonard in the film [Memento](https://en.wikipedia.org/wiki/Memento_(film)), Claude has no memory. This tool helps you leave notes for future Claude.*

---

## How CLAUDE.md Works

Claude Code automatically reads these files at session start:

| File | Scope |
|------|-------|
| `./CLAUDE.md` | Project-specific conventions |
| `~/.claude/CLAUDE.md` | Your preferences across all projects |

Memento intelligently categorizes suggestions to the right file.

---

## Roadmap

- [x] `/memento` — Session memory extraction
- [x] `/brag` — Social content ideas
- [ ] **Memento MCP** — Retrospective analysis across all past sessions
- [ ] **Memento Sync** — Backup and restore memories across machines

---

## Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## License

MIT — see [LICENSE](LICENSE)

<p align="center">
  <sub>Built with 🧠 by <a href="https://github.com/SeanZoR">Sean</a></sub>
</p>
