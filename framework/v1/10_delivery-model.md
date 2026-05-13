---
spec_id: delivery-model
rule_id: delivery-model-v1
type: framework-rule
version: v1
status: active
prompts:
  - intake-v1
---

# 10 — Delivery Model

This document defines the core delivery philosophy of ragnor.dev.
It is the philosophical foundation that all other framework rules build on.

All humans and AI agents working within this framework MUST understand and
apply this model. It is not optional.


------------------------------------------------------------
## 1. The Problem With How Software Gets Built
------------------------------------------------------------

Most software delivery fails in one of two ways:

**Analysis paralysis** — teams spend so long trying to define everything
perfectly that nothing ships. Every ambiguous decision blocks progress.
The 20% of hard decisions holds the 80% of clear work hostage.

**Reckless execution** — teams ship fast by guessing at ambiguous decisions.
AI agents fill gaps with plausible-sounding output. The result looks
productive but drifts from actual intent. Technical debt accumulates silently.
One-way doors get walked through without anyone noticing.

Both failure modes share the same root cause: **treating ambiguity and
clarity as the same thing**.

ragnor.dev separates them explicitly.


------------------------------------------------------------
## 2. The 80/20 Delivery Model
------------------------------------------------------------

Every feature, module, or system change has two zones:

### 2.1 The Clear Zone (the 80%)

Work that is:
- Well understood
- Unambiguous in intent
- Reversible if wrong
- Safe for AI to execute without human intervention

This is the work you can spec clearly, implement confidently, and ship
without second-guessing. It delivers real value immediately.

**Rule: ship the clear 80% first. Always.**

### 2.2 The Ambiguous Zone (the 20%)

Work that is:
- Unclear or underspecified
- Dependent on information you don't yet have
- Potentially irreversible (one-way decisions)
- Requiring deliberate human judgment before commitment

This is not work to skip. It is work to **explicitly defer** until you
have better information — typically from shipping the 80% first.

**Rule: never guess at the ambiguous 20%. Surface it, defer it, decide it deliberately.**

### 2.3 The Iteration

After shipping the 80%, the 20% becomes clearer:
- Real user feedback replaces assumptions
- The shipped system reveals what the ambiguous decisions actually affect
- One-way doors become visible before you walk through them

Apply the same 80/20 split to the remaining 20%. Repeat.

This is not Agile sprints. There are no ceremonies, no velocity points,
no retrospectives. There is only: **ship the clear work, defer the unclear
work, decide deliberately when you must.**


------------------------------------------------------------
## 3. Why This Model Matters for AI-Assisted Development
------------------------------------------------------------

AI agents are extremely good at executing clear intent.
AI agents are extremely dangerous when resolving ambiguity.

When an AI agent encounters unclear requirements, it does not stop and ask.
It fills the gap with the most plausible-sounding answer. That answer may be
technically coherent but wrong for your specific context, your specific
constraints, your specific users.

The 80/20 model is the primary guardrail against AI hallucination at the
architectural level. It works by:

1. **Forcing clarity before execution** — the intake process (see `20_idea-to-spec.md`)
   requires the 80/20 split to be explicit before any AI execution begins

2. **Constraining AI scope** — AI agents MUST implement only the defined 80%
   and MUST surface the 20% as explicit gaps (see `70_ai-execution-rules.md`)

3. **Making ambiguity visible** — the 20% is tracked in `todo.md` and
   `open-questions.md`, never silently resolved

4. **Protecting one-way decisions** — irreversible choices in the 20% MUST
   become ADRs before they are made (see `30_module-bootstrap.md`)


------------------------------------------------------------
## 4. One-Way Decisions
------------------------------------------------------------

A one-way decision is a choice that is difficult or impossible to reverse
once made. Examples:

- Choosing a database engine
- Defining a public API contract
- Selecting an authentication model
- Committing to a data model that will be persisted
- Choosing a third-party dependency with deep integration

One-way decisions are always in the 20% zone. They MUST NOT be made
implicitly, by default, or by AI agents without explicit human approval.

**Rule: every one-way decision MUST be recorded as an ADR before it is
implemented.**

If an AI agent encounters a one-way decision during implementation, it
MUST stop, surface the decision, and wait for explicit human instruction.
It MUST NOT make the decision autonomously.


------------------------------------------------------------
## 5. The Framework as CTO
------------------------------------------------------------

ragnor.dev does not tell you what to build.

It acts as a disciplined CTO — ensuring that:
- Decisions are made deliberately, not by default
- The clear work ships without being blocked by the unclear work
- Ambiguity is visible, not hidden
- One-way doors are identified before you walk through them
- AI executes with discipline, not with guesses

A good CTO does not slow teams down. A good CTO prevents the kind of
mistakes that take months to undo.

This framework is that CTO, encoded as rules.


------------------------------------------------------------
## 6. Relationship to Framework Artefacts
------------------------------------------------------------

The 80/20 model maps directly onto existing framework artefacts:

| Zone | Artefact | Purpose |
|---|---|---|
| Clear 80% | `module-spec.md` | Declared intent — what AI executes |
| Clear 80% | `acceptance.md` | Verification criteria for the 80% |
| Ambiguous 20% | `todo.md` | Intent gaps — explicitly deferred work |
| Ambiguous 20% | `open-questions.md` | True unknowns — not yet decidable |
| One-way decisions | `adr/` | Permanent record of deliberate choices |
| Planned 20% | `roadmap.md` | When and how the 20% will be tackled |


------------------------------------------------------------
## 7. What This Model Is Not
------------------------------------------------------------

- **Not Agile** — no sprints, no ceremonies, no velocity
- **Not Waterfall** — no requirement to define everything before starting
- **Not a backlog** — todo.md is not a prioritised task list
- **Not a guarantee** — the 80% estimate is a judgment call, not a measurement
- **Not an excuse to ship incomplete work** — the 20% must be explicitly
  deferred and tracked, never silently ignored


------------------------------------------------------------
## 8. Authority
------------------------------------------------------------

This document is authoritative for the delivery philosophy of ragnor.dev.

All intake, execution, validation, and drift detection rules derive from
this model. If any other document contradicts this philosophy, this document
takes precedence.
