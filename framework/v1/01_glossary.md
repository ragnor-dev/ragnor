---
spec_id: glossary
type: framework-spec
version: v1
status: active
---

# Glossary

This file defines framework-level terminology. All specs, tools, and AI
agents MUST use these terms consistently.

## Module

A long-lived, versioned unit that owns a coherent responsibility or domain
boundary, exposes a defined contract, and evolves independently of other
modules. A module is not a UI component, a helper library, or an
implementation detail without domain meaning.

## Module Version

A versioned specification of a single module, stored at
`@specs/modules/<module>/<module>@vN/`. The primary unit of authoring in
the framework.

## System Version

An immutable snapshot of a module composition — a declaration of which
module versions are considered coherent together, stored at
`@specs/system/system@vN/`. System versions compose module versions; they
do not redefine them.

## Draft

A version status (`status: draft`) indicating the version is
work-in-progress and may evolve freely. Draft versions are mutable.

## Released

A version status (`status: released`) indicating the version is declared
complete and MUST be treated as immutable. Released versions are permanent
historical records.

## Immutability Boundary

The point at which a version becomes immutable. A version crosses the
immutability boundary when it is marked `status: released` OR when it is
referenced by a system version marked `status: released`.

## Intent Gap

The difference between the declared intent of a module version (its
responsibilities, acceptance criteria, and contracts) and the current
implementation state. Intent gaps are tracked in `todo.md`.

## Intent Gap Ledger

The `todo.md` file. A Markdown checklist of remaining work required for
the module version to fully satisfy its declared intent. Not a task
tracker, backlog, or assignment system.

## Module-Spec

The `module-spec.md` file. The canonical entry point for a module version.
MUST contain purpose, responsibilities, and acceptance criteria at minimum.

## ADR (Architectural Decision Record)

A short document capturing a significant architectural decision: its
context, the decision made, consequences, and alternatives considered.
Stored in the `adr/` directory of a module or system version.

## Supersedes

A front matter field (`supersedes: vN`) declaring which previous version
this version replaces. Establishes the version lineage chain.

## Framework Version

A versioned set of framework rules.
Framework versions are independent of system and module versions.
Changes that alter the meaning of framework rules MUST increment the
framework version.

## Framework Snapshot

An immutable exported framework artefact stored in adopter repositories at
`@specs/framework/framework-v<major>.<minor>.md`.

## Framework Pointer

The stable file `@specs/framework.md` in an adopter repository. It points to
exactly one framework snapshot and declares the currently adopted framework
version for that repository.

## Spec Layer

The `@specs/` directory tree. The specification layer of a repository —
the structured, versioned, human- and AI-readable knowledge base that
lives alongside the code it describes.
