# PromptPipeline

A skill-based, six-stage workflow for software development. It turns stakeholder requests into requirements, plans, tasks, tests, implementation evidence, and acceptance decisions using a shared payload.

## Repository Layout

- `skills/` – Six production-ready workflow skills
- `handoff/` – Shared payload contract and living state file
- `mcp/` – MCP governance and server registry for tool selection

## The Six Skills

1. **Specify Requirements** – `skills/specify-requirements/SKILL.md`  
   Capture stakeholder intent as testable requirements.
2. **Create Plan** – `skills/create-plan/SKILL.md`  
   Sequence work with dependencies and risks.
3. **Create Tasks** – `skills/create-tasks/SKILL.md`  
   Break plan steps into atomic, assignable tasks.
4. **Design QA Strategy** – `skills/design-qa-strategy/SKILL.md`  
   Map requirements to tests, fixtures, and commands.
5. **Implement Solution** – `skills/implement-solution/SKILL.md`  
   Execute tasks, write code/tests, capture evidence.
6. **Acceptance Testing** – `skills/acceptance-testing/SKILL.md`  
   Independently verify outcomes and issue a go/no-go decision.

## Quick Start

```
"Use specify-requirements skill for: Add dark mode toggle to settings page"
"Use create-plan with payload.json"
"Use create-tasks with payload.json"
"Use design-qa-strategy with payload.json"
"Use implement-solution with payload.json"
"Use acceptance-testing with payload.json"
```

## Shared Payload

All stages share a single source of truth: `handoff/payload.json`

```json
{
  "meta": {
    "currentStage": "1-specify",
    "lastUpdatedBy": "specify-requirements-skill",
    "lastUpdatedAt": "2026-01-30T08:00:00Z"
  },
  "requirement": { /* Stage 1 output */ },
  "plan": { /* Stage 2 output */ },
  "tasks": { /* Stage 3 output */ },
  "testPlan": { /* Stage 4 output */ },
  "implementation": { /* Stage 5 output */ },
  "qaFindings": { /* Stage 6 output */ }
}
```

Each skill:
1. Validates required inputs
2. Executes its workflow
3. Updates its output section
4. Preserves all other sections
5. Updates metadata

## Documentation

- `skills/README.md` – Skills overview and layout
- `handoff/README.md` – Payload schema and usage
- `mcp/mcp_rules.md` – Tool selection governance
- `mcp/mcp_registry.md` – Available MCP servers

## Contributing

### Updating Skills
1. Maintain YAML frontmatter (`name`, `description`)
2. Keep inputs/outputs and constraints current
3. Add concise, real examples
4. Update error handling as issues are discovered

### Extending Skills
Use optional files only when needed and keep them concise:
- `EXAMPLES.md`, `REFERENCE.md`, `TROUBLESHOOTING.md`, `CHANGELOG.md`
- `scripts/` for deterministic helpers
- `resources/` for templates and fixtures

## License

License not specified. Check with repository owner before reuse or distribution.
