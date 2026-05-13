---
prompt_id: update-check-v1
implements: update-check
version: v1
agent: generic
---

# Update Check Prompt

Run automatically at the start of every AI session after the bootstrap gate passes,
and on demand via `ragnor.dev update-check`.

---

## Prompt

```text
You are running the ragnor.dev framework update check.

Step 1 — Read local version
- Read the file at `@specs/framework/v1.md`
- Extract the value of `framework_version:` from the file header
- Extract the value of `latest_url:` from the file header
- If either field is missing, STOP and report UPDATE-CHECK-SKIPPED (missing metadata)

Step 2 — Fetch latest version
- Fetch the content at the URL from `latest_url`
- Extract the `framework_version:` value from the fetched content
- If the fetch fails (network unavailable, timeout, error), STOP silently and continue the session

Step 3 — Compare versions
- Parse both versions as `v<major>.<build>` (e.g. `v1.42`)
- If local build >= latest build: output nothing and continue the session
- If latest build > local build: proceed to Step 4

Step 4 — Produce diff summary
- Compare the fetched content against the local content
- Summarise what changed at a high level (new sections, modified rules, new commands)
- Do not produce a raw line diff — produce a human-readable summary

Step 5 — Present update offer
Output exactly:

FRAMEWORK UPDATE AVAILABLE
Current:  <local framework_version>
Latest:   <latest framework_version>
URL: <latest_url>

WHAT CHANGED (<local> → <latest>):
- <change summary line 1>
- <change summary line 2>

OPTIONS:
  1. Update now — replace @specs/framework/v1.md with the latest version
  2. Skip — continue with current version
  3. Remind me later — suppress for this session only

Awaiting your choice (1/2/3):

Step 6 — Handle response
- If 1: replace `@specs/framework/v1.md` with the fetched content, report UPDATE-APPLIED, continue session
- If 2: report UPDATE-SKIPPED, continue session
- If 3: report UPDATE-DEFERRED, continue session without prompting again this session
```
