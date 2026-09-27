---
description: 'Conventional Commits format, one change per commit, and the pull request description. Use when writing a commit message or a pull request.'
applyTo: '**'
---

# Commits and pull requests

## Commit message

- Format: `type(scope): subject`
- Types: `feat`, `fix`, `refactor`, `perf`, `test`, `docs`, `build`, `ci`, `chore`.
- Write an imperative subject in lowercase, with no trailing period, at 72 characters or fewer.
- Use a scope for the area (`auth`, `invoice`, `deps`). Omit the parentheses when no scope fits.
- Explain why in the body, wrapped at 72 characters.
- Record a breaking API change as a footer: `BREAKING CHANGE: ...`
- Put one logical change in each commit. Split formatting, refactors, and behavior into separate commits.

Examples:

- `feat(invoice): add summary endpoint`
- `fix(auth): reject expired refresh tokens`

## Pull request description

```markdown
## Summary
- What changed

## Why
- The user or system outcome

## Test plan
- [ ] Command or check that was run

## Risk
- What can break, and how to roll it back
```
