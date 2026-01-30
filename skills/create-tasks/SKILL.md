---
name: create-tasks
description: Bridge strategic planning and execution by translating the approved plan into actionable, prioritized, and traceable development tasks. Decomposes high-level plan steps into atomic work items that can be assigned, tracked, and verified independently.
---

# Create Tasks

Breaks down plan steps into a hierarchical tree of atomic engineering tasks with clear owners, dependencies, acceptance criteria, and code-level traceability.

## When to Use This Skill

- After the execution plan is approved
- Before implementation begins
- When distributing work across team members
- For breaking down complex features into sprints
- To create assignable, trackable work items
- When you need granular progress visibility
- Before estimating effort or setting deadlines

## What This Skill Does

1. **Decomposes Plan Steps**: Breaks each plan step into parent → child → leaf task hierarchy
2. **Makes Tasks Atomic**: Ensures each leaf task is completable in one iteration/session
3. **Establishes Dependencies**: Maps task relationships and execution order
4. **Assigns Ownership**: Specifies who owns each task
5. **Links to Code**: References specific files, functions, tests for each task
6. **Defines Verification**: Creates acceptance checklist per task
7. **Tracks Status**: Labels tasks as ready/blocked/in-progress/completed
8. **Updates Payload**: Writes `tasks.items`, `tasks.dependencies` to handoff/payload.json

## Inputs & Outputs

**Inputs**
- Required: `handoff/payload.json` with `plan.steps` (and relevant requirements)
- Optional: Existing `tasks` section to refine or expand

**Outputs**
- Updates `tasks.items`, `tasks.dependencies`, and `meta.currentStage`
- Preserves all other payload sections

## How to Use

### Basic Usage

```
Use create-tasks skill with the plan from payload.json
```

### From Updated Plan

```
The plan in handoff/payload.json was just updated. 
Use create-tasks to generate the backlog.
```

### Review Existing Tasks

```
Use create-tasks to review and update the existing task breakdown
```

## Example

**User**: "Use create-tasks with the pagination plan from payload.json"

**Claude Executes**:
1. Loads plan step: "Implement Backend Pagination"
2. Searches `routes/users.js` and `models/user.js`
3. Identifies sub-tasks:
   - Add limit/offset parameters to route
   - Update database query with LIMIT/OFFSET
   - Add X-Total-Count header to response
   - Write unit tests
   - Update API documentation
4. Creates parent-child hierarchy
5. Links each task to specific files and line ranges
6. Marks dependencies (tests depend on implementation)

**Output**: Updated `handoff/payload.json`:
```json
{
  "tasks": {
    "items": [
      {
        "id": "task-1",
        "parentId": null,
        "name": "Implement Backend Pagination",
        "description": "Add pagination support to GET /api/users",
        "owner": "backend-team",
        "status": "ready",
        "planStepId": "2",
        "relatedFiles": [
          {
            "path": "routes/users.js",
            "type": "TO_MODIFY",
            "description": "Add limit/offset query parameter handling",
            "lineStart": 45,
            "lineEnd": 67
          }
        ]
      },
      {
        "id": "task-1.1",
        "parentId": "task-1",
        "name": "Add route parameters for limit and offset",
        "description": "Parse and validate limit/offset from query string",
        "owner": "backend-team",
        "status": "ready",
        "acceptanceCriteria": [
          "Accepts ?limit and ?offset query parameters",
          "Defaults: limit=100, offset=0",
          "Validates limit ≤ 500",
          "Returns 400 for invalid parameters"
        ],
        "relatedFiles": [
          {
            "path": "routes/users.js",
            "type": "TO_MODIFY",
            "lineStart": 48,
            "lineEnd": 52
          }
        ]
      },
      {
        "id": "task-1.2",
        "parentId": "task-1",
        "name": "Update database query with pagination",
        "description": "Add LIMIT and OFFSET to SQL query",
        "owner": "backend-team",
        "status": "ready",
        "acceptanceCriteria": [
          "Query uses LIMIT and OFFSET",
          "Maintains existing sort order",
          "Performs index scan, not full table scan",
          "Query execution < 100ms"
        ],
        "relatedFiles": [
          {
            "path": "models/user.js",
            "type": "TO_MODIFY",
            "description": "findManyWithPagination method",
            "lineStart": 120,
            "lineEnd": 135
          }
        ]
      }
    ],
    "dependencies": [
      {
        "taskId": "task-1.3",
        "dependsOn": ["task-1.1", "task-1.2"],
        "reason": "Tests require implementation completion"
      }
    ],
    "statusSummary": {
      "ready": 5,
      "blocked": 0,
      "in_progress": 0,
      "completed": 0
    }
  }
}
```

## Instructions for Claude

### Constraints & Rules

- Do not create tasks without an approved plan; request missing inputs
- Every task must map to a plan step and be independently verifiable
- Keep leaf tasks atomic and time-bounded; avoid multi-day blobs
- Include concrete file or component references for each task
- Preserve requirement and plan data; never overwrite earlier sections

### Security & Privacy

- Do not include secrets, API keys, or credentials in payloads or logs
- Redact PII or sensitive customer data from examples and evidence
- Avoid copying large proprietary code blocks; cite file paths and lines instead

When executing this skill:

1. **Validate prerequisites** - ensure requirements + plan exist in payload
2. **For each plan step**, create a parent task that mirrors it
3. **Decompose to atomic leaves** - split until each task is:
   - Completable in 1-2 days (8-16 hours)
   - Changes 1-3 files typically
   - Has clear pass/fail criteria
   - Can be coded, reviewed, and tested independently
4. **Search codebase** for every task to find exact files + line ranges
5. **Document dependencies** - mark sequential vs. parallel work
6. **Create traceability** - link tasks back to plan steps and requirements
7. **Define verification** - what makes this task "done"?
8. **Use MCP task manager** if available for backlog structuring
9. **Preserve all payload sections** - never overwrite requirements or plan

### Task Granularity Rules

Mark a task as **ready to implement** if:
- ✅ Acceptance criteria are testable
- ✅ Files to modify are identified
- ✅ Dependencies are documented
- ✅ Effort estimate is < 16 hours of work

Mark a task as **needs decomposition** if:
- ❌ Spans multiple technical domains (frontend + backend + DB)
- ❌ Estimated > 2 days
- ❌ Acceptance criteria are vague
- ❌ "And" appears multiple times in description

### Error Handling

- **If plan is missing**: Stop, request create-plan first
- **If file not found in codebase**: Mark task as blocked, document missing file
- **If uncertain about decomposition depth**: Create fewer tasks initially, can split later
- **If owner is unclear**: Mark as "unassigned", recommend assignment based on file ownership

## Tips

- **One clear goal per leaf task** - "Add pagination" is better decomposed into 3-4 tasks
- **Reference prior art** - "Use same pattern as products.js:78"
- **Include test tasks** - Don't just code tasks, include test writing
- **Mark quick wins** - Tag easy tasks for new contributors
- **Estimate conservatively** - 1 day of complexity = 2 days with reviews/testing

## Related Use Cases

- Agile story breakdown
- Sprint planning
- Work distribution across team
- Progress tracking dashboards
- Estimating project timelines
- Onboarding task lists
