# Six-Stage Workflow Skills

This directory contains 6 production-ready SKILLs that guide software development from requirements through acceptance, following industry best practices for Claude Skills.

## 📚 The Six Skills

### 1️⃣ [Specify Requirements](./specify-requirements/SKILL.md)
**When to use**: Starting new features, converting product specs into engineering requirements  
**What it does**: Transforms vague stakeholder requests into structured requirements with testable acceptance criteria  
**Output**: `handoff/payload.json` with requirements, constraints, and evidence

### 2️⃣ [Create Plan](./create-plan/SKILL.md)
**When to use**: After requirements approved, before breaking down work  
**What it does**: Creates evidence-backed execution plan with ordered steps and risk assessment  
**Output**: Plan steps, dependencies, risks, and artifacts in payload

### 3️⃣ [Create Tasks](./create-tasks/SKILL.md)
**When to use**: After plan approved, before implementation  
**What it does**: Decomposes plan into atomic, assignable development tasks  
**Output**: Hierarchical task backlog with dependencies and file references

### 4️⃣ [Design QA Strategy](./design-qa-strategy/SKILL.md)
**When to use**: After tasks defined, before coding begins  
**What it does**: Maps every requirement to test scenarios with validation commands  
**Output**: Coverage matrix, test scenarios, fixtures, and validation commands

### 5️⃣ [Implement Solution](./implement-solution/SKILL.md)
**When to use**: After QA strategy approved, ready to code  
**What it does**: Autonomously executes tasks, writes code and tests, captures evidence  
**Output**: Code changes, test results, command logs with full traceability

### 6️⃣ [Acceptance Testing](./acceptance-testing/SKILL.md)
**When to use**: After implementation complete, before merging/deploying  
**What it does**: Independently verifies work meets acceptance criteria  
**Output**: Go/no-go decision with evidence and issue tracking

---

## 🚀 Quick Start

### Full Feature Development

```
1. "Use specify-requirements skill for: Add user profile export to PDF"
2. "Use create-plan skill with payload.json"
3. "Use create-tasks skill with payload.json"  
4. "Use design-qa-strategy skill with payload.json"
5. "Use implement-solution skill with payload.json"
6. "Use acceptance-testing skill with payload.json"
```

### Fast Track (Trivial Changes)

```
1. "Use specify-requirements for: Fix typo in welcome email"
2. "Use create-plan with payload.json" (marks as trivial, single step)
3. "Use implement-solution with payload.json" (quick fix)
4. "Use acceptance-testing with payload.json" (verify)
```

---

## 📖 Claude Cookbook Alignment

These skills align to the Claude Skills custom development cookbook:

✅ **Required SKILL.md frontmatter** - `name` + `description` only, kept concise  
✅ **Single responsibility** - One stage per skill, no cross-stage blending  
✅ **Clear guidance + examples** - Concrete usage patterns with sample inputs/outputs  
✅ **Constraints & rules** - Explicit guardrails and prerequisites per stage  
✅ **Error handling** - Documented failure modes and recovery paths  
✅ **Security & privacy** - Redaction guidance and no-secrets policy  
✅ **Token discipline** - Keep root `.md` content lean (< ~5k tokens total)  
✅ **Optional scripts/resources** - Deterministic helpers live in `scripts/` with tests  

---

## 🔄 Workflow Dependencies

```
specify-requirements (Stage 1)
    ↓ requires: requirement.summary + acceptanceCriteria
create-plan (Stage 2)
    ↓ requires: plan.steps
create-tasks (Stage 3)
    ↓ requires: tasks.items
design-qa-strategy (Stage 4)
    ↓ requires: testPlan.scenarios
implement-solution (Stage 5)
    ↓ requires: implementation.changes
acceptance-testing (Stage 6)
    ↓ produces: qaFindings.status (approved/rejected)
```

---

## 📋 Shared Payload

All skills operate on `handoff/payload.json`:

```json
{
  "meta": {
    "currentStage": "1-specify",
    "lastUpdatedBy": "specify-requirements-skill",
    "lastUpdatedAt": "2026-01-30T08:00:00Z"
  },
  "requirement": { /* Stage 1 */ },
  "plan": { /* Stage 2 */ },
  "tasks": { /* Stage 3 */ },
  "testPlan": { /* Stage 4 */ },
  "implementation": { /* Stage 5 */ },
  "qaFindings": { /* Stage 6 */ }
}
```

Each skill:
- Validates required input sections
- Updates its output section
- Preserves all other sections
- Updates meta.currentStage

---

## 🎯 Key Features

### Evidence-Based
Every decision backed by:
- Codebase searches with file+line citations
- Documentation references with URLs
- Test results with metrics
- Command outputs with timestamps

### Traceable
Complete lineage:
- Requirements → Plan Steps → Tasks → Tests → Code → Issues
- Every change links back to original requirement
- Searchable history in single JSON file

### Autonomous
Stage 5 (Implement Solution) runs without approval loops:
- Executes task queue in dependency order
- Retries failed tests once
- Continues on blocks
- Captures all evidence

### Defensible
Stage 6 (Acceptance) provides audit trail:
- Replays all validation commands
- Performs exploratory testing
- Documents every finding
- Delivers go/no-go with evidence

---

## 🛠️ Extending Skills

Each skill supports the cookbook layout:

```
skill-name/
├── SKILL.md           # Required: Instructions + examples + rules
├── EXAMPLES.md        # Optional: Extra usage examples (kept concise)
├── REFERENCE.md       # Optional: Supporting notes or schemas
├── TROUBLESHOOTING.md # Optional: Common failures + fixes
├── CHANGELOG.md       # Optional: Version history for this skill
├── scripts/           # Optional: Deterministic helpers (add tests/ when used)
└── resources/         # Optional: Templates, fixtures, assets (not auto-loaded)
```

Notes:
1. All root `.md` files are loaded when the skill triggers, so keep them short.
2. Use `scripts/` for repeatable logic and add tests in `tests/` when needed.
3. Put non-doc assets in `resources/` and reference them from SKILL.md.

---

## 📚 Additional Documentation

- **[CONVERSION_SUMMARY.md](./CONVERSION_SUMMARY.md)** - How prompts became skills
- **[QUICK_REFERENCE.md](./QUICK_REFERENCE.md)** - Cheat sheet with examples
- **[../commands/README.md](../commands/README.md)** - Original workflow guide

---

## 🤝 Contributing

When updating skills:
1. Maintain YAML frontmatter format
2. Keep "When to Use" section current
3. Add real examples from usage
4. Update constraints/error handling as issues are discovered
5. Keep total `.md` content under ~5k tokens per skill
6. Document breaking changes

---

## 📝 License

Same license as parent project. See root LICENSE file.
