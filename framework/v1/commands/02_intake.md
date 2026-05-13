---
command_id: intake
version: v1
---

# intake

Run the idea-to-spec intake process for a new module or new capability.

## Usage

```
ragnor.dev intake "<raw idea>"
ragnor.dev intake "<raw idea>" --module <existing-module>
```

## What it does

Runs the five-step intake process defined in `20_idea-to-spec.md`:
1. Captures raw intent
2. Runs clarification and gap detection
3. Classifies the 80/20 split
4. Identifies one-way decisions
5. Runs the spec readiness check

Produces a completed `module-spec.md` ready for implementation.

## Rules

- No AI execution begins without a completed intake
- The human MUST confirm the spec before implementation starts
- See `20_idea-to-spec.md` for the full intake rules
- Prompt template: `prompts/intake-v1.md`
