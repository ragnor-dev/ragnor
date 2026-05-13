---
prompt_id: status-v1
implements: onboarding-v1
version: v1
agent: generic
---

# Status Prompt

Use this prompt to produce a read-only framework status report.
Run on demand for all modules or a single module.

---

## Prompt

```text
You are generating a ragnor.dev status report.

Target: <all | module-name>
Branch: <current git branch>

Step 1 — Bootstrap gate check
Verify:
1. `@specs/` exists
2. `@specs/framework.md` exists and points to an existing framework snapshot
3. At least one module version exists under `@specs/modules/`
4. `@specs/system/system@v1/` exists

Step 2 — Read system status
- Read latest `@specs/system/system@vN/system.yaml` if any exist
- Capture system version and status
- Capture referenced module map

Step 3 — Read module status
- If Target is `all`: inspect all module versions under `@specs/modules/`
- If Target is `module-name`: inspect only that module's versions
- For each module version, capture:
  - version status (draft/released, if declared)
  - open todo count from `todo.md`
  - optional drift note if available in local artefacts (otherwise `never`)

Step 4 — Read roadmap status
- Read `@specs/roadmap.md` if present
- Extract Current and Planned items if present

Step 5 — Output report (read-only)
Do not modify any files. Use this format:

ragnor.dev STATUS REPORT
Repository: <repo name>
Branch: <branch>
Framework: v1

SYSTEM
  <system@vN or NONE> — <draft|released|unknown>
  Modules: <module@version list or NONE>

MODULES
  <module>@<version> — <draft|released|unknown>
    Open todos: <N>
    Last drift check: <date or never>

ROADMAP
  Current: <items or NONE>
  Planned: <items or NONE>

BOOTSTRAP GATE
  Check 1: PASS|FAIL
  Check 2: PASS|FAIL
  Check 3: PASS|FAIL
  Check 4: PASS|FAIL
```
