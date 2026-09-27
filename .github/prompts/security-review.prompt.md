---
description: 'Review the selected change for input validation, authentication, authorization, and secrets.'
name: 'security-review'
argument-hint: 'Files or diff to review'
agent: 'ask'
---

Review the selected files or the current diff. Report findings. Do not modify files.

1. Follow [security.instructions.md](../instructions/security.instructions.md). Apply those rules, and do not restate the whole file in the report.
2. Inspect input handling, authentication, authorization, query and command construction, logging, and secret usage.
3. Report an issue only when you can point at a file and a line.

Output one table. One row per finding.

| File | Line | Severity | Explanation | Suggested fix |
| --- | --- | --- | --- | --- |

- Set Severity to `blocking` or `suggestion`.
- Use `blocking` for a missing auth check, injection, a secret in source, or sensitive data in logs.
- Use `suggestion` for hardening that is safe to defer.
- If there are no findings, answer `No security findings.`
