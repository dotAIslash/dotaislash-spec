# dotAIslash - Cursor Global Rules

## 🎯 MANDATORY FIRST ACTION

**ALWAYS READ `/AGENTS.md` BEFORE EVERY SESSION**

Before executing any task, read the `/AGENTS.md` file to understand:
- Project structure and organization
- All repositories and their purposes
- Development standards and conventions
- Critical restrictions and rules

## 🚫 CRITICAL RESTRICTIONS

### Never Create Meta-Documentation

**FORBIDDEN FILES** (examples of what NOT to create):
- `DOCUMENTATION_CONSOLIDATION.md`
- `FIXES_APPLIED_FINAL.md`
- `TEST_RESULTS_COMPLETE.md`
- `LOCAL_INTEGRATION_TEST_REPORT.md`
- `PUBLISHING_v1.0.md`
- `V1.0_LAUNCH_READY.md`
- `FINAL_VERIFICATION_COMPLETE.md`
- Any file describing "what I just did"
- Any "summary of work" files

**Rationale**: These are wasteful clutter. Results should be communicated directly or placed in proper permanent documentation.

**ALLOWED DOCUMENTATION** (where info belongs):
- `/dotaislash-spec/SPEC.md` - VERSA specification
- `/dotaislash-spec/CHANGELOG.md` - Version history
- Package-specific READMEs in each package directory
- `/dotaislash.github.io/` - Website documentation

### Git and Publishing

- ❌ Do NOT `npm publish` unless explicitly told
- ❌ Do NOT `git push` unless explicitly told
- ❌ Do NOT modify published packages
- ✅ Always test locally first
- ✅ Follow exact user instructions

## 📁 Project Structure

```
/var/www/dotAIslash/
├── AGENTS.md                  # AI agent context (read first!)
├── CLAUDE.md                  # Zero-planning coding prompt
├── dotaislash-spec/           # VERSA specification
│   ├── SPEC.md                # Main specification
│   ├── CHANGELOG.md           # Version history
│   └── docs/                  # Supporting docs
├── dotaislash-schemas/        # JSON schemas
│   ├── src/                   # Schema definitions
│   └── tests/                 # Schema validation tests
├── dotaislash-cli/            # CLI implementation
│   ├── src/                   # CLI source
│   └── tests/                 # CLI tests
├── dotaislash-conformance/    # Conformance test suite
├── dotaislash-examples/       # Example .ai/ folders
├── dotaislash-adapters/       # Tool-specific adapters
└── dotaislash.github.io/      # Documentation website
```

## 🔧 Development Standards

### Code Quality
- TypeScript strict mode enabled
- 100% type coverage required
- Vitest/Jest for all tests
- Zero `any` types (use proper typing)

### Testing
- Unit tests for all features
- Integration tests for CLI commands
- All tests must pass before commits
- Run: `npm test` in each package

### File Organization
- Spec: `dotaislash-spec/SPEC.md`
- Schemas: `dotaislash-schemas/src/`
- CLI: `dotaislash-cli/src/`
- Tests: `*/tests/` or `*/src/**/*.test.ts`

### Naming Conventions
- Files: `kebab-case.ts`
- Types: `PascalCase`
- Functions: `camelCase`
- Constants: `UPPER_SNAKE_CASE`

## 🎨 VERSA Format (.ai/ folder)

### Core Files
- `.ai/context.json` - Base configuration
- `.ai/profiles/*.json` - Agent-specific overrides
- `.ai/rules/*.md` - Human-readable rules
- `.ai/capabilities.json` - Runtime capabilities (optional)

### Key Concepts
- **Profile merging**: Combine base + profile configs
- **Agent selection**: Choose profile based on tool
- **Rule composition**: Markdown files for humans and AI
- **Schema validation**: JSON Schema v7 for structure

### Supported Tools (via adapters)
- Cursor
- Windsurf
- Cline
- Aider
- GitHub Copilot
- More planned...

## 🚀 Current State

### Development Status
- ⏳ Specification: Draft in progress
- ⏳ Schemas: Initial design phase
- ⏳ CLI: Planning stage
- ⏳ Website: Basic structure complete
- ⏳ Examples: To be created
- ⏳ Conformance: To be implemented

### Target Milestones
- v0.1.0: Initial draft spec and schemas
- v0.5.0: Working CLI and basic adapters
- v1.0.0: Stable spec, conformance suite, multiple adapters

## 💬 Communication Style

### When Executing Tasks
1. Read AGENTS.md first
2. Execute changes immediately (don't ask for permission)
3. Report what was done after completion
4. Use clear, concise summaries
5. No verbose explanations unless requested

### Code Changes
- Always preserve existing functionality
- Add proper TypeScript types
- Include tests for new features
- Update relevant documentation
- No breaking changes without approval

### Error Handling
- Debug systematically
- Check logs and error messages
- Test fixes locally
- Re-run all tests after fixes

## 🔍 Quality Checklist

Before completing any task:
- [ ] All TypeScript compiles without errors
- [ ] All tests pass (`npm test`)
- [ ] No `any` types introduced
- [ ] Code follows existing patterns
- [ ] Documentation updated if needed
- [ ] No meta-documentation created
- [ ] Changes tested locally
- [ ] Schema changes aligned with SPEC.md

## 📚 Key Resources

- **Specification**: `/dotaislash-spec/SPEC.md`
- **Schemas**: `/dotaislash-schemas/src/`
- **CLI**: `/dotaislash-cli/src/`
- **Examples**: `/dotaislash-examples/`
- **Website**: `/dotaislash.github.io/`

## 🎯 Remember

1. **Read AGENTS.md first** - Always
2. **No meta-documentation** - Ever
3. **Test before commit** - Always
4. **Follow user instructions exactly** - Precisely
5. **Execute immediately** - Don't ask for permission
6. **Align with spec** - Schema changes need SPEC.md update

---

*Last updated: 2025-10-16 (Initial creation)*
