---
description: Translate the approved requirement payload into a transparent, dependency-aware execution plan for non-trivial work.
---

## Stage Overview
- Confirm goals, constraints, and acceptance criteria captured in Stage 1.
- Decide whether detailed planning is required; document rationale either way.
- Produce an ordered plan with evidence-backed steps, risks, owners, and expected validation artifacts.

## Shared Payload Contract
- **File:** `handoff/payload.json`
- **Required before starting:** `requirement.summary`, `requirement.acceptanceCriteria`.
- **Must update:**
  - `meta.currentStage = "2-plan"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`.
  - `plan.steps` (ordered execution steps with owners, files, dependencies, statuses), `plan.risks`, `plan.artifacts` (links to diagrams, spikes, prototypes).
  - Preserve previously written sections (`requirement`, `testPlan`, `implementation`, `qaFindings`).

## Tooling & Evidence Expectations
- **MCP Reference First:** Review `mcp/mcp_registry.md` and comply with `mcp/mcp_rules.md` before selecting tooling. Favor the registry’s recommended MCP server for each activity and fall back to local tools only when MCP coverage is unavailable or out of scope.
- Desktop Commander searches (`start_search`, `rg`) to verify file ownership, existing utilities, and blast radius.
- IDE/LSP symbol lookups or targeted `start_search`/`rg` queries for fan-in/out confirmation before declaring work “trivial”.
- DeepWiki / Context7 for standards, API contracts, or version notes referenced in the plan.
- Sequential Thinking to structure reasoning and expose assumptions before locking the plan.

## Workflow
1. **Trigger & Readiness Check** – Load the payload provided in `$ARGUMENTS`. If required requirement fields are missing, halt and request Stage 1 completion.
2. **Triviality Assessment** – Run at least one repo reconnaissance pass for the highlighted domain. If the change is trivial, log a single `plan.steps` item explaining why detailed planning is unnecessary; otherwise continue.
3. **Goal & Constraint Confirmation** – Restate desired behavior, SLAs, rollout expectations, and quality bars using the requirement payload. Move any contradictions into `plan.risks`.
4. **Context Gathering & Evidence Logging** – Explore the codebase and documentation to understand current implementations, integrations, telemetry, and tests. Capture file + line references for each finding so plan steps cite concrete evidence.
5. **Plan Drafting** – Convert research into 3–6 execution steps (e.g., baseline audit → design delta → implement backend → update tests → docs/rollout). Include owners, dependencies, and success signals per step.
6. **Risk, Assumption & Dependency Tracking** – Call out blockers, sequencing requirements, and mitigations (feature flags, phased rollout, toggles). Attach verification artifacts (design doc sections, diagrams, spike outputs) to `plan.artifacts`.
7. **Alignment & Publication** – Present the plan to stakeholders (user or implementation owner) for confirmation. Apply feedback while retaining traceability.
8. **Payload Update & Handoff** – Persist the approved plan in the payload and notify the Stage 3 owner (tasks) that the execution plan is ready for task decomposition.

## Outputs & Handoff
- Ordered, evidence-backed `plan.steps` with explicit dependencies and owners ready for Stage 3 task breakdown.
- Documented risks, assumptions, and mitigation ideas.
- References to supporting artifacts and standards needed by tasks, QA, and implementation teams.

## Required User Input
```text
$ARGUMENTS
```
The supplied payload must already include Stage 1 outputs; if not, block progression and request completion of the prior stage.
