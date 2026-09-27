# Distribution strategies

This repository is the source of truth for the Copilot customization files. The table compares seven ways to get those files into application repositories and developer tools.

A method fits a strict internal security policy when the bytes come from infrastructure the organization runs, every artifact is reviewed before use, and developers do not install from an external or unvalidated registry.

| Approach | How it works | Key constraint | Fits strict internal security |
| --- | --- | --- | --- |
| Git submodule | Consumer repositories pin this repo as a submodule and copy or link `.github/` into the consumer root at the pinned commit. | Each consumer must bump the submodule pointer. A missed bump leaves that team on old rules. A submodule also cannot replace a `.github/` directory the consumer already has, so the pin usually lives beside it. | Yes. The remote is internal Git. |
| Private package registry | CI publishes the `.github/` tree as a versioned package. A scaffold or install step unpacks it into the consumer repo. | The registry has to be one the organization operates and scans. Publishing the same package to a public registry fails the policy. | Only when the registry is internal and artifacts are validated. An external or unvalidated registry does not fit. |
| Project template / scaffold | New repositories are created from an internal template (`gh repo create --template`, an internal Initializr, or a cookiecutter) that already contains `.github/`. | The copy is taken once, at creation. Existing repositories do not receive later changes. | Yes. The template is an internal Git repository. |
| Internal IDE extension | An internally built VS Code VSIX or JetBrains plugin writes the files into the user profile or contributes them as IDE customizations. | The package must be signed and updated on an internal channel. IDE version skew breaks the install, and a local file can still override it. | Yes, when the binary is built and hosted inside the network. A publish to a public gallery does not fit. |
| Internal plugin marketplace | Developer IDEs point at a private gallery that lists the Copilot customization extension next to other internal plugins. | The gallery distributes executable code. In banking and finance it is commonly rejected when any publisher is unvalidated, when listings auto-update, or when the gallery proxies a public marketplace. | Usually no. See below. |
| GitOps sync bot | A GitHub App or a workflow in a control repo opens a pull request in each consumer when this repo changes. The pull request updates `.github/` and waits for review. | The bot needs write access to every target. Branch protection can stall the pull requests, so rollout lags the source. | Yes. The change lands as a reviewed commit. No registry. |
| Internal MCP server | An MCP server on the corporate network serves the instruction text, prompts, and review checklists from a pinned commit of this repo. Copilot is configured to use that server. Clients that only read `.github/` from disk get the same files from the server or from Git, not from a gallery. | Clients must be allowed to reach the server, and the server must not call unvalidated external tools or registries. | Yes. Recommended for regulated environments. |

## Recommended approach for regulated environments

Use an internal MCP server, with this Git repository as the reviewed source.

The server runs inside the network the security team already controls. Access uses the organization's identity and network policy, and the server can log what it served and which revision it served. Updates are a new pinned commit, so teams are not left on a forgotten submodule or on a template that was copied years ago. Developers do not install a plugin from a gallery, and the customization payload is data the organization reviewed in Git.

Where a Copilot client applies `applyTo` only for files on disk, materialize those files from the same pinned revision. The MCP server remains the place clients query; Git remains the place reviewers approve. Neither step needs an external registry.

## Why a marketplace is often blocked in banking and finance

Security policy in these environments treats an IDE marketplace as an unvalidated software channel, even when the gallery is described as internal.

- Extensions run code on workstations that can reach internal networks and customer data. A customization pack does not need that privilege.
- Public galleries accept third-party publishers. A private gallery that proxies or imports those listings inherits publishers the bank has not reviewed.
- Auto-update ships new code without the change review the rest of production software goes through.
- The policy that forbids external and unvalidated registries applies to IDE galleries the same way it applies to a public npm or Maven host: provenance is unclear, and the client pulls bytes from outside the reviewed build.
- Standing up a truly closed gallery (allowlisted publishers, signed artifacts, no public proxy, no silent updates) is a product of its own. Most programs stop at "internal marketplace" and still fail the control.

Prefer the MCP server or the GitOps sync bot. Both move reviewed files. Neither asks developers to execute code from a gallery.
