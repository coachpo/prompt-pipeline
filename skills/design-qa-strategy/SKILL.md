---
name: design-qa-strategy
description: Convert the shared requirement + plan + task backlog into a concrete, traceable test strategy for implementers. Maps every requirement, plan step, and task to specific test scenarios with fixtures, data, and validation commands.
---

# Design QA Strategy

Creates comprehensive test coverage by mapping acceptance criteria, plan steps, and tasks to specific test scenarios with clear validation approaches and reusable fixtures.

## When to Use This Skill

- After tasks are defined and before implementation
- When defining "done" criteria for features
- To ensure nothing falls through testing gaps
- Before writing any production code
- When planning test data and fixtures
- To align QA and development on validation approach
- For establishing automated CI/CD test gates

## What This Skill Does

1. **Maps Coverage**: Links requirements → plan steps → tasks → test scenarios
2. **Selects Test Levels**: Chooses unit, integration, E2E, exploratory for each behavior
3. **Defines Scenarios**: Creates test cases with inputs, expected outputs, edge cases
4. **Identifies Fixtures**: Documents test data, mocks, seeds needed
5. **Specifies Commands**: Provides exact commands to run each test suite
6. **Tracks Gaps**: Flags untested behaviors or missing prerequisites
7. **Updates Payload**: Writes `testPlan.coverageMatrix`, `testPlan.scenarios` to handoff/payload.json

## Inputs & Outputs

**Inputs**
- Required: `handoff/payload.json` with `requirement`, `plan`, and `tasks` sections
- Optional: Existing `testPlan` section to refine

**Outputs**
- Updates `testPlan.coverageMatrix` and `testPlan.scenarios` (plus fixtures or commands if needed)
- Preserves all other payload sections

## How to Use

### Basic Usage

```
Use design-qa-strategy with the completed tasks from payload.json
```

### Review Existing Strategy

```
The tasks changed. Use design-qa-strategy to update test coverage.
```

### For Specific Component

```
Use design-qa-strategy to create test plan for the pagination feature
```

## Example

**User**: "Use design-qa-strategy with the pagination tasks from payload.json"

**Claude Executes**:
1. Reviews requirement: "Response time < 200ms for any page"
2. Reviews plan step: "Implement Backend Pagination"
3. Reviews tasks: parse parameters, update query, add header
4. Identifies test levels needed:
   - Unit: parameter parsing, query building
   - Integration: full endpoint with database
   - Performance: response time validation
5. Creates scenarios for happy path, edge cases, errors
6. Documents fixtures: seed 10,000 users for testing

**Output**: Updated `handoff/payload.json`:
```json
{
  "testPlan": {
    "coverageMatrix": [
      {
        "requirementId": "req-1",
        "planStepId": "2",
        "taskId": "task-1.1",
        "behavior": "Accept limit and offset parameters",
        "testLevel": "unit",
        "scenarioIds": ["scenario-1", "scenario-2", "scenario-3"]
      }
    ],
    "scenarios": [
      {
        "id": "scenario-1",
        "name": "Happy path: Paginate with default parameters",
        "type": "integration",
        "status": "ready",
        "steps": [
          "GET /api/users (no parameters)",
          "Verify: Returns 100 users",
          "Verify: X-Total-Count header present",
          "Verify: Response time < 200ms"
        ],
        "expectedOutput": {
          "statusCode": 200,
          "userCount": 100,
          "headers": ["X-Total-Count"],
          "responseTime": "<200ms"
        },
        "fixtureIds": ["seed-10k-users"]
      },
      {
        "id": "scenario-2",
        "name": "Edge case: Request beyond total count",
        "type": "integration",
        "status": "ready",
        "steps": [
          "GET /api/users?offset=15000&limit=100",
          "Verify: Returns empty array",
          "Verify: X-Total-Count still shows 10000",
          "Verify: Status 200 (not 404)"
        ],
        "expectedOutput": {
          "statusCode": 200,
          "userCount": 0,
          "totalCount": 10000
        },
        "fixtureIds": ["seed-10k-users"]
      },
      {
        "id": "scenario-3",
        "name": "Error: Invalid limit parameter",
        "type": "unit",
        "status": "ready",
        "steps": [
          "GET /api/users?limit=abc",
          "Verify: Returns 400 Bad Request",
          "Verify: Error message explains invalid limit"
        ],
        "expectedOutput": {
          "statusCode": 400,
          "error": "limit must be a positive integer"
        }
      }
    ],
    "fixtures": [
      {
        "id": "seed-10k-users",
        "name": "10,000 test users",
        "location": "test/fixtures/users-10k.sql",
        "description": "Seed database with realistic user data",
        "setupCommand": "npm run seed:test -- --fixture=users-10k"
      }
    ],
    "validationCommands": [
      {
        "suite": "pagination-unit",
        "command": "npm test -- routes/users.test.js",
        "environment": "test",
        "expectedDuration": "5s"
      },
      {
        "suite": "pagination-integration",
        "command": "npm run test:integration -- users-pagination",
        "environment": "test",
        "prerequisites": ["seed-10k-users"],
        "expectedDuration": "30s"
      }
    ]
  }
}
```

