---
spec_id: specs-meta
type: meta-spec
version: v1
status: active
---

# Spec System Meta-Specification

This document defines the repository layout and conventions for all
ragnor.dev specifications, system versions, module versions,
and supporting artefacts.

Humans and AI agents MUST follow these rules.

------------------------------------------------------------
## 1. Top-Level Structure
------------------------------------------------------------

All specification-related content lives under:

    @specs/

Top-level files:

- `specs-meta.md`    – this document (authoritative)
- `glossary.md`      – shared terminology
- `architecture.md`  – high-level system architecture view
- `roadmap.md`       – planned future work and versions

Subdirectories:

- `framework/` – framework documentation (meta-level)
- `system/`    – system versions
- `modules/`   – module versions


------------------------------------------------------------
## 2. Framework Documentation
------------------------------------------------------------

Framework documentation defines how the spec system itself works.
It is NOT part of any system or module version.

Location pattern:

    @specs/framework/vN/

Framework documents:
- define repository structure
- define versioning rules and immutability boundaries
- define metadata conventions
- define tooling and AI agent behaviour

Framework documents change infrequently and act as the canonical
reference for humans and AI agents.


------------------------------------------------------------
## 3. System Versions
------------------------------------------------------------

System versions represent the state of the entire system at a point in time.

Location pattern:

    @specs/system/system@vN/

Each system version directory MUST contain:

- `system.yaml` – metadata and module composition map (mandatory)
- `overview.md` – human-readable explanation of this snapshot

Recommended artefacts:
- `intent.md`          – why this version exists
- `todo.md`            – outstanding system-level tasks
- `acceptance.md`      – system-level acceptance criteria
- `open-questions.md`  – known unknowns
- `risks.md`           – known risks and mitigations
- `impact-analysis.md` – impact of changes in this version
- `metrics.yaml`       – summary metrics
- `adr/`               – architectural decision records

### 3.1. `system.yaml` schema

`system.yaml` MUST be a YAML document containing at least:

- `type`: `"system"`
- `version`: string (`"v1"`, `"v2"`, …)
- `modules`: mapping of module name → module version
- `status`: `draft` or `released` (optional, defaults to draft)

Example:

```yaml
type: system
version: v1
status: draft
modules:
  auth: v1
  billing: v1
```


------------------------------------------------------------
## 4. Module Versions
------------------------------------------------------------

Module versions represent the specification of a single module at a
point in time.

Location pattern:

    @specs/modules/<module-name>/<module-name>@vN/

### 4.1. `module-spec.md` front matter schema

`module-spec.md` MUST start with YAML front matter containing at least:

- `type`: `"module-cycle"`
- `module`: string (module name, must match directory)
- `version`: string (`"v1"`, `"v2"`, …, must match directory)
- `supersedes`: string or `null` (previous version)
- `status`: `draft` or `released` (optional, defaults to draft)

Optional fields:
- `issues`: object (see section 5)
- `tags`: array of strings

Example:

```yaml
---
type: module-cycle
module: auth
version: v2
status: draft
supersedes: v1
issues:
  github:
    - key: "#42"
      url: "https://github.com/org/repo/issues/42"
tags: ["backend", "security"]
---
```


------------------------------------------------------------
## 5. Issues Field Schema
------------------------------------------------------------

The `issues` field provides provider-agnostic issue tracking metadata.

### 5.1. Schema

```yaml
issues:
  <provider-id>:
    - key: <string>   # required
      url: <string>   # optional, recommended
```

### 5.2. Supported Providers

Provider IDs MUST be kebab-case:

- `jira`         – Jira
- `bitbucket`    – Bitbucket Issues
- `github`       – GitHub Issues
- `gitlab`       – GitLab Issues
- `azure-devops` – Azure DevOps Work Items

### 5.3. Validation Rules

- `issues` field is optional
- Provider IDs must be kebab-case strings
- Each array item must have a `key` field (required)
- Each array item may have a `url` field (optional but recommended)
- Issue references are metadata only — they do not affect version semantics


------------------------------------------------------------
## 6. Naming Conventions
------------------------------------------------------------

- Module names: kebab-case, stable, domain-oriented (e.g. `auth`, `billing`, `spec-validation`)
- Version tags: `v1`, `v2`, `v3`, … (simple integer, prefixed with `v`)
- Directory names must exactly match the `module` and `version` front matter values
- Renaming a module creates a new module; it does not mutate the existing one
