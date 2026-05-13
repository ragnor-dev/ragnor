---
command_id: help
version: v1
---

# help

List all available ragnor.dev commands and what they do.

## Usage

```
ragnor.dev help
ragnor.dev help <command>
```

## Commands

| Command | Description |
|---|---|
| `bootstrap` | Initialise `@specs/` in a new or existing repository |
| `intake` | Run the idea-to-spec intake process for a new module |
| `drift` | Run drift detection for a module version |
| `validate` | Run post-implementation validation for a module version |
| `status` | Show current spec status across all modules |
| `adr` | Create a new ADR for a module version |
| `version` | Bump a module to a new version |

## How to invoke

In any AI agent conversation, prefix commands with the framework name:

```
ragnor.dev help
ragnor.dev intake "build a user authentication system"
ragnor.dev drift auth@v1
ragnor.dev validate auth@v1
ragnor.dev status
```

Or in natural language:

```
help me ragnor.dev
run ragnor.dev intake for my new idea
check drift on the auth module
```

## Framework files

In adopter repositories:
- `@specs/framework.md` is the stable pointer to the adopted framework snapshot.
- `@specs/framework/` contains immutable versioned snapshots.

Read `AGENTS.md` at the repository root for AI agent entry point.
