<!-- /AGENTS.md -->
<!-- Master rules for human and AI collaborators -->
<!-- Why: keep work simple, fast, and safe -->
<!-- RELEVANT FILES: README.md, CONTRIBUTING.md -->

# AGENTS Master Guide

This is the source of truth for how humans and AI work together.

## GOAL
Ship value fast. Write clean, simple, modular code. Do exactly what is asked.

## ABOUT THE PRODUCT
VERSA (Vendor-neutral Extensible Repo Spec for Agents): universal `.ai/` folder standard for AI agent context and configuration.

Resources are limited. Favor speed and simplicity. Pick the 80:20 solution.

## PROJECT HISTORY AND VISION

**Origin**: October 2025. Created to standardize how AI agents discover and use project context across tools and platforms.

**Problem**: Every AI tool (Cursor, Windsurf, Cline, Aider, etc.) has different config formats. No portable way to define agent context.

**Solution**: One `.ai/` folder, many runtimes. Git-friendly, human-readable, vendor-neutral.

**Version History**:
- v0.1.0 (Oct 2025): Initial draft specification
- v1.0.0 (Target Q4 2025): Stable VERSA spec, CLI, conformance suite

**Current**: Early development phase. Building spec, schemas, CLI, examples, and adapters.

**Vision 2026**: De facto standard for agent configuration. Adopted by major AI coding tools. Rich ecosystem of adapters and tooling.

## MODUS OPERANDI
Prefer boring solutions. Cut scope early. Avoid hidden complexity. Explain what and why.

## TECH STACK
Backend: Node.js 22, TypeScript 5.8. Frontend: Next.js 15.3, React 18, Tailwind 3.4. Infra: GitHub Actions, npm. Data: File-based JSON/YAML, no database.

## ARCHITECTURE

**System Flow**:
```
.ai/ folder → Schema validation → Profile merging → Agent context → AI tool
                                                                        ↓
                                                     CLI → lint/init/print/context
```

**Packages**:
- dotaislash-spec: VERSA specification and rationale
- dotaislash-schemas: JSON schemas for validation
- dotaislash-cli: Reference CLI implementation
- dotaislash-conformance: Test suite for implementations
- dotaislash-examples: Example `.ai/` folders
- dotaislash-adapters: Tool-specific adapters (Cursor, Windsurf, etc.)
- dotaislash.github.io: Landing site and documentation

**Core Concepts**:
`.ai/context.json` (base context), `.ai/profiles/` (per-agent overrides), `.ai/rules/` (markdown rules), merge strategies, agent selection.

**Integration Points**:
CLI ↔ Schemas (validation), Adapters ↔ Spec (transformation), Website ↔ Spec (docs), Examples ↔ Conformance (testing).

## DEPLOYED ENVIRONMENTS
Production: https://dotaislash.github.io. NPM: @dotaislash/cli (planned). GitHub: github.com/dotAIslash.

## VERSION CONTROL
Git. Follow .cursor/rules/global-rule.md. Small commits with conventional commits: feat:, fix:, docs:, chore:.

## ESSENTIAL COMMANDS
Spec: cd dotaislash-spec && cat SPEC.md. CLI: versa init / versa lint / versa print / versa context. Website: cd dotaislash.github.io && npm run dev. Schemas: cd dotaislash-schemas && npm test. Conformance: cd dotaislash-conformance && npm test.

## HEADER COMMENTS
Every file starts with four lines: 1) exact file location, 2) what this file does, 3) why this file exists, 4) RELEVANT FILES: comma-separated list. Never remove.

## WRITING STYLE
Short sentences. Plain English. Split long sentences, add blank line. Comment non-obvious code.

## SIMPLICITY
Simple is good. Complex is bad. Fewer lines better if clarity stays high. Prefer standard library.

## ROLES AND PERMISSIONS
Product Owner: sets scope (Alphin Tom). Developers: implement, review, release. Editor AI: edits files, no npm publish without approval. Autonomous Agents: refactors/tests only when requested, no production changes.

## UI PRINCIPLES
CodeVibe design system: dark backgrounds, accent colors, clear hierarchy. Simple, minimal, accessible. Consistency across all sites.

## API CONVENTIONS
CLI: versa <command> [options]. Schemas: export JSON Schema v7 compatible objects. Adapters: transform(versaConfig) → toolConfig. Errors: throw Error with context.

## DATABASE PRINCIPLES
No database. File-based JSON/YAML/Markdown. Version control via git.

## SECURITY BASELINE
No secrets in code. Validate JSON/YAML at parse. Pin dependencies, run npm audit. Sandbox user code execution.

## PRIVACY AND DATA RETENTION
Collect no user data. Examples use synthetic data. Log no sensitive info. No telemetry in packages.

