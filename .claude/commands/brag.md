---
description: Generate social content ideas from your coding sessions
---

# Brag

Generate a beautiful HTML page with **social content IDEAS** (not copy) from your Claude Code sessions.

**Usage:** `/brag [days] [--notion]`

**Arguments:**
- `days` - Number of days to scan (default: 14)
- `--notion` - Enable Notion integration to push moments as drafts (requires config)

**Examples:**
```
/brag          # Last 14 days, no Notion
/brag 7        # Last 7 days
/brag --notion # With Notion integration
/brag 30 --notion
```

---

## Step 1: Parse Arguments

### 1.1 Parse the command arguments
- Check for `--notion` flag → Store as `$NOTION_ENABLED` (true/false)
- Check for numeric argument → Store as `$DAYS` (default: 14)

### 1.2 If `--notion` flag is set, check for config
Look for Notion configuration in `~/.claude/CLAUDE.md`:

```markdown
## Brag Config
- NOTION_TOKEN: ntn_xxx
- NOTION_DATABASE_ID: xxx
```

If not found, inform the user:
```
⚠️ Notion integration requires config in ~/.claude/CLAUDE.md

Add this section:
## Brag Config
- NOTION_TOKEN: your_notion_integration_token
- NOTION_DATABASE_ID: your_database_id

Then run /brag --notion again.
```

---

## Step 2: Gather Data

### 2.1 Scan Session Files
Read all `.jsonl` files from `~/.claude/projects/*/` modified in the last `$DAYS` days.

For each session, extract:
- Project name (from folder path)
- User messages (content)
- Assistant tool calls (especially Bash, Write, Edit)
- Timestamps

### 2.2 Scan Git History
For each project with sessions, run:
```bash
git log --since="$DAYS days ago" --format="%h|%s|%ad" --date=short --stat
```

Extract: commit messages, files changed, conventional commit types

### 2.3 Scan Claude Customizations
Check for files created/modified in last `$DAYS` days:
- `~/.claude/commands/*.md`
- `~/.claude/skills/*/SKILL.md`
- `.claude/commands/*.md` (in each project)
- `.claude/skills/*/SKILL.md` (in each project)
- `.mcp.json` files

---

## Step 3: Pattern Match for Moments

Apply these patterns to find **candidate shareable moments**:

### Keyword Patterns (in user/assistant messages)
```
SHIPPED: deployed, published, released, shipped, launched, merged, went live, pushed to prod, PR approved, v1., v2.
FIXED: fixed, resolved, bug, error, root cause, the problem was, turns out, figured out, finally working, solved
DISCOVERED: learned, found out, discovered, realized, TIL, didn't know, interesting, cool trick, better way, game changer
BUILT: created, built, implemented, new component, scaffolded, set up, from scratch, MVP, prototype
STRUGGLED: stuck, confused, frustrated, hours later, took forever, nightmare, rabbit hole, yak shaving
AUTOMATED: automated, script, cron, scheduled, pipeline, CI/CD, workflow, hook, bot
REFACTORED: refactored, cleaned up, simplified, extracted, DRY, abstracted, modularized
OPTIMIZED: optimized, faster, performance, reduced, improved, cache, lazy load, bundle size
```

### Tool Call Signals
```
SHIPPED: git push, git commit, gh pr create, gh pr merge, npm publish, wrangler deploy, vercel, docker push
BUILT: Write tool on new file, mkdir, npm init, cargo new, git init
FIXED: Edit tool after error, multiple retries on same file
```

### Structure Signals
```
CORRECTION: Assistant says "actually", "I was wrong", user says "no" then assistant adjusts
CELEBRATION: User says "nice", "perfect", "awesome", "works"
BREAKTHROUGH: Long back-and-forth then sudden resolution
```

### Git Signals
```
HIGH VALUE: feat:, fix:, perf:, closes #, fixes #, breaking change
SIGNIFICANT: >5 files changed, >100 insertions
```

Create a list of candidate moments with:
- Category (shipped/fixed/discovered/built/struggled/automated/refactored/optimized)
- Snippet (key context, max 500 chars)
- Source (project name, session date)
- Git context (related commits if any)

