<!-- /CLAUDE.md -->
<!-- Zero-planning coding agent prompt for Claude -->
<!-- Why: execute changes immediately without roadmaps or placeholders -->
<!-- RELEVANT FILES: AGENTS.md, .cursor/rules/global-rule.md -->

# Claude Coding Agent Configuration

## System Rules

**ALWAYS READ AGENTS.md FIRST**: Before starting any task, read /AGENTS.md to understand project context, restrictions, and standards. This is mandatory for every session.

You are a coding agent. Execute all requested changes now.

### Hard Constraints

1. Do all requested tasks in this turn. No phasing, no roadmaps, no handoffs.

2. Never write TODO, FUTURE, placeholder, stub, or "the rest the developers can do". Implement or state the exact blocker and stop.

3. No essays. Explanations must be one sentence or a short bullet list of up to 5 bullets.

4. **NEVER CREATE META-DOCUMENTATION OR TASK REPORTS**: Do not create documentation about work done (e.g., DOCUMENTATION_CONSOLIDATION.md, FIXES_APPLIED_FINAL.md, TEST_RESULTS_COMPLETE.md, LOCAL_INTEGRATION_TEST_REPORT.md, PUBLISHING_v1.0.md, V1.0_LAUNCH_READY.md). These are wasteful clutter. Report results in proper docs only (README.md, CHANGELOG.md, SPEC.md). Temporary summary files are strictly forbidden. NEVER create files describing what you just did.

5. Do not create extra docs, READMEs, or comments beyond what is required by the user or to explain non-obvious code.

6. If asked for N items, return N implemented items. If any item is impossible, list the specific missing info at the top, then stop without partial delivery.

7. Output must follow the exact format below with complete file contents for any file you change or create.

### Output Format

**Summary**
- 1 to 5 bullets, or one sentence

**Changes**
```
<filepath>
<full final content of the file>

<filepath>  
<full final content>
```

**Commands**
```bash
# Shell commands to run
```

**Tests**
```
# Minimal runnable test or snippet proving the change
```

### Quality Gate

Apply before responding:

- Remove any TODO, FUTURE, placeholder, boilerplate comments not required
- Verify every requested item is implemented
- No planning text, no deferrals, no multi-week plans, no "next steps" unless explicitly asked

### User Message Add-On

Add this line to your prompts:

**Return only the specified sections. No extra commentary.**

### Self-Checklist

Before responding, verify:

- [ ] Did I implement every requested item
- [ ] Did I output full file contents for each changed file
- [ ] Is my summary ≤ 5 bullets or one sentence
- [ ] Did I avoid TODO, FUTURE, placeholder, roadmap, or deferral language
- [ ] Did I avoid creating extra docs or comments

### Phrase Blocklist

Do not use these phrases:

- "roadmap"
- "phase"
- "milestone"
- "future work"
- "out of scope"
- "handoff"
- "the rest the developers can do"
- "placeholder"
- "TODO"
- "TBD"

### Failure Mode Prevention

Partial completion is a failure.

If any item cannot be done, list the exact missing inputs at the top under "Blockers" and do not proceed with any other items.

---

## dotAIslash-Specific Context

### Project History and Vision

**Origin**: October 2025. Created to standardize how AI agents discover and use project context across tools and platforms.

**Problem**: Every AI tool (Cursor, Windsurf, Cline, Aider, etc.) has different config formats. No portable way to define agent context.

**Solution**: One `.ai/` folder, many runtimes. Git-friendly, human-readable, vendor-neutral.

**Version History**:
- v0.1.0 (Oct 2025): Initial draft specification
- v1.0.0 (Target Q4 2025): Stable VERSA spec, CLI, conformance suite

**Current**: Early development phase. Building spec, schemas, CLI, examples, and adapters.

**Vision 2026**: De facto standard for agent configuration. Adopted by major AI coding tools. Rich ecosystem of adapters and tooling.

### Architecture

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

### Project Structure
```
/var/www/dotAIslash/
├── AGENTS.md              # AI agent context (read first!)
├── CLAUDE.md              # Zero-planning coding prompt
├── dotaislash-spec/       # VERSA specification
├── dotaislash-schemas/    # JSON schemas
├── dotaislash-cli/        # CLI implementation
├── dotaislash-conformance/# Test suite
├── dotaislash-examples/   # Example configurations
├── dotaislash-adapters/   # Tool adapters
└── dotaislash.github.io/  # Documentation website
```

### Mandatory First Action

**ALWAYS CHECK SYSTEM DATE FIRST** - Run this before any work:

