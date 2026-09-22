# HQ — an agent team for a solo founder

A small team of named, persistent [Claude Code](https://code.claude.com)
sessions that run a portfolio of side businesses with you: a **mentor**
that holds the whole picture, an **analyst + builder** pair per venture,
and a **designer** shared across ventures. Files are the team's memory,
git is its history, and every session resumes exactly where it left off.

```
git clone https://github.com/alex-solo/hq-template ~/code/business/hq && ~/code/business/hq/hq onboard
```

Contents: [The problem](#the-problem) · [The shape](#the-shape) ·
[Why it's built this way](#why-its-built-this-way) ·
[Working with it](#working-with-it) · [Getting started](#getting-started)
· [Commands](#commands) · [Files](#files) · [Making it yours](#making-it-yours)
· [Publishing](#publishing-how-this-repo-is-maintained) ·
[Decisions](#decisions-why-the-scaffolding-is-shaped-this-way)

## The problem

One Claude session doing everything for a side project works for an
afternoon and then falls apart:

- **Context evaporates.** Each session starts blank. The decision you
  made last Tuesday, the reason you rejected an idea, the state of the
  billing work — gone unless you re-explain it.
- **Roles bleed.** The same conversation that is writing code is also
  being asked whether the product should exist. It says yes; it is in
  the middle of building it.
- **Nobody holds the portfolio.** With two or three projects, no one is
  asking whether project B deserves your next ten hours more than
  project A, or whether either should be shut down.
- **The founder becomes the message bus**, copying context between
  chats by hand.

HQ is a way of arranging Claude Code so those problems are structural
rather than things you remember to manage.

## The shape

```
~/code/business/
├── hq/                          ← this repo: the office
│   ├── CLAUDE.md                  mentor charter (generic)
│   ├── founder.md                 who you are, goals on record   (private)
│   ├── ventures.md                portfolio registry             (private)
│   ├── ideas.md                   parking lot with challenges    (private)
│   ├── journal.md                 mentor's decision log          (private)
│   ├── team.conf                  session roster                 (private)
│   ├── hq, bin/                   the commands
│   ├── skills/                    machine-wide skills (symlinked in)
│   ├── designer/                  the designer's bundle
│   └── example/, templates/       what the private files look like
└── ventures/
    ├── <venture-a>/               its own git repo
    │   ├── CLAUDE.md                project rules + team protocol
    │   ├── TEAM.md                  the venture's shared journal
    │   ├── design-constitution.md   binds the designer here
    │   └── .claude/agents/          analyst.md, builder.md
    └── <venture-b>/ …
```

### The roles

| Session | Lives in | Does | May not |
|---|---|---|---|
| **`mentor`** | `hq/` | Business and entrepreneurship advisor with context on every project. Reads every venture repo, challenges ideas, keeps the registry, journal, and kill criteria. Records your goals when you state them and weighs advice against them. | Edit anything under `ventures/` (permission-enforced). Write scripts. |
| **`<venture>-analyst`** | that venture | Product thinking: discussion → decision → spec edit with a version bump → brief to the builder. | Touch code. Its shell is doc-scoped. |
| **`<venture>-builder`** | that venture | Implements the cited spec section, tests, commits, pushes. Pushes back through the analyst when a spec is wrong. | Silently deviate from the spec. |
| **`designer`** | `ventures/` | One designer for all ventures — one accumulated taste. Design systems, screen specs, prototypes; opens built UI in a browser and reviews it before presenting. | Write production code (that's the builder's). Read anything outside `ventures/`. |

Every role is a Claude Code session with a fixed identity: a charter
file (`CLAUDE.md` or `.claude/agents/<role>.md`), a session name other
sessions can message, a memory directory, and a fixed session UUID so
`hq team <venture>` resumes it rather than starting over.

### Two levels

**Venture level** — an analyst + builder pair per venture, launched
together with `hq team <venture>`. They share `TEAM.md` as a journal and
message each other by name (`<venture>-analyst` → `<venture>-builder`).
Everything they know about the product lives in that repo's specs.

**Portfolio level** — the mentor and the designer, launched alone with
`hq mentor` / `hq designer`, never bundled into a venture launch. They
read every venture but belong to none. The venture `CLAUDE.md` files
deliberately don't know the portfolio's goals: analysts reason about
the product, the mentor reasons about allocation.

## Why it's built this way

- **Files are the memory, not the chat.** Specs, `TEAM.md`, `ventures.md`,
  `journal.md`: every session reads the relevant files on start (a hook
  injects the journal tail), and anything decided gets written down
  before the session ends (`/handoff`). Closing a terminal loses nothing.
- **Separation is enforced, not requested.** The mentor's read-only
  access to ventures is a permission rule, not a sentence in a prompt.
  The analyst's shell is scoped to documents. The designer physically
  can't see anything outside `ventures/`. A role can't drift into
  another's job by being asked nicely.
- **Discussion → decision → record.** Every role challenges before it
  complies, the founder decides, and the decision plus any dissent goes
  into the spec or the journal. Dissent on record beats silent agreement,
  and nothing gets re-litigated because the rejection is written down.
- **The mentor is allowed to say "nothing".** A portfolio review with
  one venture and an empty parking lot must not manufacture a second
  venture to feel useful. "Stay on the current path" is always an option.
- **Kill criteria are written early.** Every venture carries them in
  `ventures.md`, set before serious building, so scrapping is mechanical
  rather than a judgment call anyone can be talked out of.
- **Ideas need evidence to enter.** An entry in `ideas.md` cites something
  external — real complaints, existing paid tools and their prices, job
  postings — and carries its own strongest case against. Ranking happens
  in batch reviews, never the moment an idea occurs. This applies to the
  founder's own long-held ideas most of all.
- **Ground truth over recollection.** Before opining on a venture, the
  mentor reads its specs, journal, and `git log`. It never relies on what
  it remembers or on what the founder summarizes from memory.
- **Capital-light by default.** The mentor works the boring numbers —
  realistic pricing, reachable market, churn, support burden,
  time-to-first-dollar — unless you tell it otherwise.
- **One question at a time.** Every role asks one concrete question with
  a proposed default, never a list.

## Working with it

A typical week:

1. **`hq team <venture>`** after any reboot — tabs open (tmux or iTerm),
   each session exactly where it left off. Re-running is safe: running
   agents are detected and skipped.
2. **Talk product with the analyst.** It pushes back, you decide, it
   batches the decision into the spec with a version bump and changelog
   entry, appends a `TEAM.md` entry, and messages the builder a brief
   citing the spec section.
3. **The builder picks up the brief**, reads the cited section (not the
   summary), implements, tests, commits. If the spec proves wrong in
   practice it messages the analyst rather than deviating.
4. **UI work starts → `hq designer`.** It reads the venture's
   design constitution, establishes tokens before screens, gets your
   sign-off, produces specs and prototypes in the venture repo, and
   after the builder implements, screenshots the result at phone and
   desktop widths and sends QA findings back.
5. **Portfolio questions → `hq mentor`.** Is this venture worth the
   next ten hours? Should the new idea enter the parking lot? Has a
   kill criterion fired? It reads the repos first, then argues. What's
   decided goes in `journal.md`; the registry in `ventures.md` stays
   current.
6. **Before closing any session: `/handoff`** — a role-tagged entry in
   the journal that session owns. That entry is what the next session
   (or the other roles) wakes up knowing.

Cross-session messages are live coordination; the journal entry is the
durable fallback. If a session isn't running, the entry *is* the handoff.

## Getting started

**Prerequisite:** Claude Code. The whole system is built on it — named
resumable sessions, agent charters, skills, permission rules — so there
is no ChatGPT or provider-neutral path. Also useful: Node (the designer's
browser tooling runs via `npx`), and tmux (`brew install tmux`) for tabs
that survive closing the terminal; without it, iTerm is used.

```
git clone https://github.com/alex-solo/hq-template ~/code/business/hq && ~/code/business/hq/hq onboard
```

The clone must live at `~/code/business/hq` — every script assumes it.
`hq onboard` does two things:

1. **`bin/setup`** (mechanical, idempotent, re-run any time): checks
   prerequisites, creates `~/code/business/ventures`, creates the private
   files from `templates/` (empty — the mentor fills them in as it
   learns), writes `team.conf` with the mentor and designer, symlinks
   `skills/` into `~/.claude/skills/`, adds the `hq` and `team` aliases
   to your shell rc.
2. **Opens your first mentor session.** It knows nothing about you yet.
   There is no interview: tell it what you want, when you want. Goals
   are optional — when you state one it records it in `founder.md`,
   dated, in your words. `example/` shows what the private files look
   like once they've filled in.

Then, in a new shell:

- **`hq new <name>`** — a short interview ("What do you want to build?",
  answer with a rough description), a proposed team shape (analyst +
  builder unless the venture genuinely needs a third role), and a
  scaffolded repo under `ventures/<name>` registered in `team.conf` and
  `ventures.md`. Let the mentor challenge the thesis before serious
  building starts.
- **`hq team <name>`** — the venture's sessions.

Your clone is your private hq. Point `origin` at a private remote of
your own and commit your journal there. Template updates come in with
`git pull <template-remote> main`; they never conflict with the private
files because those don't exist upstream.

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
  `web-design-guidelines`), and its browser tooling (`.mcp.json` —
  Playwright MCP, version-pinned, isolated profile). Nothing in the
  bundle loads in any other session. Memory:
  `ventures/.claude/agent-memory/designer-designer/` (not in git). Other
  sessions message it as `designer`.
- **`hq team <venture>`** — one command after any reboot: launches or
  resumes that venture's working team (analyst, builder, any extra
  roles), each exactly where it left off. `hq team all` launches every
  venture's team (an explicit keyword so a slip of the enter key can
  never stand up thirty ventures' agents); bare `hq team` just lists the
  known ventures. Uses tmux if installed, otherwise one iTerm window
  with a tab per agent. Re-running is safe: agents already running are
  skipped, never duplicated.
- **`hq new [name]`** — start a new venture the intelligent way: opens a
  short Claude interview, proposes the agent-team shape (default:
  analyst + builder; a third role only if the venture genuinely needs
  one), then scaffolds `~/code/business/ventures/<name>` tailored to it
  — repo, CLAUDE.md seeded from your description, journal,
  design-constitution stub, agents, hooks — and registers everything in
  `team.conf` and `ventures.md`. The interview protocol lives in
  `.claude/skills/new-venture/`.
- **`hq new --bare <name> ["thesis"]`** — skip the interview; scaffold
  the default analyst + builder shape mechanically (`bin/newventure`).
- **`hq onboard`** — first run on a fresh clone: `setup`, then your
  first mentor session (see Getting started).
- **`hq setup`** — the mechanical half alone; idempotent, re-run after
  pulling template updates that add skills.
- **`hq publish`** — mirror the shareable files into the public template
  repo (see Publishing). `hq publish --check` runs the leak test only.

`team` is a shell alias for `hq team` — so `team <venture>` works too.

## Files

- `CLAUDE.md` — the mentor's charter. Generic; it imports `founder.md`.
- `founder.md` — who the founder is, plus any goals they've put on
  record (optional; the mentor records them when told). Private; starts
  empty.
- `ventures.md` — portfolio registry, one section per venture: thesis,
  stage, revenue state, kill criteria, the mentor's current read.
- `ideas.md` — parking lot; ideas enter with evidence and a recorded
  challenge, and rejected ones stay with reasons.
- `journal.md` — mentor's dated decision log; its tail is injected into
  every mentor session at start.
- `team.conf` — session roster (`name|dir|first-launch flags|every-launch
  flags`), hand-editable. The optional 4th field is repeated on resume —
  the designer's `--plugin-dir` lives there, and a `--model` pin would
  too, since a bare `claude --resume` keeps the transcript's model.
- `templates/` — the empty private files `setup` creates on a fresh
  clone (headers and format only).
- `example/` — fictional, filled-in `founder.md`, `ventures.md`,
  `ideas.md`, `journal.md`: reference for the shape, never copied into
  place. Fiction on purpose — never a scrubbed copy of real files.
- `designer/` — the designer's bundle (a Claude Code plugin directory):
  charter, vendored skills, MCP config. To add a skill: copy its folder
  into `designer/skills/` after reading it, commit. A skill that fetches
  its instructions from a URL at run time gets that content vendored and
  pinned instead (see `web-design-guidelines/SKILL.md` for the pattern).
  To update `frontend-design`: re-copy from github.com/anthropics/skills.
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
- `.claude/settings.json` — the mentor's permission rules: read
  `~/code/business/ventures`, never edit it. Machine-independent (`~/`).
- `.team-state/` — first-launch markers (gitignored). Delete a marker to
  force `hq team` to create that session fresh instead of resuming.

## Making it yours

- **The mentor's stance** is `CLAUDE.md`. Adversarial-by-default,
  capital-light, kill criteria, evidence-bound ideas — all editable. If
  your mentor should optimize for something else, change the charter,
  not the conversation.
- **Venture roles** are the two agent files `hq new` scaffolds
  (`.claude/agents/analyst.md`, `builder.md`) plus the venture
  `CLAUDE.md`. The template is in `bin/newventure`; edit it and every
  future venture inherits the change.
- **A third role** for a venture (content, ops, research) is a
  `.claude/agents/<role>.md`, a `team.conf` line, and a mention in the
  venture's team protocol. `hq new` proposes one only when the interview
  surfaces a genuinely distinct recurring context — a separate agent
  earns its existence when its conversation would pollute or be polluted
  by another one, *and* it recurs. Cosmetic roles are bloat.
- **Models** — pin per session in `team.conf`'s every-launch field
  (`--model claude-opus-5`); aliases like `opus` float.
- **Skills** — drop a folder into `skills/`, run `hq setup` to symlink
  it. Read any third-party skill before installing it; a skill is
  instructions your agents will follow.
- **Tooling changes** — the mentor never touches scripts. Open a plain
  Claude session in `hq/`; the charter binds it to the five-step
  protocol (script in `bin/`, register in `hq`, document here, commit,
  `/xref` after renames). A command exists only when all five are done.

## Publishing (how this repo is maintained)

This public repo is a one-way **mirror** of the maintainer's private hq.
`bin/publish` copies an explicit allowlist (scripts, skills, the
designer bundle, `templates/`, `example/`, generic `CLAUDE.md`, this
README) into a checkout at `~/code/business/hq-template` (override with
`HQ_TEMPLATE`), commits there with hq's latest commit message, and
pushes. Before committing it greps the whole mirror for every token in
`.publish-deny` (names, employers, venture names — private, not mirrored)
and refuses on any hit, so the boundary is a file you can read rather
than a habit. Private files are simply not on the list.

It runs from a `post-commit` hook in the private repo, so every
structural improvement lands here within seconds of being committed.
(Hooks aren't versioned; reinstall with
`ln -s ../../bin/post-commit .git/hooks/post-commit`.) Improvements from
users arrive as pull requests here and get ported into the private repo
by hand — one-directional on purpose.

If you fork this for your own friends, the same machinery works for you:
your hq is private, `hq publish` mirrors it.

## Decisions (why the scaffolding is shaped this way)

Dated one-liners so a deliberate choice isn't undone later because it
looked redundant. Append when changing structure.

- 2026-08-18 — mentor is the `hq/` project CLAUDE.md launched as a named
  resumable session, not a subagent: it needs memory across sessions and
  its own context window.
- 2026-08-18 — venture CLAUDE.md files do NOT import the portfolio goal.
  Analysts reason about the product; portfolio allocation is the
  mentor's job only. Separation on purpose.
- 2026-08-18 — `hq/.claude/settings.json` denies Edit under
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
  (`bin/publish`) of the private repo, not the private repo made public
  with a nested private dir: the allowlist plus `.publish-deny` make a
  leak a refused command instead of a missed line, and git history stays
  private. Cost: friends' improvements are ported by hand.
- 2026-09-22 — `CLAUDE.md` imports `founder.md` rather than carrying the
  founder inline, so the charter is generic and the person is one
  private file. `example/` is fiction, never scrubbed real data.
- 2026-09-22 — no onboarding interview. The mentor is defined by having
  context on every project, not by knowing the founder's goals; goals
  are optional and recorded when stated. Private files start empty.

## How continuity works (the short version)

Sessions persist on disk and resume by a fixed UUID — closing a terminal
loses nothing. Durable knowledge lives in files (specs, `TEAM.md`
journals, this repo), which every session reads on start; live
coordination happens by named cross-session messages. When a session's
context gets long, it's summarized and the session carries on.
