# Brag

Generate social media content ideas from your Claude Code sessions.

## Overview

`/brag` scans your recent Claude Code sessions and identifies "shareable moments" - things you shipped, fixed, built, discovered, or struggled with. It then generates a beautiful HTML page with content ideas for LinkedIn and X/Twitter.

## Installation

Included with Memento. See the main [README](../README.md) for installation instructions.

## Usage

```
/brag [days] [--notion]
```

### Arguments

| Argument | Description | Default |
|----------|-------------|---------|
| `days` | Number of days to scan | 14 |
| `--notion` | Enable Notion integration | disabled |

### Examples

```bash
# Scan last 14 days (default)
/brag

# Scan last 7 days
/brag 7

# Scan 30 days with Notion integration
/brag 30 --notion
```

## How It Works

### 1. Data Gathering

Brag reads from multiple sources:

- **Session files** (`~/.claude/projects/*/`) - Your conversations with Claude
- **Git history** - Commits from projects with sessions
- **Claude customizations** - Commands, skills, MCP configs you created

### 2. Pattern Matching

It looks for specific signals that indicate shareable content:

| Category | Keywords |
|----------|----------|
| **Shipped** | deployed, published, released, merged, went live |
| **Fixed** | fixed, resolved, bug, root cause, finally working |
| **Discovered** | learned, TIL, didn't know, game changer |
| **Built** | created, built, implemented, from scratch, MVP |
| **Struggled** | stuck, hours later, rabbit hole, yak shaving |
| **Automated** | automated, script, cron, pipeline, workflow |

### 3. Analysis

Each candidate moment is analyzed for:

- **Shareability** - Is this interesting to others? (1-10 score)
- **Narrative** - Problem, journey, outcome
- **Platform fit** - LinkedIn vs Twitter angles

### 4. HTML Generation

A self-contained HTML page is generated with:

- Card-based layout with category badges
- Collapsible LinkedIn and X/Twitter angle sections
- Category filters
- Copy buttons for each suggestion
- Dark mode support

## Output

The HTML file is saved to `~/.claude/brag-{timestamp}.html` and opened in your default browser.

### Example Output

```
✨ Generated social ideas from 42 sessions

Found 15 shareable moments:
- 🚀 5 shipped
- 🐛 3 fixed
- 💡 4 discovered
- 🔧 2 built
- 🤯 1 struggled

📄 Opened: ~/.claude/brag-20260109-143022.html
```

## Notion Integration

Push selected moments to a Notion database as draft content.

### Setup

1. Create a Notion integration at [notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Share your database with the integration
3. Add to your `~/.claude/CLAUDE.md`:

```markdown
## Session-Social Config
- NOTION_TOKEN: ntn_your_integration_token
- NOTION_DATABASE_ID: your_database_id
```

### Database Schema

Your Notion database should have:

| Property | Type | Description |
|----------|------|-------------|
| Title | Title | Moment title |
| Status | Select | Options: Draft, Ready, Published |

Additional properties are optional but useful:
- Content (Rich text)
- Category (Select)
- Source Project (Text)
- Date (Date)

### Usage with Notion

```bash
/brag --notion
```

After generating the HTML, you'll be prompted to select moments to push to Notion.

## Moment Categories

### 🚀 Shipped
Things you deployed, published, or released.

**LinkedIn angles:**
- Launch story
- Building in public journey
- Problem it solves

**X/Twitter angles:**
- Demo GIF
- "Just shipped" energy
- Tag relevant accounts

### 🐛 Fixed
Bugs you squashed, issues you resolved.

**LinkedIn angles:**
- Technical deep-dive
- Debugging war story
- Lesson learned

**X/Twitter angles:**
- Quick tip thread
- "TIL" post
- Relatable frustration

### 💡 Discovered
Things you learned, new insights.

**LinkedIn angles:**
- Feature spotlight
- Workflow improvement
- Knowledge share

**X/Twitter angles:**
- Thread on cool feature
- Hot take
- "Did you know?"

### 🔧 Built
Tools, commands, features you created.

**LinkedIn angles:**
- Behind-the-scenes
- Building tools for builders
- Technical walkthrough

**X/Twitter angles:**
- Show the output
- Ask for feedback
- Open source announcement

### 🤯 Struggled
Problems that took forever, rabbit holes.

**LinkedIn angles:**
- Honest reflection
- Growth mindset
- Lessons learned

**X/Twitter angles:**
- Relatable developer moment
- Debugging war story
- "Hours of my life" humor

## Best Practices

### For Better Results

1. **Be descriptive in sessions** - The more context in your conversations, the better moments Brag can find
2. **Use conventional commits** - `feat:`, `fix:`, `perf:` help identify significant work
3. **Celebrate wins** - Saying "nice!", "works!", "shipped!" creates positive signals

### For Social Posts

Brag generates **ideas**, not copy. When writing your actual posts:

- Keep it short (1-3 sentences ideal for LinkedIn)
- Add a visual (screenshot, meme, GIF)
- Ask a question to drive engagement
- Be authentic - add your own voice

## Troubleshooting

### No moments found

- Check that you have sessions in `~/.claude/projects/*/`
- Try increasing the days: `/brag 30`
- Your sessions might not have shareable patterns - that's okay!

### Notion push fails

- Verify your token has access to the database
- Check the database ID is correct (it's in the URL)
- Ensure the database has Title and Status properties

### HTML doesn't open

- Check if the file was created: `ls ~/.claude/brag-*.html`
- Try opening manually: `open ~/.claude/brag-*.html`

## Privacy

Brag runs entirely locally. It:

- Only reads your local session files
- Does not send data to external servers
- Generates a local HTML file
- Notion push is optional and only when requested

Your session history stays on your machine.
