# HQ — an agent team for a solo founder

A small team of named, persistent [Claude Code](https://code.claude.com)
sessions that run a portfolio of side businesses with you: a **mentor**
that holds the whole picture, a **plumber** that keeps the machinery
working, an **analyst + builder** pair per venture, and a **designer**
shared across ventures. Files are the team's memory,
git is its history, and every session resumes exactly where it left off.

```
git clone https://github.com/alex-solo/hq-template ~/code/business/hq && ~/code/business/hq/hq onboard
```

Contents: [The problem](#the-problem) · [The shape](#the-shape) ·
[Why it's built this way](#why-its-built-this-way) ·
[Working with it](#working-with-it) · [Getting started](#getting-started)
· [Commands](#commands) · [Files](#files) · [Making it yours](#making-it-yours)

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
│   ├── CLAUDE.md                  house rules for the office (generic)
│   ├── .claude/agents/            mentor.md, plumber.md — the charters
│   ├── founder.md                 who you are, goals on record   (private)
│   ├── ventures.md                portfolio registry             (private)
│   ├── ideas.md                   parking lot with challenges    (private)
│   ├── journal.md                 mentor's decision log          (private)
│   ├── team.conf                  session roster                 (private)
│   ├── accounts.md                external accounts registry     (private)
│   ├── rules.md                   portfolio-wide founder rules   (private)
│   ├── hq, bin/                   the commands
│   ├── skills/                    shared skills (linked per project)
│   ├── designer/                  the designer's bundle
│   └── example/, templates/       what the private files look like
└── ventures/
    ├── CLAUDE.md                  imports rules.md; shared team protocol
    ├── <venture-a>/               its own git repo
    │   ├── CLAUDE.md                this venture's rules and files
    │   ├── TEAM.md                  the venture's shared journal
    │   ├── design-constitution.md   binds the designer here
    │   └── .claude/                 agents/ (analyst, builder), skill links
    └── <venture-b>/ …
```

### The roles

| Session | Lives in | Does | May not |
|---|---|---|---|
| **`mentor`** | `hq/` | Business and entrepreneurship advisor with context on every project. Reads every venture repo, challenges ideas, keeps the registry, journal, and kill criteria. Records your goals when you state them and weighs advice against them. | Edit anything under `ventures/` (permission-enforced). Touch the machinery — that's the plumber. |
| **`plumber`** | `hq/` | Keeps the machinery working: scripts, charters, skills, templates, permission rules, the public template mirror. Propagates a rule or charter change into every venture's `.claude/` and `CLAUDE.md`, and keeps charters and memory free of repeats (`hq tidy`, and a `/tidy` sweep when a week of work calls for one). A bare `claude` in `hq/` runs as the plumber. | Write the mentor's books (`founder.md`, `ventures.md`, `ideas.md`, `journal.md`, `accounts.md`). Touch a venture's code, specs, docs, or secrets (permission-enforced). |
| **`<venture>-analyst`** | that venture | Product thinking: discussion → decision → spec edit with a version bump → brief to the builder. | Touch code. Its shell is doc-scoped. |
| **`<venture>-builder`** | that venture | Implements the cited spec section, tests, commits, pushes. Pushes back through the analyst when a spec is wrong. | Silently deviate from the spec. |
| **`designer`** | `ventures/` | One designer for all ventures — one accumulated taste. Design systems, screen specs, prototypes; opens built UI in a browser and reviews it before presenting. | Write production code (that's the builder's). Read anything outside `ventures/`. |

Every role is a Claude Code session with a fixed identity: a charter
file (`.claude/agents/<role>.md`), a session name other
sessions can message, a memory directory, and a fixed session UUID so
`hq team <venture>` resumes it rather than starting over.

### Two levels

**Venture level** — an analyst + builder pair per venture, launched
together with `hq team <venture>`. They share `TEAM.md` as a journal and
message each other by name (`<venture>-analyst` → `<venture>-builder`).
Everything they know about the product lives in that repo's specs.

**Portfolio level** — the mentor, the plumber, and the designer, launched
alone with `hq mentor` / `hq plumber` / `hq designer`, never bundled into
a venture launch. They read every venture but belong to none. The venture `CLAUDE.md` files
deliberately don't know the portfolio's goals: analysts reason about
the product, the mentor reasons about allocation.

## Why it's built this way

- **Files are the memory, not the chat.** Specs, `TEAM.md`, `ventures.md`,
  `journal.md`: every session reads the relevant files on start (a hook
  injects the tail of `TEAM.md`, or the journal for the mentor), and anything decided gets written down
  before the session ends (`/handoff`). Closing a terminal loses nothing.
- **One home per fact.** Rules live in `rules.md`, how a role works in
  its charter, procedures in skills, product truth in specs, work in
  flight in `TEAM.md`. Agent memory keeps only what has none of those
  homes, so nothing is said twice and nothing contradicts. `hq tidy`
  checks this mechanically; when a week of real work has passed, the
  plumber suggests a `/tidy` sweep for what needs judgment. Nothing runs
  on a clock.
- **Separation is enforced, not requested.** The mentor's read-only
  access to ventures is a permission rule, not a sentence in a prompt.
  The analyst's shell is scoped to documents. The designer physically
  can't see anything outside `ventures/`. The plumber's writes into a
  venture stop at `.claude/` and `CLAUDE.md`. A role can't drift into
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
- **Judgment, not procedure.** Rules in `rules.md` say what the founder
  wants; agents apply them with judgment. Within their remit they
  decide, record, and move on. What genuinely needs you comes one
  question at a time, with a proposed default and the reasoning.

## Working with it

A new venture: `hq new` runs a short intake (what you want to build,
team shape, and your source material — exported chats and files into
`docs/samples/`, since share links are unreadable to agents). Then
`team` brings the pair up and the analyst runs its kickoff unprompted:
a stack chosen for this venture (prior stacks are candidates, never
defaults), research memos, a full spec v1 draft, and a builder brief.
The builder starts on the stack the moment it lands. Your first job is
reacting to the spec draft as a whole.

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
6. **Tooling, charters, skills → `hq plumber`.** A new command, a
   charter change, a skill every agent should have: the plumber builds
   it, documents it, mirrors it to the template, and propagates it into
   every venture. Venture sessions never relay tooling requests.
7. **Before closing any session: `/handoff`** — a role-tagged entry in
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
   learns), writes `team.conf` with the mentor, plumber and designer,
   creates `ventures/CLAUDE.md`, adds the `hq` and `team` aliases
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
- **`hq team <name>`** — the venture's sessions. On the first launch,
  Claude Code asks to approve an *external import*: `ventures/CLAUDE.md`
  imports `hq/rules.md`, which sits outside the venture's directory. Approve it. Declining disables the portfolio rules in that
  venture, and the dialog doesn't come back.

Your clone is your private hq. Point `origin` at a private remote of
your own and commit your journal there. Template updates come in with
`git pull <template-remote> main`; they never conflict with the private
files because those don't exist upstream.

## Commands

- **`hq`** (or `hq help`) — list commands.
- **`hq mentor`** — your advisor, launched alone. The mentor sits above
  the venture teams (it advises you, not them), so it has its own command
  and is never bundled into a team launch. Launched with its charter
  (`--agent mentor`) and its extra permission file
  (`.claude/settings-mentor.json`: no writes under `ventures/`) on every
  start, resume included.
- **`hq plumber`** — the tooling session, launched alone. Owns the
  scripts, charters, skills, templates, permission rules, and the public
  template mirror; propagates a rule or charter change into every
  venture's `.claude/` and `CLAUDE.md`. Any bare `claude` opened in
  `hq/` is the same role without a name (`agent: plumber` in
  `.claude/settings.json`); `hq plumber` is the named, resumable one
  other sessions can message. Charter: `.claude/agents/plumber.md`.
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
- **`hq publish`** — mirror the shareable files of this hq into a
  public template checkout for others to clone (`bin/publish` documents
  the allowlist and the leak check; `--check` runs the leak test only).
- **`hq tidy`** — the mechanical hygiene check: banned words, memory
  indexes out of step with their files, links to missing memories,
  auto-memory switched back on, `TEAM.md` tails holding entries from
  outside the team, broken skill links — and whether a `/tidy` sweep is
  due (a week since the last *and* work since). Prints only problems;
  runs after every hq commit and at the start of each plumber session.

`team` is a shell alias for `hq team` — so `team <venture>` works too.

## Files

- `CLAUDE.md` — house rules shared by every session in `hq/`: what the
  office is, the two roles, boundaries. Generic; it imports `founder.md`
  and `rules.md`.
- `.claude/agents/` — the two hq-level charters, `mentor.md` and
  `plumber.md`, loaded with `--agent`. Memory per role in
  `.claude/agent-memory/<role>/`.
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
  every session's `--model opus` lives there, and the designer's
  `--plugin-dir`, since a bare `claude --resume` keeps the transcript's
  model.
- `accounts.md` — registry of external accounts that already exist
  (API providers, hosting, DNS, messaging), where each credential lives
  (a path or variable name, never a value), and standing spend
  approvals. Venture agents read it before asking you for an account.
  Private.
- `rules-changelog.md` — history of `rules.md`, kept out of it so the
  history never loads into a session. Private.
- `rules.md` — the founder's standing rules, dated, one file for the
  whole portfolio: `ventures/CLAUDE.md` imports it for every venture
  and the designer, so a rule
  stated in one venture binds all of them the same day. Venture files
  and agent memories keep venture-specific facts only. Private.
- `templates/` — the empty private files `setup` creates on a fresh
  clone (headers and format only).
- `example/` — fictional, filled-in `founder.md`, `ventures.md`,
  `ideas.md`, `journal.md`, `accounts.md`, `rules.md`: reference for
  the shape, never copied into place. Fiction on purpose — never a scrubbed copy of real files.
- `designer/` — the designer's bundle (a Claude Code plugin directory):
  charter, vendored skills, MCP config. To add a skill: copy its folder
  into `designer/skills/` after reading it, commit. A skill that fetches
  its instructions from a URL at run time gets that content vendored and
  pinned instead (see `web-design-guidelines/SKILL.md` for the pattern).
  To update `frontend-design`: re-copy from github.com/anthropics/skills.
- `bin/` — the scripts behind `hq` subcommands.
- `skills/` — the shared skills, one copy each, linked (relative
  symlinks) into the projects that use them: `hq/.claude/skills/`,
  `ventures/.claude/skills/` for the designer, and each venture's
  `.claude/skills/`. They load only in team sessions, never machine-wide.
  Roster: `/handoff` (team protocol; prunes absorbed entries);
  `/kickoff` (a new venture's first analyst session); `/xref`
  (identity changes, hq only); `/grilling` (design-tree
  interview, rounds with recommended answers); `/wizard` (bash walkthrough
  for founder-gated setup steps); `/diagnosing-bugs` (tight-loop
  discipline); `/retro` (agent-environment retrospective, user-invoked);
  `/wait-what` (re-pitch in plain language, user-invoked);
  `/spec-review` (two-axis diff review — standards + spec — on parallel
  sub-agents; complements the bundled `/code-review` bug hunt; in
  builder sessions the two together are the pre-deploy audit, run on
  Fable);
  `writing-for-agents` (style guide for steering files and skills).
  Several adapted from [mattpocock/skills](https://github.com/mattpocock/skills) (MIT).
- `.claude/skills/new-venture/` — the `hq new` intake interview
  (hq-only, so it stays project-level).
- `.claude/settings.json` — permission rules shared by every hq
  session: read `~/code/business/ventures`; never write a venture's
  code, specs, docs, design files, secrets, or another agent's memory.
  Sets `agent: plumber` so a bare `claude` here is the plumber.
  Machine-independent (`~/`).
- `.claude/settings-mentor.json` — the mentor's extra rule, passed on
  every mentor launch: no writes under `ventures/` at all. Kept
  separate because a deny beats an allow in Claude Code, so a blanket
  deny in the shared file would fence the plumber out too.
- `.team-state/` — first-launch markers (gitignored). Delete a marker to
  force `hq team` to create that session fresh instead of resuming.
- `research/` — the plumber's research memos (rule 12) behind
  structural decisions: model policy, steering-document reviews. Private
  — they name ventures and spend.

## Making it yours

- **The mentor's stance** is `.claude/agents/mentor.md`. Adversarial-by-default,
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
- **Models** — set per session in `team.conf`'s every-launch field.
  Use the floating alias (`--model opus`): it tracks the recommended
  version as Anthropic ships new ones. A full ID (`claude-opus-5-5`
  format) freezes that exact version — use it only when you
  deliberately want to pin, and note why, or it will be forgotten.
  Every session is pinned to `opus`; an unpinned session runs on the
  account default, which may be the most expensive tier. Builder
  charters carry a three-tier policy: Opus for planning and review,
  Sonnet sub-agents for mechanical work (Haiku for read-only
  exploration), Fable sub-agents only for surprises (lasting decisions,
  stalled debugging, unresolvable spec ambiguity) and the pre-deploy
  audit — Fable is the strongest tier, and a fresh context is part of
  what those two uses buy. Analysts draft kickoff artifacts on a Fable
  sub-agent.
- **Skills** — drop a folder into `skills/` and link it into the
  projects that need it (`hq plumber` does this). Read any third-party skill before installing it; a skill is
  instructions your agents will follow.
- **Tooling changes** — `hq plumber` (or any bare `claude` in `hq/`).
  The mentor never touches scripts, and venture sessions never relay
  them. The plumber's charter carries the five-step protocol (script in
  `bin/`, register in `hq`, document here, commit, `/xref` after
  renames) and the propagation rule (edit each venture's `.claude/` and
  `CLAUDE.md`, never overwrite; fix the scaffold
  in the same change). A command exists only when all five are done.
  Routing a tooling request through an analyst costs that venture a
  relaunch.

## How continuity works (the short version)

Sessions persist on disk and resume by a fixed UUID — closing a terminal
loses nothing. Durable knowledge lives in files (specs, `TEAM.md`
journals, this repo), which every session reads on start; live
coordination happens by named cross-session messages. When a session's
context gets long, it's summarized and the session carries on.
