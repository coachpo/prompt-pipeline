---
description: Convert raw stakeholder input into an actionable, source-backed requirement payload that downstream teams can trust.
---

## Stage Overview
- Speak the stakeholder’s language while translating it into precise goals, constraints, and success signals.
- Capture unknowns, risks, and prior art once so planning, QA, and implementation inherit the same evidence base.
- Produce a refreshed `handoff/payload.json` that explains **what to build, why it matters, and how success will be judged.**

## Commander Doctrine
- Ignore backward compatibility considerations entirely; do not evaluate legacy interfaces or migration paths.
- Never introduce shim code or compatibility layers during this stage.

## Shared Payload Contract
- **File:** `handoff/payload.json`
- **Required before starting:** none (Stage 1 creates/refreshes the payload).
- **Must update:**
  - `meta.currentStage = "1-specify"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt` (ISO 8601).
  - `requirement.summary`, `requirement.constraints`, `requirement.acceptanceCriteria`, `requirement.openQuestions`.
  - Seed `plan`, `testPlan`, `implementation`, `qaFindings` sections if absent so later stages inherit consistent keys.
- **Guardrails:**
  - _Payload Reset:_ If `handoff/payload.json` already contains requirement data, warn the user, archive it as `payload-<ISO8601>.json`, then recreate the template from `handoff/README.md` before writing.
  - _Search Depth:_ When the brief touches existing code, run at least one repository-wide search (`start_search`, `rg`, or IDE symbol lookup) per impacted domain and log file+line evidence inside the payload notes.

## Tooling & Evidence Expectations
- **MCP Reference First:** Consult `mcp/mcp_registry.md` and obey `mcp/mcp_rules.md` before choosing tooling. Prefer the MCP server specified by the registry’s priority matrix, only falling back to local commands when MCP coverage does not exist.
- Desktop Commander: `start_search`, `rg --files`, `rg "<symbol>" -n` for inventorying code, configs, feature flags, migrations.
- IDE/LSP tooling: use workspace symbol search, call hierarchies, or `rg` queries to map existing APIs, call graphs, and hidden dependencies.
- DeepWiki / Context7: pull official specs, changelogs, or internal standards when referenced by the stakeholder.
- Record every search/doc call (tool, query, key findings) so downstream owners know what has already been vetted.

## Workflow
1. **Intake & Echo** – Restate the request, deadlines, rollout, compliance, and observability expectations using the stakeholder’s tone. Capture unresolved gaps as `requirement.openQuestions` with owners.
2. **Evidence Recon** – Scan the repo for affected modules, TODOs, or prior art; cite file+line references in working notes.
3. **Constraint/Dependency Map** – Document frameworks, integrations, SLAs, data contracts, feature flag needs, and any cross-team alignment required.
4. **Research & Prior Art** – Pull supporting docs via DeepWiki/Context7 (standards, API contracts, historical decisions) and summarize how they influence the new ask.
5. **Scope Signal** – Decide if the effort is trivial. If yes, justify it in the payload; if no, sketch a lightweight discover → design → implement → validate path to inform Stage 2.
6. **Risk & Assumption Log** – Tag blockers (legal, infra capacity, data readiness, compliance) as `requirement.constraints` or `requirement.openQuestions` with mitigation notes.
7. **Payload Authoring** – Populate requirement fields with testable statements, ensuring acceptance criteria map to stakeholder outcomes and metrics.
8. **Validation & Handoff** – Double-check integrity (meta timestamps, seeded sections, archive references) and alert Stage 2 that Stage 1 is complete.

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
