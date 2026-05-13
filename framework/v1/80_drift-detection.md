---
spec_id: drift-detection
rule_id: drift-detection-v1
type: framework-rule
version: v1
status: active
prompts:
  - drift-detection-v1
---

# 80 — Drift Detection

This document defines what spec-to-code drift is, how to detect it,
how to classify it, and how to resolve it.

Drift detection MUST be possible at any time, by any AI agent, using
any AI model, without requiring specialised tooling.


------------------------------------------------------------
## 1. What Is Drift
------------------------------------------------------------

Drift is a misalignment between a module's declared spec and its actual
implementation.

Drift occurs when:
- Code behaviour diverges from declared responsibilities
- An API contract changes without a spec update
- Acceptance criteria are no longer satisfied by the implementation
- Dependencies change without updating `context.md`
- A one-way decision was made during implementation without an ADR
- The 20% was implemented without going through the intake process
- A released spec was modified to match what was built (history rewriting)

Drift is not:
- An incomplete feature tracked in `todo.md` (that is an intent gap, not drift)
- A planned change in a draft module version (that is evolution, not drift)
- A known open question in `open-questions.md` (that is acknowledged ambiguity)

**The key distinction:** drift is unacknowledged misalignment.
Acknowledged gaps are not drift — they are explicitly tracked work.


------------------------------------------------------------
## 2. Types of Drift
------------------------------------------------------------

### 2.1 Silent Implementation Drift

Code was changed without updating the spec.

Example: the auth module's token refresh behaviour was changed to use
a sliding window, but `module-spec.md` still describes fixed expiry.

Risk level: HIGH — AI agents grounded on the spec will implement
against stale intent.

### 2.2 Spec Rewrite Drift

The spec was updated to match what was built, rather than what was intended.

Example: acceptance criteria were quietly changed after implementation
to match the actual behaviour rather than the original requirement.

Risk level: CRITICAL — this destroys the historical record and makes
the spec untrustworthy as a source of truth.

### 2.3 Scope Creep Drift

Features were added to the implementation that are not declared in the spec.

Example: the billing module now handles currency conversion, but this
responsibility is not declared in `module-spec.md`.

Risk level: MEDIUM — the spec is incomplete, not wrong. But AI agents
will not know about the undeclared behaviour.

### 2.4 Contract Drift

An external contract (API, event, interface) changed without a spec update
or version bump.

Example: a REST endpoint changed its response schema without updating
`acceptance.md` or creating a new module version.

Risk level: HIGH — downstream modules depending on the contract are
now operating against stale information.

### 2.5 Dependency Drift

Module dependencies changed without updating `context.md`.

Example: the dashboard module now directly queries the database, bypassing
the data module, but `context.md` still declares the data module as the
only dependency.

Risk level: MEDIUM — architectural boundaries are being violated silently.

### 2.6 Decision Drift

A one-way architectural decision was made during implementation without
an ADR.

Example: the team switched from JWT to session-based authentication
without recording the decision, the rationale, or the alternatives considered.

Risk level: HIGH — the decision will be re-argued, the rationale is lost,
and future AI agents have no context for why the architecture is the way it is.


------------------------------------------------------------
## 3. Drift Detection Rules
------------------------------------------------------------

Any AI agent MUST be able to detect drift by comparing the spec artefacts
against the codebase. No specialised tooling is required.

### 3.1 When to run drift detection

MUST run:
- Before starting any new implementation on a module
- After any significant code change to a module
- When onboarding an existing codebase
- When a new AI agent or AI model begins working on a module
- When a human suspects drift

SHOULD run:
- At the start of every development session on a module
- Before marking a module version as released

### 3.2 What to compare

For each module version being checked:

| Spec artefact | What to compare against |
|---|---|
| `module-spec.md` responsibilities | Actual code responsibilities |
| `module-spec.md` non-goals | Code does not implement these |
| `acceptance.md` criteria | Code satisfies each criterion |
| `context.md` dependencies | Actual import/call graph |
| `context.md` contracts | Actual API/event signatures |
| `adr/` decisions | Code reflects each decision |
| `todo.md` open items | Not silently implemented |

### 3.3 Drift severity classification

