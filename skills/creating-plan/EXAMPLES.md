# Examples

## Example: Pagination plan

**Input**
Use `creating-plan` with the pagination requirement in `handoff/payload.json`.

**Output**
Adds a plan section with ordered steps and risks:

```json
{
  "plan": {
    "steps": [
      {
        "id": "1",
        "name": "Define API contract",
        "owner": "backend-team",
        "dependencies": [],
        "files": ["routes/users.js", "docs/api-spec.md"],
        "status": "ready",
        "successSignal": "API spec reviewed and approved"
      },
      {
        "id": "2",
        "name": "Implement pagination",
        "owner": "backend-team",
        "dependencies": ["1"],
        "files": ["routes/users.js", "models/user.js"],
        "status": "ready",
        "successSignal": "Unit tests pass; response time < 200ms"
      }
    ],
    "risks": [
      {
        "description": "Frontend expects full user array",
        "impact": "Breaking change without coordination",
        "mitigation": "Version endpoint or add feature flag"
      }
    ]
  }
}
```
