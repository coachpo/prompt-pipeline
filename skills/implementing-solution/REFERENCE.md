# Reference

## Constraints and rules
- Execute tasks in dependency order; do not skip unless blocked.
- Implement only what is in scope for the approved plan and tasks.
- Run validation commands defined in `testPlan.validationCommands`.
- On failure: retry once, then mark blocked and continue.
- Preserve all prior payload sections.

## Evidence capture
- Record each change with affected files and rationale.
- Capture command outputs, exit codes, and durations.
- Link tests and changes to task IDs.

## Security and privacy
- Do not include secrets, API keys, or credentials in code, payloads, or logs.
- Avoid running untrusted scripts without explicit user approval.

## Error handling
- If prerequisites are missing: stop and request missing stages.
- If a task fails twice: mark it blocked and proceed.
- If commands are unsafe or require privileges: request approval before running.

## Tips
- Keep diffs focused to the task scope.
- Save logs in predictable locations if needed.
- Prefer existing patterns in the codebase.
