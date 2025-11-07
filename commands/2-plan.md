---
description: Consume the shared requirement payload, decide if planning is warranted, then persist a transparent execution plan for non-trivial backend tasks.
---

## Shared Payload Contract
- File: `handoff/payload.json`
- Required fields before starting: `requirement.summary`, `requirement.acceptanceCriteria`.
- Must update:
  - `meta.currentStage = "2-plan"`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`.
  - `plan.steps` (ordered list with owners, files, dependencies), `plan.risks`, `plan.artifacts`.
  - Maintain existing sections (`requirement`, `testPlan`, `implementation`, `qaFindings`) without wiping prior data.

## User Input — must contain the latest payload

```text
$ARGUMENTS
```

If the payload lacks the required requirement data, stop and request Stage 1 to finish first.

---

# Planning Workflow

## 0. Trigger & Inputs
- Parse the supplied user input for objectives, scope, constraints, and open questions; do not rely on any other files.
- Proceed only if the task is non-trivial (e.g., multi-file impact, new behavior, ambiguous requirements, or external dependencies).
- If trivial, record that no plan is necessary by adding a `plan.steps` entry explaining the decision, then await the next instruction.

## 1. Goal & Constraint Confirmation
- Restate the target outcome, acceptance criteria, deadlines, and quality bars using the wording from the user input.
- Capture explicit constraints (tech stack, interfaces, rollout strategy) in the plan preamble.
- Enumerate unresolved questions or assumptions; label each with an owner or required follow-up.

## 2. Context Gathering
- When documentation, API guarantees, or standards are referenced, pull them via **DeepWiki** or **Context7** as needed.
- Explore the codebase with **Desktop Commander** (`list_directory`, `read_file`, `start_search`) and **Serena** (`get_symbols_overview`, `find_symbol`) to map relevant packages, entry points, and tests.
- Record findings with file paths and line references so every plan step cites concrete evidence.

## 3. Plan Drafting
- Use **Sequential Thinking** to reason through Problem Definition → Research → Analysis → Synthesis before locking the plan.
- Translate the reasoning into 3‑6 ordered steps (inspect current behavior, design, implement, validate, clean up).
- For each step, note expected files/packages, MCP tooling, and prerequisites or dependencies.

## 4. Dependencies & Estimates
- Sequence steps based on technical dependencies; call out blockers explicitly.
- Provide lightweight estimates or risk notes so stakeholders understand effort and uncertainty.
- Define verification artifacts (tests, logs, screenshots) that will confirm completion of each step.

## 5. Publication & Alignment
- Present the draft plan to the user (or the implementation owner); request confirmation or edits before proceeding.
- Apply feedback surgically, preserving prior context for auditability.

## 6. Execution Tracking Hooks
- As implementation progresses (possibly by a different agent), update step statuses via the planning tool (`update_plan`) or by annotating `plan.steps[*].status` in the payload.
- Document deviations (scope changes, new tasks) inline and highlight their impact on later steps.

## 7. Payload Update & Handoff
- Save the enriched `plan` section back to `handoff/payload.json` without discarding other data.
- Notify the Stage 3 owner that the payload now contains the approved plan.
