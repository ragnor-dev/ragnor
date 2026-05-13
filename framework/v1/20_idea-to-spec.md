---
spec_id: idea-to-spec
rule_id: idea-to-spec-v1
type: framework-rule
version: v1
status: active
prompts:
  - intake-v1
---

# 20 — Idea to Spec

This document defines the intake process — how a raw idea becomes a
structured module spec that is ready for AI-assisted implementation.

No AI execution MUST begin until the intake process is complete.
This is a non-negotiable gate.


------------------------------------------------------------
## 1. Purpose
------------------------------------------------------------

The intake process exists to answer one question before any code is written:

> **Do we know clearly enough what we are building and why?**

"Clearly enough" does not mean perfectly. It means the 80% zone is
explicitly defined, the 20% zone is explicitly acknowledged, and no
one-way decisions are being made by default or by AI.

The intake process is the primary defence against:
- Building the wrong thing confidently
- AI agents filling ambiguity with plausible guesses
- One-way architectural decisions made without deliberation
- Scope creep disguised as requirements


------------------------------------------------------------
## 2. When to Run Intake
------------------------------------------------------------

Run the intake process when:
- Starting a new module from scratch
- Adding significant new capability to an existing module
- The scope of a change is unclear enough that you are not sure
  what the acceptance criteria would be

Do NOT run intake for:
- Closing gaps already tracked in `todo.md`
- Bug fixes with clear reproduction steps
- Wording or documentation changes
- Refactors that do not change declared behaviour


------------------------------------------------------------
## 3. The Intake Process
------------------------------------------------------------

The intake process has five steps. Each step MUST be completed before
moving to the next.

### Step 1 — Capture Raw Intent

State the idea in plain language. No technical details yet. Focus on:
- What problem does this solve?
- Who has this problem?
- Why does it matter?
- What does success look like from the user's perspective?

Output: a raw intent statement (1-5 sentences). This becomes the
`purpose` section of `module-spec.md`.

**AI role:** help articulate and sharpen the intent statement.
Ask clarifying questions. Do not add technical assumptions.

---

### Step 2 — Clarification and Gap Detection

Systematically identify what is unclear, unknown, or assumed.

For each aspect of the intent, ask:
- Is this unambiguous? Can two people read this and agree on what it means?
- What assumptions are being made that have not been stated?
- What edge cases have not been considered?
- What dependencies or constraints are not yet known?

Mark every unclear item explicitly. Use `[UNCLEAR: question]` inline.

Output: an annotated intent statement with all ambiguities surfaced.

**AI role:** generate clarifying questions. Surface assumptions.
Flag potential edge cases. Do NOT resolve ambiguities — surface them.

---

### Step 3 — 80/20 Classification

Split the clarified intent into two explicit zones:

**Clear zone (the 80%):**
- Unambiguous requirements
- Known acceptance criteria
- Reversible implementation choices
- Work AI can execute safely

**Ambiguous zone (the 20%):**
- Unclear requirements (from Step 2)
- Unknown dependencies
- Decisions requiring more information
- One-way decisions (flag these explicitly)

Output: two explicit lists. The clear zone becomes the module spec.
The ambiguous zone goes into `todo.md` and `open-questions.md`.

**AI role:** help classify each item. Flag one-way decisions.
Surface items that look clear but contain hidden assumptions.

---

### Step 4 — One-Way Decision Identification

Review the ambiguous zone for one-way decisions (see `10_delivery-model.md`
section 4).

For each one-way decision:
- Name it explicitly
- Describe what makes it irreversible
- List the options being considered
- Identify what information is needed to decide

These become ADR stubs — not yet decided, but explicitly tracked.

Output: list of one-way decision stubs for the `adr/` directory.

**AI role:** identify potential one-way decisions. Describe consequences
of each option. Do NOT make the decision.

---

### Step 5 — Spec Readiness Check

Before writing the module spec, verify:

- [ ] The purpose is stated in 1-3 sentences without technical jargon
- [ ] Responsibilities are listed without implementation details
- [ ] Acceptance criteria exist for the clear 80% (minimum 3, maximum 10)
- [ ] The ambiguous 20% is explicitly listed in `todo.md` or `open-questions.md`
- [ ] One-way decisions are identified and stubbed as ADRs
- [ ] No `[UNCLEAR]` markers remain in the clear zone
- [ ] Non-goals are stated (what this module explicitly does NOT do)

If any item fails, return to the relevant step.

Output: a completed `module-spec.md` ready for AI execution.

**AI role:** run the readiness check. Report failures. Do not proceed
to implementation until all items pass.


------------------------------------------------------------
## 4. Spec Readiness Gate
------------------------------------------------------------

A module spec is considered **ready for AI execution** when:

1. The intake process (all 5 steps) is complete
2. The spec readiness check passes
3. The human author has explicitly confirmed the spec is accurate

AI agents MUST NOT begin implementation before this gate is passed.

If an AI agent is asked to implement against an incomplete spec, it MUST:
1. Run the readiness check
2. Report which items fail
3. Ask the human to complete the intake process
4. Not proceed until the gate is passed


------------------------------------------------------------
## 5. Intake for Existing Modules
------------------------------------------------------------

When adding capability to an existing module version:

1. Read the current `module-spec.md` to understand declared intent
2. Determine if the change requires a new module version
   (see `50_version-operations.md` section 3.4)
3. If yes — run the full intake process for the new version
4. If no — run Steps 2-3 only (clarification and 80/20 classification)
   and update `todo.md` accordingly


------------------------------------------------------------
## 6. Relationship to Other Documents
------------------------------------------------------------

- `10_delivery-model.md` — the 80/20 philosophy this process implements
- `30_module-bootstrap.md` — how to create the module after intake
- `50_version-operations.md` — when intake triggers a new version
- `60_todo-management.md` — where the 20% goes after intake
- `70_ai-execution-rules.md` — what AI does after intake is complete
- `commands/02_intake.md` — the command that runs this process


------------------------------------------------------------
## 7. Authority
------------------------------------------------------------

This document is authoritative for the idea-to-spec intake process.

No AI execution begins without a completed intake. No exceptions.
If an exception is needed, it MUST be documented in `open-questions.md`
with explicit justification.
