---
prompt_id: drift-detection-v1
implements: drift-detection-v1
version: v1
agent: generic
---

# Drift Detection Prompt

Use this prompt to detect spec-to-code drift for a module version.
Copy and send to any AI agent. Supports single target or all modules.

---

## Prompt

```
You are running drift detection for ragnor.dev.

Target: <module>@<version> | --all
Branch: <current git branch>

Target handling:
- If Target is `<module>@<version>`: run steps for that module only.
- If Target is `--all`: enumerate all module versions under `@specs/modules/`
  and run steps for each, then produce an aggregate summary plus per-module reports.

Step 1 — Read the spec
Read the following files:
- @specs/modules/<module>/<module>@<version>/module-spec.md
- @specs/modules/<module>/<module>@<version>/acceptance.md (if exists)
- @specs/modules/<module>/<module>@<version>/context.md (if exists)
- @specs/modules/<module>/<module>@<version>/adr/ (all files)

Step 2 — Read the code
Read the implementation files for <module>. Focus on:
- Public interfaces and contracts
- What the code actually does (responsibilities)
- What the code actually imports or calls (dependencies)
- Whether the code satisfies each acceptance criterion

Step 3 — Compare
For each item declared in the spec, determine:
- Is it implemented?
- Is it implemented correctly (matches declared behaviour)?
- Is there code behaviour NOT declared in the spec?

Step 4 — Classify findings
For each misalignment, classify by:
- Type: silent-implementation | spec-rewrite | scope-creep | contract | dependency | decision
- Severity: CRITICAL | HIGH | MEDIUM | LOW
(See the adopted framework snapshot referenced by `@specs/framework.md`,
section 2 and 3.3 of `80_drift-detection.md`.)

Step 5 — Produce the drift report
For single target, use this exact format:

DRIFT DETECTION REPORT
Module: <module>@<version>
Branch: <branch>
Agent: <your model/tool name>

SUMMARY:
  Total findings: N
  Critical: N
  High: N
  Medium: N
  Low: N

FINDINGS:
[SEVERITY] <finding title>
  Type: <drift type>
  Spec says: <what the spec declares>
  Code does: <what the code actually does>
  Location: <file/function if known>
  Recommended action: <fix code | update spec | create new version | create ADR>

CLEAN AREAS:
  <list of spec items verified as correctly implemented>

NOTES:
  <any additional context>

For `--all`, use this format:

DRIFT DETECTION SUMMARY REPORT
Branch: <branch>
Agent: <your model/tool name>

SUMMARY:
  Modules checked: N
  Total findings: N
  Critical: N
  High: N
  Medium: N
  Low: N

MODULE REPORTS:
- <module>@<version> — Findings: N (Critical: N, High: N, Medium: N, Low: N)
  Top issues:
  - <issue 1> or NONE
  - <issue 2> or NONE

Step 6 — Do not fix anything
Report only. Do not modify the spec or the code.
All findings must be reviewed by a human before any action is taken.
```

---

## Agent-specific variants

For Claude Code: save as `.claude/commands/drift-detection.md`
For Cursor: save as `.cursor/rules/drift-detection.md`
For GitHub Copilot: save as `.github/agents/drift-detection.agent.md`

Replace the generic prompt above with the agent's native command format
while preserving all steps and the report format.
