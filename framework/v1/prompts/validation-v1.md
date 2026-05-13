---
prompt_id: validation-v1
implements: ai-execution-rules-v1
version: v1
agent: generic
---

# Validation Prompt

Use this prompt after implementation to verify the code satisfies the spec.
Run before marking any work as complete.

---

## Prompt

```
You are running post-implementation validation for ragnor.dev.

Module: <module>@<version>
Branch: <current git branch>

Read the following files:
- @specs/modules/<module>/<module>@<version>/module-spec.md
- @specs/modules/<module>/<module>@<version>/acceptance.md (if exists)
- @specs/modules/<module>/<module>@<version>/todo.md

Then read the implementation code for <module>.

Run this validation checklist:

ACCEPTANCE CRITERIA VALIDATION
For each acceptance criterion in the spec:
[ ] <criterion> — PASS | FAIL | PARTIAL
    Evidence: <what in the code satisfies or fails this criterion>

SCOPE VALIDATION
[ ] No features implemented that are not in the spec
[ ] No module boundaries violated
[ ] No undeclared dependencies added
[ ] No one-way decisions made without an ADR

SPEC INTEGRITY VALIDATION
[ ] module-spec.md was not modified to match the code
[ ] Acceptance criteria were not weakened to match the implementation
[ ] todo.md reflects current state (completed items checked, new gaps added)

ASSUMPTION VALIDATION
List all assumptions declared during implementation:
- <assumption 1> — still valid | needs review
- <assumption 2> — still valid | needs review

DRIFT CHECK
Any misalignment between spec and implementation not covered above:
- <finding> or NONE

---

VALIDATION REPORT

RESULT: PASS | FAIL | PARTIAL

FAILED CRITERIA:
- <criterion> — <reason>

SCOPE VIOLATIONS:
- <violation> or NONE

SPEC INTEGRITY ISSUES:
- <issue> or NONE

ASSUMPTIONS NEEDING REVIEW:
- <assumption> or NONE

DRIFT FINDINGS:
- <finding> or NONE

RECOMMENDED ACTIONS:
- <action 1>
- <action 2>

---

Do not modify any files. Report only.
A human reviews this report before any action is taken.
```
