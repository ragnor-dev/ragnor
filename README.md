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

1. Add `@specs/` to your repository root
2. Copy the framework files from `framework/v1/` (from this framework source repo) into `@specs/framework/v1/` in your product repo
3. Create your first module: `@specs/modules/<your-module>/<your-module>@v1/`
4. Add `module-spec.md`, `todo.md`, and an empty `adr/` directory
5. Point your AI assistant at `@specs/` and start building with grounded context

See [`framework/v1/30_module-bootstrap.md`](framework/v1/30_module-bootstrap.md)
for the full bootstrap process.

If you are working in the ragnor.dev framework source repository itself,
`@specs/modules/` and `@specs/system/` may be intentionally absent.

---

## For AI agents

See [`AGENTS.md`](AGENTS.md) for a structured description of the framework
rules written as an AI-consumable reference.

See [`llms.txt`](llms.txt) for the AI crawler entry point.

A single-file version of the full framework is available at `v1.md`
(generated — run `python merge_specs_v1.py` to produce it).

---

## Framework files

| File | Purpose |
|------|---------|
| [`framework/v1/00_overview.md`](framework/v1/00_overview.md) | Core concepts and model |
| [`framework/v1/50_version-operations.md`](framework/v1/50_version-operations.md) | Versioning rules and immutability |
| [`framework/v1/30_module-bootstrap.md`](framework/v1/30_module-bootstrap.md) | Creating a new module |
| [`framework/v1/60_todo-management.md`](framework/v1/60_todo-management.md) | todo.md format and lifecycle |
| [`framework/v1/40_authoring-guidelines.md`](framework/v1/40_authoring-guidelines.md) | What content goes where |
---

## Status

Framework v1 is active and stable.

Built by [Adrian Topala](https://topala.dev).