---

## Step 4: LLM Analysis

For each candidate moment (max 20), analyze and generate:

### Shareability Check
- Is this genuinely interesting to others?
- Would a developer find this relatable/useful?
- Score 1-10

### Extract the Narrative
- What was the problem/goal?
- What was the journey?
- What was the outcome/lesson?

### Generate Platform Ideas

**LinkedIn Angle** (professional, story-driven):
- 3 potential angles/hooks
- Suggested format (story, listicle, lesson, behind-the-scenes)
- Key insight to highlight

**X/Twitter Angle** (punchy, engagement-focused):
- 3 potential hooks (hot take, thread idea, quick tip)
- Meme/humor potential
- Accounts to potentially tag

**IMPORTANT: Generate IDEAS and ANGLES only, not the actual post copy.**

---

## Step 5: Generate HTML

Create a self-contained HTML file with:

### Design Specs
- Modern, clean card-based layout
- Purple/blue gradient accents (Claude aesthetic)
- System fonts, good typography hierarchy
- Dark mode support (prefers-color-scheme)
- Mobile responsive

### Style Tips Section (Generic)
Include a collapsible style guide with general best practices:
```html
<div class="style-guide">
  <h3>Social Media Best Practices</h3>
  <ul>
    <li><strong>Keep it short</strong> - 1-3 sentences perform best</li>
    <li><strong>Use visuals</strong> - Screenshots, memes, or photos increase engagement</li>
    <li><strong>Ask questions</strong> - Drives comments and discussion</li>
    <li><strong>One emoji max</strong> - At the end, not scattered throughout</li>
    <li><strong>Hook first</strong> - Lead with the interesting part</li>
  </ul>
</div>
```

### Structure
```html
<!DOCTYPE html>
<html>
<head>
  <title>Session Insights → Social Ideas</title>
  <style>/* All CSS inline */</style>
</head>
<body>
  <header>
    <h1>🧠 Session Insights → Social Ideas</h1>
    <p>Last {$DAYS} days • {count} shareable moments found</p>
    <div class="filters"><!-- Category filters --></div>
  </header>

  <main>
    <!-- For each moment: -->
    <article class="moment-card" data-category="{category}">
      <div class="category-badge">{emoji} {CATEGORY}</div>
      <h2>{moment title/snippet}</h2>
      <p class="source">{project} • {date}</p>

      <details class="platform linkedin">
        <summary>💼 LinkedIn Angles</summary>
        <ul>
          <li>{angle 1}</li>
          <li>{angle 2}</li>
          <li>{angle 3}</li>
        </ul>
        <p class="format">Format: {suggested format}</p>
      </details>

      <details class="platform twitter">
        <summary>🐦 X/Twitter Angles</summary>
        <ul>
          <li>{angle 1}</li>
          <li>{angle 2}</li>
          <li>{angle 3}</li>
        </ul>
        <p class="tags">Consider tagging: {accounts}</p>
      </details>

      <!-- Only if $NOTION_ENABLED -->
      <button class="btn-notion" onclick="copyNotionCurl(...)">
        Push to Notion
      </button>
    </article>
  </main>

  <footer>
    <p>Generated by /brag • {timestamp}</p>
  </footer>
</body>
</html>
```

### CSS Requirements
- Cards with subtle shadows
- Smooth hover states
- Collapsible sections for each platform
- Category color coding
- Copy buttons for each angle
- Filter by category (JS)

