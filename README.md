# HQ

Command center for the agent team and the venture portfolio. If you're
reading this after months away: run `hq` in a terminal to see the
commands, or start the mentor session and just ask — it reads this repo.

## Install (new machine, or a friend's)

Prerequisite: [Claude Code](https://code.claude.com) — the whole system
is built on it (named sessions, agent charters, skills, permission
rules), so there is no ChatGPT or provider-neutral path.

```
git clone <template-url> ~/code/business/hq && ~/code/business/hq/hq onboard
```

Two things happen. `bin/setup` (mechanical, idempotent, re-run any time)
checks prerequisites (Node for the designer's browser tooling, tmux or
iTerm), creates `~/code/business/ventures`, seeds the private files from
`example/` (a *fictional* founder, venture, idea, and journal), writes
`team.conf`, symlinks `skills/` into
`~/.claude/skills/`, and adds the `hq`/`team` aliases. Then a Claude
session opens with the `/onboard` interview (`.claude/skills/onboard/`):
three questions — who you are, what you're building toward, a first
venture if you have one — and it rewrites the
fictional files with your answers, commits them, and hands into
`/new-venture` if you named a venture. After that: open a new shell,
`hq mentor`.

Your clone is your private hq: point `origin` at a private remote of your
own and commit your journal there. Template updates come in with
`git pull <template-remote> main` — they never touch the private files,
because those don't exist upstream.

## The team

| Session   | Where                  | Role |
|-----------|------------------------|------|
| `mentor`  | `~/code/business/hq`                 | Portfolio advisor. Knows the founder and the goal (founder.md), challenges ideas, reads venture repos read-only. |
| `designer` | `~/code/business/ventures` (charter in `hq/designer/`) | Portfolio designer. Serves every venture: design systems, screen specs, prototypes, design QA in a real browser. Production code stays with builders. |
| `<venture>-analyst` | `~/code/business/ventures/<venture>` | Product analyst for that venture. Specs and decisions only, never code. |
| `<venture>-builder` | `~/code/business/ventures/<venture>` | Coding session for that venture. Implements specs, tests, commits. |

Each venture gets its own prefixed pair via `hq new`, plus a `design-constitution.md` stub —
the per-venture doc that binds the designer there.

## Commands

- **`hq`** (or `hq help`) — list commands.
- **`hq mentor`** — your advisor, launched alone. The mentor sits above
  the venture teams (it advises you, not them), so it has its own command
  and is never bundled into a team launch.
- **`hq designer`** — the portfolio designer, launched alone. HQ-level
  like the mentor (one session, one taste-memory across ventures), so
  never bundled into a venture's team launch. Runs with `ventures/` as
  its working directory — write access to every venture, none to hq or
  anything else — and loads its bundle from `hq/designer/` via
  `--plugin-dir`: the charter (`agents/designer.md`), its skills
  (`skills/`, vendored — Anthropic's `frontend-design`, Vercel's
  `web-design-guidelines`), and
  its browser tooling (`.mcp.json` — Playwright MCP, version-pinned,
  isolated profile). Nothing in the bundle loads in any other session.
  Memory: `ventures/.claude/agent-memory/designer-designer/` (not in
  git). Other sessions message it as `designer`.
- **`hq team <venture>`** — one command after any reboot: launches or
  resumes that venture's working team (analyst, builder, any extra
  roles), each exactly where it left off. `hq team all` launches every
  venture's team (an explicit keyword so a slip of the enter key can
  never stand up thirty ventures' agents); bare `hq team` just lists the
  known ventures. Uses tmux if
  installed, otherwise one iTerm window with a tab per agent. Re-running
  is safe: agents already running are skipped, never duplicated.
- **`hq new [name]`** — start a new venture the intelligent way: opens a
  short Claude interview ("What do you want to build?" — answer with a
  ~60-second voice note, plus at most a follow-up or two), proposes the
  agent-team shape (default: analyst + builder; a third role only if the
  venture genuinely needs one), then scaffolds
  `~/code/business/ventures/<name>` tailored to it — repo, CLAUDE.md
  seeded from your description, journal, agents, hooks — and registers
  everything in `team.conf` and `ventures.md`. The interview protocol
  lives in `.claude/skills/new-venture/`.
- **`hq new --bare <name> ["thesis"]`** — skip the interview; scaffold
  the default analyst + builder shape mechanically (`bin/newventure`).
- **`hq onboard`** — first run on a fresh clone: `setup`, then the
  `/onboard` interview (see Install).
- **`hq setup`** — the mechanical half alone; idempotent, re-run after
  pulling template updates that add skills.
- **`hq publish`** — mirror the shareable files into the public template
  repo (see Publishing). `hq publish --check` runs the leak test only.

`team` is a shell alias for `hq team` — so `team <venture>` works too.

## Files here

- `CLAUDE.md` — the mentor's charter. Generic; it imports `founder.md`.
- `founder.md` — who the founder is and what they're building toward;
  the mentor holds every decision against it. Private (seeded from
  `example/founder.md` by `setup`).
- `ventures.md` — portfolio registry, one section per venture.
- `ideas.md` — parking lot; ideas enter with a recorded challenge.
- `journal.md` — mentor's dated decision log.
- `example/` — fictional `founder.md`, `ventures.md`, `ideas.md`,
  `journal.md`: what the private files look like when filled in. `setup`
  copies them into place when the real ones don't exist. Fiction on
  purpose — never a scrubbed copy of real files.
- `team.conf` — session roster (name|dir|first-launch flags|every-launch
  flags), hand-editable. The optional 4th field is repeated on resume —
  the designer's `--plugin-dir` lives there, since a bare
  `claude --resume` would come back without its skills and browser tools.
