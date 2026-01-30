---
name: acceptance-testing
description: Independently verify the delivered work using the supplied payload, evidence, and commands; approve or reject with full traceability. Replays validation steps, performs exploratory testing, and delivers a defensible go/no-go decision.
---

# Acceptance Testing

Independently verifies implementation quality by replaying tests, performing exploratory validation, and delivering a traceable approve/reject decision with complete evidence.

## When to Use This Skill

- After implementation is complete and tests are passing
- Before merging to main branch or deploying to production
- When QA needs to sign off on feature completion
- To validate that acceptance criteria are truly met
- For pre-release verification and regression testing
- When stakeholders need proof of completion
- Before closing tickets or marking work done

## What This Skill Does

1. **Reviews Evidence**: Inspects code changes, tests, and implementation logs
2. **Replays Validation**: Re-runs every test command to verify results
3. **Performs Exploratory Testing**: Manually tests edge cases and user scenarios
4. **Validates Criteria**: Checks each acceptance criterion against actual behavior
5. **Tests Negative Cases**: Tries to break the implementation
6. **Measures Performance**: Validates response times, resource usage, scale
7. **Generates Report**: Creates pass/fail decision with complete evidence trail
8. **Updates Payload**: Writes `qaFindings.status`, `qaFindings.evidence`, `qaFindings.issues`

## Inputs & Outputs

**Inputs**
- Required: `handoff/payload.json` with `implementation` results and `testPlan`
- Optional: Existing `qaFindings` section to update

**Outputs**
- Updates `qaFindings.status`, `qaFindings.evidence`, `qaFindings.issues`, and `qaFindings.validationLog`
- Updates task and scenario statuses based on validation results
- Preserves all other payload sections

## How to Use

### Basic Usage

```
Use acceptance-testing with the completed implementation in payload.json
```

### Focused Verification

```
Use acceptance-testing to verify just the pagination feature
```

### Re-run After Fixes

```
Implementation was updated. Use acceptance-testing to re-verify.
```

## Example

**User**: "Use acceptance-testing with the pagination implementation in payload.json"

**Claude Executes**:

1. **Evidence Review**
   - ✅ Reviewed: Changed routes/users.js, models/user.js
   - ✅ Reviewed: Added 8 unit tests, 4 integration tests
   - ✅ Reviewed: All tests passed in implementation phase

2. **Replay Validation**
```bash
$ npm test -- routes/users.test.js
✓ GET /api/users with default pagination (45ms)
✓ GET /api/users with custom limit (32ms)
✓ GET /api/users with offset (38ms)
✓ Returns 400 for invalid limit (12ms)

$ npm run test:integration -- users-pagination
✓ Pagination with 10k users performs < 200ms (156ms)
✓ Handles offset beyond total count (89ms)
```

3. **Exploratory Testing**
   - Tested: limit=0 → ❌ Returns empty array (expected 400 error)
   - Tested: limit=1000 → ✅ Correctly capped at 500
   - Tested: offset=9999 → ✅ Returns last user
   - Tested: Concurrent requests → ✅ No race conditions

4. **Criteria Validation**
   - ✅ API supports ?limit and ?offset query parameters
   - ✅ Returns X-Total-Count header with total records
   - ✅ Response time < 200ms for any page size (measured: avg 156ms)
   - ✅ Default limit is 100 if not specified
   - ⚠️ Issue found: limit=0 should return 400, currently returns []

**Output**: Updated `handoff/payload.json`:
```json
{
  "qaFindings": {
    "status": "rejected",
    "validatedAt": "2026-01-30T09:15:00Z",
    "validatedBy": "qa-team",
    "evidence": [
      {
        "id": "evidence-1",
        "type": "test-run",
        "description": "All unit and integration tests passed",
        "artifact": "logs/test-run-20260130.log",
        "timestamp": "2026-01-30T09:10:00Z"
      },
      {
        "id": "evidence-2",
        "type": "performance",
        "description": "Response time validation across 100 requests",
        "metrics": {
          "avgResponseTime": "156ms",
          "p95ResponseTime": "189ms",
          "p99ResponseTime": "198ms"
        }
      }
    ],
    "issues": [
      {
        "id": "issue-1",
        "severity": "medium",
        "title": "limit=0 should return validation error",
        "description": "GET /api/users?limit=0 returns empty array instead of 400 Bad Request",
        "expectedBehavior": "Return 400 with error message",
        "actualBehavior": "Returns 200 with empty array",
        "reproductionSteps": [
          "curl http://localhost:3000/api/users?limit=0",
          "Observe: Status 200, body: []",
          "Expected: Status 400, body: { error: 'limit must be positive' }"
        ],
        "affectedFiles": ["routes/users.js:5"],
        "owner": "backend-team",
        "taskId": "task-1.1"
      }
    ],
    "validationLog": [
      {
        "timestamp": "2026-01-30T09:08:00Z",
        "action": "Ran unit tests",
        "command": "npm test -- routes/users.test.js",
        "result": "passed",
        "duration": "2.1s"
      },
      {
        "timestamp": "2026-01-30T09:09:30Z",
        "action": "Ran integration tests",
        "command": "npm run test:integration -- users-pagination",
        "result": "passed",
        "duration": "4.7s"
      },
      {
        "timestamp": "2026-01-30T09:12:00Z",
        "action": "Exploratory: limit=0",
        "result": "failed",
        "notes": "Should return 400, returned 200"
      }
    ]
  }
}
```

