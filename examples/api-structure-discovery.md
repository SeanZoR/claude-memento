# Example: API Structure Discovery

## Scenario
Claude searches for API endpoints in the wrong location, and the user guides them to the correct structure.

## Conversation Snippet

```
User: Add a new endpoint for user notifications

Claude: I'll add the notification endpoint. Let me find the routes...
> Looking in src/api/...
> Looking in routes/...

User: Endpoints are in src/routes/v1/ - we follow a versioned structure

Claude: Got it! I see the pattern now.
> Created src/routes/v1/notifications.ts
```

## Memento Output

```
┌─ Project Memory ──────────────────────────────────────────┐
│ ☑ API endpoints are in src/routes/v1/ following a         │
│   versioned routing structure                             │
└───────────────────────────────────────────────────────────┘
```

**Suggested entry:**
> API endpoints are in `src/routes/v1/` following a versioned routing structure

**Target file:** `./CLAUDE.md` (Project Memory)

## Why It's Actionable

This captures structural knowledge:
- Tells Claude exactly where to look
- Reveals the versioning pattern (v1/)
- Prevents searching in wrong directories

## Related Patterns

Project structure discoveries often include:
- "Controllers are in `src/controllers/`, services in `src/services/`"
- "Database models are in `prisma/schema.prisma`"
- "Config files are in `config/` not root directory"
