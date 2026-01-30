# Examples

## Example: Pagination requirement

**User request**
"Add pagination to the user list API. It is too slow with 10,000 users."

**Assistant outcome**
Updates `handoff/payload.json` with a requirement section:

```json
{
  "meta": {
    "currentStage": "1-specifying-requirements",
    "lastUpdatedBy": "specifying-requirements-skill",
    "lastUpdatedAt": "YYYY-MM-DDTHH:MM:SSZ"
  },
  "requirement": {
    "summary": "Add pagination to GET /api/users",
    "constraints": [
      "Maintain backwards compatibility during rollout",
      "No breaking changes to response structure"
    ],
    "acceptanceCriteria": [
      "Supports ?limit and ?offset query parameters",
      "Returns X-Total-Count header with total records",
      "Response time < 200ms for any page",
      "Default limit is 100 if not specified"
    ],
    "openQuestions": [
      "Should cursor-based pagination be supported?",
      "Does the mobile app use this endpoint?"
    ]
  }
}
```
