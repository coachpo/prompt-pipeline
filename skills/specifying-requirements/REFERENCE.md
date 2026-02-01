# Reference

## Constraints and rules
- Do not proceed without clarifying ambiguous requests; ask questions first.
- Use codebase evidence (file:line) for assumptions and impacted areas.
- Keep acceptance criteria testable and measurable.
- Preserve existing payload sections; never overwrite them.
- If an existing payload is provided, preserve unknown fields and non-requirement sections.

## Security and privacy
- Do not include secrets, API keys, or credentials in payloads or logs.
- Redact PII or sensitive customer data in examples and evidence.

## Error handling
- If the provided payload is invalid JSON: request a corrected payload before proceeding.
- If codebase search returns no results: record the search and add to `openQuestions`.
- If dependencies are unclear: mark as open questions with suggested owners.
- If constraints conflict: document the conflict and propose resolution options.

## Tips
- Prefer specific metrics (e.g., "response time < 200ms") over vague goals.
- Include negative cases in acceptance criteria.
- Keep examples concrete; use placeholders for timestamps.