### Notion Buttons (Only if `$NOTION_ENABLED`)
If Notion is enabled, include "Push to Notion" buttons that copy a curl command:
```javascript
function copyNotionCurl(title, content) {
  const curl = `curl -X POST 'https://api.notion.com/v1/pages' \\
    -H 'Authorization: Bearer YOUR_TOKEN' \\
    -H 'Notion-Version: 2022-06-28' \\
    -H 'Content-Type: application/json' \\
    -d '${JSON.stringify({
      parent: { database_id: "YOUR_DB_ID" },
      properties: {
        Title: { title: [{ text: { content: title } }] },
        Status: { select: { name: "Draft" } }
      },
      children: [{ object: "block", type: "paragraph", paragraph: { rich_text: [{ type: "text", text: { content } }] } }]
    })}'`;
  navigator.clipboard.writeText(curl);
  alert('Curl command copied! Paste in terminal to push to Notion.');
}
```

---

## Step 6: Save and Open

1. Save HTML to `~/.claude/brag-{timestamp}.html`
2. Open in default browser using:
   ```bash
   open ~/.claude/brag-{timestamp}.html  # macOS
   xdg-open ~/.claude/brag-{timestamp}.html  # Linux
   ```
3. Report summary:
   ```
   ✨ Generated social ideas from {X} sessions

   Found {N} shareable moments:
   - 🚀 {n} shipped
   - 🐛 {n} fixed
   - 💡 {n} discovered
   - 🔧 {n} built
   - 🤯 {n} struggled

   📄 Opened: ~/.claude/brag-{timestamp}.html
   ```

---

## Step 7: Notion Integration (Only if `--notion` flag)

**Skip this step if `$NOTION_ENABLED` is false.**

After generating the HTML and showing the summary:

### 7.1 Ask User
Use AskUserQuestion to ask:
- "Push any moments to Notion as drafts?"
- Options: "Yes, let me pick" / "No, just the HTML"

### 7.2 If Yes - Show Numbered List
Display the top moments as a numbered list:
```
Which moments to push to Notion? (comma-separated numbers)

1. 🚀 Feature X - Shipped to production
2. 🔧 Tool Y - Built from scratch
3. 💡 Discovery Z - TIL moment
```

### 7.3 Create Notion Drafts
For each selected moment, create a page using the configured database:

```bash
curl -X POST 'https://api.notion.com/v1/pages' \
  -H 'Authorization: Bearer {NOTION_TOKEN from config}' \
  -H 'Notion-Version: 2022-06-28' \
  -H 'Content-Type: application/json' \
  -d '{
    "parent": { "database_id": "{NOTION_DATABASE_ID from config}" },
    "properties": {
      "Title": {
        "title": [{ "text": { "content": "{moment_title}" } }]
      },
      "Status": {
        "select": { "name": "Draft" }
      }
    },
    "children": [
      {
        "object": "block",
        "type": "paragraph",
        "paragraph": {
          "rich_text": [{ "type": "text", "text": { "content": "{suggested_post_content}" } }]
        }
      },
      {
        "object": "block",
        "type": "divider",
        "divider": {}
      },
      {
        "object": "block",
        "type": "heading_3",
        "heading_3": {
          "rich_text": [{ "type": "text", "text": { "content": "Source Context" } }]
        }
      },
      {
        "object": "block",
        "type": "paragraph",
        "paragraph": {
          "rich_text": [{ "type": "text", "text": { "content": "Project: {project}\nDate: {date}\nCategory: {category}" } }]
        }
      }
    ]
  }'
```

### 7.4 Report Results
```
✅ Pushed {n} moments to Notion as drafts:
- "{Title 1}" → Draft
- "{Title 2}" → Draft

Open Notion to review and edit before publishing.
```

---

## Example Output Categories

### 🚀 SHIPPED
"Deployed feature X to production"
- LinkedIn: Launch story, building in public journey, problem it solves
- X: Demo GIF, "just shipped" energy, tag relevant accounts

### 🐛 FIXED
"Root caused a tricky bug - the issue was X"
- LinkedIn: Technical deep-dive, debugging war story, lesson learned
- X: Quick tip thread, "TIL" post, relatable frustration

### 💡 DISCOVERED
"Learned about feature Y for the first time"
- LinkedIn: Feature spotlight, workflow improvement
- X: Thread on cool feature, hot take on tooling

### 🔧 BUILT
"Created a custom tool/command"
- LinkedIn: Behind-the-scenes, building tools for builders
- X: Show the output, ask for feedback

### 🤯 STRUGGLED
"Spent hours on problem Z"
- LinkedIn: Honest reflection, lessons learned, growth mindset
- X: Relatable developer moment, debugging war story
