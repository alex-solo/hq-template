---
name: retro
description: "Conduct a retrospective on a work session or work block: propose improvements to the agent environment (CLAUDE.md, skills, hooks, checks), not the product."
disable-model-invocation: true
---

> Adapted from [mattpocock/skills](https://github.com/mattpocock/skills)
> (MIT), rewritten for the hq team structure: per-venture CLAUDE.md +
> living specs, machine-wide skills in hq/skills, TEAM.md journals,
> role-based sessions. There is no separate reviewer agent or
> CODING_STANDARDS.md here — judgement rules live in CLAUDE.md, and
> mechanical rules should become checks.

The user has asked for a **retrospective**. You are suggesting improvements to the agent **environment** to improve future runs — steering files, checks, tooling, information access. Product improvements belong in specs, not here.

## Steps

1. Call the Skill tool with `writing-for-agents` for the writing style guide (it governs any steering-file edit you propose).

2. Read the primary sources for the period the user specifies: the session transcript, TEAM.md entries, git log, agent-memory files. If the user doesn't specify, default to the current session.

3. Look for candidates for improvement in these categories.

- **Navigation**: how easily did the agent find the right files and facts? Would a pointer in CLAUDE.md or an agent-memory entry have saved a search? _Use when_ the session burned time locating something.
- **Automated checks**: did the agent make a mistake that typecheck, a test, a lint rule, or a hook could have caught? A **mechanical** rule (banned API, file-location rule, import shape) should become a deterministic check — a test, a hook in settings.json, a CI step — not a CLAUDE.md sentence. Reserve CLAUDE.md for genuine **judgement calls** no check can substitute for. Default to building the check over writing the rule.
- **Steering-file no-ops**: hunt CLAUDE.md (venture and hq) and skill bodies for instructions the agent already obeys by default, stale layers, and duplicated meaning. The test is behavioral: would deleting the line change anything? Long steering files are a standing cost on every turn.
- **Tool economy**: expensive or repetitive tool calls that a script, a skill, or a saved memory would collapse. (Example from the field: prod-DB one-liners repeated across sessions became an agent-memory pattern.)
- **Information access**: what did the agent NOT have eyes on that it needed? Read-only API tokens, teed logs, a dashboard endpoint. (Example from the field: a 7-day outage invisible until observability was mandated.)
- **Wizard candidates**: founder-gated manual steps that were narrated ad hoc in chat this session — each is a candidate for a `/wizard` script next time.
- **Memory hygiene**: should something surprising from this session become an agent-memory entry — or should a stale one be corrected or deleted?

4. Present the candidates to the user in order of severity, each with the concrete edit (which file, which line, which check). Implement only what the user approves; steering-file edits follow `writing-for-agents`.
