# Module Bootstrap Specification
Framework Version: v1

## Purpose

This document defines the canonical bootstrap process for creating a new module in the framework.

It explicitly covers the gap left by `version-operations.md`, which assumes that a module already exists and governs only version evolution, not initial creation.

This specification defines:
- when a new module may be created
- who decides
- how the first module cycle (`@v1`) is initialized
- how boundaries and dependencies are established
- how system integration is handled
- how correctness is validated
- how module approval works in practice

Once a module exists, all future changes follow standard version operations.

---

## 1. Core Principles

1. Module creation is a structural and architectural decision, not a filesystem operation.
2. Modules are immutable once published; mistakes are corrected through new versions or new modules.
3. The framework itself is versioned and immutable; rules defined here apply only to projects adopting this framework version.
4. System cycles never auto-update when new modules appear.
5. Governance must protect architectural clarity without introducing unnecessary process friction.

---

## 2. Definition

A module is a long-lived, versioned unit that:
- owns a coherent responsibility or domain boundary
- exposes a defined contract to other modules or systems
- evolves independently via immutable module cycles

A module is not:
- a UI component
- a helper library
- an implementation detail without domain meaning

---

## 3. Trigger Criteria

### 3.1 When to create a new module

A new module must be created when at least one of the following holds:

1. Distinct lifecycle or ownership  
   Different team, cadence, or decision authority.

2. Explicit boundary contract  
   Inputs, outputs, or responsibilities can be described independently.

3. Blast-radius isolation  
   Changes should not require revalidating an existing module’s acceptance criteria.

4. Structural separation  
   Code naturally clusters under a stable root path with mostly one-way dependencies.

If none apply, the change belongs in an existing module and requires a new module version instead.

---

### 3.2 Decision authority and approval model

Creating a new module requires explicit acknowledgment of boundary intent.

This is not a separate workflow.  
Module approval is performed via standard pull request (PR) approval.

Rules:

- A new module is introduced through a PR.
- Approval of that PR constitutes approval of:
    - the existence of the module
    - its boundary and responsibility
    - its name and placement
    - its declared dependencies

Accepted governance models:
- Solo or hackathon mode: repository owner approval.
- Tech-lead gate (recommended): Tech Lead or Architect approval via PR review.
- CODEOWNERS gate: enforced approval via ownership rules.

No separate UX, workflow, or tooling is required beyond the PR.

---

### 3.3 Rationale for approval (non-bureaucratic intent)

Module approval exists to prevent accidental architecture, not to slow development.

A module is a long-lived structural commitment:
- other modules may depend on it
- it accumulates versions and history
- removing or renaming it is expensive

Approval ensures that:
- the module boundary is intentional
- the split is durable
- the system shape remains coherent

Approval must be lightweight, asynchronous, and time-bounded.  
If approval becomes slow or ceremonial, the framework is being misapplied.

---

### 3.4 Minimum scope

A valid module must justify:
- its own `module-spec.md`
- a stable responsibility statement
- acceptance criteria that remain meaningful across versions

If the scope cannot support this, it is not a module.

---

## 4. Bootstrap Process

### 4.1 Preconditions

- A `@specs/` root exists.
- The project has adopted framework version v1.

If no system cycle exists yet, see section 8.3.

---

### 4.2 Directory creation order

1. Create the module root directory:

   `@specs/modules/<module-name>/`

2. Create the initial module cycle directory:

   `@specs/modules/<module-name>/<module-name>@v1/`

Directory creation must precede file population.

---

### 4.3 Required artefacts for `<module-name>@v1`

The following artefacts are mandatory for a valid module cycle:

- `module-spec.md` (canonical entry point)
- `todo.md`
- `adr/` (directory, may be empty)

### 4.4 Recommended artefacts (optional)

The following artefacts are recommended but not required:

- `acceptance.md`
- `context.md` (dependencies and boundaries)
- `risks.md`
- `impact-analysis.md`
- `metrics.yaml`
- `open-questions.md`

`module-spec.md` is the canonical entry point and MUST contain, at minimum:
- Purpose / Intent
- Responsibilities / Scope (context summary)
- Acceptance criteria (may be brief)

If a split artefact exists (e.g. `acceptance.md`), that file becomes the detailed source of truth and `module-spec.md` should only summarize and link to avoid duplication.

Intent, context, acceptance, risks, and impact analysis MAY live inside `module-spec.md` until split files are needed for clarity.

---

### 4.5 Required front matter (`module-spec.md`)

`module-spec.md` must start with valid YAML front matter (shown below as plain text):

    ---
    type: module-cycle
    module: <module-name>
    version: v1
    supersedes: null
    issues: {}
    tags: []
    ---

