---
command_id: adr
version: v1
---

# adr

Create a new ADR for a module version.

## Usage

```
ragnor.dev adr <module>@<version> "<decision title>"
```

## What it does

Creates a new ADR file in `@specs/modules/<module>/<module>@<version>/adr/`
with the next sequential ID and the standard ADR template.

## ADR template

```markdown
# ADR-[NNNN]: [decision title]

Status: proposed | accepted | superseded

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
```

## Rules

- One-way decisions MUST have an ADR before implementation
- ADR IDs are monotonic per module version (0001, 0002, ...)
- ADRs are immutable once the module version is released
- See `30_module-bootstrap.md` ADR Usage section for full rules
- Prompt template: `prompts/adr-v1.md`
