---
name: designing-qa-strategy
description: Maps requirements, plan steps, and tasks to test scenarios, fixtures, and validation commands. Used after tasks are defined and before implementation.
---

# Designing a QA Strategy

Create a concrete test plan that covers acceptance criteria with traceable scenarios and commands.

## When to use
- After tasks are defined and before implementation
- When defining "done" criteria and validation gates

## Inputs
- Required: handoff payload with `requirement`, `plan`, and `tasks`
- Optional: existing `testPlan` section to refine

## Outputs
- Updated handoff payload with `testPlan.coverageMatrix` and `testPlan.scenarios`
- Validation commands and fixtures documented when applicable

## Checklist
- Map every acceptance criterion to at least one scenario.
- Choose appropriate test levels (unit, integration, contract, E2E, exploratory).
- Define inputs, expected outputs, and edge cases.
- Document fixtures and exact validation commands.
- Record any coverage gaps.

## Instructions
1. Validate prerequisites and load requirements, plan, tasks.
2. Enumerate testable behaviors and map coverage.
3. Draft scenarios with fixtures and commands.
4. Return the updated payload and note gaps.

## Examples
See `EXAMPLES.md` for scenario templates.

## See also
- `REFERENCE.md` for coverage rules and error handling.
