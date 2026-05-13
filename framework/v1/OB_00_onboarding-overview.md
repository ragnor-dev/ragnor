---
spec_id: onboarding-overview
rule_id: onboarding-v1
type: framework-rule
version: v1
status: active
---

# OB_00 — Onboarding Overview

This document defines the onboarding paths into ragnor.dev.

Onboarding is the process of establishing the `@specs/` layer in a
repository. It is the first thing that happens before any framework
rules apply.

No framework rules apply until onboarding is complete.


------------------------------------------------------------
## 1. The Bootstrap Gate
------------------------------------------------------------

Every AI agent operating within this framework MUST verify that
onboarding is complete before executing any command.

The bootstrap gate check:

1. Does `@specs/` exist at the repository root?
2. Does `@specs/framework.md` exist and point to an existing framework snapshot file?
3. Does at least one module version exist under `@specs/modules/`?
4. Does `@specs/system/system@v1/` exist?

If any check fails:
- Stop immediately
- Inform the human that onboarding is required
- Do not proceed with any other command until onboarding is complete

The bootstrap gate is non-negotiable. An AI agent that skips it
is operating without a source of truth.


------------------------------------------------------------
## 2. Choosing an Onboarding Path
------------------------------------------------------------

There are two onboarding paths. Choose based on the state of the repository:

### Path A — Greenfield

**Use when:** starting a new project with no existing codebase.

The repository is empty or contains only scaffolding. There is no
existing architecture, no existing decisions, no existing documentation
to import.

→ See `OB_10_onboarding-greenfield.md`

### Path B — Brownfield

**Use when:** taking over or adopting an existing codebase.

The repository has existing code, possibly existing documentation
(Confluence, Jira, README files, wikis), and accumulated architectural
decisions that need to be captured.

→ See `OB_20_onboarding-brownfield.md`

For brownfield projects with significant external documentation to import:

→ See `OB_30_onboarding-external.md`


------------------------------------------------------------
## 3. What Onboarding Produces
------------------------------------------------------------

Regardless of path, onboarding MUST produce:

**Minimum viable output:**
- `@specs/framework.md` — framework snapshot pointer
- `@specs/framework/framework-v<major>.<minor>.md` — adopted framework snapshot
- `@specs/modules/<module>/<module>@v1/` — at least one module
- `@specs/system/system@v1/` — initial system snapshot
- `@specs/roadmap.md` — initial roadmap (may be minimal)
- `@specs/glossary.md` — project terminology

**Each module version MUST contain:**
- `module-spec.md` — purpose, responsibilities, acceptance criteria
- `todo.md` — intent gaps (may be extensive for brownfield)
- `adr/` — architectural decisions (may be empty for greenfield)

**The system snapshot MUST contain:**
- `system.yaml` — module composition map
- `overview.md` — human explanation of the initial state


------------------------------------------------------------
## 4. Onboarding Quality Bar
------------------------------------------------------------

Onboarding is complete when a new developer or AI agent can:

1. Clone the repository
2. Read `@specs/` and understand what the system does
3. Understand the module boundaries and responsibilities
4. Know what decisions have been made and why
5. Know what work remains (from `todo.md`)
6. Begin contributing without asking "what does this do?"

This is the quality bar. It is not about perfection — brownfield
onboarding will always have gaps. It is about having enough context
that the next person (human or AI) can work without starting from zero.


------------------------------------------------------------
## 5. Relationship to Other Documents
------------------------------------------------------------

- `OB_10_onboarding-greenfield.md` — greenfield path
- `OB_20_onboarding-brownfield.md` — brownfield path
- `OB_30_onboarding-external.md` — external source import
- `20_idea-to-spec.md` — intake process used during greenfield onboarding
- `commands/01_bootstrap.md` — the bootstrap command
