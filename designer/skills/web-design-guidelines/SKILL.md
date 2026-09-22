---
name: web-design-guidelines
description: Review UI code for Web Interface Guidelines compliance. Use when asked to "review my UI", "check accessibility", "audit design", "review UX", or "check my site against best practices".
metadata:
  author: vercel
  version: "1.0.0"
  argument-hint: <file-or-pattern>
---

> Vendored from [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills)
> (`063bee9`, 2026-08-28). Changed from upstream: the rules are the local,
> reviewed copy in [`guidelines.md`](guidelines.md) —
> [vercel-labs/web-interface-guidelines](https://github.com/vercel-labs/web-interface-guidelines)
> `command.md` at `e3d624b` (2026-08-17, MIT, see `LICENSE`) — where
> upstream fetches them from the `main` branch on every use. To update:
> re-copy `command.md`, read the diff, commit.

# Web Interface Guidelines

Review files for compliance with Web Interface Guidelines.

1. Read [`guidelines.md`](guidelines.md) — all rules and the output format.
2. Read the specified files (if none were specified, ask which files or
   pattern to review).
3. Check against every rule in the guidelines.
4. Output findings in the terse `file:line` format the guidelines specify.

Where a rule conflicts with the venture's design constitution (e.g. its
copy rules), the constitution wins; note the conflict in the findings.