## Instructions for Claude

### Constraints & Rules

- Do not design QA without requirements, plan, and tasks; request missing inputs
- Map every acceptance criterion to at least one test scenario
- Include negative and edge-case coverage, not just happy paths
- Provide exact validation commands when possible
- Preserve requirement, plan, and task data; never overwrite earlier sections

### Security & Privacy

- Do not include secrets, API keys, or credentials in payloads or fixtures
- Avoid real customer data in test fixtures; use synthetic or anonymized data
- Avoid copying large proprietary code blocks; cite file paths and lines instead

When executing this skill:

1. **Validate prerequisites** - block if requirements, plan, or tasks missing
2. **Enumerate all testable behaviors** from acceptance criteria
3. **For each behavior**, determine appropriate test level:
   - **Unit**: Function/method in isolation, fast (< 1s)
   - **Integration**: Multiple components, database/API, medium speed (< 30s)
   - **Contract**: API contracts match spec, medium speed
   - **E2E**: Full user workflow, slow (< 5min)
   - **Exploratory**: Manual testing, variable time
4. **Search for existing tests** - reuse patterns and fixtures
5. **Create scenario coverage** - happy path, edge cases, error cases
6. **Document fixtures needed** - mock data, seeds, test doubles
7. **Specify validation commands** - exact commands to run tests
8. **Mark gaps** - flag untestable or blocked scenarios
9. **Update task status** - mark tasks blocked if fixtures don't exist

### Coverage Checklist

Ensure coverage for:
- ✅ Happy path (expected normal usage)
- ✅ Edge cases (boundary values, limits, empty states)
- ✅ Error cases (invalid input, missing data, permissions)
- ✅ Performance requirements (response time, throughput)
- ✅ Security requirements (auth, input validation)
- ✅ Integration points (external APIs, database, services)

### Test Level Selection Guide

Choose **unit tests** when:
- Testing pure functions or business logic
- No external dependencies needed
- Fast feedback is critical

Choose **integration tests** when:
- Testing database queries
- Testing API endpoints
- Multiple components interact

Choose **E2E tests** when:
- Testing critical user workflows
- Full system integration needed
- UI + backend + database involved

### Error Handling

- **If fixture missing**: Create fixture task, mark scenario as blocked
- **If acceptance criteria vague**: Add specific test cases to clarify
- **If test level unclear**: Start with integration, can refactor later
- **If coverage gap found**: Document in `requirement.openQuestions`

## Tips

- **Test the requirements** not just the code - "Response time < 200ms" becomes a test
- **Reuse fixtures** - One "seed-10k-users" fixture serves many scenarios
- **Make tests independent** - Each scenario should run standalone
- **Fail fast** - Front-load quick unit tests, slower E2E tests last
- **Document assumptions** - "Assumes PostgreSQL 14+" helps debug failures
- **Include negative tests** - What should fail? Test that too

## Related Use Cases

- Test plan documentation
- CI/CD pipeline configuration
- QA onboarding materials
- Test automation strategy
- Acceptance criteria verification
- Regression test suites
