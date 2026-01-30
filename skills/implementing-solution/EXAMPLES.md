# Examples

## Example: Record a task implementation

**Input**
Use `implementing-solution` to complete task-1.1.

**Output**
Updates `implementation` with evidence:

```json
{
  "implementation": {
    "changes": [
      {
        "id": "change-1",
        "taskId": "task-1.1",
        "files": ["routes/users.js"],
        "rationale": "Parse limit/offset and validate inputs",
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
        "duration": "0.2s",
        "evidenceRef": "command-1"
      }
    ],
    "commands": [
      {
        "id": "command-1",
        "command": "npm test -- routes/users.test.js",
        "timestamp": "YYYY-MM-DDTHH:MM:SSZ",
        "exitCode": 0,
        "duration": "2.3s",
        "output": "tests passed"
      }
    ]
  }
}
```
