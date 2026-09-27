# Project instructions

These rules apply to every Copilot request in the repository. Stack-specific rules live in `.github/instructions/` and attach by file type.

## TypeScript

- Enable `strict` in `tsconfig`. Leave `strictNullChecks` and `noImplicitAny` enabled.
- Type exported parameters and return values.
- Use `unknown` at untrusted boundaries and narrow it before use.
- Keep `any` out of application code. A suppression needs a one-line comment naming the constraint.

## Testing

- Ship a test with every behavior change.
- Assert behavior through the public API.
- Keep tests deterministic: no live network, no shared mutable clock, and no dependence on execution order.
- Prefer a test that names the expected behavior over a snapshot of current markup.

## Commits

- Use Conventional Commits: `type(scope): subject`.
- Write the subject in the imperative, lowercase, with no trailing period.
- Put one logical change in each commit.
- Follow `.github/instructions/git-commit.instructions.md` for the type list and the pull request template.

## Pull requests

- State what changed and why.
- List the tests that were run.
- Link the ticket.
- Keep the diff inside the stated change.
- Leave secrets, credentials, and local environment files out of the diff.
