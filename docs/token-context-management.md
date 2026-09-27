# Token and context management

Copilot sends applicable instruction files with the prompt. A rule that is always attached costs tokens on every request, including requests that never use it. Keep the attached set small on purpose.

## Scope by file type

- Put repository-wide rules in `.github/copilot-instructions.md`. That file is always attached. Hold it to conventions that are true for every stack.
- Put stack and concern rules in `.github/instructions/*.instructions.md` with an `applyTo` glob. Angular rules attach for `**/*.ts` and `**/*.html`. React rules attach for `**/*.tsx` and `**/*.jsx`. Spring Boot rules attach for `**/*.java`.
- Use `applyTo: '**'` only for a short rule set that is valid on every file. In this repo that is security, performance, and commit format. If one of those grows, split it and narrow the glob.
- Prefer several short instruction files over one global document that repeats every stack.

## Do not copy rules across layers

- Write each convention once, in the instruction file that owns it.
- From a prompt or an agent, link that file with a relative Markdown link, for example `[angular.instructions.md](../instructions/angular.instructions.md)`.
- Let agent checklists stay pass/fail gates. Leave the convention wording in the instruction file.
- A skill is the exception. Copilot can load `SKILL.md` without the instruction files, so the skill repeats the steps it needs. Do not also paste those steps into the matching prompt.

## Review for staleness

- On a fixed cadence (quarterly is enough), reread each instruction file against [angular.dev](https://angular.dev), [react.dev](https://react.dev), [spring.io](https://spring.io), and the current [OWASP Top 10](https://owasp.org/www-project-top-ten/).
- Delete a rule when a linter or compiler flag already enforces it.
- Remove model names pinned in prompt frontmatter when the team no longer uses that model.
- Delete an instruction file whose `applyTo` matches no path in the repositories that consume it.

## Measure the cost per team

Estimate tokens before adding a file, and record the number for the team.

1. Pick one representative edit per stack (one Angular component, one React component, one Spring service).
2. List the files that would attach: `copilot-instructions.md`, every instruction file with `applyTo: '**'`, and the stack file whose glob matches the edit.
3. Count characters in that set and divide by 4. That is a rough token estimate, not a bill.
4. Write down the estimate per team and per stack. Compare it the next quarter.
5. When the number climbs, narrow a glob or delete a stale rule before adding another instruction file.

Treat the budget as a team limit. A rule that helps one repository and taxes every other request does not earn a permanent `**` glob.
