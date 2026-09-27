---
description: 'OWASP reminders for web changes: input validation, authentication, authorization, and secrets. Use on every change.'
applyTo: '**'
---

# Security

Use the current [OWASP Top 10](https://owasp.org/www-project-top-ten/) as the risk list. Apply the checks below on every web change.

- Validate and constrain input on the server. Client checks are a convenience, not a control.
- Authenticate non-public routes. Authorize on the server from the caller's identity.
- Parameterize SQL, JPQL, and command execution. Encode output for the context that renders it.
- Keep secrets out of source, tests, prompts, logs, and client bundles. Read them from the platform secret store or the environment.
- Do not log credentials, tokens, session identifiers, authorization headers, or personal data.
- Fail closed when an authentication or authorization check errors.
