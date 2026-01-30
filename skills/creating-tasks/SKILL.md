---
name: creating-tasks
description: Decompose an approved plan into atomic, traceable tasks with owners and acceptance criteria.
---

# Creating Tasks

Translate plan steps into a hierarchical backlog of actionable tasks that can be assigned and verified independently.

## When to use
- After a plan is approved
- Before implementation begins
- When distributing work across contributors

## Inputs
- Required: `handoff/payload.json` with `plan.steps`
- Optional: existing `tasks` section to refine

## Outputs
- Updated `handoff/payload.json` with `tasks.items` and `tasks.dependencies`
- `meta.currentStage` updated; all other sections preserved

## Checklist
- Validate requirements and plan are present.
- Create a parent task for each plan step.
- Decompose into atomic leaf tasks with clear acceptance criteria.
- Link tasks to specific files or components.
- Define dependencies and readiness status.

## Instructions
1. Load the plan and validate prerequisites.
2. Create parent tasks that mirror plan steps.
3. Split into atomic leaf tasks with verification criteria.
4. Add dependencies and update payload.

## Examples
See `EXAMPLES.md` for task breakdowns.

## See also
- `REFERENCE.md` for task granularity rules and error handling.
