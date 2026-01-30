---
name: create-plan
description: Use after requirements are approved to decide if detailed planning is needed and create an evidence-backed execution plan. Breaks down what needs to happen, in what order, with clear owners and risk mitigation. Transforms "build feature X" into concrete, sequenced steps teams can execute.
---

# Create Plan

Translates approved requirements into an ordered, dependency-aware execution plan with evidence-backed steps, risk assessment, and validation artifacts.

## When to Use This Skill

- After requirements are documented and approved
- When work is non-trivial and needs sequencing
- Before breaking down into detailed tasks
- When multiple teams need coordination
- For features touching multiple systems
- To communicate technical approach to stakeholders
- Before committing to deadlines or estimates

## What This Skill Does

1. **Evaluates Complexity**: Decides if detailed planning is warranted (trivial vs. complex)
2. **Searches Codebase**: Maps blast radius, existing patterns, affected components
3. **Sequences Work**: Orders steps with clear dependencies and parallel streams
4. **Identifies Risks**: Calls out blockers, assumptions, unknowns with mitigations
5. **Defines Owners**: Assigns responsibility for each execution step
6. **Links Artifacts**: References diagrams, spikes, prototypes, standards
7. **Updates Payload**: Writes `plan.steps`, `plan.risks`, `plan.artifacts` to handoff/payload.json

## Inputs & Outputs

**Inputs**
- Required: `handoff/payload.json` with `requirement.summary` and `requirement.acceptanceCriteria`
- Optional: Existing `plan` section to refine

**Outputs**
- Updates `plan.steps`, `plan.risks`, `plan.artifacts`, and `meta.currentStage`
- Preserves all other payload sections

## How to Use

### Basic Usage

```
Use create-plan skill with the payload from specify-requirements
```

### From File

```
Use create-plan with the payload in handoff/payload.json
```

### Skip Planning for Trivial Work

```
Use create-plan to evaluate if detailed planning is needed for this one-line config change
```

## Example

**User**: "Use create-plan with the pagination requirement from payload.json"

**Claude Executes**:
1. Loads requirement: "Add pagination to GET /api/users endpoint"
2. Searches codebase for existing pagination patterns
3. Finds similar implementation in products API
4. Identifies components: backend route, database query, frontend updates
5. Maps dependencies: frontend depends on backend API contract
6. Flags risk: breaking change if not versioned properly

**Output**: Updated `handoff/payload.json`:
```json
{
  "plan": {
    "steps": [
      {
        "id": "1",
        "name": "Design API Contract",
        "owner": "backend-team",
        "dependencies": [],
        "files": ["routes/users.js", "docs/api-spec.md"],
        "status": "ready",
        "successSignal": "API spec reviewed and approved"
      },
      {
        "id": "2",
        "name": "Implement Backend Pagination",
        "owner": "backend-team",
        "dependencies": ["1"],
        "files": ["routes/users.js", "models/user.js"],
        "status": "ready",
        "successSignal": "Unit tests pass, response time < 200ms"
      },
      {
        "id": "3",
        "name": "Update Frontend to Use Pagination",
        "owner": "frontend-team",
        "dependencies": ["1"],
        "files": ["components/UserList.jsx", "api/users.js"],
        "status": "ready",
        "successSignal": "Loads all pages without errors"
      }
    ],
    "risks": [
      {
        "description": "Frontend currently expects full user array",
        "impact": "Breaking change without coordination",
        "mitigation": "Version API endpoint or use feature flag"
      }
    ],
    "artifacts": [
      {
        "type": "diagram",
        "location": "docs/pagination-flow.mmd",
        "description": "Sequence diagram showing request flow"
      }
    ]
  }
}
```

## Instructions for Claude

### Constraints & Rules

- Do not create a plan without validated requirements; request missing inputs
- Use evidence (file+line or docs) to justify plan steps and risks
- For non-trivial work, keep plans to 3-6 ordered steps with explicit dependencies
- Preserve requirement data; never overwrite earlier sections
- Keep plan entries concise; reference files instead of pasting long excerpts

### Security & Privacy

- Do not include secrets, API keys, or credentials in payloads or logs
- Redact PII or sensitive customer data from examples and evidence
- Avoid copying large proprietary code blocks; cite file paths and lines instead

When executing this skill:

1. **Load and validate** requirements from payload - block if missing
2. **Run triviality gate** - search repo to confirm work complexity
3. **For trivial work**: Document single step with justification, skip detailed planning
4. **For complex work**: Create 3-6 ordered steps covering design → implement → validate → rollout
5. **Search for prior art** - find similar features/patterns already in codebase
6. **Document every insight** with file+line references
7. **Make dependencies explicit** - mark sequential vs. parallel work
8. **Assign realistic owners** based on codebase structure
9. **Flag ALL risks** with specific mitigations
10. **Preserve requirement data** - never overwrite earlier sections

### Triviality Criteria

Mark work as **trivial** (single-step plan) if ALL are true:
- Changes ≤ 3 files
- No new external dependencies
- No database migrations
- No API contract changes
- No cross-team coordination
- Can be completed in < 1 day

### Error Handling

- **If requirements missing**: Stop, request specify-requirements first
- **If acceptance criteria vague**: Add specific constraints to plan.risks
- **If blast radius unclear**: Document uncertainty, recommend spike/prototype
- **If multiple valid approaches**: Present options in plan.artifacts, request decision

## Tips

- **Keep steps atomic** - each should have clear done criteria
- **Parallel when possible** - mark independent streams for faster execution
- **Reference real files** - `routes/users.js:45` beats "the users file"
- **Quantify risks** - "May take 2-3 days extra" beats "might be slow"
- **Link to evidence** - Every decision should cite codebase or docs

## Related Use Cases

- Technical design documents
- Engineering RFC (Request for Comments)
- Sprint planning preparation
- Architecture decision records
- Dependency mapping for migrations
