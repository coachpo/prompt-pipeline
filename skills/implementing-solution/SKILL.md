---
name: implementing-solution
description: Executes approved tasks, implements code and tests, runs validations, and records evidence. Used after requirements, plan, tasks, and QA strategy are approved.
---

# Implementing a Solution

Work through the task backlog in dependency order, producing production-ready code with validation evidence.

## When to use
- After requirements, plan, tasks, and QA strategy are approved
- When implementation can proceed autonomously

## Inputs
- Required: handoff payload with `requirement`, `plan`, `tasks`, `testPlan`
- Optional: existing `implementation` section to resume

## Outputs
- Updated `implementation.changes`, `implementation.tests`, `implementation.commands`
- Task statuses updated; all other payload sections preserved

## Checklist
- Validate prerequisites and load the task queue.
- Implement each leaf task in dependency order.
- Add or update tests per QA strategy.
- Run validation commands and capture results.
- Record changes and evidence in the payload.

## Instructions
1. Validate prerequisites and determine next ready tasks.
2. Implement code and tests for each leaf task.
3. Run validation commands and capture outputs.
4. Update payload with changes, tests, and command evidence.

## Examples
See `EXAMPLES.md` for implementation records.

## See also
- `REFERENCE.md` for execution rules and failure handling.
