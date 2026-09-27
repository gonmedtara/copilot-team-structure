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

## Context size

This file is sent on every request. A rule parked here is paid for by every chat, including chats that never use it.

- Keep this file limited to rules that are true for every stack.
- Put Angular, React, Spring Boot, accessibility, and other scoped rules in `.github/instructions/*.instructions.md` with a narrow `applyTo` glob.
- Use `applyTo: '**'` only for a short rule set that is valid on every file. If that file grows, split it and narrow the glob.
- Write each convention once, in the instruction file that owns it.
- From a prompt or an agent, link that file. Example: `[angular.instructions.md](../instructions/angular.instructions.md)`.
- Keep agent checklists as pass/fail gates. Leave the convention wording in the instruction file.
- Repeat a rule inside a skill only because `SKILL.md` loads on its own. Do not also paste that skill into the matching prompt.
- Delete a rule when a linter or compiler flag already enforces it.
- Delete an instruction file whose `applyTo` matches no path in the repositories that consume it.
- Drop pinned model names from prompt frontmatter when the team no longer uses that model.
- When [angular.dev](https://angular.dev), [react.dev](https://react.dev), [spring.io](https://spring.io), or the [OWASP Top 10](https://owasp.org/www-project-top-ten/) changes, update the matching instruction file.

Before adding a rule, estimate the cost for one representative edit:

1. List `copilot-instructions.md`, every instruction file with `applyTo: '**'`, and the stack file whose glob matches the edit.
2. Divide the character count of that set by 4. That is a rough token estimate, not a bill.
3. Record it per team and per stack. Compare it the next quarter.
4. If the number climbs, narrow a glob or delete a stale rule before adding another file.
