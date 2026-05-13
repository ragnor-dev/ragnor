---
spec_id: version-operations
type: framework-spec
version: v1
status: active
---

# Version Operations (MVP)

This document defines the allowed operations, state transitions, and invariants
for system versions and module versions.

All tools, UI components, onboarding processes, and AI agents MUST follow these rules.
No component may invent additional behavior.

------------------------------------------------------------
## 1. Core Definitions
------------------------------------------------------------

- Module Version  
  A versioned specification of a single module, stored at:  
  @specs/modules/<module>/<module>@vN/

- System Version  
  A versioned composition of module versions, stored at:  
  @specs/system/system@vN/

- Framework Docs  
  Meta-level specifications describing how the system works.
  In adopter repositories, the active snapshot is referenced by:
  @specs/framework.md


------------------------------------------------------------
## 2. Released Version Invariants
------------------------------------------------------------

This framework enforces strict immutability boundaries once a version
is declared released. These rules are mandatory and non-negotiable.

### 2.1. Module Version Invariant

If a module version is marked with:

    status: released

then that module version is considered **closed**.

Rules:
- A released module version MUST NOT be modified.
- Any change that would affect behavior, responsibilities, dependencies,
  contracts, or acceptance criteria MUST be introduced as a new module version
  (for example: `auth@v1` → `auth@v2`).
- Released module versions exist for historical reference and system composition
  only; they are never reopened.

This rule applies regardless of:
- feature size
- perceived simplicity of the change
- urgency or delivery pressure

### 2.2. System Version Invariant

If a system version is marked with:

    status: released

then that system version is considered **closed**.

Rules:
- A released system version's module composition MUST NOT be modified.
- Any change to:
  - included module versions
  - system-level behavior
  - system-level guarantees
  MUST be expressed as a new system version
  (for example: `system@v1` → `system@v2`).
- Released system versions represent immutable snapshots in time.

### 2.3. Combined Effect

A module version MUST be treated as immutable if **either**:
- it is marked `status: released`, OR
- it is referenced by a system version marked `status: released`.

In both cases, any further evolution requires creating a new module version,
and optionally a new system version to reference it.

### 2.4. Framework Documentation
- Framework files MAY be updated at any time.
- Changes MUST increment the framework version (v1 → v2).
- Framework changes MUST NOT retroactively change the meaning of
  existing system or module versions.

## 2.5. Release Status and Immutability (System + Module)

This framework uses a minimal lifecycle status to support dog-fooding and AI-driven development
without introducing workflow engines.

### 2.4.1 Status Vocabulary

- draft: work-in-progress; may evolve via normal versioning operations
- released: declared as released/deployed; must be treated as immutable

Status is an in-repo, spec-level declaration used only for immutability rules.

### 2.4.2 System Version Status (@specs/system/system@vN/system.yaml)

A system version MAY declare:

    status: draft | released

Rules:
- If status is released:
  - the system version directory is immutable
  - tools MUST NOT modify its module mapping
  - tools MUST NOT modify its supporting artifacts
    (non-semantic fixes are technically possible but discouraged)

### 2.4.3 Module Version Status (module-spec.md front matter)

A module version MAY declare in module-spec.md front matter:

    status: draft | released

Rules:
- If a module version declares status: released, it MUST be treated as immutable.
- If a module version is referenced by a system version with status: released,
  it MUST be treated as immutable in the context of that released system snapshot.

### 2.4.4 Change Rule (After Release)

If a change would modify behavior, contract, responsibilities, dependencies,
or acceptance criteria of:
- a released module version, OR
- a module version referenced by a released system snapshot,

then the change MUST be introduced as a new module version
(for example auth@v1 → auth@v2).

If system composition must reflect the change, create a new system version
referencing the new module version.


------------------------------------------------------------
## 3. Creating a New Module Version
------------------------------------------------------------

### 3.1. Preconditions
- A previous module version MAY exist (e.g. auth@v1).
- The module directory MUST already exist under @specs/modules/.

### 3.2. Operation
When creating <module>@v(N+1):

1. Create directory:
   @specs/modules/<module>/<module>@v(N+1)/

2. Copy contents from <module>@vN as baseline.

3. Update module-spec.md front matter:
    - version → v(N+1)
    - supersedes → vN

4. Clear or review:
    - todo.md (mandatory)
    - open-questions.md (if present)
    - acceptance.md (if present, must be revalidated)
    - other optional artefacts (if present)

If optional artefacts are absent, equivalent content must exist in `module-spec.md`.

### 3.3. Postconditions
- <module>@v(N+1) becomes the latest module version.
- <module>@vN becomes immutable.
- No system version is automatically updated.


### 3.4. Version Bump Triggers (Authoring Guidance)

This section defines WHEN a new module version MUST be created during
active development. It does not introduce workflow engines.

#### 3.4.1. Create a new module version when (MUST)

Create <module>@v(N+1) when ANY of the following is true:

1) Immutability boundary
   - The current module version is marked status: released, OR
   - The module version is referenced by a system version with status: released

