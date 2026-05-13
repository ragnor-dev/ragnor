---
spec_id: architecture
type: meta-spec
version: v1
status: active
---

# Architecture

This document describes the high-level architecture of a repository
that adopts the ragnor.dev framework.

## Spec Layer

The `@specs/` directory is the specification layer. It sits alongside
the implementation code and is the source of truth for architecture,
intent, and decisions.

```
repo-root/
  @specs/
    framework/v1/     ← framework rules
    modules/          ← module versions
    system/           ← system versions
  src/                ← implementation code
  ...
```

## Two-Level Model

The framework uses two versioned units:

**Module versions** — the spec for a single domain boundary.
Each module evolves independently at its own pace.

**System versions** — an immutable snapshot of a module composition.
System versions pin which module versions are coherent together.

## Immutability

Released versions are immutable. This is the central architectural
invariant of the framework. It enables:

- stable AI context (agents can rely on released specs)
- auditable history (decisions are never rewritten)
- safe composition (system versions reference stable module versions)

## Publication

The spec layer is the source. Downstream targets (docs sites, wikis,
Confluence, AI context files) are derived views generated from `@specs/`.

The `merge_specs.py` script generates `v1.md` — a single-file
concatenation of the framework for AI consumption.
