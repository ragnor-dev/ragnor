---
command_id: bootstrap
version: v1
---

# bootstrap

Initialise `@specs/` in a repository.

## Usage

```
ragnor.dev bootstrap
ragnor.dev bootstrap --brownfield
ragnor.dev bootstrap --brownfield --external
```

## What it does

**Default (greenfield):**
- Creates `@specs/` directory structure
- Creates `@specs/framework/` snapshot directory
- Adds `@specs/framework.md` pointer
- Adds initial framework snapshot file (for example `@specs/framework/framework-v1.0.md`)
- Creates `@specs/roadmap.md`, `@specs/glossary.md`, `@specs/architecture.md`
- Prompts for first module intake

**`--brownfield`:**
- Creates `@specs/` directory structure
- Analyses existing codebase to infer module boundaries
- Creates draft module specs with `[INFERRED]` markers
- Creates `system@v1` referencing all inferred modules
- See `OB_20_onboarding-brownfield.md`

**`--brownfield --external`:**
- All of the above
- Also imports from external sources (Confluence, Jira, etc.)
- See `OB_30_onboarding-external.md`

## Rules

- Bootstrap MUST complete before any other command runs
- Human review MUST follow brownfield bootstrap before development begins
- See `OB_00_onboarding-overview.md` for the bootstrap gate
- Prompt template: `prompts/bootstrap-v1.md`
