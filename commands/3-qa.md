---
description: Transform the shared requirement + plan payload into a concrete test design and persist it for implementers.
---

## Shared Payload Contract
- File: `handoff/payload.json`
- Required fields before starting: `requirement.summary`, `plan.steps` (non-empty).
- Must update:
  - `meta.currentStage = "3-qa"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`.
  - `testPlan.coverageMatrix`, `testPlan.scenarios`, `testPlan.fixtures`.
  - Reference plan step IDs so coverage stays traceable; do not modify other sections except to append clarifications to `requirement.openQuestions` if gaps remain.

## User Input — must include the shared payload

```text
$ARGUMENTS
```

If dependencies are missing, pause and request the prior stages to complete their sections.

---

# Test Design Workflow

## 0. Preconditions & Scope
- Confirm that the user input clearly states acceptance criteria, planned code changes, and constraints; if not, request an updated brief.
- Extract all behaviors, edge cases, and success metrics directly from the supplied text—do not rely on other files.
- Note explicit quality bars (latency, SLAs, compliance, observability) that must shape test depth.
- Run at least one repository sweep before mapping coverage:
  - `start_search`/`rg` for the target module name to understand existing tests or helpers.
  - **Serena** `find_referencing_symbols` for the primary function/class to reveal integration points your tests must cover.
  - Summarize findings (file + line) in `testPlan.coverageMatrix` rationale columns.

## 1. Coverage Mapping
- Map every acceptance criterion and planned change to concrete behaviors the tests must observe (API responses, edge-case handling, error propagation, logging, metrics).
- Use **Sequential Thinking** to ensure each behavior has at least one verification idea before moving on.
- Record a traceability matrix (criterion → behavior → planned test) within `testPlan.coverageMatrix`.

## 2. Test Level Selection
- Decide for each coverage point whether verification belongs in unit, integration, contract, or end-to-end suites.
- Inspect the repository with **Desktop Commander** (`list_directory`, `read_file`, `start_search`, `rg`) and **Serena** (`get_symbols_overview`, `find_symbol`, `find_referencing_symbols`) to locate the exact packages or directories to extend; prefer multiple searches (by API name, event key, telemetry string) to avoid shallow coverage.
- Capture rationale for the chosen test level so future contributors understand the placement.

## 3. Scenario Definition
- For every coverage point, spell out inputs, expected outputs, edge cases, and failure modes.
- When table-driven testing is appropriate, define the table schema (field names, data types, defaults) and naming conventions for rows.
- Include both happy paths and negative/degenerate cases to guard against regressions.

## 4. Fixtures, Mocks, and Test Data
- Inventory required fixtures, doubles, helpers, or snapshot updates.
- Before creating new assets, search with **Desktop Commander** or **Serena** to reuse existing fixtures (`start_search "FixtureName"`). When fixtures span services, call **DeepWiki** or **Context7** to verify contract schemas and note doc links alongside the fixture description.
- Document any new test data (e.g., JSON payloads, SQL migrations) and the directories where they will live.

## 5. Success Criteria & Tooling
- Define the assertions, snapshot diffs, metrics checks, or log inspections that will prove each requirement.
- Specify the commands needed to run the suites (`make test`, `go test ./internal/...`, `npm run contract-tests`, etc.) and any supporting tooling (coverage, linters).
- If external validation or manual QA is required, describe how evidence will be collected and stored.

## 6. Coordination & Handoff
- Share the mini test plan with the implementation team; incorporate feedback when new insights surface.
- Track scenario status (planned, implemented, deferred, dropped) so downstream owners know what remains.
- Persist all outputs inside `handoff/payload.json` and notify the next stage when ready.
