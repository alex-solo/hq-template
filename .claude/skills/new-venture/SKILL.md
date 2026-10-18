---
name: new-venture
description: Stage 2 of a new venture (`hq new`) — read the idea's ideas.md entry, ask only the setup questions it leaves open, then scaffold the venture with the entry as its founding brief
---

You are running the new-venture setup. The argument (if any) is the
proposed directory name. Stage 1, the mentor's business grilling
(`idea-intake`), has already settled what the venture is; this stage
never asks again what its entry answers. Ask in the `grilling` style:
one question per message, lettered suggested answers, a recommendation.

## 1. Find the entry

- Read `ideas.md` and find the idea's entry (by the name given, or ask
  which idea, offering the `stage: ready` entries as the choices).
- **No entry, or one not `ready`:** stop and offer two answers:
  (a, recommended) run stage 1 with the mentor first (`hq mentor`, say
  which idea to start), then come back; (b) skip it. Only his explicit
  skip goes on: then ask what he wants to build (he usually dictates a
  rough description), and the founding brief is that description,
  headed "Business intake skipped by the founder, YYYY-MM-DD."
- Tell him in three lines what you took from the entry: the shape, the
  product in a sentence, the cost cap.

## 2. Setup questions

Only these, and only what the entry doesn't answer:

1. **Team shape.** Default **analyst + builder + designer**. A venture
   with no screens he will use (a backend API) drops the designer. A
   third role is proposed only for a genuinely distinct recurring
   context: a separate agent earns its existence when its conversation
   would pollute or be polluted by another one, *and* it recurs.
   Cosmetic roles are bloat.
2. **Source material.** What exists — chats, spreadsheets, notes,
   screenshots? The entry's Source material section may already list
   it. Share links (claude.ai, ChatGPT) are unreadable to agents: he
   exports transcripts and downloads artifacts, and they go into the
   new repo's `docs/samples/` as files.
3. **Directory name**, if none was given: propose one (short,
   kebab-case).

Product questions (screens, scope) and the stack are not asked here:
the analyst's kickoff asks them, starting from the founding brief.

## 3. Scaffold

- Write the entry, exactly as it stands, to a file in your scratchpad
  and run `bin/newventure <name> "<one-line thesis from the entry>"
  --brief <that file>` via Bash. It creates the repo with the entry as
  `founding-brief.md`, CLAUDE.md skeleton, TEAM.md, BACKLOG.md,
  design-constitution.md stub, the agents, the SessionStart hook, roster
  registration and the ventures.md stub.
- Then tailor it:
  - Replace the venture CLAUDE.md `## Project` section with a faithful
    2–4 sentence distillation of the brief (compressed, not
    embellished).
  - A dropped designer: remove its line from `team.conf` and from the
    venture CLAUDE.md team line, and adjust the ventures.md stub's team
    line. An extra role: create `<venture>/.claude/agents/<role>.md`
    (mirror the analyst/builder style: frontmatter with name,
    description and tools scoped to the role, then a short charter),
    add a `<name>-<role>|<dir>|--agent <role>` line to `team.conf`, and
    list the role in the venture CLAUDE.md team section.
  - The new venture's `.claude/` and `CLAUDE.md` are inside the
    plumber's write scope; nothing else in the repo is. Commit with
    `git -C`.
  - Ask him to drop the source material into `<venture>/docs/samples/`
    now. If a share link was the only form, wait for the export: the
    analyst's kickoff reads that directory first.
- Commit the venture repo and the hq changes (conventional single-line
  messages).
- Message `mentor`: the venture is scaffolded from the named entry, so
  it graduates the entry (`idea-intake` step 5).

## 4. Close

Tell him: the venture exists, which sessions were registered, and that
`hq team <name>` brings them up. Then what happens without prompting:
the analyst's kickoff asks the product questions the brief leaves open,
one at a time, then brings him the technical design (the stack with its
reasons, a diagram, and every service with its monthly cost) as a link.
The builder starts on the stack once he approves it, while the analyst
goes on to research, the customer map and spec v1, which he reviews as
a whole.
