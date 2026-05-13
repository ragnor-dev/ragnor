---
command_id: update-check
version: v1
---

# update-check

Check whether a newer version of the ragnor.dev framework is available.

## Usage

```
ragnor.dev update-check
```

## When it runs

- Automatically at the start of every AI session, after the bootstrap gate passes
- Explicitly on demand via `ragnor.dev update-check`
- Automatically as part of `ragnor.dev bootstrap` and `ragnor.dev status`

## What it does

1. Reads `framework_version` from the local `@specs/framework/v1.md`
2. Fetches the latest `framework_version` from the canonical URL declared in `latest_url` of that same file
3. Compares the two versions
4. If a newer version exists: reports the version delta, shows a diff summary, and offers to download and apply the update
5. If up to date: silent — does not interrupt the session

## Output when update is available

```
FRAMEWORK UPDATE AVAILABLE
Current:  v1.42
Latest:   v1.57
URL: https://raw.githubusercontent.com/ragnor-dev/ragnor/main/versions/v1.md

WHAT CHANGED (v1.42 → v1.57):
- <summary of additions or changes between versions>

OPTIONS:
  1. Update now — replace @specs/framework/v1.md with the latest version
  2. Skip — continue with current version
  3. Remind me later — suppress for this session only

Awaiting your choice (1/2/3):
```

## Rules

- Requires network access to fetch the latest version header
- If network is unavailable, skip silently and continue
- Never update automatically without explicit human confirmation (option 1)
- The update replaces only `@specs/framework/v1.md` — no other files are modified
- Prompt template: `prompts/update-check-v1.md`
