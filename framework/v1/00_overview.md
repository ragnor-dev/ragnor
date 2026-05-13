---
spec_id: overview
type: framework-spec
version: v1
status: active
---

# ragnor.dev Framework – v1 Overview

This document defines the core concepts, repository model, versioning model,
supporting artefacts, and metadata conventions of the ragnor.dev framework.

All humans and AI agents working within a repository that adopts this framework
MUST treat this document as the authoritative conceptual reference.

Operational rules (what to do, when, and how) are defined in the linked
specification files. This document explains the *why* and the *what*.


------------------------------------------------------------
## 1. Problem
------------------------------------------------------------

Software knowledge is fragmented:

- Implementation lives in Git
- Work planning lives in issue trackers
- Architecture and decisions live in wikis, or nowhere
- Critical context lives in people's heads
- AI coding assistants hallucinate without grounded context

The result: the codebase drifts from its declared intent. Decisions get
re-argued. New team members and AI agents lack the context to contribute
safely.


------------------------------------------------------------
## 2. Core Idea
------------------------------------------------------------

ragnor.dev introduces a **specification layer inside the repository** that
becomes the shared, versioned source of truth for:

- developers, testers, architects, and product owners
- AI coding assistants and automated validation tools
- any downstream publication target (wikis, docs sites, Confluence, etc.)

The repository is the source. Everything else is a derived view.

This is the same model as infrastructure-as-code (the YAML is the truth,
the cloud console is a view) applied to architecture, decisions, and intent.

Key properties:
- **Repository-native** — specs live next to the code they describe
- **Diffable** — every spec change is a PR, reviewable and auditable
- **AI-native** — agents get grounded context by cloning the repo
- **Tool-agnostic** — no required tooling; plain Markdown and YAML
- **Publication-ready** — specs can be extracted and pushed to any target


------------------------------------------------------------
## 3. Repository Structure
------------------------------------------------------------

All spec artefacts live under a single root:

    @specs/

Top-level governance files:

- `@specs/specs-meta.md`  — canonical structure and conventions (authoritative)
- `@specs/glossary.md`    — shared terminology
- `@specs/architecture.md`— high-level system architecture view
- `@specs/roadmap.md`     — planned future work
- `@specs/framework.md`   — adopted framework snapshot pointer

Version directories:

- `@specs/system/system@vN/`                        — system versions
- `@specs/modules/<module>/<module>@vN/`            — module versions
- `@specs/framework/framework-v<major>.<minor>.md`  — immutable framework snapshots

Framework documentation:

- `framework/vN/`                                  — framework rules in the framework source repository


------------------------------------------------------------
## 4. Two-Level Versioning Model
------------------------------------------------------------

The framework uses two types of versioned units:

## 4.1 Module Versions

A module version represents the specification of a single domain boundary
at a point in time.

Location:

    @specs/modules/<module>/<module>@vN/

A module owns a coherent responsibility, exposes a defined contract, and
evolves independently of other modules.

Modules are the primary unit of authoring. Most day-to-day spec work
happens at the module level.

## 4.2 System Versions

A system version is an immutable snapshot of a module composition — a
declaration of which module versions are considered coherent together.

Location:

    @specs/system/system@vN/

System versions do not redefine module specs. They compose them.

Example (`system.yaml`):

    type: system
    version: v3
    modules:
      auth: v2
      billing: v1
      dashboard: v4

Meaning: system@v3 consists of auth@v2 + billing@v1 + dashboard@v4.

## 4.3 Relationship Between Module and System Versions

- Module versions are the canonical source of truth for each module.
- System versions explicitly reference specific module versions.
- Modules evolve independently; a module advancing does not automatically
  update any system version.
- System versions act as coordinated, pinned snapshots.

See: version-operations.md sections 3.4 and 4.4 for versioning triggers.


------------------------------------------------------------
## 5. Immutability Model
------------------------------------------------------------

The framework enforces strict immutability once a version is declared released.

## 5.1 Status vocabulary

Two statuses are defined:

- `draft`    — work-in-progress; may evolve freely
- `released` — declared complete; MUST be treated as immutable

Status is declared in front matter. It is used only for immutability
enforcement. No other lifecycle states exist in v1.

