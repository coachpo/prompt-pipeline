# Examples

## Example: Acceptance report

**Input**
Use `running-acceptance-tests` for the pagination implementation in the handoff payload.

**Output**
Records an approval decision with evidence:

```json
{
  "qaFindings": {
    "status": "rejected",
    "validatedAt": "YYYY-MM-DDTHH:MM:SSZ",
    "validatedBy": "qa-team",
    "evidence": [
      {
        "id": "evidence-1",
        "type": "test-run",
        "description": "All unit tests passed",
        "timestamp": "YYYY-MM-DDTHH:MM:SSZ"
      }
    ],
    "issues": [
      {
        "id": "issue-1",
        "severity": "medium",
        "title": "limit=0 should return validation error",
        "expectedBehavior": "Return 400 with error message",
        "actualBehavior": "Returns 200 with empty array",
        "reproductionSteps": [
          "GET /api/users?limit=0",
          "Observe: status 200, body []"
        ]
      }
    ]
  }
}
```
