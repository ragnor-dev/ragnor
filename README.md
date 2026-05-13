# ragnor.dev

**A spec-as-code framework for teams that build with AI.**

Your architecture lives in your head. Your decisions live in a wiki nobody reads.
Your AI assistant hallucinates because it has no grounded context.

ragnor.dev fixes this by putting a structured specification layer directly inside
your repository — versioned, diffable, and readable by both humans and AI agents.

---

## The idea in one sentence

> Specs live in the repo. Everything else — wikis, docs, AI context — is a derived view.

This is the same model as infrastructure-as-code: the YAML is the truth, the
cloud console is just a view. ragnor.dev applies that model to architecture,
decisions, and intent.

---

## Scope

ragnor.dev is a **framework and governance layer** for software delivery with AI.
It defines how intent is declared, versioned, and validated inside a repository.

This repository is the **framework source repository** (meta-level), not a
product implementation repository. Repositories that adopt ragnor.dev apply
these rules to their own module and system specs under `@specs/`.

---

## What it is not

- Not a task tracker or sprint board (`todo.md` is an intent-gap ledger)
- Not a CI/CD or agent orchestration engine
- Not a replacement for implementation code or tests
- Not a project management workflow with owners/dates/priorities

---

## What you get

- **Grounded AI** — AI agents clone your repo and get your architecture for free.
  No more re-explaining your system every session.

- **Diffable specs** — every spec change is a PR. Reviewable, auditable, reversible.

- **Immutability by default** — released versions are frozen. History is never rewritten.
  New intent = new version.

- **Tool-agnostic** — plain Markdown and YAML. No required tooling, no lock-in.
  Works with any editor, any CI, any AI assistant.

- **Publication-ready** — specs can be extracted and pushed to Confluence, Notion,
  a docs site, or anywhere else. The repo is the source; everything else is a view.

---

## How it works

ragnor.dev defines two versioned units:

**Module versions** — the spec for a single domain boundary at a point in time.

```
@specs/modules/auth/auth@v1/
  module-spec.md   ← purpose, responsibilities, acceptance criteria
  todo.md          ← remaining intent gaps (not a task tracker)
  adr/             ← architectural decisions
```

**System versions** — an immutable snapshot of which module versions are coherent together.

```
@specs/system/system@v1/
  system.yaml      ← module composition map
  overview.md      ← human explanation
```

Modules evolve independently. System versions pin a coherent composition.
Once released, versions are immutable. New intent requires a new version.

---

## Quick start

If you are adopting ragnor.dev in a software repository:

1. Copy [`versions/v1.md`](versions/v1.md) from this repo into `@specs/framework/v1.md` in your product repo
2. Point your AI assistant at `@specs/framework/v1.md`
3. Tell your AI to run `ragnor.dev bootstrap` — it will create the full `@specs/` structure and guide you through your first module

The entire onboarding process is AI-assisted. The framework file gives your agent everything it needs.

If you are working in the ragnor.dev framework source repository itself,
`@specs/modules/` and `@specs/system/` may be intentionally absent.

---

## For AI agents

See [`AGENTS.md`](AGENTS.md) for a structured description of the framework
rules written as an AI-consumable reference.

See [`llms.txt`](llms.txt) for the AI crawler entry point.

A single-file version of the full framework is available at [`versions/v1.md`](versions/v1.md)
(generated — run `python @scripts/merge_specs_v1.py` to produce it).

---

## Framework files

| File | Purpose |
|------|---------|
| [`framework/v1/00_overview.md`](framework/v1/00_overview.md) | Core concepts and model |
| [`framework/v1/01_glossary.md`](framework/v1/01_glossary.md) | Framework terminology |
| [`framework/v1/02_specs-meta.md`](framework/v1/02_specs-meta.md) | Repository layout and conventions |
| [`framework/v1/10_delivery-model.md`](framework/v1/10_delivery-model.md) | Delivery model and workflow |
| [`framework/v1/20_idea-to-spec.md`](framework/v1/20_idea-to-spec.md) | Idea intake and spec creation |
| [`framework/v1/30_module-bootstrap.md`](framework/v1/30_module-bootstrap.md) | Creating a new module |
| [`framework/v1/40_authoring-guidelines.md`](framework/v1/40_authoring-guidelines.md) | What content goes where |
| [`framework/v1/50_version-operations.md`](framework/v1/50_version-operations.md) | Versioning rules and immutability |
| [`framework/v1/60_todo-management.md`](framework/v1/60_todo-management.md) | todo.md format and lifecycle |
| [`framework/v1/70_ai-execution-rules.md`](framework/v1/70_ai-execution-rules.md) | Rules for AI agent execution |
| [`framework/v1/80_drift-detection.md`](framework/v1/80_drift-detection.md) | Detecting spec/code drift |
| [`framework/v1/90_roadmap.md`](framework/v1/90_roadmap.md) | Framework roadmap |
| [`framework/v1/95_git-integration.md`](framework/v1/95_git-integration.md) | Git workflow integration |
| [`framework/v1/OB_00_onboarding-overview.md`](framework/v1/OB_00_onboarding-overview.md) | Onboarding overview |
| [`framework/v1/OB_10_onboarding-greenfield.md`](framework/v1/OB_10_onboarding-greenfield.md) | Greenfield onboarding path |
| [`framework/v1/OB_20_onboarding-brownfield.md`](framework/v1/OB_20_onboarding-brownfield.md) | Brownfield onboarding path |
| [`framework/v1/OB_30_onboarding-external.md`](framework/v1/OB_30_onboarding-external.md) | External onboarding path |
| [`framework/v1/commands/`](framework/v1/commands/) | Command definitions (help, bootstrap, intake, drift, validate, status, adr, version, update-check) |
| [`framework/v1/prompts/`](framework/v1/prompts/) | Prompt templates for each command |

---

## Status

Framework v1 is active and stable.

Built by [Adrian Topala](https://topala.dev).
