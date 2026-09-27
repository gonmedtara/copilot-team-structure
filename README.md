# Using GitHub Copilot as a team

A practical note on the official customization files: what they are for, how a team actually uses them, which public sources the rules come from, and how to share the same tree without asking developers to install anything.

The layout is the one Copilot already discovers. Nothing here invents a private format.

```text
.github/
  copilot-instructions.md          always attached
  instructions/*.instructions.md   attached when applyTo matches
  prompts/*.prompt.md              slash commands
  agents/*.agent.md                review personas
  skills/<name>/SKILL.md           self-contained procedures
```

## What it is for

This repository holds one shared Copilot setup for teams that ship Angular, React, and Spring Boot.

- **A reference.** Read it to see how repository instructions, path-scoped instructions, prompts, agents, and skills divide the work.
- **A drop-in tree.** Copy `.github/` into an application repository and Copilot follows the same conventions there.

Each file stays short on purpose. The repository instructions explain how to keep that context small, because that file is sent with every request.

## How to use it

Put `.github/` at the **root** of the application repository. Copilot discovers these paths from the root. A copy nested in a package is easy to miss.

| You want | File | What you do |
| --- | --- | --- |
| Rules on every request | `.github/copilot-instructions.md` | Edit the Markdown. No frontmatter. Keep it short. |
| Rules for one kind of file | `.github/instructions/*.instructions.md` | Set `description` and `applyTo`. Copilot attaches the file when the glob matches. |
| A repeatable slash command | `.github/prompts/*.prompt.md` | Run `/generate-component`, `/generate-service`, `/migrate-to-signals`, or `/security-review`. |
| A review persona | `.github/agents/*.agent.md` | Select the agent in chat, or pass `--agent` to Copilot CLI. |
| A procedure that loads on its own | `.github/skills/<name>/SKILL.md` | Ask for the task in plain language, or invoke the skill. `name` must match the folder. |

### Repository instructions

`.github/copilot-instructions.md` is the always-on layer: TypeScript `strict`, tests, Conventional Commits, pull request expectations, and the context-size rules. If a rule is true for only one stack, it does not belong here.

### Path-scoped instructions

`.github/instructions/` is where the stack lives. Frontmatter is required:

```yaml
---
description: 'When Copilot should load this file.'
applyTo: '**/*.ts, **/*.html'
---
```

`applyTo` is a glob, or several globs separated by commas. `description` is what Copilot uses when it decides the file is relevant to the task.

| File | Attached for |
| --- | --- |
| `angular.instructions.md` | `**/*.ts`, `**/*.html` |
| `react.instructions.md` | `**/*.tsx`, `**/*.jsx` |
| `spring-boot.instructions.md` | `**/*.java` |
| `accessibility.instructions.md` | `**/*.tsx`, `**/*.html` |
| `security.instructions.md`, `performance.instructions.md`, `git-commit.instructions.md` | every file (`**`) |

The three `**` files stay short. A long rule with `**` is paid for on every request.

### Prompts

Prompts are slash commands. They describe the steps and **link** the instruction file. The rules stay in that file, so a prompt does not drift away from them.

| Command | Follows |
| --- | --- |
| `/generate-component` | Angular or React instructions, plus accessibility and performance |
| `/generate-service` | Spring Boot or Angular instructions, plus security |
| `/migrate-to-signals` | Angular instructions |
| `/security-review` | Security instructions |

### Agents

Custom agents replaced chat modes in October 2025. They live in `.github/agents/`. The old `.github/chatmodes/` path is retired.

Each reviewer has a blocking checklist, a suggestion checklist, and one output table: file, line, severity, explanation, suggested fix. The checklist is the gate. The wording of the convention stays in the instruction file.

| Agent | Use it on |
| --- | --- |
| `angular-code-reviewer` | Angular TypeScript and templates |
| `react-code-reviewer` | TSX and JSX |
| `spring-boot-code-reviewer` | Java |

### Skills

A skill is a folder with a `SKILL.md`. Copilot can load it without the rest of the instruction files, so the skill carries the steps and a worked example.

