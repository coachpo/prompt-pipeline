---
description: Independently verify the delivered work using the supplied payload, evidence, and commands; approve or reject with full traceability.
---

## Stage Overview
- Reproduce the implementer’s validation steps exactly, then layer exploratory/negative checks targeting the riskiest plan steps and leaf tasks.
- Compare observed behavior against requirements, plan steps, parent/leaf tasks, and QA scenarios/commands to ensure nothing regressed or fell through.
- Produce a defensible go/no-go decision with linked evidence, defect details, and instructions for rerunning validation.

## Shared Payload Contract
- **File:** `handoff/payload.json`
- **Required before starting:** `requirement.summary`, `plan.steps`, `tasks.items` (with parent-child linkage), `testPlan.scenarios`, `testPlan.validationCommands`, `implementation.changes`, `implementation.tests`, `implementation.commands` (with evidence references).
- **Must update:**
  - `meta.currentStage = "6-acceptance"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`.
  - `qaFindings.status` (`approved`, `rejected`, `blocked`), `qaFindings.evidence` (command outputs, logs, screenshots, metrics), `qaFindings.issues` (each issue includes traceable IDs, steps, expected vs. actual, severity, owner, reproduction assets).
  - `qaFindings.validationLog` (chronological record of commands/tests executed, outcomes, timestamps) if not already present.
  - Append pass/fail/blocked annotations to every validated `tasks.items[*]` (leaf and parent) and `testPlan.scenarios[*]`; ensure notes cite evidence IDs.

## Tooling & Evidence Expectations
- **MCP Reference First:** Follow `mcp/mcp_registry.md` and `mcp/mcp_rules.md` when picking tooling. Use the registry’s preferred MCP server (e.g., Desktop Commander for execution, DeepWiki/Context7 for docs, `mcp-shrimp-task-manager` for task status updates) before defaulting to other methods.
- Desktop Commander for repo inspection, command execution, and artifact capture.
- IDE/LSP tooling or Desktop Commander searches for verifying symbol changes, referencing call graphs, and confirming affected files.
- DeepWiki / Context7 when acceptance requires comparing behavior against external specs or contracts.
- Maintain an audit trail of commands, logs, screenshots, or metrics snapshots attached to `qaFindings.evidence`.

## Workflow
1. **Intake & Risk Review** – Parse payload + `$ARGUMENTS` for acceptance criteria, residual risks, feature flags, rollout notes, and prioritized leaf tasks. Build a validation checklist aligned with `testPlan.validationCommands` and note any missing evidence before proceeding.
2. **Evidence Inspection** – Walk through `implementation.changes`, diffs, configs, telemetry updates, and tests to confirm they map to referenced requirements/plan steps/tasks. Verify that coverage claims match `testPlan.scenarios` and record discrepancies.
3. **Automated Validation** – Re-run every documented command from `implementation.commands` and `testPlan.validationCommands`. Capture raw outputs, exit codes, metrics, and timestamps inside `qaFindings.validationLog`. Compare against expected thresholds (coverage, performance, error budgets) and flag deviations immediately.
4. **Exploratory & Negative Testing** – Extend beyond scripted steps to probe edge cases, rollback paths, feature-flag toggles, and observability hooks using payload fixtures. Document any new commands or manual procedures plus resulting evidence IDs.
5. **Criteria & Traceability Audit** – For each acceptance criterion, plan step, parent task, leaf task, and scenario, determine pass/fail/blocked outcomes referencing the collected evidence. Update `tasks.items[*].status`, `testPlan.scenarios[*].status`, and note parent roll-ups when child leaves fail.
6. **Decision & Reporting** – Set `qaFindings.status` (`approved`, `rejected`, `blocked`) based on aggregate results. When issues exist, add detailed entries with reproduction steps, expected vs. actual behavior, severity, owner, and links to validation logs. Summarize residual risk even when approved.
7. **Payload Finalization & Handoff** – Ensure all findings, validation logs, and issue references are persisted in `handoff/payload.json`. Highlight any follow-up tasks or required redeploy steps and notify release owners that acceptance is complete.

## Outputs & Handoff
- Signed QA decision anchored by `qaFindings.validationLog` and evidence artifacts that trace back to each requirement/plan/task/scenario.
- Updated task and scenario statuses (parent + leaf) plus issue lists routed to owning engineers or epics.
- Explicit rerun instructions, including commands, data prerequisites, feature-flag states, and any blockers that postpone release.

## Required User Input
```text
$ARGUMENTS
```
Acceptance relies solely on the provided payload and evidence. Request missing context before attempting validation.