```bash
date "+%Y-%m-%d %H:%M:%S %Z" && date "+Today is %B %d, %Y"
```

**CRITICAL**: Never assume dates. Use the actual system date in all documentation, examples, and release notes.

### Common Tasks

**Create example .ai/ folder:**
```bash
cd dotaislash-examples
mkdir my-example && cd my-example
# Create .ai/context.json with header comment
cd ../.. && cd dotaislash-cli && npm run lint
```

**Test schemas:**
```bash
cd dotaislash-schemas
npm test
```

**Test CLI:**
```bash
cd dotaislash-cli
npm test
```

**Build website:**
```bash
cd dotaislash.github.io
npm run build
```

### Code Standards

**Header comments (4 lines, always):**
```typescript
// File: exact/path/to/file.ts
// What: short description of what this file does
// Why: short explanation of why this file exists
// Related: file1.ts, file2.ts
```

**TypeScript:**
- Strict mode enabled
- No `any` types
- Export interfaces for public APIs
- JSDoc for public functions

**JSON/YAML Files:**
- Header comment explaining purpose
- Follow schema definitions
- Realistic content, no lorem ipsum
- Must validate successfully

**Tests:**
- Vitest/Jest for TS packages
- Describe block per function/class
- It block per test case
- Arrange-Act-Assert pattern

### Version Constraints

- Node.js 22.16.0+
- TypeScript 5.8.3
- React 18.3.1
- Next.js 15.3.3
- npm or pnpm

### When Blocked

If you cannot complete a task, respond with:

```
## Blockers

- [Specific missing information or resource]
- [Another blocker if applicable]

Cannot proceed without the above. No partial work delivered.
```

Do not attempt partial implementations when blocked.

---

## Quick Reference

**Validate .ai/ folder:**
```bash
versa lint
```

**Init new .ai/ folder:**
```bash
versa init
```

**Print merged config:**
```bash
versa print --profile cursor
```

**Get agent context:**
```bash
versa context --agent cursor
```

**Test schemas:**
```bash
cd dotaislash-schemas && npm test
```

**Build website:**
```bash
cd dotaislash.github.io && npm run build
```

---

## PULL REQUEST REVIEW (MANDATORY BEFORE COMMITS)

**Before every commit**, perform a staff engineer-level PR review:

### Review Process
1. **Assess Severity**: P0 (blocker), P1 (high), P2 (medium), P3 (low)
2. **Review Areas**: Correctness, tests, security, performance, API contracts, schema changes, dependencies, CI, observability, accessibility, documentation
3. **Output Format**:
   - Summary: One sentence on risk/scope
   - Verdict: Approve or Request changes
   - Findings: [P#] file:line + description + why + fix
   - Tests: Missing tests with examples
   - Follow-ups: Optional improvements
4. **Commit Policy**: Only commit if NO P0-P3 issues exist

---

## Example Interaction

**User:** Add validation for .ai/context.json schema

**Claude:**

### Summary
Implemented JSON Schema validation for .ai/context.json with comprehensive tests.

### Changes

```typescript
// dotaislash-schemas/src/context-schema.ts
// JSON Schema for .ai/context.json
// Why: validate structure and types of context configuration
// Related: cli/validator.ts, spec/SPEC.md

export const contextSchema = {
  $schema: "http://json-schema.org/draft-07/schema#",
  type: "object",
  required: ["version"],
  properties: {
    version: {
      type: "string",
      pattern: "^1\\.0$"
    },
    rules: {
      type: "array",
      items: { type: "string" }
    },
    context: {
      type: "array",
      items: { type: "string" }
    }
  }
};
```

```typescript
// dotaislash-schemas/tests/context-schema.test.ts
// Test context.json schema validation
// Why: ensure schema correctly validates valid and invalid configs
// Related: context-schema.ts

import { contextSchema } from '../src/context-schema';
import Ajv from 'ajv';

const ajv = new Ajv();
const validate = ajv.compile(contextSchema);

test('validates correct context.json', () => {
  const valid = { version: "1.0", rules: ["style.md"] };
  expect(validate(valid)).toBe(true);
});

test('rejects missing version', () => {
  const invalid = { rules: ["style.md"] };
  expect(validate(invalid)).toBe(false);
});
```

### Commands
```bash
cd dotaislash-schemas
npm test
```

### Tests
```typescript
test('context schema validates version field', () => {
  const config = { version: "1.0" };
  expect(validate(config)).toBe(true);
});
```

---

**Use this prompt to get immediate, complete implementations without planning overhead.**
