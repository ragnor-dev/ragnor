---
spec_id: onboarding-external
type: framework-rule
version: v1
status: active
---

# OB_30 — External Source Onboarding

This document defines how to import knowledge from external sources
(Confluence, Jira, Notion, wikis, etc.) during brownfield onboarding.

External source import is an extension of brownfield onboarding.
Run `OB_20_onboarding-brownfield.md` first, then use this document
to enrich the draft specs with external knowledge.


------------------------------------------------------------
## 1. Supported External Sources
------------------------------------------------------------

| Source | What it provides | Trust level |
|---|---|---|
| Confluence | Architecture docs, decision records, design docs | HIGH |
| Jira | Feature intent, acceptance criteria, bug context | MEDIUM |
| Notion | Team documentation, product specs | MEDIUM |
| GitHub/GitLab Issues | Feature requests, bug reports, discussions | MEDIUM |
| PR descriptions | Implementation intent, review decisions | HIGH |
| Slack/Teams exports | Decision context, informal discussions | LOW |
| Email threads | Decision context | LOW |

Trust level affects how confidently inferred items are marked.
HIGH trust sources may produce confirmed spec items.
LOW trust sources always produce `[INFERRED]` items.


------------------------------------------------------------
## 2. Import Process
------------------------------------------------------------

### Step 1 — Identify available sources

List all external sources that contain knowledge about the system.
For each source, note:
- What type of content it contains
- How current it is (last updated)
- How trustworthy it is (was it maintained or abandoned?)

### Step 2 — Extract intent signals

For each source, extract:
- Feature descriptions and requirements
- Architectural decisions and rationale
- Acceptance criteria and test scenarios
- Known constraints and non-goals
- Open questions and unresolved decisions

### Step 3 — Map to framework artefacts

For each extracted item, determine where it belongs:

| Extracted item | Framework artefact |
|---|---|
| Feature description | `module-spec.md` purpose/responsibilities |
| Acceptance criteria | `module-spec.md` or `acceptance.md` |
| Architectural decision | `adr/` |
| Open question | `open-questions.md` |
| Known gap or debt | `todo.md` |
| Constraint or non-goal | `module-spec.md` non-goals |

### Step 4 — Enrich draft specs

Add extracted items to the draft specs created during brownfield onboarding.

Mark items by source trust level:
- HIGH trust: `[FROM: <source>]` — verified, can be confirmed
- MEDIUM trust: `[INFERRED FROM: <source>]` — needs human verification
- LOW trust: `[SUGGESTED FROM: <source>]` — treat as a hint only

### Step 5 — Human review

Same as brownfield onboarding Step 8 — a human MUST review all
imported items and confirm, correct, or flag each one.


------------------------------------------------------------
## 3. The Reverse Flow — Publishing Back to External Sources
------------------------------------------------------------

Once `@specs/` is established as the source of truth, teams may want
to publish specs back to their external tools for stakeholders who
do not work in the repository.

This is a derived view — the spec is the source, the external tool
is the view.

### 3.1 What can be published

- Module specs → Confluence pages, Notion docs
- System versions → architecture overview pages
- Roadmap → project roadmap views in Jira/Confluence
- ADRs → decision log pages
- Todo items → Jira tickets, GitHub issues

### 3.2 Publication rules

- The `@specs/` layer is always the source of truth
- External publications are read-only views
- Changes MUST be made in `@specs/` first, then published
- External tools MUST NOT be edited directly — changes go through the spec

### 3.3 Sync tooling

Automated sync between `@specs/` and external tools is a planned
capability. In v1, publication is manual or via custom scripts.

The sync direction is always: `@specs/` → external tool.
Never the reverse after initial import.

**Note:** Automated sync tooling between `@specs/` and external
platforms (Confluence, Jira, Notion) is a planned commercial feature.
The framework itself is free. The sync tooling is a service.


------------------------------------------------------------
## 4. Relationship to Other Documents
------------------------------------------------------------

- `OB_20_onboarding-brownfield.md` — run this first
- `80_drift-detection.md` — external sources can reveal drift
- `90_roadmap.md` — Jira epics and milestones map to roadmap entries
