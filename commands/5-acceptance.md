---
description: Independently verify the delivered work using only the supplied brief, evidence, and instructions; sign off or return defects with full traceability.
---

## Shared Payload Contract
- File: `handoff/payload.json`
- Required fields before starting: `requirement.summary`, `plan.steps`, `testPlan.scenarios`, `implementation.changes`.
- Must update:
  - `meta.currentStage = "5-acceptance"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`.
  - `qaFindings.status` (`approved`, `rejected`, `blocked`), `qaFindings.evidence`, `qaFindings.issues`.
  - Append pass/fail annotations to `testPlan.scenarios[*].status` when validated.

## User Input — complete QA packet required

```text
$ARGUMENTS
```

If critical context is missing or contradictory, pause and request clarification before executing tests.

---

# Acceptance Workflow

## 0. Intake & Risk Review
- Parse the input for acceptance criteria, telemetry/config expectations, verification commands, and residual risks; rely on no other files.
- Convert these details into a QA checklist that tracks pass/fail status for every criterion.
- Flag ambiguous items or missing evidence and request updates before continuing.

## 1. Evidence Inspection
- Examine the referenced code paths and notes using **Desktop Commander** (`read_file`, `start_search`) or **Serena** (`find_symbol`, `find_referencing_symbols`) to confirm what changed.
- Validate that the documented tests correspond to the stated behaviors and that traceability links remain intact.

## 2. Automated Validation
- Run every prescribed command (e.g., `make test`, `go test ./...`, contract/coverage jobs) via **Desktop Commander** (`start_process`, `interact_with_process`).
- Capture logs, exit codes, and coverage artifacts; confirm thresholds meet or exceed the input’s expectations.
- Investigate failures, collect repro data, and compare outcomes against the documented expected results.

## 3. Exploratory / Manual Checks
- Using realistic configs/fixtures from the brief, exercise the new paths (API calls, CLI flows, telemetry outputs).
- Validate edge cases, rollback behavior, and error handling; store inputs/outputs, screenshots, or metrics snapshots as evidence.
- Confirm any external dashboards, alerts, or documentation updates reflect the change.

## 4. Criteria Comparison
- For each acceptance criterion, compare observed behavior with expectations and record pass/fail plus reproduction details.
- Maintain a findings log noting endpoints, files, or commands involved so issues can be reproduced quickly.

## 5. Decision & Reporting
- If all checks pass, produce a sign-off summary that lists commands run, artifacts captured, and residual risks, then set `qaFindings.status = "approved"`.
- If gaps remain, generate clear bug reports (`qaFindings.issues`) detailing steps to reproduce, expected results, and links to logs/artifacts.
- Share outcomes (pass/fail) and next steps with stakeholders, referencing the payload so traceability is preserved.
