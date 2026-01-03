# Example: Test Location Convention

## Scenario
Claude looks for tests in a `/tests` directory, but the project uses colocated `__tests__` folders.

## Conversation Snippet

```
User: Can you run the tests for the auth module?

Claude: I'll find and run the auth tests.
> Looking in /tests/auth...
No tests found at /tests/auth

User: Tests are colocated with the source. Look in src/auth/__tests__/

Claude: Found them! Running the tests.
> pnpm test src/auth/__tests__/
```

## Memento Output

```
┌─ Project Memory ──────────────────────────────────────────┐
│ ☑ Tests are colocated in __tests__/ folders next to       │
│   source files, not in a root /tests directory            │
└───────────────────────────────────────────────────────────┘
```

**Suggested entry:**
> Tests are colocated in `__tests__/` folders next to source files, not in a root `/tests` directory

**Target file:** `./CLAUDE.md` (Project Memory)

## Why It's Actionable

This is project-specific knowledge that:
- Tells Claude exactly where to find tests
- Prevents wasted time searching wrong locations
- Applies to this specific codebase structure

## Related Patterns

Similar conventions Memento might capture:
- "Unit tests use `.spec.ts`, integration tests use `.test.ts`"
- "Test fixtures are in `__fixtures__/` directories"
- "Run tests with `pnpm test:unit` not `npm test`"
