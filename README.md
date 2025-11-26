# PromptPipeline

PromptPipeline is a lightweight, text-first workflow for running backend tasks through six consistent stages (specify → plan → tasks → QA design → implement → acceptance). Each stage is a prompt in `commands/` and all handoffs flow through a single shared file `handoff/payload.json`.

## Repository Layout
- `commands/` – role-specific stage prompts (`1-specify` … `6-acceptance`) plus a usage guide.
- `handoff/` – shared payload contract: schema explainer (`README.md`) and the living payload file (`payload.json`).
- `mcp/` – Model Context Protocol governance (`mcp_rules.md`) and server registry (`mcp_registry.md`) for selecting tools responsibly.

## How the Workflow Runs
1. Sync the latest `handoff/payload.json` (and its history if applicable).
2. Open the stage file you own in `commands/<n>-*.md`.
3. Paste the current payload into the `$ARGUMENTS` block, follow the prompt, and update only the sections assigned to that stage.
4. Set `meta.currentStage`, `meta.lastUpdatedBy`, and `meta.lastUpdatedAt` before saving.
5. Hand the updated payload to the next owner; they repeat from step 2.

## Stage Reference (what each file is for)
- `1-specify`: Clarify the ask, record constraints, acceptance criteria, and open questions. Archive/refresh payload if needed.
- `2-plan`: Decide if planning is required; capture ordered steps, risks, and supporting artifacts.
- `3-tasks`: Break the plan into actionable tasks with dependencies, owners, and evidence links.
- `4-qa`: Design verification—coverage matrix, scenarios, fixtures, and tooling aligned to requirements.
- `5-implement`: Perform code and test changes; log commands and validations.
- `6-acceptance`: Final QA sign-off with evidence and any defects.

## Payload Schema (top-level keys)
- `meta`: Stage marker plus audit fields.
- `requirement`: Summary, constraints, acceptance criteria, and open questions.
- `plan`: Ordered steps, risks, and artifacts.
- `testPlan`: Coverage matrix entries, scenarios, fixtures/data needs.
- `implementation`: Code changes, tests, and commands run.
- `qaFindings`: Pass/fail status, evidence, and issues.

## MCP Usage Rules
Follow `mcp/mcp_rules.md` and select servers via `mcp/mcp_registry.md`. Use local tools first; prefer a single, scoped MCP call per round, and record what you queried so downstream roles inherit the evidence trail.

## Getting Started
- Clone or pull the repository.
- Read `commands/README.md` for the quick stage table.
- Confirm tool access: basic text editor + git; optionally configure MCP servers listed in `mcp/mcp_registry.md`.
- Work through stages in order unless the payload already specifies a later handoff.

## Contributing
- Keep stage prompts text-only and deterministic.
- Update `mcp_registry.md` when adding or changing MCP servers.
- If you modify the payload schema, document the change in `handoff/README.md` and adjust stage prompts accordingly.

## License
License is not specified; check with the repository owner before reuse or distribution.
