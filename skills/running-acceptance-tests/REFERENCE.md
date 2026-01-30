# Reference

## Constraints and rules
- Do not modify implementation code during acceptance testing.
- Re-run all commands from `implementation.commands` and `testPlan.validationCommands`.
- Mark each acceptance criterion with objective evidence.
- Record reproducible steps for every issue.
- Preserve requirement, plan, task, test, and implementation data.

## Evidence requirements
- Include command outputs, exit codes, and timestamps.
- Capture exploratory test notes and inputs.
- Link issues to affected files and task IDs when possible.

## Security and privacy
- Do not include secrets, API keys, or credentials in evidence.
- Redact PII or sensitive customer data from findings.

## Error handling
- If implementation section is missing or incomplete: stop and request re-run of implementation.
- If replayed tests fail: mark status as rejected and capture evidence.
- If exploratory tests reveal issues: document and reject until fixed.

## Tips
- Focus on edge cases and negative paths.
- Compare results against acceptance criteria, not assumptions.
- Keep evidence concise and traceable.
