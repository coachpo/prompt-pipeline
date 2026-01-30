# Examples

## Example: Pagination tasks

**Input**
Use `creating-tasks` with the pagination plan in `handoff/payload.json`.

**Output**
Creates parent and leaf tasks with dependencies:

```json
{
  "tasks": {
    "items": [
      {
        "id": "task-1",
        "parentId": null,
        "name": "Implement pagination",
        "owner": "backend-team",
        "status": "ready",
        "planStepId": "2",
        "relatedFiles": [
          { "path": "routes/users.js", "type": "TO_MODIFY" }
        ]
      },
      {
        "id": "task-1.1",
        "parentId": "task-1",
        "name": "Add limit/offset parsing",
        "owner": "backend-team",
        "status": "ready",
        "acceptanceCriteria": [
          "Accepts ?limit and ?offset",
          "Defaults to limit=100, offset=0",
          "Returns 400 for invalid values"
        ],
        "relatedFiles": [
          { "path": "routes/users.js", "type": "TO_MODIFY" }
        ]
      }
    ],
    "dependencies": [
      { "taskId": "task-1.2", "dependsOn": ["task-1.1"], "reason": "Tests require parsing" }
    ]
  }
}
```
