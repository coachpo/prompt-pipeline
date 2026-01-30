---
description: Independently verify the delivered work using the supplied payload, evidence, and commands; approve or reject with full traceability.
---

## Stage Overview
- Replay the implementer’s validation steps exactly, then layer targeted exploratory/negative checks on the riskiest plan steps and leaf tasks.
- Compare observed behavior against requirements, plan steps, parent/leaf tasks, and QA scenarios/commands to confirm nothing regressed or escaped detection.
- Deliver a defensible go/no-go decision backed by evidence, defect details, and rerun instructions.

## Commander Doctrine
- Validate only the new behavior without evaluating backward compatibility expectations.
- Reject any solution that introduces shim code or compatibility layers during implementation or testing.

## Shared Payload Contract
- **File:** `handoff/payload.json`
- **Required before starting:** `requirement.summary`, `plan.steps`, `tasks.items` (with parent-child linkage), `testPlan.scenarios`, `testPlan.validationCommands`, `implementation.changes`, `implementation.tests`, `implementation.commands` (with evidence references).
- **Must update:**
  - `meta.currentStage = "6-acceptance"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`.
  - `qaFindings.status` (`approved`, `rejected`, `blocked`), `qaFindings.evidence` (command outputs, logs, screenshots, metrics), `qaFindings.issues` (each issue includes traceable IDs, steps, expected vs. actual, severity, owner, reproduction assets).
  - `qaFindings.validationLog` (chronological record of commands/tests executed, outcomes, timestamps) if not already present.
  - Append pass/fail/blocked annotations to every validated `tasks.items[*]` (leaf and parent) and `testPlan.scenarios[*]`; ensure notes cite evidence IDs.

## Tooling & Evidence Expectations
- **MCP Reference First:** Follow `mcp/mcp_registry.md` and `mcp/mcp_rules.md` when picking tooling. Use the registry’s preferred MCP server (Desktop Commander for execution, DeepWiki/Context7 for docs, `mcp-shrimp-task-manager` for task status) before defaulting to other methods.
- Desktop Commander handles repo inspection, command execution, and artifact capture; tag every run inside `qaFindings.validationLog`.
- IDE/LSP tooling or Desktop Commander searches verify symbol changes, call graphs, and touched files; cite file+line references when confirming coverage.
- Use DeepWiki / Context7 whenever acceptance requires comparing behavior against external specs or contracts.
- Attach every command log, screenshot, metric, or dataset to `qaFindings.evidence` so the go/no-go decision is auditable.

## Workflow
1. **Intake & Risk Review** – Parse payload + `$ARGUMENTS` for acceptance criteria, residual risks, feature flags, rollout notes, and prioritized leaf tasks. Build a validation checklist aligned to `testPlan.validationCommands` and note missing evidence before touching code.
2. **Evidence Inspection** – Walk through `implementation.changes`, diffs, configs, telemetry, and tests to confirm they map to the referenced requirements/plan steps/tasks. Verify that coverage claims match `testPlan.scenarios`; record discrepancies immediately.
3. **Automated Validation** – Re-run every command listed in `implementation.commands` and `testPlan.validationCommands`. Capture raw output, exit codes, metrics, and timestamps inside `qaFindings.validationLog`, then compare against expected thresholds (coverage, performance, error budgets).
4. **Exploratory & Negative Testing** – Push beyond scripted steps to probe edge cases, rollback paths, feature-flag toggles, and observability hooks using the payload fixtures. Log any new commands or manual procedures plus their evidence IDs.
5. **Criteria & Traceability Audit** – For each acceptance criterion, plan step, parent task, leaf task, and scenario, determine pass/fail/blocked outcomes referencing the collected evidence. Update `tasks.items[*].status`, `testPlan.scenarios[*].status`, and roll up parent statuses when children fail.
6. **Decision & Reporting** – Set `qaFindings.status` (`approved`, `rejected`, `blocked`) based on aggregate results. When issues exist, add entries capturing reproduction steps, expected vs. actual behavior, severity, owner, and links to validation logs. Always summarize residual risk.
7. **Payload Finalization & Handoff** – Persist every finding, validation log, and issue reference in `handoff/payload.json`. Highlight follow-up tasks or redeploy actions and notify release owners once acceptance is complete.

## Outputs & Handoff
- Signed QA decision anchored by `qaFindings.validationLog` and evidence artifacts that trace back to each requirement/plan/task/scenario.
- Updated task and scenario statuses (parent + leaf) plus issue lists routed to owning engineers or epics.
- Explicit rerun instructions, including commands, data prerequisites, feature-flag states, and any blockers that postpone release.

## Required User Input
```text
$ARGUMENTS
```
Acceptance relies solely on the provided payload and evidence. Request missing context before attempting validation.
