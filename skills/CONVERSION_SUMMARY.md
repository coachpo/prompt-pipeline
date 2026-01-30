# Prompt to SKILL Conversion Summary

This document explains the conversion of the workflow prompts from `commands/` into reusable SKILLs in `.agent/skills/`.

## What Changed

The 6 stage-specific prompt files in `commands/` have been converted into SKILL format and organized in `.agent/skills/`. Each SKILL maintains the exact same workflow logic, tooling expectations, and payload contracts as the original prompts, but now follows the standardized SKILL format with YAML frontmatter and structured markdown.

## File Mapping

| Original Prompt | New SKILL | Description |
|-----------------|-----------|-------------|
| `commands/1-specify.md` | `.agent/skills/specify-requirements/SKILL.md` | Convert stakeholder input into actionable requirement payload |
| `commands/2-plan.md` | `.agent/skills/create-plan/SKILL.md` | Translate requirements into evidence-backed execution plan |
| `commands/3-tasks.md` | `.agent/skills/create-tasks/SKILL.md` | Decompose plan into hierarchical, traceable task backlog |
| `commands/4-qa.md` | `.agent/skills/design-qa-strategy/SKILL.md` | Create traceable test coverage and validation approach |
| `commands/5-implement.md` | `.agent/skills/implement-solution/SKILL.md` | Execute plan autonomously with full traceability |
| `commands/6-acceptance.md` | `.agent/skills/acceptance-testing/SKILL.md` | Independent verification with go/no-go decision |

## SKILL Format Structure

Each SKILL.md file follows this structure:

```markdown
---
name: [Human-Readable Skill Name]
description: [One-line description of what the skill does]
---

## Stage Overview
[High-level purpose and goals]

## Commander Doctrine
[Backward compatibility and architectural constraints]

## Shared Payload Contract
[File location, prerequisites, required updates, guardrails]

## Tooling & Evidence Expectations
[MCP servers, commands, and evidence requirements]

## Workflow
[Step-by-step execution instructions]

## Outputs & Handoff
[Deliverables and handoff criteria]

## Required User Input
[Format and expectations for $ARGUMENTS]
```

## Key Benefits of SKILL Format

1. **Discoverability**: Skills are automatically listed in the agent interface
2. **Reusability**: Skills can be invoked across different projects and contexts
3. **Consistency**: Standardized YAML frontmatter makes skills machine-readable
4. **Modularity**: Each skill is self-contained in its own directory
5. **Extensibility**: Skills can include additional resources (scripts/, examples/, resources/)

## Using the SKILLs

To use a SKILL, the agent will:
1. Read the SKILL.md file using the `view_file` tool
2. Follow the instructions exactly as documented
3. Accept `$ARGUMENTS` as defined in the "Required User Input" section
4. Update the `handoff/payload.json` according to the Shared Payload Contract

## Backward Compatibility

The original `commands/` directory remains unchanged. Teams can:
- Continue using the old `commands/*.md` files manually
- Transition to using the agent-invoked SKILLs at `.agent/skills/*/SKILL.md`
- Use both approaches in parallel during migration

## Next Steps

You may want to:
1. Archive the `commands/` directory once you've fully transitioned to SKILLs
2. Add example payloads in each SKILL's directory under `examples/`
3. Add helper scripts in each SKILL's directory under `scripts/`
4. Update project documentation to reference the new SKILL locations
5. Train team members on how to invoke SKILLs through the agent interface

## Maintenance

When updating workflow logic:
- Update the corresponding SKILL.md file in `.agent/skills/*/SKILL.md`
- Keep the `handoff/` payload schema in sync
- Document any breaking changes in the SKILL's description or workflow sections
