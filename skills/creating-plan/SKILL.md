---
name: creating-plan
description: Turns approved requirements into an ordered execution plan with risks, dependencies, and evidence. Used after requirements are approved and before task breakdown.
---

# Creating a Plan

Translate approved requirements into a dependency-aware plan that teams can execute with clear success signals.

## When to use
- After requirements are documented and approved
- Before breaking work into tasks
- When sequencing or risk assessment is needed

## Inputs
- Required: handoff payload with `requirement.summary` and `requirement.acceptanceCriteria`
- Optional: existing `plan` section to refine

## Outputs
- Updated handoff payload with `plan.steps`, `plan.risks`, `plan.artifacts`
- `meta.currentStage` updated; all other sections preserved

## Checklist
- Validate requirements are present and specific.
- Run a triviality gate (scope, dependencies, coordination).
- Search for prior art and capture file:line evidence.
- Create 3-6 ordered steps with dependencies and owners.
- Document risks and mitigation actions.

## Instructions
1. Load requirements and validate prerequisites.
2. Determine whether the work is trivial or complex.
3. Draft plan steps with success signals and dependencies.
4. Add risks and artifacts, then return the updated payload.

## Examples
See `EXAMPLES.md` for plan samples.

## See also
- `REFERENCE.md` for rules, triviality criteria, and error handling.