## Instructions for Claude

### Constraints & Rules

- Do not modify implementation code during acceptance testing; report issues instead
- Re-run all validation commands and compare to implementation results
- Mark each acceptance criterion with objective evidence (pass/fail/partial)
- Record reproducible steps for every issue
- Preserve requirement, plan, task, test, and implementation data

### Security & Privacy

- Do not include secrets, API keys, or credentials in evidence or logs
- Redact PII or sensitive customer data from findings
- Avoid copying large proprietary code blocks; cite file paths and lines instead

When executing this skill:

1. **Validate prerequisites** - Block if implementation section is missing/incomplete
2. **Review implementation evidence**:
   - Read every file listed in `implementation.changes`
   - Verify changes align with task descriptions
   - Check that requirements are addressed
3. **Replay all validation commands**:
   - Run every command from `implementation.commands`
   - Run every command from `testPlan.validationCommands`
   - Compare results with implementation-phase results
   - Flag any new failures
4. **Perform exploratory testing**:
   - Test edge cases from acceptance criteria
   - Try to break the feature with invalid inputs
   - Test error handling and negative scenarios
   - Verify observability (logs, metrics, errors)
5. **Validate each acceptance criterion**:
   - Mark ✅ if objectively met with evidence
   - Mark ❌ if failed, create issue with reproduction steps
   - Mark ⚠️ if partially met, document limitations
6. **Update all related statuses**:
   - Set `tasks.items[*].status` based on findings
   - Set `testPlan.scenarios[*].status` based on results
   - Roll up parent task statuses
7. **Make go/no-go decision**:
   - `approved`: All acceptance criteria met, no blocking issues
   - `rejected`: Blocking issues found, needs remediation
   - `blocked`: Cannot validate due to missing prerequisites

### Acceptance Decision Criteria

**APPROVE** when:
- ✅ All acceptance criteria objectively met
- ✅ All automated tests passing
- ✅ No critical or high-severity issues
- ✅ Performance requirements met
- ✅ Error handling works as expected

**REJECT** when:
- ❌ Any acceptance criterion not met
- ❌ Critical or high-severity issues found
- ❌ Tests failing or flaky
- ❌ Performance degrades
- ❌ Breaking changes introduced

**BLOCK** when:
- ⛔ Cannot run tests (missing fixtures, environment issues)
- ⛔ Implementation incomplete
- ⛔ Acceptance criteria ambiguous

### Severity Guidelines

- **Critical**: Data loss, security vulnerability, complete feature breakdown
- **High**: Core functionality broken, poor user experience, workaround exists
- **Medium**: Edge case issues, minor UX problems, easy to fix
- **Low**: Cosmetic issues, nice-to-haves, documentation gaps

### Error Handling

- **If test fails that passed in implementation**: Mark as regression, create critical issue
- **If cannot reproduce implementation results**: Document in issues, request clarification
- **If new edge case discovered**: Test it, document as issue if broken
- **If acceptance criteria unclear**: Flag in `requirement.openQuestions`, request clarification

## Tips

- **Be thorough but fair** - Look for real issues, not nitpicks
- **Document everything** - Tomorrow you won't remember why you rejected
- **Test like a user** - Not just happy path
- **Measure objectively** - "Response time < 200ms" is testable, "feels fast" is not
- **Give credit** - If it works well, say so in evidence
- **Be specific with issues** - Reproduction steps should be copy-paste runnable

## Related Use Cases

- Pre-production release verification
- Pull request review automation
- Regression testing before deployment
- Stakeholder demo preparation
- Compliance audit trails
- Quality gate enforcement
