# Shared Payload Contract

All workflow skills exchange data through `handoff/payload.json`. This single JSON file serves as the source of truth for the entire development lifecycle.

## How It Works

1. **Stage 1 (Specify Requirements)** creates or refreshes `payload.json`
2. Each subsequent skill reads the payload, validates prerequisites, and updates its section
3. Skills preserve all other sections - never overwriting upstream work
4. The `meta` section tracks current stage and last update timestamp
5. The payload travels with the code through git, maintaining full history

## Usage with Skills

```bash
# Stage 1: Creates payload.json from stakeholder input
"Use specify-requirements skill for: [your requirement]"

# Stages 2-6: Read and update the payload
"Use create-plan with payload.json"
"Use create-tasks with payload.json"
"Use design-qa-strategy with payload.json"  
"Use implement-solution with payload.json"
"Use acceptance-testing with payload.json"
```

## Payload Schema

### Meta Section
```json
{
  "meta": {
    "currentStage": "1-specify",
    "lastUpdatedBy": "specify-requirements-skill",
    "lastUpdatedAt": "2026-01-30T08:00:00Z"
  }
}
```

**Updated by**: Every skill  
**Purpose**: Track workflow state and audit trail

---

### Requirement Section
```json
{
  "requirement": {
    "summary": "Brief description of what to build",
    "constraints": [
      "Framework limitations",
      "Compliance requirements",
      "Integration dependencies"
    ],
    "acceptanceCriteria": [
      "Testable criterion 1",
      "Testable criterion 2",
      "Performance requirement"
    ],
    "openQuestions": [
      {
        "question": "Unresolved item",
        "owner": "stakeholder-name",
        "severity": "blocking|important|nice-to-have"
      }
    ]
  }
}
```

**Owned by**: Stage 1 (Specify Requirements)  
**Required by**: All subsequent stages  
**Purpose**: Define what to build and how success is measured

---

### Plan Section
```json
{
  "plan": {
    "steps": [
      {
        "id": "1",
        "name": "Step name",
        "owner": "team-name",
        "dependencies": [],
        "files": ["path/to/file.js"],
        "status": "ready|in-progress|completed|blocked",
        "successSignal": "Observable outcome"
      }
    ],
    "risks": [
      {
        "description": "Risk description",
        "impact": "Potential consequence",
        "mitigation": "How to address"
      }
    ],
    "artifacts": [
      {
        "type": "diagram|spike|prototype",
        "location": "path/to/artifact",
        "description": "What it shows"
      }
    ]
  }
}
```

**Owned by**: Stage 2 (Create Plan)  
**Required by**: Stages 3-6  
**Purpose**: Ordered execution strategy with evidence

---

### Tasks Section
```json
{
  "tasks": {
    "items": [
      {
        "id": "task-1",
        "parentId": null,
        "name": "Task name",
        "description": "Detailed description",
        "owner": "engineer-name",
        "status": "ready|blocked|in-progress|completed",
        "planStepId": "1",
        "acceptanceCriteria": ["Criterion 1", "Criterion 2"],
        "relatedFiles": [
          {
            "path": "src/file.js",
            "type": "TO_MODIFY|REFERENCE|CREATE|DEPENDENCY",
            "description": "Why this file matters",
            "lineStart": 45,
            "lineEnd": 67
          }
        ]
      }
    ],
    "dependencies": [
      {
        "taskId": "task-2",
        "dependsOn": ["task-1"],
        "reason": "Why the dependency exists"
      }
    ],
    "statusSummary": {
      "ready": 5,
      "blocked": 0,
      "in_progress": 2,
      "completed": 3
    }
  }
}
```

**Owned by**: Stage 3 (Create Tasks)  
**Required by**: Stages 4-6  
**Purpose**: Atomic, assignable work items with traceability

---

### Test Plan Section
```json
{
  "testPlan": {
    "coverageMatrix": [
      {
        "requirementId": "req-1",
        "planStepId": "2",
        "taskId": "task-1.1",
        "behavior": "What behavior is tested",
        "testLevel": "unit|integration|e2e|exploratory",
        "scenarioIds": ["scenario-1"]
      }
    ],
    "scenarios": [
      {
        "id": "scenario-1",
        "name": "Scenario name",
        "type": "unit|integration|e2e",
        "status": "ready|blocked|passed|failed",
        "steps": ["Step 1", "Step 2"],
        "expectedOutput": {
          "statusCode": 200,
          "responseTime": "<200ms"
        },
        "fixtureIds": ["fixture-1"]
      }
    ],
    "fixtures": [
      {
        "id": "fixture-1",
        "name": "Fixture name",
        "location": "test/fixtures/data.json",
        "description": "What data it provides",
        "setupCommand": "npm run seed:test"
      }
    ],
    "validationCommands": [
      {
        "suite": "unit-tests",
        "command": "npm test",
        "environment": "test",
        "expectedDuration": "5s"
      }
    ]
  }
}
```

