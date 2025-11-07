# Shared Handoff Contract

All workflow stages exchange data through `handoff/payload.json`. Each agent edits the same file manually after finishing their step—no scripts required.

## Workflow
1. Pull/receive the latest `handoff/payload.json`.
2. Update only the sections owned by your stage.
3. Set `meta.currentStage` to the stage you just completed and fill `meta.lastUpdatedBy` / `meta.lastUpdatedAt`.
4. Share/commit the updated file for the next teammate.

## Schema Overview
- `requirement`: Filled by Stage 1 (Specify). Captures clarified summary, constraints, acceptance criteria, and open questions.
- `plan`: Updated by Stage 2 (Plan). Contains ordered steps, risks, and supporting artifacts/links.
- `testPlan`: Added during Stage 3 (QA/Test Design). Holds coverage matrix entries, concrete scenarios, and required fixtures/data.
- `implementation`: Owned by Stage 4 (Implement). Tracks code changes, updated tests, and commands/results.
- `qaFindings`: Completed by Stage 5 (Acceptance). Stores pass/fail status, evidence, and defect reports.

Each stage must validate that the sections it depends on are non-empty before proceeding. If required data is missing, stop and request clarification rather than guessing.
