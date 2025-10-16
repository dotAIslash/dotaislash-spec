# Contributing to dotAIslash

Thank you for your interest in contributing! This guide explains how to propose
changes and contribute across dotAIslash repositories.

## Ground Rules
- Be kind and respectful. Read and follow our [Code of Conduct](./CODE_OF_CONDUCT.md).
- Use the provided issue and PR templates. Provide clear context and rationale.
- Follow Conventional Commits for commit messages:
  - `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`, `perf:`, `build:`
- All changes must go through Pull Requests (PRs). `main` is protected.
- At least 1 approving review is required to merge (maintainers may bypass).

## Getting Started
1. Fork the repository and create a feature branch:
   - `git checkout -b feat/short-description`
2. Make your changes with tests and docs:
   - Keep code simple, readable, and well-typed
   - Add/adjust tests where applicable
3. Run linters and tests locally before opening a PR.
4. Open a PR using our template. Link related issues and add screenshots if helpful.

## Repository Notes
- Specification (`dotaislash-spec`):
  - Changes to `SPEC.md` must include clear rationale and examples.
  - Keep normative language unambiguous (MUST/SHOULD/MAY as appropriate).
- Code repos (`cli`, `schemas`, `adapters`, `conformance`, `examples`, `website`):
  - Prefer small, focused PRs
  - Ensure typings, error handling, and docs are updated

## Review & Merge Process
- CI must pass.
- Resolve all review comments and conversations.
- Squash merge is preferred unless otherwise requested.

## Reporting Bugs / Requesting Features
- Use the issue templates (Bug/Feature). Provide reproduction steps, logs, and context.
- For design-level proposals, start an Idea in Discussions first.

## Security
- Do not disclose security issues publicly. Open a minimal issue asking for a
  maintainer contact for private follow-up.

## License
- By contributing, you agree your contributions are licensed under the repository's
  license (MIT for code repositories; CC BY 4.0 for the specification).

## Community
- Join Discussions: https://github.com/orgs/dotAIslash/discussions
- Maintainer: [Alphin Tom](https://github.com/alpha912)
