---
description: Convert the shared requirement + plan + task backlog into a concrete, traceable test strategy for implementers.
---

## Stage Overview
- Guarantee every acceptance criterion, plan step, and atomic task (leaf) has explicit, testable coverage mapped to observable behaviors.
- Define the right mix of unit, integration, contract, end-to-end, and exploratory tests, including justification and owner per scenario.
- Produce fixtures, scenario tables, and repeatable execution commands so implementation can pick up any task and know exactly how to validate it in isolation and in-system.

## Commander Doctrine
- Design coverage with zero regard for backward compatibility; legacy behaviors are out of scope.
- Reject any QA approach that depends on shim code or compatibility harnesses.

## Shared Payload Contract
- **File:** `handoff/payload.json`
- **Required before starting:** `requirement.summary`, `plan.steps` (non-empty), `tasks.items` (non-empty backlog with parent-child linkage).
- **Must update:**
  - `meta.currentStage = "4-qa"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`.
  - `testPlan.coverageMatrix`, `testPlan.scenarios`, `testPlan.fixtures` (each row references requirement IDs, plan step IDs, parent task IDs, and the specific leaf task IDs covered).
  - `testPlan.validationCommands` (if absent) describing how to run each scenario or suite, including data/fixture prerequisites.
  - Append clarifications to `requirement.openQuestions` or `tasks.statusSummary` when coverage gaps or blockers remain; never overwrite other sections.
  - Confirm `tasks.statusSummary` reflects any QA-driven ready/blocked state changes discovered while designing coverage.

## Tooling & Evidence Expectations
- **MCP Reference First:** Always consult `mcp/mcp_registry.md` and follow `mcp/mcp_rules.md` when selecting tooling. Choose the registry’s preferred MCP server for each activity (analysis, documentation, automation) before falling back to local commands.
- Desktop Commander searches (`start_search`, `rg`) to inventory existing tests, helpers, and fixtures for reuse.
- IDE/LSP symbol lookups or targeted Desktop Commander searches to understand integration points and ensure coverage beyond obvious code paths.
- DeepWiki / Context7 for external API contracts, schema definitions, or compliance standards that influence test design.
- `mcp-shrimp-task-manager` or similar backlog MCPs to keep task coverage status in sync when updating `tasks.statusSummary`.
- Sequential Thinking to walk through coverage mapping before locking scenarios.

## Workflow
1. **Precondition Check** – Verify `requirement.summary`, `plan.steps`, and `tasks.items` are current and that every plan step already has leaf tasks. If anything is missing or stale, pause and request updates before drafting QA guidance.
2. **Coverage Inventory** – Enumerate acceptance criteria, plan steps, and leaf tasks. For each, capture observable behaviors (responses, events, telemetry, error states) plus existing tests/fixtures discovered via MCP or code search. Log gaps inside `requirement.openQuestions`.
3. **Traceable Coverage Matrix** – Build `testPlan.coverageMatrix` entries that map Requirement → Plan Step → Parent Task → Leaf Task → Behavior. Include evidence (file paths + lines, MCP findings) showing where coverage will live or what code is touched.
4. **Test Level Strategy** – Assign the optimal test level(s) per behavior (unit, integration, contract, E2E, exploratory) and justify selections with risk/impact notes. Flag scenarios needing shared fixtures or specialized environments.
5. **Scenario Detailing** – Author `testPlan.scenarios` with inputs, expected outputs, edge cases, failure modes, and status (ready/blocked/deferred). Include sequence/parallel indicators and reference the commands that will execute them.
6. **Fixtures, Data, and Tooling** – Update `testPlan.fixtures` (and `testPlan.validationCommands` when needed) with datasets, mocks, diagnostics, and automation scripts required for the scenarios. Prefer reusing discovered assets; document creation tasks when new assets are mandatory.
7. **Readiness Feedback** – Feed QA insights back into `tasks.statusSummary` and affected task notes (e.g., mark tasks blocked until fixtures exist). Outline mitigation or follow-up MCP work when coverage cannot yet be finalized.
8. **Payload Update & Handoff** – Persist updates in `handoff/payload.json`, validate referential integrity (IDs resolve), and notify Stage 5 that the QA strategy is ready for execution.

## Outputs & Handoff
- Traceable coverage matrix connecting requirements → plan steps → tasks → scenarios.
- Prioritized list of scenarios with clear success signals and planned test levels.
- Fixture/data checklist with ownership and storage locations, ready for Stage 5 implementation.

## Required User Input
```text
$ARGUMENTS
```
Only proceed when the supplied payload already contains the approved plan from Stage 2 and the backlog from Stage 3. Otherwise, request updated inputs before drafting the QA strategy.
