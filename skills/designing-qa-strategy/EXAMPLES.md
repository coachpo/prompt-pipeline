# Examples

## Example: Pagination test plan

**Input**
Use `designing-qa-strategy` with pagination tasks in the handoff payload.

**Output**
Creates coverage matrix and scenarios:

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
        "scenarioIds": ["scenario-1", "scenario-2"]
      }
    ],
    "scenarios": [
      {
        "id": "scenario-1",
        "name": "Happy path: default pagination",
        "type": "integration",
        "steps": [
          "GET /api/users",
          "Verify: returns default page size",
          "Verify: X-Total-Count header present"
        ],
        "expectedOutput": {
          "statusCode": 200,
          "headers": ["X-Total-Count"]
        }
      }
    ]
  }
}
```
