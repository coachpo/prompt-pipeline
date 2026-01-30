# Reference

## Constraints and rules
- Do not create a plan without validated requirements.
- Keep plans to 3-6 steps for non-trivial work.
- Use evidence (file:line or docs) to justify plan steps and risks.
- Preserve requirement data and all existing payload sections.
- Prefer references over long excerpts.

## Triviality criteria
Mark work as trivial (single-step plan) only if all are true:
- Changes <= 3 files
- No new external dependencies
- No database migrations
- No API contract changes
- No cross-team coordination
- Can be completed in < 1 day

## Security and privacy
- Do not include secrets, API keys, or credentials in payloads or logs.
- Redact PII or sensitive customer data from examples and evidence.

## Error handling
- If requirements are missing: request specifying-requirements first.
- If acceptance criteria are vague: document constraints as risks.
- If blast radius is unclear: document uncertainty and recommend a spike.
- If multiple valid approaches exist: list options in `plan.artifacts` and request a decision.

## Tips
- Make dependencies explicit (sequential vs parallel).
- Use realistic owners based on code ownership.
- Quantify risks when possible.
