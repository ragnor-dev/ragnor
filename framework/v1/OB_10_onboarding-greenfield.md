---
spec_id: onboarding-greenfield
type: framework-rule
version: v1
status: active
---

# OB_10 — Greenfield Onboarding

This document defines the onboarding process for new projects
starting from scratch with no existing codebase.


------------------------------------------------------------
## 1. When to Use This Path
------------------------------------------------------------

Use greenfield onboarding when:
- The repository is empty or contains only scaffolding
- There is no existing architecture to reverse-engineer
- You are starting with a raw idea and building forward

If you have an existing codebase, use `OB_20_onboarding-brownfield.md`.


------------------------------------------------------------
## 2. The Greenfield Onboarding Process
------------------------------------------------------------

### Step 1 — Scaffold the spec layer

Create the directory structure:

```
@specs/
  framework/v1/     ← copy framework files here
  modules/          ← empty, modules added during intake
  system/           ← empty, system@v1 created after first modules
  roadmap.md        ← initial roadmap
  glossary.md       ← project terminology
  architecture.md   ← high-level architecture view
  specs-meta.md     ← repository conventions
```

AI agent command: `ragnor.dev bootstrap`

### Step 2 — Run the idea intake process

Before creating any module, run the intake process for your first idea.

See `20_idea-to-spec.md` for the full intake process.

Output: a completed `module-spec.md` for your first module.

### Step 3 — Create the first module

Using the output of the intake process, create the first module version:

```
@specs/modules/<module-name>/<module-name>@v1/
  module-spec.md    ← from intake process
  todo.md           ← the 20% from intake
  adr/              ← empty initially
```

See `30_module-bootstrap.md` for module creation rules.

### Step 4 — Create system@v1

Once at least one module exists, create the initial system snapshot:

```
@specs/system/system@v1/
  system.yaml       ← references all current module@v1 versions
  overview.md       ← what this system is
```

### Step 5 — Write the initial roadmap

Create `@specs/roadmap.md` with:
- Current: the module(s) being actively built
- Planned: the next modules you know you will need
- Deferred: the 20% from the intake process

### Step 6 — Verify the bootstrap gate

Run the bootstrap gate check (see `OB_00_onboarding-overview.md` section 1).
All checks must pass before development begins.


------------------------------------------------------------
## 3. Greenfield Onboarding Quality Bar
------------------------------------------------------------

Greenfield onboarding is complete when:

- [ ] `@specs/` structure exists and is valid
- [ ] At least one module spec passes the readiness check
- [ ] `system@v1` references all current modules
- [ ] `roadmap.md` exists with at least a Current section
- [ ] The bootstrap gate passes
- [ ] An AI agent can read `@specs/` and understand what is being built


------------------------------------------------------------
## 4. Common Mistakes
------------------------------------------------------------

**Creating modules before running intake**
Modules created without the intake process will have incomplete specs.
The 20% will be hidden, not tracked. Run intake first.

**Creating too many modules upfront**
Start with the minimum number of modules that represent real domain
boundaries. You can always add modules later. Premature decomposition
creates complexity without value.

**Skipping the roadmap**
The roadmap is not optional. Even a minimal roadmap with one Current
entry is better than none. It is the planning layer that keeps the
20% visible at the project level.