2) Spec meaning changes
   Any change to declared meaning, including:
   - responsibilities, scope, or non-goals
   - external contract (APIs, events, invariants, inputs/outputs)
   - dependencies, boundaries, or layering constraints
   - acceptance criteria (added, removed, or reinterpreted)
   - risk posture affecting guarantees

3) Re-baselining acceptance
   - Acceptance criteria must be rewritten or revalidated due to changed behavior

4) Dependency or boundary change
   - New depends-on / provides-to relationships appear or are removed
   - Forbidden dependencies or layering rules change

5) Structural rewrite invalidating spec-to-code mapping
   - Code structure/root changes such that the spec becomes misleading

#### 3.4.2. Do NOT create a new module version when (SHOULD NOT)

Remain in the same draft version when:
- closing implementation gaps already tracked in todo.md
- wording cleanup or clarification without meaning change
- adding examples or detail that supports existing acceptance

#### 3.4.3. Work-in-progress handling

- Draft module versions MAY evolve freely
- todo.md is the canonical ledger of remaining intent gaps
- New versions represent new intent baselines, not incremental commits

#### 3.4.4. Release boundary note

The framework does not define when to release.
It defines what happens AFTER release (immutability and versioning rules).


------------------------------------------------------------
## 4. Creating a New System Version
------------------------------------------------------------

### 4.1. Preconditions
- At least one module version MUST exist.
- The previous system version MAY exist.

### 4.2. Operation
When creating system@v(N+1):

1. Create directory:
   @specs/system/system@v(N+1)/

2. Initialize system.yaml by:
    - copying from system@vN if it exists
    - OR constructing from latest module versions if this is v1

3. Explicitly set module mapping in system.yaml, for example:
   modules:
   auth: v2
   billing: v1

4. Populate supporting artefacts:
    - overview.md
    - intent.md
    - todo.md
    - others MAY be copied as baseline

### 4.3. Postconditions
- system@v(N+1) is a new immutable system snapshot.
- system@vN remains unchanged.
- No module versions are modified.


### 4.4. System Version Creation Triggers (Authoring Guidance)

System versions are immutable snapshots of module compositions.

Create system@v(N+1) when ANY of the following is true:

1) Release checkpoint
   - A coherent system state is being declared or prepared as released

2) Composition change
   - The system should reference newer module versions

3) Cross-module guarantees change
   - Integration behavior, compatibility, or sequencing changes

4) Onboarding milestone
   - system@v1 was auto-generated; system@v2 is the first curated snapshot

5) Pinned baseline needed
   - Reporting, rollups, or audit requires a non-drifting snapshot

Do NOT create a new system version when:
- only a module evolves in draft with no need for a pinned composition


------------------------------------------------------------
## 5. Validation Rules
------------------------------------------------------------

### 5.1. Module Version Validation
Validation MAY be run against a module version.

Validation scope:
- files belonging to the module
- module-spec.md (mandatory)
- optional artefacts (if present)

Validators MUST fail only if:
- `module-spec.md` is missing
- `todo.md` is missing
- `adr/` directory is missing
- `module-spec.md` front matter is invalid or mismatched

Validators MUST NOT:
- require optional artefacts (`acceptance.md`, `context.md`, `risks.md`, etc.)
- require empty placeholder files
- fail due to missing recommended artefacts

Warnings are allowed for missing recommended artefacts; errors are not.

Failures:
- MUST be reported as structured findings
- MUST NOT modify specs automatically in MVP

### 5.2. System Version Validation
Validation MAY be run against a system version.

Validation scope:
- all module versions referenced in system.yaml
- cross-module consistency checks (MVP: shallow only)

Blocking conditions:
- referenced module version does not exist
- malformed system.yaml


------------------------------------------------------------
## 6. Onboarding Audit Rules (MVP)
------------------------------------------------------------

If @specs/ does not exist, onboarding MUST:

1. Infer modules from repository structure and commit history.
2. Create one module version per inferred module:
   <module>@v1
3. Create a system version:
   system@v1
   referencing all created module versions.

Onboarding MUST NOT:
- overwrite existing specs
- create v2+ versions automatically


------------------------------------------------------------
## 7. Failure Handling
------------------------------------------------------------

Tools MUST fail fast and visibly when:

- A system version references a non-existent module version
- A module version is missing mandatory files (`module-spec.md`, `todo.md`, `adr/`)
- `module-spec.md` front matter is invalid or mismatched
- An operation attempts to modify an immutable version
- Attempting to modify a module version marked `status: released`
- Attempting to modify a system version marked `status: released`
- Attempting to modify a module version referenced by a released system version

Tools MUST NOT fail when optional artefacts are missing.

Allowed reactions:
- block operation
- surface clear error message
- suggest corrective action
- provide remediation options for label issues

Silent repair is NOT allowed in MVP.


------------------------------------------------------------
## 8. Non-Goals (Explicit)
------------------------------------------------------------

The MVP does NOT include:

- automatic version bumping on merge
- dependency resolution graphs
- semantic version enforcement
- branching semantics
- sprint or iteration coupling


------------------------------------------------------------
## 9. Authority
------------------------------------------------------------

This document is authoritative for all version state transitions.

If any other document, tool, or AI agent contradicts this specification,
this specification takes precedence.
