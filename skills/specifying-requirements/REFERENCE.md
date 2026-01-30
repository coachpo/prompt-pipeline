# Reference

## Constraints and rules
- Do not proceed without clarifying ambiguous requests; ask questions first.
- Use codebase evidence (file:line) for assumptions and impacted areas.
- Keep acceptance criteria testable and measurable.
- Preserve existing payload sections; never overwrite them.
- If `handoff/payload.json` exists, create a timestamped backup before updating.

## Security and privacy
- Do not include secrets, API keys, or credentials in payloads or logs.
- Redact PII or sensitive customer data in examples and evidence.

## Error handling
- If `handoff/payload.json` exists: warn, create backup, then proceed.
- If codebase search returns no results: record the search and add to `openQuestions`.
- If dependencies are unclear: mark as open questions with suggested owners.
- If constraints conflict: document the conflict and propose resolution options.

## Tips
- Prefer specific metrics (e.g., "response time < 200ms") over vague goals.
- Include negative cases in acceptance criteria.
- Keep examples concrete; use placeholders for timestamps.
