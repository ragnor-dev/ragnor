---
command_id: status
version: v1
---

# status

Show current spec status across all modules and the system.

## Usage

```
ragnor.dev status
ragnor.dev status <module>
```

## What it does

Produces a status summary showing:
- All module versions and their status (draft/released)
- Open todo items per module
- Current system version
- Roadmap current items
- Any bootstrap gate failures

## Output format

```
ragnor.dev STATUS REPORT
Repository: <repo name>
Branch: <current branch>
Framework: v1

SYSTEM
  system@v1 — draft
  Modules: auth@v1, billing@v1, dashboard@v1

MODULES
  auth@v1 — draft
    Open todos: 3
    Last drift check: <date or never>

  billing@v1 — draft
    Open todos: 7
    Last drift check: <date or never>

ROADMAP
  Current: auth@v2 (OAuth support)
  Planned: billing@v2, system@v2

BOOTSTRAP GATE
  ✓ All checks pass
```

## Rules

- Status is read-only reporting
- Does not modify any files
- Prompt template: `prompts/status-v1.md`