**Owned by**: Stage 4 (Design QA Strategy)  
**Required by**: Stages 5-6  
**Purpose**: Comprehensive test coverage mapping

---

### Implementation Section
```json
{
  "implementation": {
    "changes": [
      {
        "id": "change-1",
        "taskId": "task-1",
        "planStepId": "2",
        "requirementId": "req-1",
        "files": ["src/file.js"],
        "rationale": "Why this change was made",
        "linesChanged": 15,
        "evidenceRef": "command-1"
      }
    ],
    "tests": [
      {
        "id": "test-1",
        "taskId": "task-1",
        "suite": "routes/users.test.js",
        "type": "unit|integration|e2e",
        "status": "passed|failed",
        "coverage": {
          "statements": 95.5,
          "branches": 100,
          "functions": 100,
          "lines": 94.7
        },
        "duration": "127ms",
        "evidenceRef": "command-1"
      }
    ],
    "commands": [
      {
        "id": "command-1",
        "command": "npm test -- routes/users.test.js",
        "taskId": "task-1",
        "timestamp": "2026-01-30T08:45:23Z",
        "exitCode": 0,
        "duration": "2.3s",
        "output": "Test output summary"
      }
    ]
  }
}
```

**Owned by**: Stage 5 (Implement Solution)  
**Required by**: Stage 6  
**Purpose**: Complete implementation audit trail

---

### QA Findings Section
```json
{
  "qaFindings": {
    "status": "approved|rejected|blocked",
    "validatedAt": "2026-01-30T09:15:00Z",
    "validatedBy": "qa-engineer-name",
    "evidence": [
      {
        "id": "evidence-1",
        "type": "test-run|performance|screenshot",
        "description": "What was verified",
        "artifact": "path/to/evidence.log",
        "timestamp": "2026-01-30T09:10:00Z"
      }
    ],
    "issues": [
      {
        "id": "issue-1",
        "severity": "critical|high|medium|low",
        "title": "Issue summary",
        "description": "Detailed description",
        "expectedBehavior": "What should happen",
        "actualBehavior": "What actually happens",
        "reproductionSteps": ["Step 1", "Step 2"],
        "affectedFiles": ["src/file.js:45"],
        "owner": "engineer-name",
        "taskId": "task-1"
      }
    ],
    "validationLog": [
      {
        "timestamp": "2026-01-30T09:08:00Z",
        "action": "Ran unit tests",
        "command": "npm test",
        "result": "passed|failed|blocked",
        "duration": "2.1s"
      }
    ]
  }
}
```

**Owned by**: Stage 6 (Acceptance Testing)  
**Final output**: Go/no-go decision  
**Purpose**: Independent verification and release decision

---

## Validation Rules

Each skill must:
1. **Check prerequisites** - Verify required sections exist before proceeding
2. **Preserve other sections** - Never modify sections owned by other stages
3. **Update metadata** - Always set `meta.currentStage` and timestamp
4. **Validate on read** - Confirm schema matches expectations
5. **Handle missing data** - Block and request clarification, don't guess

## Archiving Payloads

When Stage 1 (Specify Requirements) finds existing payload data:
1. Warns user about overwrite
2. Archives as `payload-{ISO8601}.json`
3. Creates fresh payload from template
4. Maintains history for audit trail

## Example Lifecycle

```bash
# Initial state: No payload exists
$ cat handoff/payload.json
# File not found

# After Stage 1
$ cat handoff/payload.json
{
  "meta": {"currentStage": "1-specify", ...},
  "requirement": {...}
}

# After Stage 2
{
  "meta": {"currentStage": "2-plan", ...},
  "requirement": {...},
  "plan": {...}
}

# After Stage 6
{
  "meta": {"currentStage": "6-acceptance", ...},
  "requirement": {...},
  "plan": {...},
  "tasks": {...},
  "testPlan": {...},
  "implementation": {...},
  "qaFindings": {"status": "approved", ...}
}
```

## Best Practices

- **Commit frequently** - Payload is source-controlled, commit after each stage
- **Review diffs** - Use git diff to see what changed
- **Keep it readable** - Use consistent JSON formatting
- **Don't edit manually** - Let skills manage the structure
- **Archive old payloads** - Keep history when starting new features

## Skills Reference

See [Skills README](../skills/README.md) for usage details.
