---
prompt_id: execution-check-v1
implements: ai-execution-rules-v1
version: v1
agent: generic
---

# Execution Check Prompt

Use this prompt before implementation to enforce the pre-execution gate.
Run before writing any code.

---

## Prompt

```
You are running the pre-execution check for ragnor.dev.

Target: <module>@<version>
Branch: <current git branch>

Step 1 — Bootstrap gate
Verify all are true:
- @specs/ exists
- @specs/framework.md exists and points to an existing framework snapshot
- At least one module version exists under @specs/modules/
- @specs/system/system@v1/ exists

If any check fails: STOP and report onboarding is required.

Step 2 — Spec readiness gate
Confirm the target module spec is ready for AI execution:
- Intake process completed (see 20_idea-to-spec.md)
- Readiness checklist passed
- Human explicitly confirmed the spec

If any item fails: STOP and report what is missing.

Step 3 — Scope declaration
Read the target module spec and produce:
- IMPLEMENTING (clear 80%)
- NOT IMPLEMENTING (ambiguous 20%)
- ONE-WAY DECISIONS (if any)
- ASSUMPTIONS (if any)

Step 4 — Version boundary check
Confirm whether requested work would change:
- responsibilities/scope
- external contracts
- acceptance criteria
- dependencies/boundaries

If yes: STOP and report that a version bump decision is required
(see 50_version-operations.md section 3.4).

Step 5 — Human confirmation
Ask for explicit confirmation of scope before any implementation.

Output format:

EXECUTION CHECK REPORT
Module: <module>@<version>
Branch: <branch>

BOOTSTRAP GATE: PASS | FAIL
SPEC READINESS: PASS | FAIL
VERSION BOUNDARY: IN-SCOPE | VERSION-BUMP-REQUIRED

IMPLEMENTING:
- <item>

NOT IMPLEMENTING:
- <item>

ONE-WAY DECISIONS:
- <item> or NONE

ASSUMPTIONS:
- <item> or NONE

NEXT ACTION:
- <wait for human confirmation | onboarding required | intake required | version decision required>
```
