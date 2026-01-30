---
name: specify-requirements
description: Use when starting a new feature or project to capture stakeholder needs and convert them into structured, evidence-backed requirements with clear acceptance criteria. Transforms vague requests like "we need better performance" into testable goals and searchable codebase evidence.
---

# Specify Requirements

Converts raw stakeholder input (emails, meeting notes, verbal requests) into a structured requirement payload that planning, QA, and development teams can execute against with zero ambiguity.

## When to Use This Skill

- Starting a new feature or project from stakeholder description
- Converting product specs into engineering requirements
- Documenting API changes or system integrations
- Clarifying vague requests with concrete acceptance criteria
- Creating a shared understanding before planning work
- Replacing verbal agreements with written, searchable records
- Kicking off any work that requires cross-team handoff

## What This Skill Does

1. **Restates Requirements**: Echoes back what you heard to confirm understanding
2. **Searches Codebase**: Finds affected files, modules, APIs with file+line citations
3. **Maps Dependencies**: Documents frameworks, integrations, compliance needs
4. **Researches Context**: Pulls official docs, standards, and prior art
5. **Identifies Risks**: Calls out blockers, assumptions, and unknowns
6. **Creates Acceptance Criteria**: Defines testable success metrics
7. **Produces JSON Payload**: Writes structured `handoff/payload.json` for downstream stages

## Inputs & Outputs

**Inputs**
- Required: Stakeholder request or requirements source (message, notes, or file path)
- Optional: Existing `handoff/payload.json` to update (preserve non-requirement sections)

**Outputs**
- Creates or updates `handoff/payload.json` with `meta` and `requirement` sections
- Preserves any existing `plan`, `tasks`, `testPlan`, `implementation`, and `qaFindings` data
- Creates a timestamped backup when an existing payload is found

## How to Use

### Basic Usage

```
Use the specify-requirements skill with this request:
"Add dark mode toggle to the settings page with system preference detection"
```

### From Meeting Notes

```
Use specify-requirements to document this feature discussed in today's standup:
- Users want to export reports as PDF
- Must include charts and tables
- Should match the on-screen formatting
```

### With Existing Context

```
I have requirements from the product team in requirements.txt. 
Use specify-requirements to create the payload.
```

## Example

**User**: "Use specify-requirements skill for this: We need to add pagination to the user list API. It's loading too slowly with 10,000 users."

**Claude Executes**:
1. Searches codebase for user list API implementations
2. Finds `GET /api/users` in `routes/users.js:45-67`
3. Checks for existing pagination patterns in codebase
4. Discovers limit/offset pattern used in products API
5. Identifies constraint: frontend currently expects full array
6. Creates acceptance criteria:
   - API returns max 100 users per request
   - Supports offset and limit parameters
   - Returns total count in response header
   - Response time < 200ms for any page

**Output**: `handoff/payload.json` with:
```json
{
  "meta": {
    "currentStage": "1-specify",
    "lastUpdatedAt": "2026-01-30T08:15:00Z"
  },
  "requirement": {
    "summary": "Add pagination to GET /api/users endpoint",
    "constraints": [
      "Must maintain backwards compatibility during rollout",
      "Frontend migration required in parallel",
      "No breaking changes to response structure"
    ],
    "acceptanceCriteria": [
      "API supports ?limit and ?offset query parameters",
      "Returns X-Total-Count header with total records",
      "Response time < 200ms for any page size",
      "Default limit is 100 if not specified"
    ],
    "openQuestions": [
      "Should we support cursor-based pagination for better performance?",
      "Does mobile app use this endpoint?"
    ]
  }
}
```

## Instructions for Claude

### Constraints & Rules

- Do not proceed without clarifying ambiguous requests; ask questions first
- Never overwrite an existing payload without backing it up and confirming
- Use evidence for assumptions (file+line or authoritative docs)
- Keep acceptance criteria measurable and testable
- Keep payload entries concise; prefer references over long excerpts

### Security & Privacy

- Do not include secrets, API keys, or credentials in payloads or logs
- Redact PII or sensitive customer data from examples and evidence
- Avoid copying large proprietary code blocks; cite file paths and lines instead

When executing this skill:

1. **Always start by confirming** the requirement in plain language before searching code
2. **Run at least one codebase search** per impacted domain (backend API, frontend, database, etc.)
3. **Record every search** with file paths and line numbers in working notes
4. **Archive existing payload** if one exists - never overwrite without warning
5. **Ask clarifying questions** if the request is ambiguous - don't guess
6. **Flag ALL assumptions** as open questions with suggested owners
7. **Make acceptance criteria testable** - avoid "better performance", use specific metrics
8. **Check `mcp/mcp_registry.md`** before choosing search tools

### Error Handling

- **If payload.json already exists**: Warn user, create `payload-{timestamp}.json` backup, confirm before proceeding
- **If codebase search returns 0 results**: Flag as open question, document search attempted
- **If dependencies are unclear**: Mark as blocked, request stakeholder clarification
- **If conflicting constraints found**: Document conflict in `openQuestions`, propose resolution options

## Tips

- **Be specific with acceptance criteria** - "Page loads in <500ms" beats "faster loading"
- **Include negative cases** - "Handles 404 when user not found"  
- **Document the "why"** - Future maintainers need context
- **Link to prior art** - Reference similar features already implemented
- **Save time later** - 30 minutes here saves hours in rework

## Related Use Cases

- Creating RFC (Request for Comments) documents
- Onboarding new team members with requirements context
- API design documentation
- Technical specification writing
- Feature flag planning
