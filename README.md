# copilot-team-structure

A reference kit for structuring GitHub Copilot across a team. Every customization file uses a path and frontmatter that GitHub Copilot already discovers.

## Purposes

**Community reference.** Copy `.github/` into an application repository so Copilot follows the same conventions for Angular, React, Spring Boot, security, accessibility, performance, and commits. Teams can adopt the layout without inventing a private file format.

**AI Champion candidacy.** This repository is working evidence for an internal AI Champion application: how to standardize Copilot for a team, how to keep instruction context small, and how to distribute that standard inside an organization that rejects external or unvalidated registries. The distribution comparison is in [docs/distribution-strategies.md](docs/distribution-strategies.md). Rules for context size are in [docs/token-context-management.md](docs/token-context-management.md).

## Use each file type

Place `.github/` at the root of the application repository, or distribute it with a method from [docs/distribution-strategies.md](docs/distribution-strategies.md). Supported Copilot clients discover the paths below from the repository root.

| Path | When it loads | How to use it in a project |
| --- | --- | --- |
| `.github/copilot-instructions.md` | On every request in the repository | Keep only rules that apply to every stack. This file has no frontmatter. |
| `.github/instructions/*.instructions.md` | When `applyTo` matches a file in the request, or when `description` matches the task | One concern per file. Frontmatter requires `description` and `applyTo` (a glob, or comma-separated globs). |
| `.github/prompts/*.prompt.md` | When a person runs it as a slash command (`/generate-component`) | Encode a repeatable workflow. Link the instruction file. Leave the rules in that file. |
| `.github/agents/*.agent.md` | When a person selects the agent, or passes `--agent` to Copilot CLI | Define a review persona, a blocking checklist, a suggestion checklist, and the output table. |
| `.github/skills/<name>/SKILL.md` | When the request matches `description`, or when a person invokes the skill | Ship a self-contained procedure. `name` must match the folder name. |

Custom agents are the current name for what used to be chat modes. GitHub renamed them in October 2025. Store them in `.github/agents/`. The retired path is `.github/chatmodes/`.

Slash-command prompts remain the documented `.prompt.md` format. VS Code Agent Host sessions load skills rather than prompt files, so `generate-component` and `migrate-to-signals` also exist as skills. Those skills carry their own steps because a skill is loaded on its own.

Format references:

- [Custom instructions](https://code.visualstudio.com/docs/copilot/customization/custom-instructions) (`.github/copilot-instructions.md`, `.github/instructions/*.instructions.md`)
- [Prompt files](https://code.visualstudio.com/docs/copilot/customization/prompt-files) (`.github/prompts/*.prompt.md`)
- [Custom agents configuration](https://docs.github.com/en/copilot/reference/custom-agents-configuration) (`.github/agents/*.agent.md`)
- [Agent skills](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills) (`.github/skills/<name>/SKILL.md`)

## External sources

Treat these as the upstream references for the instruction files. When they change, update the matching file in this repo. Do not paste their guides into Copilot context.

| Stack | Source | Take from it |
| --- | --- | --- |
| Copilot customization | [github/awesome-copilot](https://github.com/github/awesome-copilot) | Instructions, prompts, agents, and skills that already use the official layout |
| Angular | [angular.dev](https://angular.dev) | Standalone components, signals, and change detection |
| React | [react.dev](https://react.dev) | Server Components, client boundaries, and hooks |
| Spring Boot | [spring.io](https://spring.io) | Layered structure, dependency injection, and testing guides |
| Security | [OWASP Top 10](https://owasp.org/www-project-top-ten/) | Current web-application risk categories |

## Layout

```text
.github/
  copilot-instructions.md
  instructions/*.instructions.md
  prompts/*.prompt.md
  agents/*.agent.md
  skills/<name>/SKILL.md
docs/
  token-context-management.md
  distribution-strategies.md
```
