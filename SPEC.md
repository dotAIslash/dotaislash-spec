# VERSA 1.0 - Core Specification

**Vendor-neutral Extensible Repo Spec for Agents**

> **VERSA** - Inspired by **Uni-VERSA-l Rules for AI Agents**

---

## Status

**Version:** 1.0.0-draft  
**Status:** Draft for Review  
**Last Updated:** 2025-10-16  
**Authors:** dotAIslash Community

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Design Principles](#2-design-principles)
3. [Folder Structure](#3-folder-structure)
4. [Core Files](#4-core-files)
5. [Canonical Primitives](#5-canonical-primitives)
6. [Profile System](#6-profile-system)
7. [Merge Strategies](#7-merge-strategies)
8. [Validation Rules](#8-validation-rules)
9. [Security Model](#9-security-model)
10. [Examples](#10-examples)
11. [Conformance](#11-conformance)

---

## 1. Introduction

### 1.1 Purpose

VERSA defines a **vendor-neutral** standard for configuring AI coding agents. It solves the fragmentation problem where every tool (Cursor, Windsurf, Claude, Aider, etc.) has its own configuration format.

### 1.2 Goals

- **Portability**: Write configuration once, use with any VERSA-compatible tool
- **Simplicity**: Plain JSON and Markdown - no DSLs, no magic
- **Security**: Explicit permissions with deny-first policies
- **Extensibility**: Support future capabilities without breaking changes
- **Git-Friendly**: Text-based, diff-able, merge-able

### 1.3 Scope

This specification defines:
- The `.ai/` folder structure
- Required and optional files
- JSON schema for configuration files
- Markdown metadata format for rules
- Profile merging behavior
- Security and permission model

This specification does NOT define:
- How tools implement VERSA internally
- UI/UX for agent interaction
- Model-specific behaviors
- Network protocols

---

## 2. Design Principles

### 2.1 Boring is Beautiful

VERSA uses only **JSON** and **Markdown** - formats that:
- Every developer understands
- Every tool can parse
- Git handles natively
- Diff tools display clearly

### 2.2 Convention over Configuration

Sensible defaults mean minimal configuration:
- Most projects need only `context.json`
- Profiles override only what differs
- Optional features activate when files exist

### 2.3 Explicit is Better

No implicit behavior:
- Permissions default to deny
- File references must be explicit
- Merge strategies declared in profiles
- Version field is required

### 2.4 Security by Default

Safety first:
- Deny-first permission model
- Secrets never in configuration
- Redaction built into knowledge ingestion
- User consent for sensitive operations

---

## 3. Folder Structure

### 3.1 Canonical Layout

```
your-project/
└── .ai/
    ├── context.json           # REQUIRED: Base configuration
    ├── profiles/              # OPTIONAL: Tool-specific overrides
    │   ├── cursor.json
    │   ├── windsurf.json
    │   └── claude.json
    ├── rules/                 # OPTIONAL: Markdown guidelines
    │   ├── style.md
    │   ├── security.md
    │   └── architecture.md
    ├── agents/                # OPTIONAL: Agent definitions
    │   ├── code-reviewer.json
    │   ├── documenter.json
    │   └── tester.json
    ├── prompts/               # OPTIONAL: Reusable templates
    │   ├── bug-report.md
    │   └── feature-spec.md
    ├── tools/                 # OPTIONAL: Tool configurations
    │   └── mcp-servers.json
    ├── knowledge/             # OPTIONAL: Ingestion configs
    │   └── sources.json
    └── memory/                # OPTIONAL: Retention policies
        └── policies.json
```

### 3.2 Discovery

Tools MUST search for `.ai/` in this order:
1. Current directory
2. Parent directories (up to repository root)
3. User home directory (`~/.ai/` as global fallback)

Tools MUST stop at the first `.ai/` folder found.

### 3.3 File Naming

- All JSON files MUST use `.json` extension
- All Markdown files MUST use `.md` extension
- All filenames MUST be lowercase with hyphens (kebab-case)
- No spaces in filenames

---

## 4. Core Files

### 4.1 context.json (REQUIRED)

The base configuration file. All tools MUST read this file.

**Minimum Valid Example:**
```json
{
  "version": "1.0"
}
```

**Full Example:**
```json
{
  "version": "1.0",
  "rules": [
    "rules/style.md",
    "rules/security.md"
  ],
  "context": [
    "src/**/*.ts",
    "docs/**/*.md"
  ],
  "agents": [
    "agents/code-reviewer.json"
  ],
  "prompts": [
    "prompts/bug-report.md"
  ],
  "tools": [
    "tools/mcp-servers.json"
  ],
  "knowledge": [
    "knowledge/sources.json"
  ],
  "settings": {
    "model": "claude-sonnet-4",
    "temperature": 0.7,
    "maxTokens": 4096
  },
  "permissions": {
    "files": {
      "read": ["src/**", "docs/**"],
      "write": ["src/**"],
      "deny": ["*.key", "*.pem"]
    },
    "network": {
      "allow": ["https://api.openai.com"],
      "deny": ["*"]
    }
  }
}
```

**Required Fields:**
- `version` (string): MUST be `"1.0"`

**Optional Fields:**
- `rules` (array of strings): Paths to rule files
- `context` (array of strings): Glob patterns for context files
- `agents` (array of strings): Paths to agent definitions
- `prompts` (array of strings): Paths to prompt templates
- `tools` (array of strings): Paths to tool configurations
- `knowledge` (array of strings): Paths to knowledge sources
- `settings` (object): Model and runtime settings
- `permissions` (object): Security policies

### 4.2 Profiles (OPTIONAL)

Tool-specific configuration overrides.

**Location:** `.ai/profiles/{tool-name}.json`

**Example: `.ai/profiles/cursor.json`**
```json
{
  "version": "1.0",
  "merge": "deep",
  "rules": [
    "rules/cursor-specific.md"
  ],
  "settings": {
    "shortcuts": {
      "review": "agents/code-reviewer.json",
      "document": "agents/documenter.json"
    },
    "sidebar": "expanded"
  }
}
```

**Required Fields:**
- `version` (string): MUST be `"1.0"`
- `merge` (string): MUST be one of `"deep"`, `"shallow"`, `"replace"`

**Merge Strategies:**
- `"deep"`: Recursively merge objects and concatenate arrays
- `"shallow"`: Top-level fields only, arrays replaced
- `"replace"`: Ignore base config, use profile only

---

## 5. Canonical Primitives

VERSA defines 8 canonical primitives:

### 5.1 Rules

**Purpose:** Persistent project context and guidelines

**Format:** Markdown files with `ai:meta` front matter

**Example: `.ai/rules/style.md`**
```markdown
---
ai:meta
  priority: high
  attach: always
  scope: global
---

# Code Style Guide

## TypeScript

- Use strict mode
- Prefer functional components
- Maximum line length: 100 characters

## Imports

- Group by: external, internal, types
- Sort alphabetically within groups
```

**Metadata Fields:**
- `priority` (string): `"low"` | `"medium"` | `"high"` | `"critical"`
- `attach` (string): `"always"` | `"on-demand"` | `"never"`
- `scope` (string): `"global"` | `"local"` | custom scope name

### 5.2 Prompts

**Purpose:** Reusable templates with variables

**Format:** Markdown with variable substitution

**Example: `.ai/prompts/bug-report.md`**
```markdown
---
ai:meta
  variables:
    - name: issue_number
      type: string
      required: true
    - name: severity
      type: string
      enum: [low, medium, high, critical]
      default: medium
---

# Bug Report Template

Issue: #{issue_number}
Severity: {severity}

## Description

[Describe the bug clearly]

## Steps to Reproduce

1. 
2. 
3. 

## Expected Behavior

## Actual Behavior

## Environment

- OS:
- Browser:
- Version:
```

### 5.3 Agents

**Purpose:** Declarative agent configurations

**Example: `.ai/agents/code-reviewer.json`**
```json
{
  "version": "1.0",
  "name": "Code Reviewer",
  "description": "Reviews code for quality and best practices",
  "model": "claude-sonnet-4",
  "temperature": 0.3,
  "maxTokens": 4096,
  "tools": [
    "tools/linter.json",
    "tools/test-runner.json"
  ],
  "rules": [
    "rules/review-checklist.md"
  ],
  "systemPrompt": "You are an expert code reviewer. Focus on:\n- Code quality\n- Best practices\n- Security issues\n- Performance concerns"
}
```

### 5.4 Memory

**Purpose:** Retention policies for agent memory

**Example: `.ai/memory/policies.json`**
```json
{
  "version": "1.0",
  "retention": {
    "session": {
      "enabled": true,
      "duration": "1 hour",
      "maxSize": "10MB"
    },
    "project": {
      "enabled": true,
      "duration": "30 days",
      "maxSize": "100MB",
      "persist": [
        "decisions",
        "patterns",
        "conventions"
      ]
    },
    "persistent": {
      "enabled": false
    }
  }
}
```

### 5.5 Knowledge

**Purpose:** Document ingestion configuration

**Example: `.ai/knowledge/sources.json`**
```json
{
  "version": "1.0",
  "sources": [
    {
      "type": "repository",
      "url": "https://github.com/your-org/docs",
      "paths": ["docs/**/*.md"],
      "refresh": "daily"
    },
    {
      "type": "url",
      "url": "https://docs.example.com/api",
      "selector": "article.content",
      "refresh": "weekly"
    }
  ],
  "redaction": {
    "patterns": [
      "sk-[a-zA-Z0-9]{32}",
      "ghp_[a-zA-Z0-9]{36}"
    ],
    "fields": ["password", "token", "secret"]
  }
}
```

### 5.6 Tools

**Purpose:** MCP server and API configurations

**Example: `.ai/tools/mcp-servers.json`**
```json
{
  "version": "1.0",
  "servers": [
    {
      "name": "linter",
      "type": "mcp",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-linter"],
      "env": {
        "LINTER_CONFIG": ".eslintrc.json"
      }
    },
    {
      "name": "github",
      "type": "http",
      "baseUrl": "https://api.github.com",
      "auth": {
        "type": "bearer",
        "tokenEnv": "GITHUB_TOKEN"
      }
    }
  ]
}
```

### 5.7 Settings

**Purpose:** Model and runtime preferences

**Embedded in `context.json`:**
```json
{
  "settings": {
    "model": "claude-sonnet-4",
    "temperature": 0.7,
    "maxTokens": 4096,
    "topP": 1.0,
    "frequencyPenalty": 0,
    "presencePenalty": 0,
    "streaming": true,
    "ui": {
      "theme": "dark",
      "sidebar": "expanded",
      "fontSize": 14
    }
  }
}
```

### 5.8 Permissions

**Purpose:** Security policy enforcement

**Embedded in `context.json`:**
```json
{
  "permissions": {
    "files": {
      "read": ["src/**", "docs/**"],
      "write": ["src/**"],
      "deny": ["*.key", "*.pem", ".env*"]
    },
    "network": {
      "allow": [
        "https://api.openai.com",
        "https://api.anthropic.com"
      ],
      "deny": ["*"]
    },
    "commands": {
      "allow": ["npm test", "git status"],
      "deny": ["rm -rf", "sudo"]
    },
    "secrets": {
      "bindings": {
        "OPENAI_API_KEY": "env:OPENAI_API_KEY",
        "GITHUB_TOKEN": "vault:github-token"
      }
    }
  }
}
```

---

## 6. Profile System

### 6.1 Profile Selection

Tools MUST use this priority order:
1. Command-line flag: `--profile cursor`
2. Environment variable: `VERSA_PROFILE=cursor`
3. Tool auto-detection based on execution context

### 6.2 Profile Merging

When a profile is selected, tools MUST:
1. Load `context.json`
2. Load `profiles/{selected}.json`
3. Merge according to the `merge` strategy
4. Validate the result

### 6.3 Merge Strategies

#### Deep Merge (`"merge": "deep"`)
- Objects: Recursively merge all properties
- Arrays: Concatenate (base + profile)
- Primitives: Profile overrides base

**Example:**
```json
// Base
{
  "rules": ["rules/style.md"],
  "settings": {
    "model": "gpt-4",
    "temperature": 0.7
  }
}

// Profile (deep merge)
{
  "rules": ["rules/cursor-specific.md"],
  "settings": {
    "temperature": 0.5,
    "shortcuts": { "review": "agents/reviewer.json" }
  }
}

// Result
{
  "rules": ["rules/style.md", "rules/cursor-specific.md"],
  "settings": {
    "model": "gpt-4",
    "temperature": 0.5,
    "shortcuts": { "review": "agents/reviewer.json" }
  }
}
```

#### Shallow Merge (`"merge": "shallow"`)
- Objects: Merge only top-level properties
- Arrays: Replace (profile replaces base)
- Primitives: Profile overrides base

#### Replace (`"merge": "replace"`)
- Ignore base configuration entirely
- Use profile as complete configuration
- Useful for completely different tool requirements

---

## 7. Merge Strategies

### 7.1 Array Concatenation

Arrays MUST be concatenated in deep merge:
```json
// Base
{ "rules": ["a.md", "b.md"] }

// Profile
{ "rules": ["c.md"] }

// Result (deep)
{ "rules": ["a.md", "b.md", "c.md"] }
```

### 7.2 Conflict Resolution

When conflicts occur:
1. Profile values ALWAYS override base values
2. Arrays concatenate (deep) or replace (shallow)
3. Undefined values in profile preserve base values
4. Null values in profile remove base values

---

## 8. Validation Rules

### 8.1 Schema Validation

All JSON files MUST validate against JSON Schema (see `dotaislash-schemas`).

### 8.2 File References

All file paths MUST:
- Be relative to `.ai/` folder
- Use forward slashes (`/`)
- Exist when configuration is loaded
- Not contain `..` (parent directory references)

### 8.3 Version Compatibility

Tools MUST:
- Check `version` field in all JSON files
- Reject configurations with unsupported versions
- Log clear error messages for version mismatches

### 8.4 Circular Dependencies

Tools MUST detect and reject circular dependencies:
```json
// agents/a.json references agents/b.json
// agents/b.json references agents/a.json
// = CIRCULAR DEPENDENCY ERROR
```

---

## 9. Security Model

### 9.1 Permission Model

VERSA uses a **deny-first** security model:
1. Everything is denied by default
2. Explicit `allow` rules grant access
3. Explicit `deny` rules override `allow` rules

### 9.2 Secret Management

Secrets MUST NEVER appear in configuration files:
```json
// ❌ BAD
{
  "apiKey": "sk-abc123..."
}

// ✅ GOOD
{
  "secrets": {
    "bindings": {
      "OPENAI_API_KEY": "env:OPENAI_API_KEY"
    }
  }
}
```

### 9.3 File Access

Tools MUST:
- Respect `permissions.files.deny` patterns
- Request user consent for writes
- Log all file operations
- Never access files outside allowed patterns

### 9.4 Network Access

Tools MUST:
- Respect `permissions.network.allow` list
- Request user consent for new domains
- Log all network requests
- Block disallowed domains

---

## 10. Examples

### 10.1 Minimal Configuration

```json
{
  "version": "1.0"
}
```

### 10.2 TypeScript Project

```json
{
  "version": "1.0",
  "rules": [
    "rules/typescript-style.md",
    "rules/react-patterns.md"
  ],
  "context": [
    "src/**/*.ts",
    "src/**/*.tsx"
  ],
  "settings": {
    "model": "claude-sonnet-4",
    "temperature": 0.5
  }
}
```

### 10.3 Monorepo Configuration

```json
{
  "version": "1.0",
  "rules": [
    "rules/workspace.md"
  ],
  "context": [
    "packages/*/src/**"
  ],
  "agents": [
    "agents/monorepo-reviewer.json"
  ],
  "settings": {
    "model": "gpt-4-turbo"
  }
}
```

---

## 11. Conformance

### 11.1 Conformance Levels

**Level 1: Basic (Minimum)**
- Parse `context.json`
- Support `version` field
- Read `rules` array
- Load Markdown rules

**Level 2: Standard**
- Profile support
- Deep merge strategy
- File permissions
- Agent definitions

**Level 3: Full**
- All merge strategies
- Complete permission model
- Memory management
- Knowledge ingestion
- Tool integration

### 11.2 Testing

Tools claiming VERSA compatibility MUST pass the conformance test suite:
- `dotaislash-conformance` repository
- Minimum 90% test pass rate for certification

---

## Appendix A: Glossary

- **Agent**: Configured AI assistant with specific capabilities
- **Context**: Files and rules provided to the agent
- **Profile**: Tool-specific configuration override
- **Primitive**: One of the 8 canonical configuration categories
- **Rule**: Markdown file with project guidelines

---

## Appendix B: Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0-draft | 2025-10-16 | Initial draft specification |

---

## Appendix C: References

- JSON Schema: https://json-schema.org
- Markdown Spec: https://commonmark.org
- MCP Protocol: https://modelcontextprotocol.io

---

## License

This specification is licensed under the MIT License.

Copyright (c) 2025 dotAIslash

---

**VERSA 1.0** - Universal Rules for AI Agents  
Maintained by the dotAIslash community
