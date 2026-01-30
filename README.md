# PromptPipeline

A production-ready, six-stage workflow system for software development powered by Claude Skills. Takes features from stakeholder requirements through acceptance testing with full traceability and evidence-based decision making.

## Repository Layout

- **`.agent/skills/`** – Six production-ready Claude Skills for the complete development workflow
- **`handoff/`** – Shared payload contract: schema documentation and living state file
- **`mcp/`** – Model Context Protocol governance and server registry for responsible tool selection
- **`commands.archived/`** – Legacy prompt files (archived, use skills instead)

## The Six Skills

### 1️⃣ [Specify Requirements](.agent/skills/specify-requirements/SKILL.md)
Converts stakeholder input into structured requirements with testable acceptance criteria.  
**Use when**: Starting features, clarifying vague requests, documenting APIs

### 2️⃣ [Create Plan](.agent/skills/create-plan/SKILL.md)
Translates requirements into evidence-backed execution plan with risk assessment.  
**Use when**: Non-trivial work needs sequencing, multiple teams need coordination

### 3️⃣ [Create Tasks](.agent/skills/create-tasks/SKILL.md)
Decomposes plan into atomic, assignable tasks with clear dependencies.  
**Use when**: Breaking down features into sprints, distributing work across team

### 4️⃣ [Design QA Strategy](.agent/skills/design-qa-strategy/SKILL.md)
Maps requirements to test scenarios with validation commands and fixtures.  
**Use when**: Before implementation, defining "done" criteria, establishing test gates

### 5️⃣ [Implement Solution](.agent/skills/implement-solution/SKILL.md)
Autonomously executes tasks, writes code and tests, captures validation evidence.  
**Use when**: Ready to code with approved plan, tasks, and test strategy

### 6️⃣ [Acceptance Testing](.agent/skills/acceptance-testing/SKILL.md)
Independently verifies work meets acceptance criteria with go/no-go decision.  
**Use when**: Before merging, deploying, or releasing to users

## Quick Start

### Using Skills in Claude

```
"Use specify-requirements skill for: Add dark mode toggle to settings page"
```

Claude will automatically:
- Search your codebase for affected files
- Document dependencies and constraints
- Create testable acceptance criteria
- Write structured payload to `handoff/payload.json`

Then proceed through remaining skills:
```
"Use create-plan with payload.json"
"Use create-tasks with payload.json"
"Use design-qa-strategy with payload.json"
"Use implement-solution with payload.json"
"Use acceptance-testing with payload.json"
```

### Full Feature Example

```bash
# Stage 1: Specify
"Use specify-requirements skill: Add pagination to user list API, 
currently loading 10k users is too slow"

# Stage 2: Plan  
"Use create-plan with payload.json"

# Stage 3: Tasks
"Use create-tasks with payload.json"

# Stage 4: QA Strategy
"Use design-qa-strategy with payload.json"

# Stage 5: Implement
"Use implement-solution with payload.json"

# Stage 6: Acceptance
"Use acceptance-testing with payload.json"
```

## How It Works

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
1. Validates required input sections exist
2. Executes its workflow
3. Updates its output section
4. Preserves all other sections
5. Updates metadata

## Documentation

- **[Skills Overview](.agent/skills/README.md)** - Complete guide to all 6 skills
- **[Quick Reference](.agent/skills/QUICK_REFERENCE.md)** - Cheat sheet with examples
- **[Payload Schema](handoff/README.md)** - JSON contract documentation
- **[MCP Rules](mcp/mcp_rules.md)** - Tool selection governance
- **[MCP Registry](mcp/mcp_registry.md)** - Available MCP servers

## Key Features

### 🎯 Evidence-Based
Every decision backed by codebase searches, documentation references, and test results with file+line citations.

### 🔗 Fully Traceable
Complete lineage: Requirements → Plan → Tasks → Tests → Code → Issues. Every change links back to original requirement.

### 🤖 Autonomous Execution
Stage 5 runs without approval loops - executes task queue, retries failures, captures all evidence.

### ✅ Defensible Decisions
Stage 6 provides audit trail with replayed validations, exploratory testing, and documented findings.

### 📊 Single Source of Truth
One JSON file contains entire project state. Searchable, versionable, auditable.

## Workflow Patterns

### Full Lifecycle (Substantial Features)
Use all 6 stages for complex work requiring team handoffs and detailed planning.

### Fast Track (Trivial Changes)
Stages 1-2 mark work as trivial with single-step plan, skip to implementation and verification.

### Planning Only (Architecture Review)
Run stages 1-4 to scope and design, hand off to external team for implementation.

### Implementation + Verification (Existing Plan)
Start at stage 5 with existing payload, finish with stage 6 acceptance.

## MCP Usage

Skills prefer MCP servers when available:
- **Desktop Commander** - File operations, searches, shell commands
- **DeepWiki/Context7** - Documentation and standards lookup
- **mcp-shrimp-task-manager** - Task backlog structuring

Consult `mcp/mcp_registry.md` before tool selection.  
Follow `mcp/mcp_rules.md` for governance.

## Migration from Legacy Commands

Legacy `commands/*.md` files have been archived to `commands.archived/`.  
All workflow logic now lives in `.agent/skills/*/SKILL.md` following industry best practices.

See [Conversion Summary](.agent/skills/CONVERSION_SUMMARY.md) for migration details.

## Contributing

### Updating Skills
1. Maintain YAML frontmatter format
2. Keep "When to Use This Skill" section current
3. Add real examples from production usage
4. Update error handling as edge cases discovered
5. Document breaking changes

### Extending Skills
Add optional subdirectories:
- `scripts/` - Helper automation
- `examples/` - Reference payloads
- `resources/` - Templates and fixtures

### Modifying Payload Schema
1. Update `handoff/README.md` with schema changes
2. Update affected skill instructions
3. Provide migration path in changelog

## License

License not specified. Check with repository owner before reuse or distribution.

---

**Built with ❤️ using [Claude Skills](https://github.com/ComposioHQ/awesome-claude-skills)**
