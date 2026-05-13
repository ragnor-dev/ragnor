---
command_id: drift
version: v1
---

# drift

Run drift detection for a module version.

## Usage

```
ragnor.dev drift <module>@<version>
ragnor.dev drift --all
```

## What it does

Compares the module spec against the implementation and reports
any misalignment (drift).

Produces a structured drift report with findings classified by
type and severity.

## Rules

- Does not modify any files — reports only
- Human reviews findings before any action is taken
- See `80_drift-detection.md` for drift classification rules
- Prompt template: `prompts/drift-detection-v1.md`
