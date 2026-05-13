---
command_id: version
version: v1
---

# version

Bump a module to a new version.

## Usage

```
ragnor.dev version <module>@<current-version>
```

## What it does

Creates `<module>@v(N+1)` by:
1. Creating the new version directory
2. Copying contents from the current version as baseline
3. Updating front matter (version, supersedes)
4. Clearing todo.md for review
5. Marking the current version as immutable

## When to use

Use when a version bump trigger is met (see `50_version-operations.md` section 3.4):
- Current version is released
- Spec meaning changes
- Acceptance criteria must be rewritten
- Dependency or boundary changes
- Structural rewrite invalidates spec-to-code mapping

## Rules

- Do not bump versions for wording cleanup or closing todo items
- The new version starts as draft
- The previous version becomes immutable
- See `50_version-operations.md` for full version operation rules
- Prompt template: `prompts/version-v1.md`
