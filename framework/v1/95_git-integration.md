---
spec_id: git-integration
rule_id: git-integration-v1
type: framework-rule
version: v1
status: active
---

# 95 — Git Integration

This document defines how ragnor.dev relates to git — branches,
commits, merges, and team collaboration patterns.

The framework does not prescribe a specific git workflow. It defines
the relationship between spec status and branch status, which works
with any git workflow a team chooses.


------------------------------------------------------------
## 1. Core Principle
------------------------------------------------------------

**Spec status follows branch status.**

- A spec on a feature branch is a draft, regardless of its `status` field
- A spec on the main branch is the declared intent for that branch
- A released spec is a tagged, immutable snapshot

This means the same spec file can be in different states depending on
which branch you are reading it from. AI agents MUST be aware of which
branch they are operating on.


------------------------------------------------------------
## 2. Branch and Spec Relationships
------------------------------------------------------------

### 2.1 Main branch

The `main` (or `master`) branch holds the authoritative specs.

- Module specs on main represent declared, current intent
- Released module versions on main are immutable
- System versions on main are the authoritative composition snapshots
- AI agents default to main branch specs as the source of truth

### 2.2 Feature branches

Feature branches hold draft specs alongside draft code.

Naming convention (recommended):

```
feature/<module>-v<N>-<short-description>
```

Examples:
```
feature/auth-v2-oauth-support
feature/billing-v1-initial
feature/system-v2-composition
```

Rules:
- A feature branch spec is always draft
- The spec and code on a feature branch evolve together
- A feature branch spec becomes authoritative when merged to main
- AI agents operating on a feature branch MUST declare which branch
  they are on and confirm the spec they are reading

### 2.3 The merge as a spec gate

Merging a feature branch to main is a spec gate, not just a code gate.

Before merging, verify:
- [ ] The module spec is complete and passes the readiness check
- [ ] The 20% is explicitly tracked in `todo.md`
- [ ] One-way decisions are recorded as ADRs
- [ ] Drift detection has been run and findings resolved
- [ ] The spec version matches the directory path

A PR that merges code without a corresponding spec update is a
framework violation unless the change is explicitly in scope of
an existing spec.


------------------------------------------------------------
## 3. Git Workflows
------------------------------------------------------------

ragnor.dev is compatible with any git workflow. The two most
common patterns and how they map to the framework:

### 3.1 GitHub Flow (simple, recommended for most teams)

```
main
  └── feature/auth-v2-oauth
  └── feature/billing-v1-initial
```

- One long-lived branch: `main`
- Short-lived feature branches
- Merge directly to main via PR

Framework mapping:
- `main` = authoritative specs
- Feature branches = draft specs + draft code
- PR = spec gate + code gate
- Merge = spec becomes authoritative

### 3.2 Git Flow (structured, for teams with release cycles)

```
main (production)
  └── develop (integration)
        └── feature/auth-v2-oauth
        └── feature/billing-v1-initial
  └── release/v2.0
  └── hotfix/auth-token-fix
```

Framework mapping:
- `main` = released system versions
- `develop` = current draft system version
- Feature branches = draft module versions
- Release branches = system version candidates
- Hotfix branches = patch specs (LOW severity drift fixes only)

### 3.3 Choosing a workflow

The framework does not prescribe which workflow to use.
The rule is simple: **whatever branch is your source of truth
for production code is your source of truth for released specs.**


------------------------------------------------------------
## 4. AI Agent Git Awareness
------------------------------------------------------------

AI agents MUST be git-aware when operating within this framework.

### 4.1 Before execution

An AI agent MUST:
1. Identify the current branch
2. Confirm which spec version it is reading
3. Confirm the spec is appropriate for the current branch
4. Declare this context in its execution summary

### 4.2 Branch-spec mismatch

If an AI agent detects a mismatch between the current branch and
the spec it is reading (e.g. reading a main branch spec while on
a feature branch), it MUST:
1. Surface the mismatch
2. Ask which spec to use
3. Not proceed until confirmed

### 4.3 Concurrent work

When multiple developers or AI agents are working on different
feature branches simultaneously:
- Each branch has its own draft spec
- Specs do not automatically merge
- Conflicts between specs are resolved at PR review time
- The human resolves spec conflicts, not the AI

### 4.4 Commit messages and spec references

Recommended commit message format when spec changes are included:

```
<type>(<module>@v<N>): <description>

spec: <what changed in the spec>
code: <what changed in the code>
drift: <any drift found and resolved>
```

Examples:
```
feat(auth@v2): add OAuth support

spec: added OAuth provider contract to acceptance.md
code: implemented OAuth flow in auth service
drift: none

fix(billing@v1): correct tax calculation

spec: clarified acceptance criteria for EU tax rates
code: fixed rounding error in tax calculation
drift: LOW - acceptance criteria wording was ambiguous
```


------------------------------------------------------------
## 5. Immutability and Git Tags
------------------------------------------------------------

When a module version or system version is marked `status: released`,
it SHOULD be tagged in git:

```
git tag <module>@v<N>-released
git tag system@v<N>-released
```

This creates a permanent, auditable record of when the version was
released and what the codebase looked like at that point.

Tags are immutable by convention. A released tag MUST NOT be moved
or deleted. If a mistake was made, create a new version — do not
rewrite history.


------------------------------------------------------------
## 6. Onboarding an Existing Repo
------------------------------------------------------------

When running brownfield onboarding on an existing repo
(see `OB_20_onboarding-brownfield.md`), the git history is a
primary source of intent reconstruction.

The onboarding process SHOULD:
- Scan recent commit history for intent signals
- Extract issue references from commit messages
- Infer module boundaries from file path patterns
- Use commit authors and frequency to identify ownership patterns

The git history does not replace the spec. It informs the initial
draft specs that humans then review and approve.


------------------------------------------------------------
## 7. Relationship to Other Documents
------------------------------------------------------------

- `50_version-operations.md` — version immutability rules
- `70_ai-execution-rules.md` — AI agent behaviour during execution
- `80_drift-detection.md` — drift detection before and after merges
- `90_roadmap.md` — how roadmap entries relate to branches
- `OB_20_onboarding-brownfield.md` — using git history for onboarding


------------------------------------------------------------
## 8. Authority
------------------------------------------------------------

This document is authoritative for the relationship between
ragnor.dev specs and git.

The framework does not own your git workflow. It defines how specs
relate to branches. Teams choose their own workflow and apply
these rules within it.
