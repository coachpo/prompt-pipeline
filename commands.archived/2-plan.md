---
description: Translate the approved requirement payload into a transparent, dependency-aware execution plan for non-trivial work.
---

## Stage Overview
- Re-confirm goals, constraints, and acceptance criteria captured in Stage 1.
- Decide whether detailed planning is required and document the rationale either way.
- Publish an ordered plan with evidence-backed steps, risks, owners, and expected validation artifacts.

## Commander Doctrine
- Exclude backward compatibility from all planning decisions; legacy consumers must not influence scope.
- Do not propose, design, or rely on shim code to bridge behaviors.

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
1. **Trigger & Readiness Check** – Load the payload passed via `$ARGUMENTS`. If `requirement.summary` or `requirement.acceptanceCriteria` are missing/stale, stop and request Stage 1 completion.
2. **Triviality Gate** – Run at least one repo reconnaissance pass for the impacted domain. If work is trivial, log a single `plan.steps` entry that justifies skipping detailed planning; otherwise continue.
3. **Goal & Constraint Confirmation** – Restate desired behaviors, SLAs, rollout strategy, and quality bars. Push any contradictions or unknowns into `plan.risks`.
4. **Context & Evidence Sweep** – Explore the codebase and documentation to understand existing behavior, integrations, telemetry, and validation. Store file+line citations for each insight so plan steps can reference hard evidence.
5. **Plan Drafting** – Convert findings into 3–6 execution steps (baseline audit → design deltas → implementation streams → validation → rollout). Each step captures owner, dependencies, success signals, and supporting artifacts.
6. **Risk / Assumption / Dependency Tracking** – Document blockers, sequencing requirements, mitigations, and artifacts (diagrams, spikes) inside `plan.risks` and `plan.artifacts`.
7. **Alignment Loop** – Share the draft plan with stakeholders, apply feedback, and keep traceability for any revisions.
8. **Payload Sync & Handoff** – Persist the approved plan and alert Stage 3 that the decomposition phase may begin.

## Outputs & Handoff
- Ordered, evidence-backed `plan.steps` with explicit dependencies and owners ready for Stage 3 task breakdown.
- Documented risks, assumptions, and mitigation ideas.
- References to supporting artifacts and standards needed by tasks, QA, and implementation teams.

## Required User Input
```text
$ARGUMENTS
```
The supplied payload must already include Stage 1 outputs; if not, block progression and request completion of the prior stage.
