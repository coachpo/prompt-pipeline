---
name: implement-solution
description: Execute the approved plan, task backlog, and test strategy to deliver production-ready code plus proof of validation. Operates autonomously through the task list, implementing code, running tests, and capturing evidence for acceptance review.
---

# Implement Solution

Executes the complete implementation workflow autonomously: writes code, creates tests, runs validations, and documents everything with full traceability back to requirements.

## When to Use This Skill

- After QA strategy is approved and ready
- When all prerequisites (requirements, plan, tasks, tests) are documented
- To autonomously execute a defined backlog
- For implementing features with clear acceptance criteria
- When you need reproducible evidence of completion
- Before acceptance testing / QA review
- To deliver production-ready code with validation proof

## What This Skill Does

1. **Executes Task Queue**: Works through dependency-ordered leaf tasks
2. **Implements Code**: Writes production code following plan specifications
3. **Creates Tests**: Implements unit, integration, and E2E tests per QA strategy
4. **Runs Validations**: Executes test commands and captures outcomes
5. **Handles Failures**: Retries once, blocks on second failure, continues with next task
6. **Documents Changes**: Records every file edit with rationale and evidence
7. **Captures Proof**: Saves command outputs, logs, coverage reports
8. **Updates Payload**: Writes `implementation.changes`, `implementation.tests`, `implementation.commands`

## Inputs & Outputs

**Inputs**
- Required: `handoff/payload.json` with `requirement`, `plan`, `tasks`, and `testPlan`
- Optional: Existing `implementation` section to resume or append

**Outputs**
- Updates `implementation.changes`, `implementation.tests`, `implementation.commands`, and `meta.currentStage`
- Produces code changes plus referenced logs/artifacts
- Preserves all other payload sections

## How to Use

### Basic Usage

```
Use implement-solution with payload.json to execute all tasks
```

### Resume After Failure

```
Use implement-solution to resume from where we left off
```

### Implement Specific Task

```
Use implement-solution for task-1.2 only
```

## Example

**User**: "Use implement-solution with payload.json for the pagination feature"

**Claude Executes**:

**Task 1.1**: Add route parameters
```javascript
// routes/users.js (before)
app.get('/api/users', async (req, res) => {
  const users = await User.findAll();
  res.json(users);
});

// routes/users.js (after)
app.get('/api/users', async (req, res) => {
  const limit = Math.min(parseInt(req.query.limit) || 100, 500);
  const offset = parseInt(req.query.offset) || 0;
  
  if (limit < 1 || offset < 0) {
    return res.status(400).json({ 
      error: 'limit must be positive, offset must be non-negative' 
    });
  }
  
  const users = await User.findWithPagination(limit, offset);
  const total = await User.count();
  
  res.set('X-Total-Count', total.toString());
  res.json(users);
});
```

**Test**: Runs validation
```bash
$ npm test -- routes/users.test.js
✓ GET /api/users with default pagination (45ms)
✓ GET /api/users with custom limit (32ms)
✓ GET /api/users with offset (38ms)
✓ Returns 400 for invalid limit (12ms)

4 passing (127ms)
```

**Output**: Updated `handoff/payload.json`:
```json
{
  "implementation": {
    "changes": [
      {
        "id": "change-1",
        "taskId": "task-1.1",
        "planStepId": "2",
        "requirementId": "req-1",
        "files": ["routes/users.js"],
        "rationale": "Added limit/offset parameter parsing with validation per acceptance criteria",
        "linesChanged": 15,
        "evidenceRef": "command-1"
      }
    ],
    "tests": [
      {
        "id": "test-1",
        "taskId": "task-1.1",
        "suite": "routes/users.test.js",
        "type": "unit",
        "status": "passed",
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
        "taskId": "task-1.1",
        "timestamp": "2026-01-30T08:45:23Z",
        "exitCode": 0,
        "duration": "2.3s",
        "output": "4 passing (127ms)"
      }
    ]
  }
}
```

## Instructions for Claude

### Constraints & Rules

- Execute tasks in dependency order; do not skip unless blocked
- Implement only what is in scope for the approved plan and tasks
- Run the validation commands defined in the QA strategy and capture results
- If a task fails twice, mark it blocked and continue with remaining tasks
- Preserve requirement, plan, task, and test data; never overwrite earlier sections

### Security & Privacy

- Do not include secrets, API keys, or credentials in code, payloads, or logs
- Avoid running untrusted scripts or external binaries without explicit user approval
- Redact sensitive data in test outputs; prefer synthetic fixtures

When executing this skill:

1. **Validate prerequisites** - Block if requirements/plan/tasks/testPlan missing or stale
2. **Load task queue** - Order by dependencies, identify parallel vs sequential work
3. **For each leaf task**:
   - Echo task name, acceptance criteria, files to modify
   - Search codebase for current implementation
   - Implement code changes following plan guidance
   - Write or update tests per QA strategy
   - Run validation commands exactly as specified
   - Capture full output (stdout, stderr, exit code)
   - On success: mark task complete, move to next
   - On failure: retry once, then mark blocked and continue
4. **Update task status** in `tasks.items[*]` and payload
5. **Record MCP usage** - log every tool call with inputs/outputs
6. **Preserve evidence** - save command transcripts, logs, artifacts
7. **Check MCP rules** before every operation
8. **Never expand scope** - only implement what's in the task definition

### Autonomous Execution Rules

- ✅ **DO** auto-run commands marked safe in testPlan.validationCommands
- ✅ **DO** retry failed tests exactly once
- ✅ **DO** continue to next task if one blocks
- ✅ **DO** preserve all prior payload sections
- ❌ **DON'T** ask for approval mid-task (record decisions in payload instead)
- ❌ **DON'T** create new tasks not in backlog
- ❌ **DON'T** modify payload schema outside implementation section
- ❌ **DON'T** skip tests even if code "looks correct"

### Error Handling

- **If test fails (attempt 1)**: Retry with same command
- **If test fails (attempt 2)**: Mark task blocked, capture diagnostics, move to next task
- **If file not found**: Mark task blocked, document missing file in payload
- **If dependency not met**: Skip task, mark as blocked, continue with independent tasks
- **If scope unclear**: Log uncertainty in implementation.changes, proceed with best interpretation
- **If MCP tool unavailable**: Document failure, use fallback (per mcp/mcp_rules.md), note in payload

## Tips

- **Run tests early and often** - Don't wait until all code is written
- **Commit frequently** - Small, atomic commits easier to review
- **Document decisions** - "Chose HashMap over Array for O(1) lookup"
- **Capture metrics** - Response times, memory usage, coverage percentages
- **Link everything** - Every change, test, command linked to task & requirement
- **Leave breadcrumbs** - Future you (or QA) will thank you for detailed logs

## Related Use Cases

- Automated feature development
- Bug fix implementation with tests
- Technical debt reduction sprints
- Code generation from specifications
- CI/CD pipeline execution
- Proof-of-concept implementations