All values must match the directory path exactly.

---

## 5. Code and Module Creation in PRs

### 5.1 Single-PR default

By default, the PR that introduces a new module:
- may include the initial implementation code for that module
- includes the complete module specification

This is the recommended path.

Benefits:
- boundaries are reviewed against real usage
- no empty or speculative modules
- lower coordination overhead

---

### 5.2 The hard rule

Module approval is based on the declared boundary and contract, not on implementation details.

The module spec must be understandable and reviewable without reading code.

Code serves as evidence, not justification.

---

### 5.3 When to split into multiple PRs

Splitting module creation and code into separate PRs is allowed only when:
- boundary-first refactors are needed
- large extractions require structural agreement first
- the module is explicitly provisional or experimental

Long-lived empty modules are discouraged.

---

## 6. Module Boundaries and Dependencies

### 6.1 Boundary definition

A module owns:
- the domain concepts it defines
- the contracts it exposes
- the invariants it must maintain

A module must not own:
- generic utilities without domain meaning
- cross-cutting infrastructure unless explicitly modeled as a module

---

### 6.2 Dependency declaration

Dependencies are declared textually in `context.md`:

- Depends on: list of modules and justification
- Provides to: list of modules and contract description
- Optional: forbidden dependencies or layering constraints

Automated dependency resolution is out of scope for framework v1.

---

### 6.3 Boundary correction

Incorrect boundaries are corrected by evolution, not mutation:

- Split: create new modules at `@v1`, bump the original module to a new version describing the extraction.
- Merge: introduce a new module or designate a survivor; deprecate others.
- Rename: treated as a new module plus a deprecation notice.

Historical meaning is never rewritten.

---

## 7. Validation

### 7.1 Automated validation (minimum)

A module bootstrap is invalid if any of the following fail:

- Missing path `@specs/modules/<module-name>/<module-name>@v1/`
- Missing any required artefact listed in section 4.3
- Missing or malformed front matter in `module-spec.md`
- `module` or `version` values do not match the directory path
- Missing `adr/` directory

---

### 7.2 Review and approval

Approval follows the governance model defined in section 3.2.

Reviewers must confirm:
- trigger criteria are met
- module boundaries are coherent
- dependencies are intentional
- naming is appropriate and stable

---

## 8. System Integration

### 8.1 No implicit system changes

Creating a module does not automatically update any existing system cycle.

System cycles are immutable snapshots; adding a module requires creating a new system version.

---

### 8.2 Modules before system integration

Modules may exist independently before being referenced by any system cycle.

This supports incremental development and isolated validation.

---

### 8.3 When no system cycle exists yet

If no system cycle exists, create `system@v1` that references all known module `@v1` cycles, including the new module.

This establishes the first system snapshot.

---

## 9. Naming Conventions

### 9.1 Module name format

- Kebab-case only (e.g. `spec-validation`)
- Names must be stable; renaming creates a new module
- Avoid version numbers or team names
- Prefer domain or business boundaries over technical layers

Examples:
- Good: `spec-validation`, `issue-links`, `onboarding-audit`
- Risky: `common`, `misc`, `backend-utils`

---

### 9.2 Conflict handling

- Module names must be globally unique by path.
- Overlapping proposals must be resolved via PR review.
- Duplicate or near-duplicate modules must not coexist.

---

## 10. Appendix: Minimum Viable Content

### 10.1 `module-spec.md` (mandatory content)

- Purpose / Intent (1–3 sentences)
- Responsibilities / Scope (context summary)
- Acceptance criteria (may be brief, 3–10 verifiable criteria)

### 10.2 Optional split files (when needed for clarity)

#### `acceptance.md`
- Detailed acceptance criteria and test scenarios

#### `context.md`
- Owned responsibility
- Depends on
- Provides to
- Optional constraints

#### `risks.md`
- Risk assessment and mitigation strategies

#### `metrics.yaml`
Minimum form (shown below as plain text):

    version: 1
    signals: []


------------------------------------------------------------
ADR Usage
------------------------------------------------------------

The adr/ directory is mandatory to prevent architectural amnesia.

Write an ADR when a decision is:
- hard to reverse
- architecturally constraining
- a trade-off likely to be re-argued
- a policy tools or AI must follow
- primarily about "why", not obvious from code

Do NOT write ADRs for:
- routine refactors
- obvious choices
- status updates

ADR naming (MUST):
- adr/0001-short-title.md
- adr/0002-short-title.md

IDs are monotonic per module version.

ADR template (MUST):
- Title
- Status (proposed | accepted | superseded)
- Context
- Decision
- Consequences
- Alternatives considered
- Links