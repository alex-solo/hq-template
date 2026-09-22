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

## 1. Interview — one question at a time, three questions total

Ask these in order, one per message. Wait for each answer. Propose a
default where one exists so a short answer ("yes", "fine") is enough.

1. **Who are you?** First name, where you're based, what you do by day,
   how much time per week goes to side businesses. One or two sentences.
2. **What are you building toward?** In their own words, whatever the
   mentor should hold every decision against — a financial target, a
   launch, independence, learning a domain, a lifestyle. Don't steer it
   toward money; take what they say. Push once only if it's empty
   ("build stuff" → "and what would tell you it's working?").
3. **Do you have a first venture in mind?** Yes → get its one-line
   thesis and a short directory name; you'll start `/new-venture` at
   the end. No → fine; the mentor session and `ideas.md` are where
   ideas go first, and that's the recommended order anyway.

## 2. Stand it up

Write, in this order, then show the founder what changed:

- **`founder.md`** — overwrite the example with the real founder,
  keeping the example's two sections (who; what they're building toward,
  as a dated statement in their own words). Date it today.
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
job, not a malfunction. If question 3 produced a venture, say you're
starting the venture intake now and invoke `/new-venture <dir-name>`
in this same session. Otherwise end here.
