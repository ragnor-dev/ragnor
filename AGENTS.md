# AGENTS.md

This file is the entry point for AI agents (Claude, Cursor, Copilot, Lovable,
and others) working in a repository that adopts the ragnor.dev framework.

Read this file first. Then read the framework files listed in section 3.

**Before doing anything else: verify the bootstrap gate (section 7).**

**Repository mode note:** this repository is the ragnor.dev framework source
repository (meta-level). Canonical framework files are under `framework/v1/`
here. In adopter repositories, they live under `@specs/framework/v1/`.

---

## 1. What ragnor.dev is

ragnor.dev is a spec-as-code framework. It defines a structured specification
layer inside a Git repository — versioned Markdown and YAML files that describe
the architecture, intent, and decisions of a software system.

The `@specs/` directory is the specification layer. It is the source of truth
for architecture and intent. Code is the implementation. When they conflict,
investigate — do not silently prefer one over the other.

---

## 2. Core rules you MUST follow

### 2.1 Immutability

A module version or system version MUST be treated as immutable if:
- it declares `status: released` in its front matter, OR
- it is referenced by a system version that declares `status: released`

NEVER modify an immutable version. If a change is needed, create a new version.

### 2.2 Version creation

Create a new module version (`<module>@v(N+1)`) when:
- the current version is immutable (see 2.1)
- responsibilities, scope, or non-goals change
- the external contract changes (APIs, events, invariants)
- acceptance criteria are added, removed, or reinterpreted
- dependencies or boundary relationships change

Do NOT create a new version for:
- closing gaps already tracked in `todo.md`
- wording cleanup without meaning change
- adding examples that support existing acceptance criteria

### 2.3 todo.md is the intent gap ledger

`todo.md` tracks remaining work to satisfy declared intent. It is not a
task tracker, backlog, or assignment system.

Every todo item MUST use the ID format: `<module>@v<N>-<NNN>`
Example: `auth@v1-001`

IDs are monotonically increasing and never reused.

### 2.4 module-spec.md is the canonical entry point

Every module version MUST have a `module-spec.md` containing at minimum:
- purpose / intent
- responsibilities / scope
- acceptance criteria

It MUST NOT become a work log. Work belongs in `todo.md`.

### 2.5 ADRs capture architectural decisions

The `adr/` directory is mandatory. Write an ADR when a decision is:
- hard to reverse
- architecturally constraining
- a trade-off likely to be re-argued
- a policy you or other agents must follow

ADR naming: `adr/0001-short-title.md`, `adr/0002-short-title.md`, ...

### 2.6 System versions are snapshots, not live state

System versions pin a specific composition of module versions. They do not
auto-update when modules evolve. Creating a new module version does NOT
automatically update any system version.

---

## 3. Framework files to read

Read these files in order for full context:

1. `framework/v1/00_overview.md` — core concepts and model
2. `framework/v1/50_version-operations.md` — all versioning rules
3. `framework/v1/30_module-bootstrap.md` — how to create a new module
4. `framework/v1/60_todo-management.md` — todo.md rules
5. `framework/v1/40_authoring-guidelines.md` — what content goes where
6. `framework/v1/01_glossary.md` — terminology

For a single-file version of the full framework, read `v1.md` if present.

---

## 4. Repository structure

```
framework/v1/       ← framework rules in this source repository

@specs/             ← adopter repository specification layer
  framework/v1/     ← framework copy in adopter repositories
  modules/          ← module versions (one directory per module)
  system/           ← system versions (immutable snapshots)
  specs-meta.md     ← repository layout conventions
  glossary.md       ← terminology
```

---

## 5. What you MUST NOT do

- Modify any file inside a released version directory
- Create `v2+` versions automatically without explicit instruction
- Invent lifecycle states beyond `draft` and `released`
- Add owners, due dates, or priorities to `todo.md`
- Treat `todo.md` as a backlog or sprint board
- Reuse or renumber todo IDs
- Silently repair spec inconsistencies — surface them instead
- Begin implementation before the spec readiness gate passes
- Guess at ambiguous requirements — surface them instead
- Make one-way architectural decisions without explicit human approval
- Silently fix drift — report it and wait for human instruction

---

## 6. Command vocabulary

This framework defines a set of commands you can execute.
Invoke them by name in any conversation:

| Command | What it does |
|---|---|
| `ragnor.dev help` | List all commands |
| `ragnor.dev bootstrap` | Initialise `@specs/` in this repository |
| `ragnor.dev intake "<idea>"` | Run idea-to-spec intake for a new module |
| `ragnor.dev drift <module>@<version>` | Run drift detection |
| `ragnor.dev validate <module>@<version>` | Run post-implementation validation |
| `ragnor.dev status` | Show spec status across all modules |
| `ragnor.dev adr <module>@<version> "<title>"` | Create a new ADR |
| `ragnor.dev version <module>@<version>` | Bump to a new module version |
| `ragnor.dev update-check` | Check for a newer framework version |

Command definitions:
- framework-source repo: `framework/v1/commands/`
- adopter repo: `@specs/framework/v1/commands/`

Prompt templates:
- framework-source repo: `framework/v1/prompts/`
- adopter repo: `@specs/framework/v1/prompts/`

---

## 7. Bootstrap gate

This gate applies to repositories that **adopt** ragnor.dev for a concrete
system.

For the **ragnor.dev framework source repository itself** (meta/framework-only
repo), `@specs/modules/` and `@specs/system/system@v1/` MAY be intentionally
absent. In that case:
- treat this repository as framework-only
- skip checks 3 and 4 below
- continue with framework/spec authoring tasks

Before executing any command, verify:

1. Does `@specs/` exist at the repository root?
2. Does the framework path exist with framework files?
   - framework-source repo: `framework/v1/`
   - adopter repo: `@specs/framework/v1.md`
3. Does at least one module version exist under `@specs/modules/`?
4. Does `@specs/system/system@v1/` exist?

If any check fails:
- Stop immediately
- Inform the human that onboarding is required
- Suggest running `ragnor.dev bootstrap`
- Do not proceed with any other command

Once the gate passes, run `ragnor.dev update-check` before proceeding.
See `framework/v1/commands/08_update-check.md` for update-check rules.

---

## 8. When in doubt

If you are unsure whether a change requires a new version, apply this test:

> Does this change alter the declared meaning, contract, or acceptance
> criteria of the module? If yes, new version. If no, stay in the current draft.

If you find a spec inconsistency (e.g. code contradicts a released spec),
surface it as a finding. Do not silently resolve it in either direction.
