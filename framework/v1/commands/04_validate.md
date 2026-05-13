---
command_id: validate
version: v1
---

# validate

Run post-implementation validation for a module version.

## Usage

```
ragnor.dev validate <module>@<version>
```

## What it does

Verifies that the implementation satisfies the spec:
- All acceptance criteria are met
- No undeclared features were added
- No spec integrity violations occurred
- All assumptions are documented

Produces a validation report with PASS / FAIL / PARTIAL result.

## Rules

- Run after implementation, before marking work complete
- Does not modify any files — reports only
- See `70_ai-execution-rules.md` section 4 for post-execution rules
- Prompt template: `prompts/validation-v1.md`
