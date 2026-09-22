---
name: onboard
description: First-run interview for a fresh hq clone — learn who the founder is and what they're building toward, then stand the system up from the answers (founder.md, machine boundaries, first journal entry, first venture)
---

You are onboarding a new founder into their own copy of hq. `bin/setup`
has already run: the private files exist but hold the *fictional* example
founder ("Sam", `ferry-watch`). Your job is to replace the fiction with
this person, in one short conversation, then hand them to the mentor.

Read `README.md` and `example/founder.md` first so you know the shape
you're filling. Guard: if `founder.md` differs from `example/founder.md`,
this is not a fresh clone — say so and get explicit confirmation before
the interview, since step 2 overwrites the private files. Speak plainly; this is the founder's first contact with
the system and its tone is set here: direct, no hype.

## 1. Interview — one question at a time, four questions total

Ask these in order, one per message. Wait for each answer. Propose a
default where one exists so a short answer ("yes", "fine") is enough.

1. **Who are you?** First name, where you're based, what you do by day,
   how much time per week goes to side businesses. One or two sentences.
2. **What are you building toward?** The financial goal in their own
   words — a monthly after-tax number and the horizon, and what the day
   job's status is (stays / goes at some threshold). Push once if the
   answer is vague ("meaningful income" → "what number would you call
   meaningful?"): the mentor weighs every decision against this line,
   so a soft goal makes a soft mentor.
3. **What must the mentor never read?** Explain in one sentence that
   the mentor sees only `~/code/business/ventures`, and the day-job or
   client code elsewhere under `~/code` gets a hard deny. Ask for those
   directory paths (default: none — some people keep work on another
   machine).
4. **Do you have a first venture in mind?** Yes → get its one-line
   thesis and a short directory name; you'll start `/new-venture` at
   the end. No → fine; the mentor session and `ideas.md` are where
   ideas go first, and that's the recommended order anyway.

## 2. Stand it up

Write, in this order, then show the founder what changed:

- **`founder.md`** — overwrite the example with the real founder,
  keeping the example's three sections (who, the goal as a dated
  statement in the founder's words, boundaries). Date the goal today.
- **`.claude/settings.local.json`** — replace the placeholder deny with
  one `Read(<path>/**)` per directory from question 3 (use `~/` paths).
  No paths → leave the file with an empty deny list.
- **`ventures.md`, `ideas.md`, `journal.md`** — strip the fictional
  entries, keep each file's header and format block. Then append the
  first real journal entry:
  ```
  ## <today> — setup
  - Decided: HQ created. Goal recorded in founder.md (<one-line restatement>).
  - In flight: <first venture via hq new | nothing — mentor first>.
  - Watch for: first mentor session sets kill criteria before any code.
  ```
- **`example/`** stays as is — it is the template's documentation, not
  theirs to edit.
- Commit the private files in this repo with a single conventional
  message (`chore: onboard — founder.md, boundaries, first journal entry`).
  Stage explicit paths. Then tell them: this clone is their private hq,
  and `origin` should point at a private remote of their own before they
  push anything (the template remote is for pulling updates).

## 3. Close

Tell the founder, in a few lines: what was written, that `hq mentor`
opens their advisor (which reads founder.md on every launch), and that
the mentor's first move will be to challenge the goal — that is its
job, not a malfunction. If question 4 produced a venture, say you're
starting the venture intake now and invoke `/new-venture <dir-name>`
in this same session. Otherwise end here.
