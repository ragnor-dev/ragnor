# Todo Management Specification
Framework Version: v1

## Purpose

This document defines how `todo.md` is used for day-to-day work inside a **specific module version**.

`todo.md` is a lightweight, persistent working document used to track the **known gap** between:
- the declared intent of a module version (intent, context, acceptance, and other module specs), and
- the current implementation state.

It is designed for AI-augmented development workflows and must remain simple, diff-friendly, and low-friction.

---

## 1. Scope and Non-Goals

### 1.1 Scope

This specification applies to:
- `@specs/modules/<module>/<module>@vN/todo.md`

It defines:
- purpose and meaning of todo items
- lifecycle and ownership
- required formatting
- identity rules (todo IDs)
- completion and archival rules
- reporting/UX rules derived from todos
- relationship to external systems (Jira/Bitbucket/GitHub/etc.)

### 1.2 Non-goals

`todo.md` is not:
- a backlog or roadmap
- a work tracker with workflow states
- an assignment system (no owners, no due dates)
- a priority system (no formal priority fields)
- a replacement for Jira/Bitbucket/GitHub issues

---

## 2. Definition

A todo item is a statement of **remaining intent gap** for the current module version.

A todo item represents work that must be done (or explicitly deferred) for the module version to fully satisfy its declared intent and acceptance criteria.

Todo items are **not** commitments to schedule, staffing, or timelines.

---

## 3. Authorship and Responsibility

### 3.1 Who creates todo items

Todo items are authored primarily by the **module version authoring agent**:
- a human author
- an AI assistant
- or a human+AI pairing

Other contributors and reviewers may propose items, but the authoring agent is responsible for maintaining `todo.md` as an accurate intent-gap ledger.

### 3.2 When todo items are created

Todo items are created when a gap is discovered between spec intent and current reality, typically:
- at module version bootstrap (initial gap list)
- after spec changes (new gaps)
- after implementation changes (update/remove gaps)
- during review (missing requirements are captured as todo items)

---

## 4. File Location and Required Presence

Each module version directory must contain:

- `todo.md`

A module version is invalid if `todo.md` is missing.

---

## 5. Format

### 5.1 Required format

`todo.md` must use plain Markdown checklists:

- unchecked item: `- [ ] ...`
- checked item: `- [x] ...`

No structured YAML schema is permitted for todos in v1.

### 5.2 Recommended sections

`todo.md` should include these sections (optional but recommended):

- `## Outstanding`
- `## Deferred` (only if you explicitly defer to a future module version or to external tracking)

### 5.3 Optional inline annotations

Allowed:
- short informal hints in text (e.g., “(important)”)
- optional references to external issue keys in text (see section 8)

Not allowed:
- assignees, due dates, workflow states, or priority fields presented as a formal system

---

## 6. Todo Item Identity (IDs)

### 6.1 Purpose of IDs

Todo IDs provide stable identity for:
- review discussions
- dashboard/reporting UX
- cross-references in PRs and external systems

IDs exist to support tooling without turning todos into a separate tracker.

### 6.2 ID format

Each todo item must include a unique ID with the following format:

`<module>@v<major>-<NNN>`

Examples:
- `auth@v1-001`
- `auth@v1-002`
- `spec-validation@v2-014`

Rules:
- `<module>` must match the module directory name.
- `v<major>` must match the module version directory.
- `<NNN>` is a zero-padded integer (3 digits).

### 6.3 ID monotonicity and stability

Within a given module version:
- IDs must be monotonically increasing.
- IDs must never be renumbered.
- IDs must never be reused (even if an item is removed).

If a gap is deferred into a new module version, it receives a new ID in that new version.

### 6.4 Canonical line format

A todo line must follow:

`- [ ] <ID> <description>`

Examples:

- `- [ ] auth@v1-001 Define retry semantics for token refresh`
- `- [x] auth@v1-002 Add migration notes for config rename`

Backticks around IDs are optional.

---

## 7. Completion Rules

### 7.1 Primary completion invariant

A module version must not be marked complete while `todo.md` contains unchecked items.

### 7.2 How to resolve remaining items

Unchecked todo items must be resolved by one of the following:

1. Implement the work and check the item.
2. Explicitly defer the work to a future module version:
    - create the next module version and add a new todo item there, or
    - adjust module specs to declare it as a non-goal for the current version.
3. Move the work to external tracking (e.g., Jira) when it is cross-cutting or not specific to completing this module version:
    - link the external issue in the appropriate module version `module-spec.md` via `issues.<provider>`, and
    - remove or mark the item as deferred with clear reference.

Silent leftovers are not allowed.

---

## 8. Relationship to External Issues (Jira/Bitbucket/GitHub/etc.)

### 8.1 Canonical linkage location

The canonical location for linking external issue systems to a module version is `module-spec.md` front matter:

`issues.<provider>`

This supports reporting and management visibility at the module version level.

### 8.2 Linking issues to individual todo items

Linking external issues to individual todos is optional.

If used, references must be informal text appended to the todo description, e.g.:

- `- [ ] auth@v1-003 Add rate-limit handling (ABC-123)`
- `- [ ] auth@v1-004 Confirm behavior under retries (org/repo#789)`

There is no required one-to-one mapping between todo items and external issues.

---

## 9. Reporting and Management UX Contract

### 9.1 Read-only derived reporting

Management/reporting UX must treat `todo.md` as a derived signal source and must be read-only.

The UX may present:
- module@version open todo count
- list of outstanding todo items (text)
- linked external issues from `module-spec.md`
- roll-ups at system@version level (sum of open todo counts across included modules)

### 9.2 Prohibited UX behavior (to avoid tracker duplication)

Management/reporting UX must not introduce:
- assignment, owners, due dates
- workflow states beyond “open/checked”
- priority fields
- editing of `todo.md` items outside version control workflows

All edits occur via PRs to preserve reviewability and audit trail.

---

## 10. PR Review Expectations

Reviewers should treat `todo.md` as a completeness and intent-gap signal.

Review questions:
- Are remaining gaps accurately captured?
- Are deferred items explicitly handled (future version or external issue)?
- Is the module version being presented as “complete” while open todos remain?

Reviewers should not use `todo.md` to drive staffing, scheduling, or task assignment.

---

## 11. Tooling Guidance (Optional for v1)

Tooling may validate:
- `todo.md` exists for each module version
- each todo line contains a valid ID
- ID prefix matches module@version
- IDs are unique within the file

Tooling should avoid enforcing:
- 1:1 mapping to external issues
- prioritization or assignment semantics
- any workflow beyond check/uncheck