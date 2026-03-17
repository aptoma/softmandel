# Aptoma Agent Guidelines

Rules for AI coding agents working in Aptoma repositories. Derived from [SOFT MANDEL](README.md). These rules apply regardless of model, tool, or framework.

Project-specific instructions (e.g., `AGENTS.md`, `CLAUDE.md`, `.github/copilot-instructions.md`) may extend or override these guidelines for their repository. When in conflict, project-specific rules take precedence.

## General Principle

Read before writing. Adopt the existing codebase's conventions — naming, patterns, structure, error handling, configuration, file organization — rather than introducing your own. This applies to all sections below.

## Code Quality

- Follow the coding style configured for the project. Run the project's linter before committing. Do not commit code with lint errors.
- Use TypeScript for all non-trivial JavaScript/TypeScript code. Do not introduce untyped code into a typed codebase. Do not add `any` types or `@ts-ignore` without justification.
- Do not add comments that restate what the code does. Only comment non-obvious intent or constraints.
- Do not add, remove, or upgrade dependencies without explicit approval. Prefer existing dependencies and standard library features over new packages. When a dependency would genuinely help, propose it with justification — but do not pull in libraries for things that can be solved with a few lines of code.

## Scope and Refactoring

- Only modify code directly related to the current task. Do not refactor, restyle, or "improve" code outside the immediate scope unless explicitly asked to.
- Do not reorganize file structure, rename modules, or change public APIs beyond what the task requires.
- Do not modify version numbers, changelogs, or release note files unless explicitly instructed.
- If you encounter code that should be improved but is out of scope, note it — do not fix it.

## Source Control

- Every commit on `master` must represent a working state. Do not commit code that breaks the build, fails tests, or introduces regressions.
- Each commit should be an atomic discrete change. Do not mix unrelated changes in a single commit.
- Write commit messages following [Conventional Commits](https://www.conventionalcommits.org/): `type(scope): subject`. Keep the subject under 72 characters and in the imperative style. Add a body separated by a blank line when the change needs explanation.
- Never skip pre-commit hooks or bypass CI checks.
- Never force push to `master`.

## Pull Requests

- Each PR should cover one atomic change or a coherent set of related commits.
- Always provide a descriptive PR title and body detailing the "why" behind the change, not just the "what."
- Document any deployment considerations (migrations, service dependencies, rollback procedures) in the PR description.
- If your change affects user-facing behavior, public APIs, or setup instructions, update the relevant documentation in the same PR.
- A human must review and approve all PRs before merge. Do not merge without approval.
- Do not post comments, reviews, or any content to GitHub or other external services without explicit approval.

## Testing

- All changes must have appropriate test coverage. Do not submit code that reduces existing coverage without justification.
- Do not generate tests that merely mirror the implementation. Tests should validate behavior and catch regressions, not parrot the code structure.
- Run the project's test suite before committing. Do not commit with failing tests.
- Never modify expected test outputs to make tests pass without understanding and confirming the change is correct.

## Technology

- Node.js is the default backend runtime. Do not introduce other server runtimes without explicit approval.
- Write automation scripts in JavaScript, TypeScript, or shell script. Do not use other languages for scripting.
- Prefer database engines supported by Amazon RDS. Do not introduce new database technologies without explicit approval.
- Do not introduce application frameworks (React, Vue, Angular, etc.) beyond what the project already uses. Libraries and smaller dependencies are acceptable with explicit approval.

## Architecture

- Prefer multitenant designs.
- Do not split modules into separate services without explicit approval. A well-structured monolith is preferable to unjustified decomposition.
- Follow the existing module boundaries in the project. If a change would benefit from restructuring, propose it — do not execute it unilaterally.

## System Identification

All HTTP backends must identify themselves via a `User-Agent` header. Format per [RFC9110](https://www.rfc-editor.org/rfc/rfc9110#field.user-agent):

```
User-Agent: aptoma <service-name>/<version> (env:<environment>)
```

- First identifier is always `aptoma`, followed by service name and version.
- Parenthesized section must include at minimum `env:<environment>`.
- Do not include information already present elsewhere in the request (authentication, trace headers).
- Frontends must not overwrite the browser's `User-Agent`.

## Security

- Do not commit secrets, credentials, API keys, or tokens. Do not commit `.env` files.
- Do not introduce code patterns vulnerable to injection (SQL, command, XSS), path traversal, or other OWASP Top 10 risks.
- Do not disable security features, authentication checks, or access controls, even temporarily.

## Communication

- Be direct and technical. State what you changed and why.
- When uncertain about scope, intent, or correctness, ask rather than guess.
- Do not fabricate information about the codebase. If you don't know, say so.
