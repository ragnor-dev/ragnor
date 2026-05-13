---
prompt_id: adr-v1
implements: ai-execution-rules-v1
version: v1
agent: generic
---

# ADR Prompt

Use this prompt to create a new ADR for a module version.
Run when a one-way decision must be recorded.

---

## Prompt

```text
You are creating an ADR for ragnor.dev.

Target module version: [module]@[version]
Decision title: [title]
Branch: [current git branch]

Step 1 — Validate target
- Confirm path exists: `@specs/modules/[module]/[module]@[version]/`
- Confirm `adr/` directory exists (create only if missing and version is mutable)
- If target version is immutable/released, STOP and report `IMMUTABLE-TARGET`.

Step 2 — Determine next ADR ID
- Enumerate existing ADR files in:
  `@specs/modules/[module]/[module]@[version]/adr/`
- Extract numeric IDs from `ADR-0001`, `ADR-0002`, ...
- Choose next monotonic ID (`NNNN`, zero-padded to 4 digits)

Step 3 — Create ADR file
Path:
`@specs/modules/[module]/[module]@[version]/adr/[NNNN]-[kebab-title].md`

Content template:

# ADR-[NNNN]: [decision title]

Status: proposed

Context:
[what situation led to this decision]

Decision:
[what was decided]

Consequences:
[what this means going forward]

Alternatives considered:
- [option 1] — [why not chosen]
- [option 2] — [why not chosen]

Links:
- [related ADR or spec]

Step 4 — Output report
ADR CREATION REPORT
Module: [module]@[version]
Created: [adr file path]
ADR ID: [NNNN]
Status: SUCCESS | IMMUTABLE-TARGET | FAILED
Notes:
- [note] or NONE
```
