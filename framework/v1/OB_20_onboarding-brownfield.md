---
spec_id: onboarding-brownfield
type: framework-rule
version: v1
status: active
---

# OB_20 — Brownfield Onboarding

This document defines the onboarding process for existing codebases
that do not yet have a `@specs/` layer.

Brownfield onboarding is the process of reconstructing architectural
intent from an existing codebase and its history.


------------------------------------------------------------
## 1. When to Use This Path
------------------------------------------------------------

Use brownfield onboarding when:
- The repository has existing code
- There is no `@specs/` directory
- You are adopting the framework mid-project

If you have external documentation to import (Confluence, Jira, etc.),
also read `OB_30_onboarding-external.md`.


------------------------------------------------------------
## 2. The Brownfield Onboarding Process
------------------------------------------------------------

### Step 1 — Scaffold the spec layer

Same as greenfield — create the `@specs/` directory structure.

AI agent command: `ragnor.dev bootstrap --brownfield`

### Step 2 — Infer module boundaries

Analyse the existing codebase to identify natural module boundaries.

An AI agent SHOULD examine:
- Directory structure and file organisation
- Import/dependency graphs
- Commit history — which files change together
- Existing README files and documentation
- Test structure — what is tested in isolation

Output: a proposed list of modules with draft boundary descriptions.

**Human review required.** The AI proposes, the human approves.
Module boundaries are architectural decisions — they MUST NOT be
made autonomously by an AI agent.

### Step 3 — Reconstruct intent for each module

For each identified module, reconstruct:
- Purpose (what does this module do?)
- Responsibilities (what does it own?)
- Contracts (what does it expose to other modules?)
- Dependencies (what does it depend on?)
- Known decisions (what architectural choices were made?)

Sources for reconstruction (in trust order):
1. Existing documentation (README, wikis, Confluence)
2. Test files (tests describe intended behaviour)
3. Code comments and docstrings
4. Commit messages and PR descriptions
5. Git history patterns
6. Code structure inference (lowest trust)

### Step 4 — Create draft module specs

For each module, create a draft `module-spec.md` with:
- Reconstructed purpose and responsibilities
- Best-effort acceptance criteria
- Known dependencies
- Explicit gaps marked as `[INFERRED: needs verification]`

Every inferred item MUST be marked. Nothing should appear as
certain when it was reconstructed from code rather than from
explicit documentation.

### Step 5 — Populate todo.md with known gaps

Brownfield `todo.md` files will typically be extensive.
This is expected and correct.

Categories of gaps to capture:
- Missing documentation (things the code does but the spec doesn't declare)
- Missing tests (acceptance criteria with no test coverage)
- Known technical debt (things that should be refactored)
- Ambiguous behaviour (things that work but nobody knows why)
- Missing ADRs (decisions that were made but never recorded)

### Step 6 — Create ADR stubs for known decisions

For every significant architectural decision that can be inferred
from the codebase, create an ADR stub:

```markdown
# ADR-0001: <decision title>

Status: accepted (reconstructed)

Context: <reconstructed from code/history>

Decision: <what was decided, inferred from implementation>

Consequences: <what this means for the system>

Note: This ADR was reconstructed during brownfield onboarding.
The original decision context may be incomplete.
```

Reconstructed ADRs are better than no ADRs. They restore
architectural memory even if imperfectly.

### Step 7 — Create system@v1

Create `system@v1` referencing all identified module@v1 versions.
This is the baseline snapshot — the system as it exists today.

### Step 8 — Human review and approval

**This step is mandatory and cannot be delegated to an AI agent.**

A human with knowledge of the system MUST review:
- All inferred module boundaries
- All `[INFERRED]` items in module specs
- All reconstructed ADRs
- The system@v1 composition

For each reviewed item, the human either:
- Confirms it (removes the `[INFERRED]` marker)
- Corrects it (updates the spec)
- Flags it as unknown (moves to `open-questions.md`)

### Step 9 — Verify the bootstrap gate

Run the bootstrap gate check. All checks must pass.


------------------------------------------------------------
## 3. Brownfield Quality Bar
------------------------------------------------------------

Brownfield onboarding is complete when:

- [ ] All major modules are identified and have draft specs
- [ ] All `[INFERRED]` items have been reviewed by a human
- [ ] Known gaps are tracked in `todo.md`
- [ ] Known decisions have ADR stubs
- [ ] `system@v1` exists and references all modules
- [ ] The bootstrap gate passes
- [ ] A new developer or AI agent can read `@specs/` and understand
      the system without reading the code first

Brownfield onboarding does not need to be perfect. It needs to be
honest — gaps acknowledged, inferences marked, unknowns tracked.
A spec with explicit gaps is more useful than no spec.


------------------------------------------------------------
## 4. Ongoing Brownfield Improvement
------------------------------------------------------------

After initial brownfield onboarding, the specs will be incomplete.
This is normal. Improvement happens incrementally:

- When working on a module, improve its spec as part of the work
- When a gap is discovered, add it to `todo.md`
- When a decision is made, write the ADR
- When an `[INFERRED]` item is verified, remove the marker

The framework does not require perfect specs on day one.
It requires honest specs that improve over time.


------------------------------------------------------------
## 5. Relationship to Other Documents
------------------------------------------------------------

- `OB_30_onboarding-external.md` — importing from Confluence, Jira, etc.
- `20_idea-to-spec.md` — intake process for new modules added post-onboarding
- `80_drift-detection.md` — drift detection is especially important post-onboarding
- `95_git-integration.md` — using git history during onboarding