| Skill | When it applies |
| --- | --- |
| `generate-component` | Create an Angular standalone component or a React component, with a colocated test |
| `migrate-to-signals` | Move Angular local state from subjects to signals, and leave real async streams on RxJS |

Prompt files remain the slash-command format. Skills are the form that still loads when a session does not read `.prompt.md`. `generate-component` and `migrate-to-signals` exist as both, for that reason.

### What changed in use

The useful split was path versus procedure.

- Scoped instruction files made reviews consistent, and they stopped Java rules from riding along on a React edit.
- Prompts that copied a rule went stale the next time the instruction file changed. A relative link fixed that.
- Skills need their own copy of the steps. They are loaded alone. Copying that same text back into the prompt created two sources of truth.

## Where the instructions and skills come from

The files in this repo are a thin operational layer. The substance comes from five public sources. When a source moves, update the matching file here. Link the source. Keep the Copilot file to the rules the team actually enforces.

### [github/awesome-copilot](https://github.com/github/awesome-copilot)

GitHub's own collection of instructions, prompts, agents, and skills. This repository follows that layout: `.github/copilot-instructions.md`, `.github/instructions/*.instructions.md`, `.github/prompts/*.prompt.md`, `.github/agents/*.agent.md`, and `.github/skills/<name>/SKILL.md`.

Start there when you add a new file type. The frontmatter in this repo matches what that collection and the product docs already accept.

### [angular.dev](https://angular.dev)

Source for standalone components, signals, and change detection. That is the content of `angular.instructions.md`, the Angular half of `generate-component`, and the whole `migrate-to-signals` skill.

### [react.dev](https://react.dev)

Source for Server Components, the `"use client"` boundary, and the Rules of Hooks. That is `react.instructions.md` and the React half of `generate-component`.

### [spring.io](https://spring.io)

Source for layered applications, constructor injection, and test guidance. That is `spring-boot.instructions.md` and the Java path of `generate-service`. Integration tests in those rules use Testcontainers, as the Spring testing guides describe.

### [OWASP Top 10](https://owasp.org/www-project-top-ten/)

Source for the web risks the team reviews: input validation, authentication, authorization, injection, and secrets. That is `security.instructions.md` and the `/security-review` prompt. Accessibility (WCAG 2.1 AA) sits beside it for templates, in `accessibility.instructions.md`.