- `designer/` — the designer's bundle (a Claude Code plugin directory):
  charter, vendored skills, MCP config. To add a skill: copy its folder
  into `designer/skills/` after reading it, commit. A skill that fetches
  its instructions from a URL at run time gets that content vendored and
  pinned instead (see `web-design-guidelines/SKILL.md` for the pattern). To update
  `frontend-design`: re-copy from github.com/anthropics/skills.
- `bin/` — the scripts behind `hq` subcommands.
- `skills/` — canonical machine-wide skills, symlinked into
  `~/.claude/skills/` so they load in every session. ONE copy each: edit
  here, git-versioned here, live everywhere instantly. Current roster:
  `/handoff`, `/xref` (team protocol); `/grilling` (design-tree
  interview, rounds with recommended answers); `/wizard` (bash walkthrough
  for founder-gated setup steps); `/diagnosing-bugs` (tight-loop
  discipline); `/retro` (agent-environment retrospective, user-invoked);
  `/wait-what` (re-pitch in plain language, user-invoked);
  `writing-for-agents` (style guide for steering files and skills).
  Several adapted from [mattpocock/skills](https://github.com/mattpocock/skills) (MIT).
- `.claude/skills/new-venture/` — the `hq new` intake interview
  (hq-only, so it stays project-level).
- `.claude/skills/onboard/` — the `hq onboard` first-run interview.
- `.team-state/` — first-launch markers (gitignored). Delete a marker to
  force `hq team` to create that session fresh instead of resuming.

## Publishing (keeping the shared template in sync)

The structure lives in this private repo and is mirrored, not shared
directly: `bin/publish` copies an explicit allowlist (scripts, skills,
the designer bundle, `example/`, generic `CLAUDE.md`, this README) into a
checkout at `~/code/business/hq-template` (override with `HQ_TEMPLATE`),
commits there with hq's latest commit message, and pushes. Before
committing it greps the whole mirror for every token in `.publish-deny`
(name, employer path, venture names — private, not mirrored) and refuses
on any hit, so the boundary is a file you can read rather than a habit.
Private files (`founder.md`, `ventures.md`, `ideas.md`, `journal.md`,
`team.conf`, `.team-state/`, memory) are simply not on the list.

Automatic: a `post-commit` hook in this repo runs `bin/publish` after
every commit (hooks aren't versioned — reinstall on a new machine with
`ln -s ../../bin/post-commit .git/hooks/post-commit`). Push failures are
non-fatal; the next commit catches up. Improvements from a friend arrive
as PRs on the template and get ported here by hand — one-directional on
purpose.

## Decisions (why the scaffolding is shaped this way)

Dated one-liners so a deliberate choice isn't undone later because it
looked redundant. Append when changing structure.

- 2026-08-18 — mentor is the `hq/` project CLAUDE.md launched as a named
  resumable session, not a subagent: it needs memory across sessions and
  its own context window.
- 2026-08-18 — venture CLAUDE.md files do NOT import the portfolio goal.
  Analysts reason about the product; portfolio allocation is the
  mentor's job only. Separation on purpose.
- 2026-08-18 — `hq/.claude/settings.json` denies Edit/Write under
  `ventures/` so the mentor (and any ad-hoc hq session) is read-only
  there; venture changes flow through the analyst/builder sessions.
  (2026-09-22: paths use `~/` so the file is machine-independent.)
- 2026-08-18 — `ideas.md` is the idea pipeline AND the graveyard (rejected
  entries stay with reasons). Split into one-file-per-idea only if it
  outgrows a single file (~6+ live entries).
- 2026-08-21 — no prospector/steward agents: idea research is an ad-hoc
  hq session following the `ideas.md` entry rules; tooling/structure
  work is an ad-hoc hq session following the Tooling-changes protocol.
  Promote either to a skill only when it recurs weekly.
- 2026-08-21 — marketing/design are trigger-based seats per venture
  (spawn at launch-prep / UI work), not standing agents.

- 2026-09-18 — SUPERSEDES the design half of the line above: design is a
  standing HQ-level `designer` session (founder decision 2026-09-15 —
  one taste-memory across ventures beats a seat per venture). Marketing
  remains a trigger-based seat.
- 2026-09-18 — designer runs from `ventures/` with its bundle passed by
  `--plugin-dir`, NOT from `hq/`: launched in hq it would inherit the
  mentor's CLAUDE.md and the deny rule on venture writes, and loosening
  that deny would un-guard the mentor. Skills are vendored into the
  bundle (read before copying) rather than installed via `npx skills` —
  no third-party installer runs on this machine, and design skills stay
  confined to this system's sessions.
- 2026-09-18 — Playwright MCP over Claude-in-Chrome for the designer: an
  isolated throwaway profile and explicit viewport control, versus an
  agent driving the founder's logged-in personal browser.

- 2026-09-22 — shareable template is a one-way allowlist mirror
  (`bin/publish`) of this private repo, not this repo made public with a
  nested private dir: the allowlist plus `.publish-deny` make a leak a
  refused command instead of a missed line, and git history stays
  private. Cost: friends' improvements are ported by hand.
- 2026-09-22 — `CLAUDE.md` imports `founder.md` rather than carrying the
  founder inline, so the charter is generic and the person is one
  private file. `example/` is fiction, never scrubbed real data.

## How continuity works (the short version)

Sessions persist on disk and resume by name — closing a terminal loses
nothing. Durable knowledge lives in files (specs, TEAM.md journals, this
repo), which every session reads on start; live coordination happens by
named cross-session messages. Full design rationale: memory note
`multi-session-agent-team` in the first venture's project memory.
