---
description: Convert raw stakeholder input into an actionable, source-backed requirement payload that downstream teams can trust.
---

## Stage Overview
- Define the problem space, desired outcomes, constraints, and done-done signals using the stakeholder’s wording.
- Capture unknowns, risks, and prior art so planning, QA, and implementation inherit the same evidence base.
- Produce a clean `handoff/payload.json` that documents what to build, why, and how success will be measured.

## Shared Payload Contract
- **File:** `handoff/payload.json`
- **Required before starting:** none (Stage 1 creates/refreshes the payload).
- **Must update:**
  - `meta.currentStage = "1-specify"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt` (ISO 8601).
  - `requirement.summary`, `requirement.constraints`, `requirement.acceptanceCriteria`, `requirement.openQuestions`.
  - Seed `plan`, `testPlan`, `implementation`, `qaFindings` sections if absent so later stages inherit consistent keys.
- **Guardrails:**
  - _Payload Reset:_ If `handoff/payload.json` already contains requirement data, warn the user, archive it as `payload-<ISO8601>.json`, and recreate the template from `handoff/README.md` before writing.
  - _Search Depth:_ Whenever the brief affects existing code, run at least one repository-wide search (`start_search`, `rg`, language-server symbol lookup) per impacted domain and log file + line evidence inside the payload notes.

## Tooling & Evidence Expectations
- **MCP Reference First:** Consult `mcp/mcp_registry.md` and obey `mcp/mcp_rules.md` before choosing tooling. Prefer the MCP server specified by the registry’s priority matrix, only falling back to local commands when MCP coverage does not exist.
- Desktop Commander: `start_search`, `rg --files`, `rg "<symbol>" -n` for inventorying code, configs, feature flags, migrations.
- IDE/LSP tooling: use workspace symbol search, call hierarchies, or `rg` queries to map existing APIs, call graphs, and hidden dependencies.
- DeepWiki / Context7: pull official specs, changelogs, or internal standards when referenced by the stakeholder.
- Record every search/doc call (tool, query, key findings) so downstream owners know what has already been vetted.

## Workflow
1. **Requirement Intake & Confirmation** – Restate the request, success metrics, deadlines, rollout expectations, and compliance/observability bars. Close obvious gaps immediately; otherwise, list open questions with owners.
2. **Initial Discovery & Evidence Capture** – Inspect the repo for affected modules, existing TODOs, or prior implementations; cite files/lines in your working notes.
3. **Constraint & Dependency Mapping** – Document platform requirements (framework versions, data contracts, integrations, SLAs, feature flag strategies) and highlight areas that need alignment with other teams.
4. **Research & Prior Art Review** – Pull supporting documentation (design docs, standards, API references) using DeepWiki/Context7; summarize applicability or deltas versus the new request.
5. **Scope Assessment & Micro-Planning** – Determine whether the task is trivial. For non-trivial work, outline a lightweight path (discover → design → implement → validate) to set expectations for Stage 2.
6. **Risk & Assumption Logging** – Note constraints that could block progress (legal, infra capacity, data readiness) and tag them as risks or assumptions inside `requirement.constraints` / `requirement.openQuestions`.
7. **Payload Authoring & Validation** – Populate the requirement fields with succinct, testable statements; double-check that acceptance criteria map directly to stakeholder goals.
8. **Handoff Prep** – Confirm the payload’s integrity, archive your evidence references, and notify the planning owner that Stage 1 is complete.

## Outputs & Handoff
- Clean requirement payload with searchable evidence references.
- Ordered acceptance criteria plus explicit success metrics.
- Documented unknowns/risks routed to owners.
- Notes on repository reconnaissance to bootstrap Stage 2.

## Required User Input
```text
$ARGUMENTS
```
Proceed only when the stakeholder brief is present and unambiguous; otherwise, request clarification before writing to the payload.