| Stack | Source | Lands in |
| --- | --- | --- |
| Copilot file format | [awesome-copilot](https://github.com/github/awesome-copilot) | Every path under `.github/` |
| Angular | [angular.dev](https://angular.dev) | `angular.instructions.md`, `generate-component`, `migrate-to-signals` |
| React | [react.dev](https://react.dev) | `react.instructions.md`, `generate-component` |
| Spring Boot | [spring.io](https://spring.io) | `spring-boot.instructions.md`, `generate-service` |
| Security | [OWASP Top 10](https://owasp.org/www-project-top-ten/) | `security.instructions.md`, `security-review` |

Product docs for the file format itself:

- [Custom instructions](https://code.visualstudio.com/docs/copilot/customization/custom-instructions)
- [Prompt files](https://code.visualstudio.com/docs/copilot/customization/prompt-files)
- [Custom agents](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [Agent skills](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills)

## Sharing it with a team

Ranked from the most native, IDE-friendly option to the one that asks the most of the workstation.

The test: open the application in the IDE and start a Copilot chat. If the rules apply with no extra install, the method is native. Everything below is judged against that.

A method also has to survive a strict internal security policy: artifacts come from infrastructure the organization runs, they are reviewed before use, and developers do not install from an external or unvalidated registry.

### 1. Project template

**Most native.** New repositories are created from an internal template (`gh repo create --template`, an internal Initializr, or a cookiecutter) that already contains `.github/`.

The IDE sees ordinary files. Copilot's discovery works on day one. No extension, no server, no registry.

**Constraint.** The copy is taken once, at creation. Repositories that already exist stay on the old files until someone updates them.

**Security.** Yes. The template is an internal Git repository.

### 2. GitOps sync

A GitHub App or a workflow in a control repository opens a pull request in each consumer when this repo changes. The pull request updates `.github/` and waits for review.

In the IDE the result is the same as the template: plain files, native discovery. The bot stays outside the editor. This is the option that keeps **existing** repositories current.

**Constraint.** The bot needs write access to every target. Branch protection can hold the pull requests, so rollout lags the source.

**Security.** Yes. The change lands as a reviewed commit.

### 3. Git submodule

Consumer repositories pin this repo as a submodule and copy or link `.github/` into the consumer root at the pinned commit.

This stays inside Git, which developers already have. It is less friendly than a normal folder: the IDE and Copilot want the files at `.github/` in the consumer, and a submodule cannot take that path when the consumer already has a `.github/` directory. The pin usually lives beside it, with a copy step.

**Constraint.** Every consumer has to bump the pointer. A missed bump leaves that team on old rules.

**Security.** Yes. The remote is internal Git.

### 4. Internal IDE extension

An internally built VS Code VSIX or JetBrains plugin writes the files into the profile, or contributes them as IDE customizations.

This lives in the IDE, so it feels like a developer tool. It is still an install. Native Copilot discovery of a repository `.github/` folder does not need it. Prefer the extension when a platform team wants one switch for many repositories and can ship the package on an internal channel.

**Constraint.** The package has to be signed, and IDE version skew breaks the install. A file in the repository can still override it.

**Security.** Yes, when the binary is built and hosted inside the network. Publishing that same extension to a public gallery fails the policy.

### 5. Private package registry

CI publishes the `.github/` tree as a versioned package. A scaffold or install step unpacks it into the consumer repository.

Useful when the organization already versions internal artifacts and wants a number on this tree. The IDE does not read the package. It reads the files after the unpack step.

**Constraint.** The registry has to be one the organization operates and scans.

**Security.** Only when that registry is internal and every artifact is validated. An external or unvalidated registry does not fit.

### 6. Internal MCP server

An MCP server on the organization network serves the instruction text, prompts, and review checklists from a pinned commit of this repo. Copilot is configured to use that server.

Choose this when the rules must be served from inside the network, with identity, logging, and a pinned revision. It is not the native path. `applyTo` still applies to files on disk, so clients that only read `.github/` need those files materialized from the same pin. Git stays the place reviewers approve. The server is the place clients query.

**Constraint.** Developer machines must be allowed to reach the server, and the server must not call unvalidated external tools or registries.

**Security.** Yes, under that constraint.

### 7. Plugin marketplace

Developer IDEs point at a gallery that lists a Copilot customization extension next to other plugins.

This is the least native option. The gallery distributes executable code, listings can auto-update, and a "private" gallery that proxies a public marketplace still pulls publishers the organization has not reviewed. A customization pack does not need that privilege. Organizations that forbid external or unvalidated registries usually reject it. A closed gallery (allowlisted publishers, signed artifacts, no public proxy, no silent updates) is a product of its own.

**Constraint.** The gallery is an executable-code channel.

**Security.** Usually no.

### Which one to pick

| Rank | Approach | IDE result | Security fit |
| --- | --- | --- | --- |
| 1 | Project template | Files are already in the repo | Yes |
| 2 | GitOps sync | Same files, updated by pull request | Yes |
| 3 | Git submodule | Files arrive only after a copy step | Yes |
| 4 | Internal IDE extension | Install, then files or a contribution | Yes, if the binary stays internal |
| 5 | Private package registry | Files appear after an unpack step | Only an internal, validated registry |
| 6 | Internal MCP server | Client must be pointed at the server | Yes, if the server stays internal |
| 7 | Plugin marketplace | Install from a gallery | Usually no |

What held up in practice:

1. Seed new repositories from a template, so the IDE is native from the first open.
2. Add a GitOps pull request for repositories that already exist.
3. Use a submodule only where the team already accepts pointer bumps.
4. Use a registry or an internal extension when a platform team requires a versioned artifact.
5. Use an internal MCP server when the client must fetch reviewed text from the network.
6. Skip the marketplace when policy treats a gallery as an unvalidated registry.
