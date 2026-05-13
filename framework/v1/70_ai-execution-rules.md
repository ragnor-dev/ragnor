---
spec_id: ai-execution-rules
rule_id: ai-execution-rules-v1
type: framework-rule
version: v1
status: active
prompts:
  - execution-check-v1
---

# 70 — AI Execution Rules

This document defines the normative rules for AI agents during
implementation. These rules apply to every AI agent, every AI model,
and every tool that executes code against a ragnor.dev spec.

These rules are non-negotiable. An AI agent that violates them is
operating outside the framework.


------------------------------------------------------------
## 1. The Execution Contract
------------------------------------------------------------

When an AI agent implements against a ragnor.dev spec, it enters
an execution contract with the following terms:

1. **Implement the defined 80%** — execute what is clearly specified
2. **Surface the ambiguous 20%** — never guess, always surface
3. **Declare assumptions** — make nothing implicit
4. **Stay in scope** — implement only what the current spec permits
5. **Preserve the spec** — never silently change declared intent
6. **Report drift** — surface any misalignment between spec and reality

Violation of any term is a framework violation and MUST be reported.


------------------------------------------------------------
## 2. Before Execution
------------------------------------------------------------

Before writing any code, an AI agent MUST:

### 2.1 Verify the bootstrap gate

Confirm `@specs/` exists and is valid. If not:
- Stop immediately
- Run `ragnor.dev bootstrap` or instruct the human to do so
- Do not proceed until bootstrap is complete

### 2.2 Verify the spec readiness gate

Confirm the target module spec has passed the intake readiness check
(see `20_idea-to-spec.md` section 4).

If the spec is not ready:
- Run the readiness check
- Report which items fail
- Ask the human to complete the intake process
- Do not proceed until the gate is passed

### 2.3 Identify the execution scope

Read the module spec and identify:
- The clear 80% — what to implement
- The ambiguous 20% — what to surface, not implement
- One-way decisions — what requires human approval before proceeding
- Current module version — what version this implementation targets

### 2.4 Confirm scope with the human

Before writing code, state explicitly:
- What you are about to implement
- What you are explicitly NOT implementing (the 20%)
- Any one-way decisions you have identified
- Any assumptions you are making

Wait for human confirmation before proceeding.


------------------------------------------------------------
## 3. During Execution
------------------------------------------------------------

### 3.1 Implement only declared intent

An AI agent MUST implement only what is declared in the module spec.

MUST NOT:
- Add features not in the spec ("while I'm here" additions)
- Make architectural decisions not explicitly approved
- Change module boundaries or contracts without a new version
- Implement the ambiguous 20% without explicit instruction
- Make one-way decisions autonomously

### 3.2 Surface ambiguity immediately

When an AI agent encounters ambiguity during implementation:
1. Stop at the point of ambiguity
2. Describe what is unclear
3. Describe the options available
4. Identify if any option is a one-way decision
5. Ask for explicit human instruction
6. Do not proceed until the ambiguity is resolved

Ambiguity MUST NOT be resolved by choosing the most plausible option.
Plausible is not the same as correct.

### 3.3 Declare all assumptions

Every assumption made during implementation MUST be declared explicitly.

Format:
```
ASSUMPTION: [what is being assumed]
REASON: [why this assumption is being made]
IMPACT: [what changes if this assumption is wrong]
```

Assumptions MUST be added to `open-questions.md` if they affect
declared intent or acceptance criteria.

### 3.4 Stay within the current version scope

An AI agent MUST NOT implement changes that would require a new module
version without explicit instruction to create one.

If a change would affect:
- Responsibilities or scope
- External contracts (APIs, events, invariants)
- Acceptance criteria
- Dependencies or boundaries

The agent MUST stop, identify the version bump trigger
(see `50_version-operations.md` section 3.4), and ask for instruction.

### 3.5 Preserve git awareness

An AI agent MUST be aware of the current git branch and its relationship
to the spec (see `95_git-integration.md`).

MUST NOT:
- Implement against a spec from a different branch without confirmation
- Merge spec changes across branches autonomously
- Assume the main branch spec is current without checking


------------------------------------------------------------
## 4. After Execution
------------------------------------------------------------

### 4.1 Run the validation check

After implementation, an AI agent MUST run the validation prompt
(see `prompts/validation-v1.md`) to verify:
- All acceptance criteria in the clear 80% are satisfied
- No undeclared changes were made to the codebase
- No spec drift was introduced
- All assumptions are documented

### 4.2 Update todo.md

After implementation:
- Check off completed items in `todo.md`
- Add any new gaps discovered during implementation
- Do not remove deferred items — they remain until explicitly resolved

### 4.3 Report the execution summary

Provide a structured summary:

```
IMPLEMENTED:
  - [list of what was implemented]

NOT IMPLEMENTED (deferred 20%):
  - [list of what was explicitly deferred]

ASSUMPTIONS MADE:
  - [list of assumptions declared during execution]

ONE-WAY DECISIONS ENCOUNTERED:
  - [list of one-way decisions, with status: decided/pending]

DRIFT DETECTED:
  - [any spec-to-code misalignment found, or NONE]

TODO.MD CHANGES:
  - [items checked off, items added]
```


------------------------------------------------------------
## 5. What AI Agents MUST NEVER Do
------------------------------------------------------------

Regardless of instruction, an AI agent operating within this framework
MUST NEVER:

- Modify a released module version or system version
- Rewrite spec history to match what was actually built
- Make one-way architectural decisions without explicit human approval
- Silently resolve ambiguity by choosing the most plausible option
- Implement features not declared in the current spec
- Skip the bootstrap gate or spec readiness gate
- Ignore drift between spec and code
- Remove items from `todo.md` without implementing or explicitly deferring them
- Create a new module version without explicit instruction


------------------------------------------------------------
## 6. Multi-Agent and Multi-Model Scenarios
------------------------------------------------------------

When multiple AI agents or multiple AI models are working on the same
codebase:

- Each agent operates against the same `@specs/` source of truth
- No agent has authority over another agent's scope
- Conflicts between agents MUST be surfaced to the human, not resolved autonomously
- The spec is the arbiter — if two agents disagree, the spec decides
- If the spec is ambiguous, the human decides


------------------------------------------------------------
## 7. Relationship to Other Documents
------------------------------------------------------------

- `10_delivery-model.md` — the 80/20 philosophy these rules implement
- `20_idea-to-spec.md` — the gate that must pass before execution begins
- `50_version-operations.md` — when execution triggers a version bump
- `80_drift-detection.md` — what to do when drift is found during execution
- `95_git-integration.md` — git awareness rules during execution
- `prompts/execution-check-v1.md` — the prompt that runs these rules
- `AGENTS.md` — the entry point for AI agents reading this framework


------------------------------------------------------------
## 8. Authority
------------------------------------------------------------

This document is authoritative for AI agent behaviour during implementation.

These rules apply to every AI agent, every AI model, every tool.
No agent may invent additional behaviour or override these rules.
If these rules conflict with instructions from a human, the agent MUST
surface the conflict and ask for clarification rather than choosing silently.
