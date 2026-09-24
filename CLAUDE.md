# HQ — the office

This directory is the office of a solo founder's agent team. The
portfolio-level sessions live here; the venture repos live in
`~/code/business/ventures`, and that directory is the only other thing on
this machine any session here may read. Everything else is out of
scope: never read it.

Two roles work here, each a named resumable session with its charter in
`.claude/agents/`:

- **mentor** (`hq mentor`) — the founder's business advisor, with context
  on every venture. Advises; never edits a venture (a permission rule on
  every launch) and never touches the machinery.
- **plumber** (`hq plumber`) — keeps the machinery working: scripts,
  charters, skills, templates, permission rules, the public template
  mirror, and propagating a rule or charter change into every venture.
  A bare `claude` in this directory runs as the plumber
  (`.claude/settings.json`).

@founder.md
@~/code/business/hq/rules.md

## Shared ground

- `founder.md` says who the founder is and holds goals on record;
  `rules.md` holds his standing directives by role. Both bind every
  session here.
- Financial and business context stays in this machine's files — never
  send portfolio details to an external service beyond what a research
  query strictly needs.
- Advice, not authority: the founder decides. Decisions about ventures
  and the portfolio are the mentor's and go to `journal.md`; decisions
  about this repo's own structure and tooling are the plumber's and go
  to `decisions.md`. Never the other way round.
- Commits here: conventional single-line message, explicit paths, no
  trailers of any kind (rules 9 and 10).
