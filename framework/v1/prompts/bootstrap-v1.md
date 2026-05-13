---
prompt_id: bootstrap-v1
implements: onboarding-v1
version: v1
agent: generic
---

# Bootstrap Prompt Contract

Use this prompt to run ragnor.dev bootstrap with any AI assistant.
It defines the minimum contract for scaffolding `@specs/` directly via AI file edits.

---

## Prompt

```text
You are running ragnor.dev bootstrap.

Mode: <greenfield | brownfield | brownfield-external>
Repository root: <absolute or repo-relative path>

Goal:
Create the required `@specs/` structure and files for onboarding, without inventing extra workflow.
Do not write application code. Only create/update spec-layer files needed for bootstrap.

Rules:
1. Follow OB_00/10/20/30 onboarding docs exactly.
2. If a required input is unknown, ask a clarifying question before creating placeholder content.
3. For brownfield inference, mark inferred content explicitly:
   - `[INFERRED: needs verification]`
   - `[INFERRED FROM: <source>]`
4. Do not claim certainty for inferred items.
5. Keep everything in Markdown/YAML (no proprietary formats).

Step 1 — Detect onboarding path
- If Mode is greenfield: follow OB_10.
- If Mode is brownfield: follow OB_20.
- If Mode is brownfield-external: follow OB_20 + OB_30.

Step 2 — Create base structure
Create:
- `@specs/`
- `@specs/framework/`
- `@specs/framework.md` (must point to the adopted snapshot)
- `@specs/modules/`
- `@specs/system/`
- `@specs/roadmap.md`
- `@specs/glossary.md`
- `@specs/architecture.md`
- `@specs/specs-meta.md`

Step 3 — Create minimum module + system bootstrap output
Ensure bootstrap gate can pass by creating at least one module version and `system@v1`.

Required minimum:
- `@specs/modules/<module>/<module>@v1/module-spec.md`
- `@specs/modules/<module>/<module>@v1/todo.md`
- `@specs/modules/<module>/<module>@v1/adr/` (directory)
- `@specs/system/system@v1/system.yaml`
- `@specs/system/system@v1/overview.md`

module-spec.md front matter must include:
- `type: module-cycle`
- `module: <module>`
- `version: v1`
- `supersedes: null`
- `status: draft` (optional but recommended)
- `issues: {}`
- `tags: []`

system.yaml must include:
- `type: system`
- `version: v1`
- `status: draft` (optional but recommended)
- `modules:` mapping of module -> version

Step 4 — Mode-specific behavior

GREENFIELD:
- Ask for first module idea if absent.
- Run intake-style clarification enough to produce:
  - purpose
  - responsibilities
  - non-goals
  - at least 3 acceptance criteria
- Put ambiguous items in `todo.md` (as intent gaps).

BROWNFIELD:
- Infer candidate modules from code/docs/history.
- Create draft specs with explicit `[INFERRED: needs verification]` markers.
- Populate `todo.md` with known intent gaps.
- Create reconstructed ADR stubs where one-way decisions are evident.

BROWNFIELD-EXTERNAL:
- Do brownfield behavior.
- Import external knowledge by trust level and label:
  - `[FROM: <source>]`
  - `[INFERRED FROM: <source>]`
  - `[SUGGESTED FROM: <source>]`

Step 5 — Run bootstrap gate check
Verify:
1. `@specs/` exists
2. `@specs/framework.md` exists and points to an existing framework snapshot
3. At least one module version exists under `@specs/modules/`
4. `@specs/system/system@v1/` exists

If any check fails, output FAIL with exact missing paths.
If all pass, output PASS.

Step 6 — Output report (required)
Output exactly this structure:

BOOTSTRAP REPORT
Mode: <mode>
Repository: <path>

CREATED:
- <path>

UPDATED:
- <path> or NONE

INFERRED ITEMS REQUIRING HUMAN REVIEW:
- <file + item> or NONE

BOOTSTRAP GATE:
- Check 1: PASS|FAIL
- Check 2: PASS|FAIL
- Check 3: PASS|FAIL
- Check 4: PASS|FAIL

NEXT ACTION:
- <onboarding complete | human review required | missing input>
```

---

## Notes

- This is a prompt contract, not a CLI requirement.
- It is intentionally tool-agnostic: any capable AI assistant can execute it.
- Human review remains mandatory for brownfield inferred boundaries and decisions.
