---
description: Bridge strategic planning and execution by translating the approved plan into actionable, prioritized, and traceable development tasks.
---

## Stage Overview
- Convert every plan step into a hierarchical tree of engineering tasks (parent → child → leaf) until each leaf is atomic enough to finish, review, and test within a single iteration or MCP invocation.
- Maintain explicit traceability: every task inherits its ancestor plan step IDs plus requirement references, acceptance criteria anchors, and QA hooks.
- Output a dependency-aware backlog (owners, estimates, statuses) that downstream stages can consume without rediscovering context or guessing ordering.

## Shared Payload Contract
- **File:** `handoff/payload.json`
- **Required before starting:** `requirement.summary`, `plan.steps` (non-empty).
- **Must update:**
  - `meta.currentStage = "3-tasks"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`.
  - `tasks.items` (hierarchical task objects with IDs, parent IDs, descriptions, owners, linked plan step IDs, acceptance hooks, MCP/tooling notes).
  - `tasks.dependencies` (graph of cross-task or external blockers), `tasks.statusSummary` (ready / blocked / deferred), `tasks.artifacts` (links to diagrams, spikes, or decision logs reused during implementation).
  - Maintain existing sections (`requirement`, `plan`, `testPlan`, `implementation`, `qaFindings`) without rewriting their data.
  - Ensure every plan step has at least one atomic leaf task in `tasks.items` and every leaf references exactly one parent.

## Tooling & Evidence Expectations
- **MCP Reference First:** Consult `mcp/mcp_registry.md` and obey `mcp/mcp_rules.md` before choosing tooling. Favor registry-prioritized MCP servers (e.g., `mcp-shrimp-task-manager` for backlog structuring, `Sequential Thinking` for reasoning, Desktop Commander for repo evidence) and fall back to local commands only when MCP coverage does not exist.
- `mcp-shrimp-task-manager`: generate, split, and manage structured task lists, dependencies, and verification criteria.
- Desktop Commander + IDE/LSP tooling: inspect repositories, collect evidence (files + lines), and ensure each task references concrete code artifacts.
- DeepWiki / Context7: capture external standards, API docs, or architectural decisions linked to particular tasks.
- Record every MCP/local search (tool, query, findings) referenced inside task notes so downstream owners inherit the research trail.

## Workflow
1. **Input Validation & Context Sync** – Load `$ARGUMENTS`, confirm `requirement.summary` and `plan.steps` freshness, and capture any stakeholder sequencing constraints. Raise blockers in `requirement.openQuestions` before continuing.
2. **Plan Decomposition Strategy** – For each `plan.steps[*]`, outline the expected deliverables, repos, and validation targets, then decide the depth of decomposition needed to reach atomic work units. Document this strategy in task notes for traceability.
3. **Hierarchical Task Expansion** – Create parent tasks that mirror the plan intent, then iteratively split them into sequential child tasks until each leaf:
   - Can be executed without additional scoping research.
   - Targets a bounded code surface (files, modules, APIs) and verification method.
   - Produces a measurable artifact (PR, doc, script, test) and acceptance checklist.
   Record parent-child IDs so automation can reconstruct ancestry.
4. **Task Definition & Evidence** – For every task, capture Who/What/Where/Why, acceptance tests, telemetry or logging hooks, plus concrete evidence references (file paths with line numbers, MCP results, doc sections). Note any required MCP tooling or scripts.
5. **Dependencies, Ordering & Readiness** – Assign effort/complexity bands, owners, and statuses. Populate `tasks.dependencies` with prerequisites (other tasks, environments, external approvals) and highlight mitigations or fallback options. Order tasks according to critical path and mark parallelizable streams.
6. **Quality Hooks** – Embed verification recipes (commands, datasets, telemetry checks) so each task is reviewable in isolation. Ensure QA hooks roll up to the originating plan acceptance criteria.
7. **Backlog Publication** – Summarize ready/blocked/deferred counts, ensure IDs are unique, and confirm every plan step has at least one ready leaf task. Provide slicing metadata (component, epic, owner) for downstream automation.
8. **Payload Update & Handoff** – Persist updates to `handoff/payload.json`, verifying parent-child linkage fields and dependency graphs are machine-readable. Notify Stage 4 (QA) that the backlog is ready for coverage design.

## Outputs & Handoff
- Structured `tasks.items` backlog with traceability to requirements and plan steps.
- Dependency graph plus readiness notes for implementation and QA.
- Evidence links (code refs, MCP call notes, docs) embedded within tasks for rapid onboarding.

## Required User Input
```text
$ARGUMENTS
```
Proceed only when the payload includes the approved plan from Stage 2. If plan data is missing or stale, block progress and request an updated payload before creating tasks.