| Severity | Definition | Required action |
|---|---|---|
| CRITICAL | Spec was rewritten to match code | Restore original spec, create new version |
| HIGH | Code behaviour contradicts spec | Create new version or fix code |
| MEDIUM | Spec is incomplete (missing declared behaviour) | Update spec or create new version |
| LOW | Minor wording misalignment, no behaviour change | Update spec in place (draft only) |


------------------------------------------------------------
## 4. The Drift Detection Prompt Pattern
------------------------------------------------------------

Any AI agent can run drift detection using the following pattern.
This is the human-readable rule. The executable prompt is in
`prompts/drift-detection-v1.md`.

**Step 1 — Read the spec**
Read `module-spec.md`, `acceptance.md`, and `context.md` for the
target module version.

**Step 2 — Read the code**
Read the implementation files for the module. Focus on:
- Public interfaces and contracts
- Declared responsibilities (what the code actually does)
- Dependencies (what the code actually imports/calls)
- Behaviour (does it match acceptance criteria)

**Step 3 — Compare**
For each item in the spec, ask:
- Is this implemented?
- Is it implemented correctly (matches declared behaviour)?
- Is there code that is NOT in the spec?

**Step 4 — Classify findings**
For each misalignment found, classify by type (section 2) and
severity (section 3.3).

**Step 5 — Report**
Produce a structured drift report (see section 5).

**Step 6 — Do not silently fix**
An AI agent MUST NOT silently fix drift by updating the spec to match
the code or updating the code to match the spec.

All drift findings MUST be reported to the human before any action is taken.


------------------------------------------------------------
## 5. Drift Report Format
------------------------------------------------------------

```
DRIFT DETECTION REPORT
Module: <module>@<version>
Date: <date>
Agent: <ai model/tool used>

SUMMARY:
  Total findings: N
  Critical: N
  High: N
  Medium: N
  Low: N

FINDINGS:

[CRITICAL/HIGH/MEDIUM/LOW] <finding title>
  Type: <drift type from section 2>
  Spec says: <what the spec declares>
  Code does: <what the code actually does>
  Location: <file/function/line if known>
  Recommended action: <fix code | update spec | create new version | create ADR>

CLEAN AREAS:
  <list of spec items verified as correctly implemented>

NOTES:
  <any additional context>
```


------------------------------------------------------------
## 6. Drift Resolution
------------------------------------------------------------

When drift is found, the resolution depends on the module version status:

### 6.1 Draft module version

Options (human decides):
- **Fix the code** — bring implementation in line with spec
- **Update the spec** — if the implementation is correct and the spec
  was wrong or incomplete (LOW severity only)
- **Create a new version** — if the change represents new intent

### 6.2 Released module version

A released module version MUST NOT be modified.

Options (human decides):
- **Fix the code** — bring implementation in line with the released spec
- **Create a new module version** — declare the new intent in a new version

Spec rewrite drift (type 2.2) on a released version is a CRITICAL violation.
The original spec MUST be restored. A new version MUST be created if the
new behaviour is intentional.

### 6.3 One-way decision drift (type 2.6)

An ADR MUST be created retroactively, documenting:
- The decision that was made
- The context at the time
- The rationale (best reconstruction)
- The consequences
- Status: `accepted` (already implemented)

This does not undo the decision. It restores the architectural memory.


------------------------------------------------------------
## 7. Prevention
------------------------------------------------------------

Drift is prevented by following the execution rules in `70_ai-execution-rules.md`.

The primary prevention mechanisms are:
- The spec readiness gate (no execution without a complete spec)
- The execution contract (AI declares assumptions, surfaces ambiguity)
- The post-execution validation check
- The version bump triggers (changes to intent require new versions)

Drift is a symptom of skipping these gates. The gates exist to prevent drift.


------------------------------------------------------------
## 8. Relationship to Other Documents
------------------------------------------------------------

- `10_delivery-model.md` — why the 20% must be surfaced, not guessed
- `50_version-operations.md` — when drift resolution requires a new version
- `70_ai-execution-rules.md` — the execution rules that prevent drift
- `prompts/drift-detection-v1.md` — the executable drift detection prompt
- `commands/03_drift.md` — the command that runs drift detection


------------------------------------------------------------
## 9. Authority
------------------------------------------------------------

This document is authoritative for drift detection and resolution.

Drift MUST be surfaced. Drift MUST NOT be silently fixed.
The spec is the source of truth — when spec and code disagree,
a human decides which one is correct.