## 5.2 Immutability boundary

A module version MUST be treated as immutable if either:
- it declares `status: released`, OR
- it is referenced by a system version that declares `status: released`

Once immutable, any further evolution requires creating a new version.

## 5.3 What immutability protects

Released versions are permanent historical records. They can be:
- referenced by system versions
- cited in ADRs
- used as baselines for impact analysis
- consumed by AI agents as stable context

They are never reopened, edited, or deleted.

The framework defines immutability boundaries, not development workflow.
Teams decide *when* to release. The framework defines *what happens after*.


------------------------------------------------------------
## 6. Supporting Artefacts
------------------------------------------------------------

Specs alone are not sufficient for continuous development. Each version
directory includes supporting artefacts so that humans and AI agents can
progress with only the repository as context.

## 6.1 Module version artefacts

Mandatory:
- `module-spec.md` — canonical entry point (purpose, responsibilities, acceptance)
- `todo.md`        — intent gap ledger (remaining work to satisfy declared intent)
- `adr/`           — architectural decision records (directory, may be empty)

Recommended (optional):
- `acceptance.md`      — detailed acceptance criteria and test scenarios
- `context.md`         — dependencies, boundaries, layering constraints
- `risks.md`           — version-specific risks and mitigations
- `impact-analysis.md` — migration, rollout, compatibility notes
- `metrics.yaml`       — signals to measure
- `open-questions.md`  — true unknowns (actionable work belongs in todo.md)

See authoring-guidelines.md for content guidance and when to split files.

## 6.2 System version artefacts

- `system.yaml`        — authoritative module composition (mandatory)
- `overview.md`        — human explanation of this snapshot
- `intent.md`          — why this snapshot exists
- `todo.md`            — system-level gaps
- `acceptance.md`      — system-level guarantees
- `risks.md`           — system-level risks
- `impact-analysis.md` — changes vs previous system version
- `metrics.yaml`       — system-level signals
- `adr/`               — system-level architectural decisions


------------------------------------------------------------
## 7. Metadata Conventions
------------------------------------------------------------

Specs use YAML front matter. Required fields are minimal; additional
fields are allowed and ignored by framework validation.

## 7.1 Module version front matter

    ---
    type: module-cycle
    module: <module-name>
    version: vN
    status: draft | released
    supersedes: vN-1 | null
    issues: {}
    tags: []
    ---

## 7.2 System version front matter (system.yaml)

    type: system
    version: vN
    status: draft | released
    modules:
      <module>: vN

## 7.3 Issue tracking metadata

Specs may link to external issue trackers using a provider-agnostic schema:

    issues:
      <provider-id>:
        - key: <string>   # required
          url: <string>   # optional, recommended

Supported provider IDs (kebab-case):
`jira`, `bitbucket`, `github`, `gitlab`, `azure-devops`

Issue references are metadata only. They do not affect version semantics.

## 7.4 Extensibility

Tools and AI agents MUST treat unknown front matter fields as
non-breaking extensions. The schema is intentionally open.


------------------------------------------------------------
## 8. Framework Documentation
------------------------------------------------------------

Framework files define how the spec system itself works.
They are not part of any system or module version.

Location in framework source repository: `framework/vN/`

Current v1 files:

- `overview.md`           — this document
- `specs-meta.md`         — repository layout and conventions
- `version-operations.md` — allowed operations and state transitions
- `module-bootstrap.md`   — creating a new module from scratch
- `todo-management.md`    — todo.md format and lifecycle rules
- `authoring-guidelines.md` — what content belongs where
- `glossary.md`           — framework terminology

Framework files MAY be updated. Changes that alter meaning MUST increment
the framework version (v1 → v2). Framework changes MUST NOT retroactively
alter the meaning of existing released system or module versions.


------------------------------------------------------------
## 9. Authority
------------------------------------------------------------

This document is the conceptual entry point for the ragnor.dev framework v1.

For operational rules, the authoritative documents are:
- version-operations.md — versioning rules and state transitions
- module-bootstrap.md   — module creation rules
- todo-management.md    — todo.md rules
- specs-meta.md         — repository structure rules

If any tool, AI agent, or other document contradicts those specifications,
those specifications take precedence.
