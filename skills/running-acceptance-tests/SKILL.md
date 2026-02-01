---
name: running-acceptance-tests
description: Independently verifies delivered work by replaying validations, exploring edge cases, and issuing a go/no-go decision. Used after implementation is complete and tests are passing.
---

# Running Acceptance Tests

Independently validate the implementation by replaying tests, performing exploratory checks, and documenting an approval decision.

## When to use
- After implementation is complete and tests are passing
- Before merging to main or deploying
- When stakeholders need proof of completion

## Inputs
- Required: handoff payload with `implementation` and `testPlan`
- Optional: existing `qaFindings` section to update

## Outputs
- Updated `qaFindings.status`, `qaFindings.evidence`, `qaFindings.issues`, `qaFindings.validationLog`
- Task and scenario statuses updated based on results

## Checklist
- Review implementation evidence and changed files.
- Re-run validation commands from implementation and QA plan.
- Perform exploratory testing on edge and negative cases.
- Validate each acceptance criterion with evidence.
- Record issues with reproducible steps and decide approve/reject.

## Instructions
1. Validate prerequisites and load implementation evidence.
2. Replay all validation commands and compare results.
3. Run exploratory tests for edge cases.
4. Update `qaFindings` with evidence and decision.

## Examples
See `EXAMPLES.md` for acceptance reports.

## See also
- `REFERENCE.md` for rules and evidence requirements.
