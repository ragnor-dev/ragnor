---
spec_id: authoring-guidelines
type: framework-spec
version: v1
status: active
---

Authoring Guidelines (MVP)
-------------------------

This document defines what content belongs in each artifact and
when to split content out of module-spec.md.

1. General Principles
---------------------

- module-spec.md is the canonical entry point
- Supporting artifacts exist to preserve clarity and diffability
- Split files only when clarity improves
- Do not create empty placeholder files

2. Module Version Artifacts
---------------------------

Location:
@specs/modules/<module>/<module>@vN/

2.1 module-spec.md (stable intent)

MUST contain:
- purpose / intent
- responsibilities / scope
- acceptance criteria (brief is acceptable)

SHOULD contain when relevant:
- non-goals
- contract summary
- dependency/boundary summary
- changes vs superseded version
- links to split artifacts

MUST NOT become a work log.

2.2 todo.md (intent gap ledger)

- Tracks remaining work to satisfy declared intent
- Markdown checklist only
- Stable IDs required
- No owners, dates, priorities, or workflows

2.3 acceptance.md (detailed verification)

Use when:
- acceptance exceeds what fits in module-spec.md
- scenarios or edge cases matter

Contains:
- detailed criteria
- test scenarios and negative cases

2.4 context.md (boundaries and dependencies)

Use when:
- dependencies are non-trivial
- layering or forbidden dependencies exist

Contains:
- owned responsibilities
- depends on + justification
- provides to + contract
- optional constraints

2.5 risks.md

Use when risks are meaningful and version-specific.
Avoid generic filler.

2.6 impact-analysis.md

Use when:
- migration, rollout, or compatibility is affected

2.7 open-questions.md

For true unknowns only.
Actionable work belongs in todo.md.

2.8 metrics.yaml

Only include signals you intend to measure.
Empty signals list is acceptable.

3. When to Split Content
------------------------

Split when:
- the section hides the entry-point value
- content changes at a different rate
- focused review is needed
- duplication would occur

Otherwise keep content in module-spec.md.

4. System Version Artifacts (Brief)
-----------------------------------

Location:
@specs/system/system@vN/

- system.yaml: authoritative composition
- overview.md: human explanation
- intent.md: why this snapshot exists
- todo.md: system-level gaps
- acceptance.md: system guarantees
- risks.md: system risks
- impact-analysis.md: changes vs previous version
- metrics.yaml: system signals
- adr/: rare, system-level decisions only