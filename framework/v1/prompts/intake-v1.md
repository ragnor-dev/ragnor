---
prompt_id: intake-v1
implements: idea-to-spec-v1
version: v1
agent: generic
---

# Intake Prompt

Use this prompt to run the idea-to-spec intake process.
Copy and send to any AI agent with your raw idea.

---

## Prompt

```
You are running the ragnor.dev intake process.

Raw idea: <describe your idea in plain language here>
Target module: <new-module | existing-module-name>

Follow these steps exactly. Complete each step before moving to the next.
Do not write any code. Do not make technical decisions.

STEP 1 — CAPTURE RAW INTENT
Restate the idea as a clear purpose statement (1-3 sentences).
Focus on: what problem does this solve, who has this problem, why does it matter.
Do not include technical details.

Output: PURPOSE STATEMENT: <statement>

---

STEP 2 — CLARIFICATION AND GAP DETECTION
For the purpose statement, identify everything that is unclear, assumed, or ambiguous.
Ask clarifying questions for each gap.
Mark ambiguities inline as [UNCLEAR: <question>].
Do NOT resolve ambiguities — surface them.

Output: CLARIFICATION QUESTIONS: <numbered list>
        ANNOTATED PURPOSE: <purpose with [UNCLEAR] markers>

---

STEP 3 — 80/20 CLASSIFICATION
Split the clarified intent into two explicit lists:

CLEAR ZONE (the 80%) — unambiguous, reversible, safe for AI to implement:
- <item 1>
- <item 2>
...

AMBIGUOUS ZONE (the 20%) — unclear, needs more information, or one-way decisions:
- <item 1> [reason: ambiguous | one-way-decision | needs-feedback]
- <item 2>
...

---

STEP 4 — ONE-WAY DECISION IDENTIFICATION
From the ambiguous zone, identify any one-way decisions.
For each one-way decision:
- Name it
- Describe what makes it irreversible
- List 2-3 options being considered
- Identify what information is needed to decide

Output: ONE-WAY DECISIONS: <list with options>

---

STEP 5 — SPEC READINESS CHECK
Run this checklist:
[ ] Purpose is stated in 1-3 sentences without technical jargon
[ ] Responsibilities are listed without implementation details
[ ] Acceptance criteria exist for the clear 80% (minimum 3)
[ ] The ambiguous 20% is explicitly listed
[ ] One-way decisions are identified
[ ] No [UNCLEAR] markers remain in the clear zone
[ ] Non-goals are stated

Report: READINESS CHECK: <pass/fail for each item>

If all pass: output the completed module-spec.md template.
If any fail: report which items fail and ask for clarification.

For target module handling:
- If `Target module` is `new-module`: produce a new module `@v1` spec.
- If `Target module` is an existing module name:
  1. Read current `@specs/modules/<module>/<module>@vN/module-spec.md`
  2. Check if the requested change triggers a new version (see `50_version-operations.md` section 3.4)
  3. If version bump required: STOP and report `VERSION-BUMP-REQUIRED`
  4. If no version bump required: output updates for current draft spec + `todo.md` only

---

COMPLETED MODULE SPEC (only if readiness check passes):

---
type: module-cycle
module: <module-name>
version: v1
status: draft
supersedes: null
issues: {}
tags: []
---

# <Module Name> — v1

## Purpose
<1-3 sentence purpose statement>

## Responsibilities
- <responsibility 1>
- <responsibility 2>

## Non-Goals
- <non-goal 1>

## Acceptance Criteria
- [ ] <criterion 1>
- [ ] <criterion 2>
- [ ] <criterion 3>
```

---

## After the prompt

Take the completed module spec and:
1. If new module: create `@specs/modules/<module>/<module>@v1/` and save `module-spec.md`
2. If existing module (in-scope draft): update current draft `module-spec.md` in place
3. Create or update `todo.md` with the ambiguous 20% as intent gaps
4. Ensure `adr/` directory exists
5. Add one-way decision stubs to `adr/` as ADR-0001, ADR-0002, etc., when needed
