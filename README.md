# PromptPipeline

A skill-based, six-stage workflow for software development. It turns stakeholder requests into requirements, plans, tasks, tests, implementation evidence, and acceptance decisions using a shared payload.

## Repository Layout

- `skills/` – Six production-ready workflow skills
- `handoff/` – Shared payload contract and living state file

## The Six Skills

1. **Specifying Requirements** – `skills/specifying-requirements/SKILL.md`  
   Capture stakeholder intent as testable requirements.
2. **Creating a Plan** – `skills/creating-plan/SKILL.md`  
   Sequence work with dependencies and risks.
3. **Creating Tasks** – `skills/creating-tasks/SKILL.md`  
   Break plan steps into atomic, assignable tasks.
4. **Designing a QA Strategy** – `skills/designing-qa-strategy/SKILL.md`  
   Map requirements to tests, fixtures, and commands.
5. **Implementing a Solution** – `skills/implementing-solution/SKILL.md`  
   Execute tasks, write code/tests, capture evidence.
6. **Running Acceptance Tests** – `skills/running-acceptance-tests/SKILL.md`  
   Independently verify outcomes and issue a go/no-go decision.

## Quick Start

```
"Use specifying-requirements skill for: Add dark mode toggle to settings page"
"Use creating-plan with payload.json"
"Use creating-tasks with payload.json"
"Use designing-qa-strategy with payload.json"
"Use implementing-solution with payload.json"
"Use running-acceptance-tests with payload.json"
```

## Shared Payload

All stages share a single source of truth: `handoff/payload.json`

```json
{
  "meta": {
    "currentStage": "1-specifying-requirements",
    "lastUpdatedBy": "specifying-requirements-skill",
    "lastUpdatedAt": "YYYY-MM-DDTHH:MM:SSZ"
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
