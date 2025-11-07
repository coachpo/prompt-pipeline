# Commands Workflow Guide

This folder contains the role-specific prompts that humans run manually (on any machine) to move a backend task from requirement intake through acceptance. Each prompt expects the latest shared payload (`handoff/payload.json`) as its user input and writes its results back into that same file so downstream teammates never have to copy/paste text.

## How To Use
1. **Sync the payload** – pull or download the current `handoff/payload.json` plus its history (`handoff/README.md` explains the schema).
2. **Run your stage prompt** – open the relevant `commands/<n>-*.md` file in your agent/IDE, paste the payload JSON into the `$ARGUMENTS` block (along with any fresh context), and follow the instructions.
3. **Update the payload** – after finishing your stage, edit `handoff/payload.json` to reflect your outputs, set `meta.currentStage`, `meta.lastUpdatedBy`, `meta.lastUpdatedAt`, then share the updated file with the next role.
4. **Hand off** – notify the next owner that the payload has been updated; they repeat the same process with their command file.

## Stage Reference
| Stage | File | Purpose | Prereqs (payload keys) | Key Output Fields |
|-------|------|---------|------------------------|-------------------|
| 1 | `commands/1-specify.md` | Clarify raw requirements, capture constraints and acceptance criteria. Includes guardrail to archive any existing payload before reinitializing. | _(none)_ | `requirement.*`, seeded sections |
| 2 | `commands/2-plan.md` | Decide if planning is needed and record the ordered execution plan, risks, and artifacts. | `requirement.summary`, `requirement.acceptanceCriteria` | `plan.steps`, `plan.risks`, `plan.artifacts` |
| 3 | `commands/3-qa.md` | Design verification: coverage mapping, scenarios, fixtures, tooling. | `requirement.summary`, `plan.steps` | `testPlan.coverageMatrix`, `testPlan.scenarios`, `testPlan.fixtures` |
| 4 | `commands/4-implement.md` | Perform code + test changes per the plan and log validations. | `requirement.summary`, `plan.steps`, `testPlan.scenarios` | `implementation.changes`, `implementation.tests`, `implementation.commands` |
| 5 | `commands/5-acceptance.md` | QA sign-off: rerun suites, manual checks, record evidence/issues. | `implementation.changes` plus prior sections | `qaFindings.status`, `qaFindings.evidence`, `qaFindings.issues` |

Because each stage only depends on the payload, different people can run their prompts on different PCs at different times while maintaining a single source of truth. If you ever need to reset the workflow, follow the guardrail instructions in Stage 1 to archive and reinitialize `payload.json` before restarting.
