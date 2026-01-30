# ⚠️ ARCHIVED - Legacy Command Files

This directory contains the original prompt-based workflow files. These have been **replaced** by production-ready Claude Skills.

## Migration Complete ✅

All workflow logic has been migrated to:

📍 **New Location**: `.agent/skills/`

The skills provide the same functionality with major improvements:
- ✅ Better structure following industry best practices
- ✅ Concrete examples with real code and JSON
- ✅ Clear error handling and edge case guidance
- ✅ User-friendly "When to Use" sections
- ✅ Executable command examples
- ✅ Tips and related use cases

## File Mapping

| Archived File | New Skill Location |
|---------------|-------------------|
| `1-specify.md` | `.agent/skills/specify-requirements/SKILL.md` |
| `2-plan.md` | `.agent/skills/create-plan/SKILL.md` |
| `3-tasks.md` | `.agent/skills/create-tasks/SKILL.md` |
| `4-qa.md` | `.agent/skills/design-qa-strategy/SKILL.md` |
| `5-implement.md` | `.agent/skills/implement-solution/SKILL.md` |
| `6-acceptance.md` | `.agent/skills/acceptance-testing/SKILL.md` |
| `README.md` | `.agent/skills/README.md` + `.agent/skills/QUICK_REFERENCE.md` |

## How to Use New Skills

Instead of manually copying prompts, simply invoke skills naturally:

```bash
# Old way (manual prompt copying)
# 1. Open commands/1-specify.md
# 2. Copy content
# 3. Paste into Claude
# 4. Manually paste payload

# New way (natural invocation)
"Use specify-requirements skill for: Add dark mode to settings"
```

## Why The Change?

### Before (Legacy Commands)
- Manual prompt copying required
- Technical documentation style
- No concrete examples
- User had to figure out when to use each stage
- Error handling embedded in text

### After (Skills)
- Auto-discovered by Claude
- User-friendly with clear use cases
- Real-world examples with actual code
- "When to Use This Skill" section guides usage
- Dedicated error handling sections
- Follows [awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) best practices

## Documentation

- **[Skills README](../.agent/skills/README.md)** - Complete guide
- **[Quick Reference](../.agent/skills/QUICK_REFERENCE.md)** - Cheat sheet
- **[Conversion Summary](../.agent/skills/CONVERSION_SUMMARY.md)** - Migration details
- **[Main README](../README.md)** - Project overview

## Keeping This Archive

This directory is preserved for historical reference only:
- Shows evolution of the workflow
- Provides fallback if needed during transition
- Documents original design decisions

**Do not use these files for new work.** Use the skills instead.

## Questions?

See [Skills README](../.agent/skills/README.md) for comprehensive documentation.

---

*Archived on: 2026-01-30*  
*Superseded by: `.agent/skills/*`*
