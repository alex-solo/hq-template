---
name: kickoff
description: A new venture's first session — the analyst's unprompted path from founding brief to a builder holding an approved stack and a brief. Use when a venture repo holds only its founding brief and docs/samples/.
---

Rule 16, step by step. Run it without being asked. The founder is
asked in steps 2 and 3 and for gated items (rule 7), nowhere else.

1. **Intake.** Read `founding-brief.md` and everything in
   `docs/samples/`. If `docs/samples/` holds nothing but its README,
   ask once whether there is material to add, then go on with whatever
   he answers.
2. **Product questions.** Grill him (the `grilling` skill: one question
   at a time) on what the brief leaves open about the product: who uses
   it, every screen he will open (rule 17), what v1 holds and what
   waits. Never ask what the brief or the samples answer. The answers
   seed the spec.
3. **Technical design** — before any code, so the stack is chosen for
   the product, not ahead of it.
   - **Research.** One fresh `opus` sub-agent per load-bearing choice,
     in parallel: frontend, backend, database, auth, hosting, AI calls
     and background jobs, email, and any other outside service the
     product needs. Each compares the main options against current
     primary sources (rule 12) and returns a pick with its reasons. The
     default is the popular, modern, widely used choice for a web app;
     anything else needs a reason from this product's requirements,
     written down. Other ventures' stacks are candidates, never
     defaults.
   - **The memo**, `docs/research-stack.md`, is the record: the
     requirements, each choice with the alternatives and why it won,
     every service (what it is for, whether the account exists per
     `accounts.md` or is new, its monthly cost against the target in
     `founder.md`), and the risks.
   - **Builder review.** Message the builder to review the memo: it
     builds on it and lives with it. Each disagreement goes in the memo
     as an open question for him, not settled quietly.
   - **The page**, `docs/technical-design.html`: one HTML file, no build
     step. A diagram of the pieces and how they connect (app, server,
     database, jobs, outside services), the services table with
     monthly costs and the new accounts marked, and each choice in one
     line with why it beat the alternatives. It shows the memo; when
     they differ the memo wins and the page is redrawn. Send it as an
     `hq preview` link (rule 36).
   - **Discussion.** Take his challenges one at a time and revise the
     memo and page. His go-ahead covers the new accounts and the spend
     the page lists (rule 7): record it in `TEAM.md` the same day,
     saying which services and amounts. Then message the builder: it
     scaffolds on the stack, proves the build and deploy path, and
     wires auth and the platform shell while the spec matures.
     Features wait for the spec; the foundation does not.
   - While he reviews the page, go on with step 4.
4. **Research memos** in `docs/`: the domain; competitors' features and
   pricing.
5. **Customer map** — `customers.md` at the root, every part rule 31
   lists, built on the step 4 research. Places are verified, never
   guessed; the outreach messages and interview script are ready for
   him to send as they stand.
6. **Spec v1** — every section filled, each decision with its
   rationale, a build order, and the items gated on the founder. Written
   so a blank-context agent can build from it (rule 24).
7. **Hand off** — the build order as `ready` items in `BACKLOG.md`
   (rule 28), a `TEAM.md` entry, and a message to the builder.

Draft the spec on a fresh `opus` sub-agent (`model: opus`) given the
brief, the step 2 answers and `docs/samples/` in full; review, edit,
and own the result.

Done when the builder holds an approved stack and a brief. The founder
then reviews the spec as a whole and reacts; revise from that.
