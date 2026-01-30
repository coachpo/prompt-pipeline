# Reference

## Constraints and rules
- Do not design QA without `requirement`, `plan`, and `tasks` sections.
- Map every acceptance criterion to at least one scenario.
- Include happy path, edge cases, and negative tests.
- Provide exact validation commands when possible.
- Preserve all existing payload sections.

## Test levels
- Unit: single function or module (< 1s)
- Integration: multiple components or DB/API (< 30s)
- Contract: API or schema compatibility
- E2E: full workflow (< 5min)
- Exploratory: manual checks

## Security and privacy
- Avoid real customer data in fixtures; use synthetic or anonymized data.
- Do not include secrets, API keys, or credentials.

## Error handling
- If prerequisites are missing: request the prior stage first.
- If fixtures do not exist: document setup steps or add as a dependency.
- If coverage gaps remain: list them explicitly.

## Tips
- Reuse existing tests and fixtures when possible.
- Keep scenarios short and measurable.
- Note performance thresholds in expected output.
