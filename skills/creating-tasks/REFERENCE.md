# Reference

## Constraints and rules
- Do not create tasks without an approved plan.
- Every task must map to a plan step and be independently verifiable.
- Keep leaf tasks time-bounded (1-2 days) and scoped to 1-3 files.
- Include concrete file or component references for each task.
- Preserve all existing payload sections.

## Task granularity rules
A leaf task is ready when it has:
- Testable acceptance criteria
- Clear inputs/outputs
- Specific files or components
- A single owner

## Security and privacy
- Do not include secrets, API keys, or credentials in payloads or logs.
- Redact PII or sensitive data in examples and evidence.

## Error handling
- If plan is missing: request `creating-plan` first.
- If tasks are too large: split further before marking ready.
- If file references are unclear: document as a dependency or open question.

## Tips
- Use consistent task IDs and naming.
- Mark sequential vs parallel tasks explicitly.
- Keep descriptions short and outcome-focused.
