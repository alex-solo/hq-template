---
name: new-venture
description: Interview the founder about a new venture idea, propose the right agent-team shape, then scaffold the venture tailored to it
---

You are running the new-venture intake. The argument (if any) is the
proposed directory name. Keep the whole exchange short — this is a
lightweight intake, not a spec session.

## 1. Interview

- First question, always: **"What do you want to build?"** The founder
  will answer with a rough description, often a ~60-second voice note.
- Second, always: **source material.** "What already exists — chats,
  spreadsheets, notes, screenshots?" Share links (claude.ai, ChatGPT)
  are unreadable to agents, so say so: he exports the transcript and
  downloads any artifacts, and they go into the new repo's
  `docs/samples/` as files. Everything pasted, nothing linked.
- Then at most one or two follow-ups, and only ones that would change
  the scaffolding — e.g. is there a heavy recurring non-product workstream
  (content production, ops/compliance, research) distinct from
  building and product thinking? Don't ask about features, market, or
  tech stack — that's the analyst's and mentor's job later.
- If no directory name was given, propose one (short, kebab-case).

## 2. Propose the shape

- Default is **analyst + builder** — recommend it unless the interview
  surfaced a genuinely distinct recurring context. The rule (same one the
  whole system is built on): a separate agent earns its existence only
  when its conversation would pollute or be polluted by another one, AND
  it recurs. Cosmetic roles are bloat.
- Design is already covered: the portfolio `designer` (HQ-level,
  `hq designer`) serves every venture, bound by the
  `design-constitution.md` stub the scaffold creates. A venture-level
  design role is a duplicate, whatever the interview surfaces.
- If a third role is warranted, propose it with a one-line charter and
  say honestly what it does that analyst/builder can't. Ask the founder
  to confirm the shape (one question, 2–3 options max).

## 3. Scaffold

- Run `bin/newventure <name> "<one-line thesis distilled from the
  interview>"` via Bash. This creates the base: repo, CLAUDE.md skeleton,
  TEAM.md, design-constitution.md stub, analyst + builder agents, the
  SessionStart hook, roster registration, ventures.md stub.
- Then tailor it:
  - Replace the venture CLAUDE.md `## Project` section with a faithful
    2–4 sentence distillation of the founder's description (his words,
    compressed — don't embellish).
  - For each extra agreed role: create
    `<venture>/.claude/agents/<role>.md` (mirror the analyst/builder file
    style: frontmatter with name/description/tools scoped to the role's
    actual needs, then a short charter), add a
    `<name>-<role>|<dir>|--agent <role>` line to `team.conf`, and list
    the role in the venture CLAUDE.md team-protocol section.
  - The new venture's `.claude/` and `CLAUDE.md` are inside the
    plumber's write scope; nothing else in the repo is (hq settings deny
    code, docs, design, memory). Commit with `git -C`.
  - Update the venture's entry in `ventures.md` if the shape is
    non-default, so the mentor knows the team it's advising.
  - Ask the founder to drop the source material into
    `<venture>/docs/samples/` now (the scaffold created it). If a share
    link was the only form, wait for the export — the analyst's kickoff
    reads that directory first, and an empty one costs a day.
- Commit the venture repo and the hq changes (conventional single-line
  messages).

## 4. Close

Tell the founder: the venture exists, which sessions were registered,
that `team` will bring the new panes up, and what then happens without
prompting — the analyst runs its kickoff (stack chosen for this venture,
research memos, spec v1 draft, builder brief) and the builder starts on
the stack the moment `docs/research-stack.md` lands. His job is to react
to the spec draft as a whole and answer the gated items. The mentor sets
kill criteria with him in parallel, before the first feature ships.
