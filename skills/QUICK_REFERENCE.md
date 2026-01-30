# Quick Reference: Using the Six-Stage Workflow SKILLs

## Overview
The six-stage workflow has been converted into reusable SKILLs that guide development from requirements through acceptance testing. Each SKILL operates on a shared `handoff/payload.json` file to maintain traceability.

## The Six Stages

### 1️⃣ Specify Requirements
**Location:** `.agent/skills/specify-requirements/SKILL.md`  
**Purpose:** Converts raw stakeholder input into structured requirements  
**Prerequisites:** None (creates/refreshes payload)  
**Outputs:** `requirement.*` fields in payload

**When to use:** At project kickoff or when clarifying new features

---

### 2️⃣ Create Plan
**Location:** `.agent/skills/create-plan/SKILL.md`  
**Purpose:** Translates requirements into execution plan  
**Prerequisites:** `requirement.summary`, `requirement.acceptanceCriteria`  
**Outputs:** `plan.steps`, `plan.risks`, `plan.artifacts`

**When to use:** After requirements are approved, before breaking down work

---

### 3️⃣ Create Tasks
**Location:** `.agent/skills/create-tasks/SKILL.md`  
**Purpose:** Decomposes plan into hierarchical task backlog  
**Prerequisites:** `requirement.summary`, `plan.steps`  
**Outputs:** `tasks.items`, `tasks.dependencies`, `tasks.statusSummary`

**When to use:** After plan is approved, to create actionable work items

---

### 4️⃣ Design QA Strategy
**Location:** `.agent/skills/design-qa-strategy/SKILL.md`  
**Purpose:** Creates test coverage matrix and validation approach  
**Prerequisites:** `requirement.summary`, `plan.steps`, `tasks.items`  
**Outputs:** `testPlan.coverageMatrix`, `testPlan.scenarios`, `testPlan.fixtures`

**When to use:** After tasks are defined, before implementation

---

### 5️⃣ Implement Solution
**Location:** `.agent/skills/implement-solution/SKILL.md`  
**Purpose:** Executes code changes with full traceability  
**Prerequisites:** All payload sections from stages 1-4  
**Outputs:** `implementation.changes`, `implementation.tests`, `implementation.commands`

**When to use:** After QA strategy is approved, to build the solution

---

### 6️⃣ Acceptance Testing
**Location:** `.agent/skills/acceptance-testing/SKILL.md`  
**Purpose:** Independent verification and go/no-go decision  
**Prerequisites:** All payload sections including implementation  
**Outputs:** `qaFindings.status`, `qaFindings.evidence`, `qaFindings.issues`

**When to use:** After implementation, before release

---

## Quick Start Examples

### Example 1: Start a new feature
```
1. Ask agent to use "Specify Requirements" skill with your feature description
2. Review the generated payload.json
3. Proceed through stages 2-6 sequentially
```

### Example 2: Skip to implementation (for trivial work)
```
1. Use "Specify Requirements" to document the need
2. Use "Create Plan" and mark as trivial (single step)
3. Use "Create Tasks" to create one atomic task
4. Use "Design QA Strategy" for minimal coverage
5. Use "Implement Solution" to execute
6. Use "Acceptance Testing" to verify
```

### Example 3: Agent invocation
```text
"Use the Specify Requirements skill to capture this feature request:
Add a dark mode toggle to the settings page..."
```

---

## Stage Dependencies

```
Stage 1: Specify Requirements
    ↓
Stage 2: Create Plan ← requires Stage 1
    ↓
Stage 3: Create Tasks ← requires Stages 1-2
    ↓
Stage 4: Design QA Strategy ← requires Stages 1-3
    ↓
Stage 5: Implement Solution ← requires Stages 1-4
    ↓
Stage 6: Acceptance Testing ← requires Stages 1-5
```

---

## Key Principles

1. **Sequential Flow**: Each stage builds on previous stages' outputs
2. **Single Source of Truth**: `handoff/payload.json` is the canonical state
3. **Traceability**: Every change links back to requirements
4. **Evidence-Based**: Decisions backed by searchable code/doc references
5. **Autonomous Execution**: Stage 5 runs without mid-stream approvals
6. **No Backward Compatibility**: Commander Doctrine excludes legacy concerns

---

## Common Patterns

### Full Lifecycle
Use all 6 stages for substantial features with team handoffs.

### Solo Developer Fast Path
Stages 1-2-3-4-5-6 in one session, minimal payload detail.

### Planning Only
Stages 1-2-3-4 to scope work, hand off to external team for stage 5.

### Implementation + Verification
Start at Stage 5 with existing payload, finish with Stage 6.

---

## Troubleshooting

**"Payload missing prerequisites"**  
→ Run the prerequisite stage(s) first

**"Not sure which stage to use"**  
→ Start with Stage 1 if you have raw requirements  
→ Start with Stage 2 if requirements are already documented  
→ Start with Stage 5 if plan/tasks/QA already exist

**"Want to modify existing work"**  
→ Update the relevant payload section, then rerun impacted stages

---

## Learn More

- Read `CONVERSION_SUMMARY.md` for migration details
- Check `handoff/README.md` for payload schema
- Review `commands/README.md` for the original workflow guide