## LOGGING AND TELEMETRY
Levels: debug, info, warn, error. Schema errors include file path and line. CLI: stderr for logs, stdout for output.

## ERROR HANDLING
Parse: fail fast with clear error location. Validation: report all schema errors. CLI: exit 1 on error, 0 on success. Helpful messages, no internal stacks in production.

## PERFORMANCE BUDGETS
CLI init: <500ms. CLI lint: <1s for typical project. CLI context: <2s with all profiles. Website: LCP <2s on 4G.

## ACCESSIBILITY AND I18N
WCAG 2.1 AA for website. Keyboard navigation. Screen reader support. CLI messages in English only for v1.0.

## FEATURE FLAGS AND ROLLOUT
New fields in spec start experimental. Document in SPEC.md with version tag. Parsers accept but warn. Promote to stable after implementations prove it.

## DEPENDENCIES
Prefer standard library. Schemas: ajv for validation. CLI: commander, chalk. Website: Next.js, React, Tailwind. Review licenses (MIT/Apache/BSD preferred).

## FORMATTING AND LINTING
TypeScript: ESLint v9 strict. Formatter: Prettier for TS/JS/JSON. Run in CI. JSON Schema: validate with ajv.

## RELEASE AND ROLLBACK
Publish from main with version tag. Follow semver. Breaking spec changes = major bump. Monitor GitHub issues. Rollback: npm deprecate if critical bug.

## MANDATORY FIRST ACTIONS

**ALWAYS RUN THESE COMMANDS FIRST** before any work:

1. **Get Current Date/Time** - NEVER assume dates:
   ```bash
   date "+%Y-%m-%d %H:%M:%S %Z" && date "+Today is %B %d, %Y"
   ```
   Use this date in ALL documentation, examples, and release notes.

2. **Read Context** - Read AGENTS.md for project rules.

**CRITICAL**: Do not use assumed dates like "January 2025" or "today". Always get the actual system date first.

## RESTRICTIONS
Do not npm publish unless told. Do not push to remote unless told. Do not modify published packages. Do exactly what is asked. Test locally first.

**NEVER CREATE META-DOCUMENTATION**: Do not create documentation about documentation work (e.g., DOCUMENTATION_CONSOLIDATION.md, FIXES_APPLIED_FINAL.md, TEST_RESULTS_COMPLETE.md, PUBLISHING_v1.0.md, LOCAL_INTEGRATION_TEST_REPORT.md, V1.0_LAUNCH_READY.md). Results go in proper docs (README, CHANGELOG.md, SPEC.md). Temporary work reports are forbidden. Only create docs requested by user or required for public API.

## FILE LENGTH
Keep under 300 lines. Spec: split by section if needed. CLI: one command per file. Examples: one scenario per folder.

## READING FILES
Read full file before editing. Read related files in same module. Check examples when changing schemas. Check SPEC.md when adding features.

## DATABASE CHANGES
No database in project. Spec format changes require SPEC.md update first and version bump discussion.

## OUTPUT STYLE
State assumptions clearly. State conclusions briefly. Show code changes inline. Include test commands when relevant.

## PULL REQUEST REVIEW PROCESS

**MANDATORY BEFORE EVERY COMMIT**: Act as a staff engineer performing a Pull Request review using a P# severity scheme: P0 blocker, P1 high, P2 medium, P3 low. Read the diff and context, attempt to build and run tests if possible, and assess:

- Correctness, test coverage and edge cases
- Error handling and input validation
- Concurrency and race conditions
- Security including secrets, auth, OWASP risks
- Performance and complexity, memory use
- API contracts and backward compatibility
- Schema changes and spec alignment
- Dependency changes and licenses
- CI configuration
- Logging and observability
- Accessibility and i18n
- Readability and style
- Documentation and comments
- Alignment with architectural guidelines

**Output Format**:
- **Summary**: One sentence on risk and scope
- **Verdict**: Approve or Request changes with rationale
- **Findings**: Numbered list where each item starts with [P#] then file:line then description then why it matters then minimal patch
- **Tests**: List missing or flaky tests with sample cases
- **Follow-ups**: Optional small improvements

**Commit Policy**: Only commit if there are NO P0-P3 issues found.

---

## CHECKLIST FOR EVERY CHANGE
- [ ] PR review process completed (no P0-P3 issues)
- [ ] Scope confirmed
- [ ] Relevant files read including SPEC.md
- [ ] Header comments present and updated
- [ ] File under 300 lines or split
- [ ] Security checked: no secrets, input validated
- [ ] Errors return helpful messages
- [ ] Tests pass: npm test in affected package
- [ ] Examples still validate if schemas changed
- [ ] Version bumped if publishing
- [ ] CHANGELOG updated if publishing
