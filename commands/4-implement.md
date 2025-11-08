---
description: Execute the supplied implementation + test instructions exactly as provided, producing production-ready code and proof of validation.
---

## Shared Payload Contract
- File: `handoff/payload.json`
- Required fields before starting: `requirement.summary`, `plan.steps`, `testPlan.scenarios`.
- Must update:
  - `meta.currentStage = "4-implement"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`.
  - `implementation.changes` (file-level notes, diffs, rationale), `implementation.tests` (added/updated suites), `implementation.commands` (commands run + outcomes).
  - May append clarifications to `plan.steps[*].status` and `testPlan.scenarios[*].status` but must not delete prior data.

## User Input — contains the full brief

```text
$ARGUMENTS
```

If any prerequisite sections are missing, pause and request upstream owners to finish their updates.

---

# Implementation Workflow

## 0. Intake & Focus
- Parse the input for task order, target files, success metrics, and validation expectations; capture unknowns as follow-up questions.
- Confirm tooling (language versions, build scripts, linters) described in the brief so environment surprises do not derail execution.
- Before touching code, verify that discovery work from prior stages is still current:
  - Re-run critical `start_search`/`rg` queries for the primary symbols to catch merges that landed since planning.
  - Use **Serena** (`get_symbols_overview`) to snapshot current signatures so diffs reference concrete baselines.
  - If the brief references external APIs or frameworks, refresh the authoritative docs through **Context7** or **DeepWiki** and link them in `implementation.changes`.

## 1. Environment Recon
- Inspect the referenced packages using **Desktop Commander** (`list_directory`, `read_file`, `start_search`, `rg`) to understand current implementations; favor layered searches (e.g., controller name, telemetry key, config flag) so no dependency is missed.
- For semantic insight into functions and call chains, use **Serena** (`get_symbols_overview`, `find_symbol`, `find_referencing_symbols`) and attach the findings (file + line) to your working log.
- Note baseline behavior, TODOs, or guardrails from the code so changes stay context-aware; include references inside `implementation.changes`.

## 2. Stepwise Execution
- Follow the ordered tasks from the brief; keep each code change tightly scoped and commit-ready.
- Before writing a function, outline its signature, invariants, and links to acceptance criteria/tests stated in the input.
- Use **Serena** editing APIs (`insert_before_symbol`, `replace_symbol_body`, `write_file`) for symbol-aware modifications; fall back to **Desktop Commander** (`write_file`, `edit_block`) for file-level edits.
- Each edit should cite the discovery search that justified it (e.g., “mirrors behavior in `foo/service.go:118`”); record this inside `implementation.changes`.
- Maintain a lightweight progress log so other contributors can see which tasks are complete (update `plan.steps[*].status`).

## 3. Pattern Alignment
- Reuse existing utilities, error handling, and logging conventions surfaced during recon to keep code idiomatic.
- If introducing new abstractions, document the rationale inline or in the handoff summary so reviewers understand the change.

## 4. Immediate Test Work
- As soon as a function compiles, implement or update the mapped tests, fixtures, and mocks described in the input.
- When ambiguity arises, revisit the user input and clarify before deviating from the specified scenarios.
- Keep tests co-located with their targets unless the brief dictates another structure. Run `start_search` for the test subject name to double-check for existing suites before adding new files.

## 5. Validation Loop
- Execute the prescribed commands (e.g., `go test ./internal/foo`, `npm run contract-tests`, `make test`) via **Desktop Commander** (`start_process`, `interact_with_process`).
- Capture outputs for each run so logs can be shared during review.
- Rerun targeted suites after every meaningful change to catch regressions early.

## 6. Iteration & Polish
- Iterate until every planned behavior and test passes locally; address flakiness or race conditions uncovered during validation.
- Remove temporary instrumentation, run formatters/linters, and synchronize documentation or comments promised in the brief.
- Record any deviations from the supplied plan along with mitigation steps or follow-up tasks.

## 7. Payload Update & Handoff
- Summarize the final state under `implementation.*` and update relevant `plan`/`testPlan` statuses.
- Provide instructions for reproducing validation (commands, environment variables, MCP calls) so downstream owners can re-run checks confidently.
- Notify the acceptance owner that the payload is ready for Stage 5.
