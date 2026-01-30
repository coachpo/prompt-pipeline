---
name: specifying-requirements
description: Convert stakeholder input into structured, testable requirements with evidence and a handoff payload.
---

# Specifying Requirements

Translate stakeholder requests into a clear requirement summary, constraints, acceptance criteria, and open questions in `handoff/payload.json`.

## When to use
- Starting a new feature or project
- Converting product specs into engineering requirements
- Clarifying vague requests before planning

## Inputs
- Required: stakeholder request (message, notes, or file path)
- Optional: existing `handoff/payload.json`

## Outputs
- Updated `handoff/payload.json` with `meta` and `requirement`
- All other payload sections preserved

## Checklist
- Restate the requirement in plain language and confirm scope.
- Search the codebase for impacted areas and capture file:line evidence.
- Define constraints and measurable acceptance criteria.
- Record open questions and assumptions with suggested owners.
- Write or update `handoff/payload.json`, backing up if it already exists.

## Instructions
1. Validate inputs and detect an existing payload.
2. Run targeted codebase searches per impacted domain.
3. Draft `requirement.summary`, `constraints`, `acceptanceCriteria`, `openQuestions`.
4. Update `meta.currentStage` and timestamps, preserve other sections.

## Examples
See `EXAMPLES.md` for concrete inputs and payload snippets.

## See also
- `REFERENCE.md` for rules, error handling, and tips.
