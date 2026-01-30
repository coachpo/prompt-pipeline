---
description: Execute the approved plan, task backlog, and test strategy to deliver production-ready code plus proof of validation.
---

## Stage Overview
- Execute **only** the scope documented in the approved plan, backlog, and QA strategy. If inputs conflict or appear incomplete, stop, log the issue in `implementation.changes`, and notify upstream owners instead of silently patching gaps.
- Build the code, tests, docs, and automation artifacts needed for every leaf task’s acceptance criteria and mapped QA scenarios. Treat each leaf as a self-contained deliverable with its own design + validation notes.
- Capture deterministic evidence (diffs, MCP command transcripts, logs, coverage) so Stage 6 can replay validation in a clean workspace with zero guesswork.
- Operate autonomously—no mid-stage approval loops. Use MCP checkpointing plus payload logs to record state transitions and support recovery.

## Commander Doctrine
- Implementation work must ignore backward compatibility; do not preserve legacy APIs or data formats.
- Never create shim code, adapters, or compatibility layers as part of the solution.

## Shared Payload Contract
- **File:** `handoff/payload.json`
- **Required before starting:** `requirement.summary`, `plan.steps`, `tasks.items` (with parent-child linkage), `testPlan.scenarios`, `testPlan.validationCommands` (or equivalent execution notes).
- **Must update:**
  - `meta.currentStage = "5-implement"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`.
  - `implementation.changes` (per-change rationale, files touched, linked requirement/plan/task/test IDs, evidence references).
  - `implementation.tests` (suites added/updated with pass/fail status, coverage deltas, artifacts stored).
  - `implementation.commands` (exact commands run, environment/fixture notes, timestamps, summarized outcomes) referencing `testPlan.validationCommands` where applicable.
  - Optional but encouraged: append incremental status notes to `plan.steps[*]`, `tasks.items[*]`, and `testPlan.scenarios[*]`; never delete prior context.

## Tooling & Evidence Expectations
- **MCP-first mandate:** Before running any local command, consult `mcp/mcp_registry.md` and `mcp/mcp_rules.md`. Note the governing rule ID in the payload whenever the tooling choice is not obvious.
- Log every MCP invocation (tool, intent, inputs, artifacts, success/failure) in `implementation.commands[*].mcpLog`. If a tool is unavailable or fails twice, record the failure reason, fallback action, and the rule that allowed the fallback.
- Use Desktop Commander for local edits (`write_file`, `edit_block`), searches, listings, or shell processes. Persist PID/command pairs so you can resume from an interrupted state.
- Lean on IDE/LSP tooling for semantic navigation (symbols, call hierarchies) and cite exact file+line references in the payload evidence.
- Pull authoritative documentation via DeepWiki / Context7; record doc versions, permalinks, and ≤25-word snippets per lookup.
- Keep `mcp-shrimp-task-manager` (or successor) mirrors in sync with `tasks.items[*]` whenever task status changes.
- Execute lint/test/build commands exactly as documented in `testPlan.validationCommands`. If you must adapt a command (fixture path, env var), capture the delta, rationale, and resulting artifacts.

## Compliance & Safety Protocol
- Validate every intended action—especially writes, migrations, or external interactions—against `mcp/mcp_rules.md`. Abort or quarantine steps that violate scope, data boundaries, or security posture; annotate `implementation.changes` with the blocked rule reference and recommended remediation.
- On recoverable failures, perform exactly one automated retry. After the second failure, mark the task as `blocked`, log diagnostics, and continue with the next unblocked task to maintain autonomous progress.
- Preserve least-privilege: request elevated permissions only when a rule explicitly authorizes it and the payload documents the justification. Record who/what granted escalation and its duration.
- Maintain chronological, append-only logs capturing timestamps, executor identity, MCP/local command identifiers, and resulting artifacts for auditability.

## Workflow
1. **Intake & Environment Confirmation** – Parse `$ARGUMENTS` plus `handoff/payload.json` to assemble the dependency-ordered leaf task queue. Confirm `meta`, `plan`, `tasks`, and `testPlan` fields exist and are current; if any prerequisite is missing or stale, set the stage status to `blocked` and stop until upstream remediation is logged. Snapshot toolchain versions, lint/format rules, and environment variables; store the snapshot reference in `implementation.changes`.
2. **Baseline Recon** – Rehydrate prior search context via Desktop Commander plus IDE/LSP indexing to detect upstream drift (new commits, config changes, feature flags). Capture file hashes, symbol signatures, or schema versions before editing and attach them to the relevant plan/test IDs in the payload.
3. **Leaf Task Execution Loop** – Iterate through the leaf queue autonomously:
   - Restate the task’s acceptance criteria, dependencies, and QA hooks at execution time.
   - Select the appropriate MCP toolchain; document rule compliance and the exact commands/scripts used.
   - Implement code/tests/docs in the minimal diff necessary, commit artifacts to source control (or stage them) if mandated, and record file + line references.
   - Upon completion (or failure), update the task status in both `tasks.items[*]` and the MCP task tracker.
4. **Testing & Quality Gates** – Trigger the validation commands linked to the current leaf. Attach raw logs, summarized outcomes, coverage deltas, and artifact URIs to both `implementation.tests` and `implementation.commands`. When a test fails, capture the triage steps, retry attempt, and final disposition (fixed, flaky, blocked).
5. **Documentation, Telemetry & Ops Artifacts** – Enforce documentation deliverables promised by the plan: READMEs, runbooks, migrations, dashboards, feature flags, metrics. Document where each artifact lives (path, dashboard URL, dataset) and how Stage 6 should inspect it.
6. **Risk & Scope Management** – For any newly discovered risk, open question, or dependency shift, immediately update `requirement.openQuestions`, `tasks.dependencies`, or `plan.steps[*].notes`. Record mitigation steps and responsible follow-ups; do not expand scope without explicit upstream approval captured in the payload.
7. **Evidence Packaging & Payload Sync** – After each leaf, append the associated diffs, logs, MCP transcripts, and validation notes to `implementation.*`. Before exiting the stage, ensure every payload section touched includes timestamps, executor identity, and pointers to artifacts so acceptance can replay the work deterministically.

## Outputs & Handoff
- Implementation summary mapping each change/test artifact to the originating requirement, plan step, task, and QA scenario ID.
- Comprehensive test ledger covering automated and manual validations, their outcomes, retry history, and artifact locations.
- Explicit rerun instructions (commands, environment vars, seed data, fixtures) plus any outstanding risks or TODOs Stage 6 must resolve prior to acceptance.
- MCP call ledger summarizing every tool interaction, fallback, and result for auditability.

## Required User Input
```text
$ARGUMENTS
```
Work only from the provided payload/brief; if prerequisite sections (plan, tasks, or test plan) are missing or stale, block execution until upstream stages complete their work.
