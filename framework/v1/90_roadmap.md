---
spec_id: roadmap
rule_id: roadmap-v1
type: framework-rule
version: v1
status: active
---

# 90 — Roadmap

This document defines how to plan and track delivery across module
versions and system versions within ragnor.dev.

A roadmap is planned intent. It is not a commitment, a sprint board,
or a backlog. It is a structured declaration of what is coming,
in what order, and why.


------------------------------------------------------------
## 1. What a Roadmap Is
------------------------------------------------------------

A roadmap answers three questions:

1. **What** — which module versions and system versions are planned
2. **Why** — what intent or outcome each planned version serves
3. **Order** — which versions depend on others and should come first

A roadmap does NOT answer:
- When exactly (no dates unless explicitly chosen)
- Who (no ownership or assignment)
- How long (no estimates unless explicitly chosen)

These omissions are intentional. Dates and estimates create false
precision and invite the wrong conversations. The roadmap is about
intent and sequence, not schedule.


------------------------------------------------------------
## 2. Roadmap Location
------------------------------------------------------------

Every project using ragnor.dev MUST have a roadmap at:

    @specs/roadmap.md

This is the single source of truth for planned delivery.
It is a living document — updated as intent evolves.

It is NOT immutable. Unlike module specs and system versions,
the roadmap reflects current planning intent and changes as
understanding improves.


------------------------------------------------------------
## 3. Roadmap Format
------------------------------------------------------------

The roadmap is a Markdown document with the following structure:

```markdown
---
spec_id: roadmap
version: [current date YYYY-MM-DD]
status: active
---

# Roadmap

## Current (in progress)

### [module]@v[N] — [short title]
Intent: [why this version exists, what outcome it serves]
Depends on: [other versions that must be complete first, or none]
Status: in-progress | blocked | ready-for-review
Notes: [optional context]

## Planned (not yet started)

### system@v[N] — [short title]
Intent: [why this system snapshot is being planned]
Includes: [module versions this system version will pin]
Depends on: [prerequisites]

### [module]@v[N] — [short title]
Intent: [why]
Depends on: [prerequisites or none]

## Deferred (explicitly not now)

### [item] — [short title]
Intent: [what this was going to be]
Deferred because: [reason — ambiguous / blocked / deprioritised]
Revisit when: [condition that would bring this back]

## Released

### system@v[N] — [short title]
Released: [date or version tag]
Outcome: [what was actually delivered]
```

---

### 3.1 Required sections

- **Current** — what is actively being worked on
- **Planned** — what is next, in priority order
- **Deferred** — explicitly not now (with reason)
- **Released** — immutable record of what shipped

### 3.2 Required fields per entry

Every roadmap entry MUST have:
- A version identifier (`[module]@v[N]` or `system@v[N]`)
- An intent statement (why this version exists)

Optional but recommended:
- Dependencies
- Status (for current items)
- Notes

### 3.3 The Deferred section

The Deferred section is critical. It is the roadmap equivalent of
`todo.md` — it makes the 20% visible at the planning level.

An item in Deferred is not abandoned. It is explicitly acknowledged
as not-now, with a reason and a revisit condition.

Items MUST NOT silently disappear from the roadmap. They move to
Deferred with a reason, or they move to Released when complete.


------------------------------------------------------------
## 4. Roadmap and the 80/20 Model
------------------------------------------------------------

The roadmap is where the 80/20 delivery model operates at the
planning level.

**Current and Planned** = the clear 80% at the planning horizon.
These are versions with enough clarity to commit to.

**Deferred** = the ambiguous 20% at the planning horizon.
These are versions that cannot be clearly defined yet, or that
depend on information from shipping the current work first.

The roadmap enforces the same discipline as the module spec:
ship the clear work, explicitly defer the unclear work,
never pretend the unclear work doesn't exist.


------------------------------------------------------------
## 5. Relationship Between Roadmap and Specs
------------------------------------------------------------

The roadmap is a planning layer above the spec layer.

| Roadmap entry | Spec artefact |
|---|---|
| Planned module version | Future `module-spec.md` (not yet created) |
| Current module version | Active draft `module-spec.md` |
| Released module version | Immutable released `module-spec.md` |
| Planned system version | Future `system.yaml` (not yet created) |
| Released system version | Immutable released `system.yaml` |

A roadmap entry becomes a spec when intake is complete
(see `20_idea-to-spec.md`). Until then it is planned intent,
not declared intent.

**Rule:** a roadmap entry MUST NOT be treated as a spec.
AI agents MUST NOT implement against roadmap entries.
Only completed module specs are valid execution targets.


------------------------------------------------------------
## 6. Roadmap and System Versions
------------------------------------------------------------

System versions are immutable snapshots of module compositions.
The roadmap is where you plan which system version to create next
and what it will include.

A planned system version entry in the roadmap answers:
- Which module versions will it pin?
- What coherent state does it represent?
- What is the trigger for creating it?

See `50_version-operations.md` section 4.4 for system version
creation triggers.


------------------------------------------------------------
## 7. AI Agent Interaction With the Roadmap
------------------------------------------------------------

AI agents MAY read the roadmap for context.

AI agents MUST NOT:
- Implement against roadmap entries (only against completed specs)
- Modify the roadmap without explicit human instruction
- Move items between sections autonomously
- Add items to the roadmap based on their own judgment

AI agents MAY:
- Suggest roadmap entries based on gaps found during drift detection
- Flag when a current implementation implies a future roadmap entry
- Help draft intent statements for planned entries
- Identify dependencies between planned versions


------------------------------------------------------------
## 8. Relationship to Other Documents
------------------------------------------------------------

- `10_delivery-model.md` — the 80/20 philosophy the roadmap implements
- `20_idea-to-spec.md` — how roadmap entries become specs
- `50_version-operations.md` — version creation triggers
- `95_git-integration.md` — how roadmap entries relate to branches


------------------------------------------------------------
## 9. Authority
------------------------------------------------------------

This document is authoritative for roadmap format and usage.

The roadmap at `@specs/roadmap.md` is the single source of truth
for planned delivery. It is maintained by humans. AI agents read it
for context but do not own it.
