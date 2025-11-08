---
description: Analyze the raw requirement input, clarify it, and persist the result into the shared payload for downstream stages.
---

## Shared Payload Contract
- File: `handoff/payload.json`
- Required before starting: none (this stage initializes the payload).
- Guardrail — Existing Payload Detection:
  1. Before writing anything, inspect `handoff/payload.json`. If it already contains non-empty requirement data (e.g., `requirement.summary` or any populated arrays), you **must** warn the user that a fresh payload will be created.
  2. After warning, archive the previous payload by renaming it to `handoff/payload-<ISO8601 timestamp>.json` (e.g., `mv handoff/payload.json handoff/payload-2025-11-07T1802Z.json`).
  3. Re-create a clean `handoff/payload.json` using the template in `handoff/README.md` before continuing.
- Search Depth Guardrail — Whenever the incoming brief implies touching existing code (e.g., “add a new function” or “extend service Foo”), run at least one repository-wide search before drafting the requirement summary:
  - Use **Desktop Commander** (`start_search`, `rg --files`, `rg "<symbol>" -n`) or **Serena** (`find_symbol`, `find_referencing_symbols`) to inventory existing functions, handlers, or configs.
  - Capture the most relevant hits (file + line) in your working notes and reference them when restating the requirement so downstream stages inherit the context.
- Must update:
  - `meta.currentStage = "1-specify"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt` (ISO 8601).
  - `requirement.summary`, `requirement.constraints`, `requirement.acceptanceCriteria`, `requirement.openQuestions`.
  - Optionally seed `plan`, `testPlan`, `implementation`, `qaFindings` with placeholders if missing.

## User Input — must not be empty

```text
$ARGUMENTS
```

Load the user input together with the current payload. If input is absent or unclear, stop and request clarification.

---

# Requirement Handling Workflow

## 1. Requirement Intake & Confirmation
- Restate every incoming requirement in your own words and confirm constraints, success metrics, deadlines, and testing expectations.
- Capture explicit acceptance criteria and unknowns; ask the user to resolve blockers before proceeding.
- If ambiguity remains, open a short clarification loop before touching the codebase.

## 2. Guidance, Prior Art & Research Depth
- Aggregate relevant material: repository docs, project standards, recent conversation context, and linked specs; note every source in `requirement.constraints` or `requirement.openQuestions`.
- When documentation lives in external repos or wikis, call **DeepWiki** (`read_wiki_structure`, `read_wiki_contents`, `ask_question`) before relying on memory.
- For official API references or version differences, call **Context7** (`resolve-library-id`, `get-library-docs`). Prefer multiple MCP calls (e.g., spec + changelog) when the feature crosses version boundaries.
- If internal tooling or historical tasks exist, run **Desktop Commander** `start_search` across `handoff/` and `commands/` to surface precedents; summarize whether they reuse-ready or need updates.
- Record which MCP servers and repository searches were executed so downstream stages know the baseline evidence set.

## 3. Scope Assessment & Micro-Planning
- Decide whether the task is trivial. If not, outline a lightweight plan (e.g., assess current behavior → design delta → implement → adjust tests).
- Use **Sequential Thinking** to structure the plan: stage the problem definition, research, analysis, and synthesis thoughts until the work is fully scoped.
- Socialize the plan with the user when effort is non-trivial or when trade-offs exist.

## 4. Codebase Reconnaissance
- Before finalizing the requirement summary, capture concrete evidence of the current behavior:
  - Run at least one `start_search` or `rg` query per target domain (API layer, job, infra) to reveal existing functions, guard clauses, and TODOs.
  - Use **Serena** (`get_symbols_overview`, `find_symbol`, `find_referencing_symbols`) to map the call graph of the most relevant symbol; note any surprises in `requirement.constraints`.
- Maintain a findings log (filenames + line numbers) so you can cite evidence during implementation and reporting; link this log inside the payload when possible.

## 5. Implementation Execution Guidance
- If quick fixes are needed to validate assumptions, document them but leave execution to later stages unless explicitly requested.
- Use **Serena** or **Desktop Commander** editing APIs sparingly, focusing on investigation rather than changes at this stage.

## 6. Validation & Quality Gates
- Run targeted spikes or prototypes only when necessary to confirm feasibility; capture outputs.
- Document known risks or open issues that later stages must track.

## 7. Payload Update & Handoff
- Write the clarified requirement data into `handoff/payload.json` (after applying the guardrail, if triggered).
- Highlight any unresolved questions in `requirement.openQuestions`.
- Notify the planning owner that the payload is ready for Stage 2.
