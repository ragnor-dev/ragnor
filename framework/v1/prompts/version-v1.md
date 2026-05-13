---
prompt_id: version-v1
implements: version-operations
version: v1
agent: generic
---

# Version Prompt

Use this prompt to create the next module version (`vN` -> `vN+1`).
Run only after human confirmation that a version bump is required.

---

## Prompt

```text
You are running a module version bump for ragnor.dev.

Target: <module>@<current-version>
Branch: <current git branch>

Step 1 — Validate source
- Confirm source exists:
  `@specs/modules/<module>/<module>@<current-version>/`
- Confirm `module-spec.md`, `todo.md`, and `adr/` exist in source.
- If source is missing, STOP and report `SOURCE-NOT-FOUND`.

Step 2 — Compute next version
- Parse `<current-version>` as `vN`
- Next version is `v(N+1)`
- New path:
  `@specs/modules/<module>/<module>@v(N+1)/`
- If destination already exists, STOP and report `DESTINATION-EXISTS`.

Step 3 — Create new version baseline
- Create destination directory
- Copy source contents to destination
- Update destination `module-spec.md` front matter:
  - `version: v(N+1)`
  - `supersedes: vN`
  - `status: draft` (if present, set to draft)

Step 4 — Reset review artefacts in new version
- Clear or rewrite destination `todo.md` for fresh review
- Review `open-questions.md` and `acceptance.md` if present; keep only still-valid content

Step 5 — Preserve immutability rules
- Do not modify released content in place
- Do not mutate historical meaning of previous version

Step 6 — Output report
VERSION BUMP REPORT
Module: <module>
From: <current-version>
To: <next-version>
Created: <new version path>
Updated:
- <file paths changed in new version>
Status: SUCCESS | SOURCE-NOT-FOUND | DESTINATION-EXISTS | FAILED
Notes:
- <note> or NONE
```

