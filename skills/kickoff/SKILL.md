---
name: kickoff
description: A new venture's first session — the analyst's unprompted path from founding brief to a builder holding a stack and a brief. Use when a venture repo holds only its founding brief and docs/samples/.
---

Rule 16, step by step. Run it without being asked, and ask the founder
nothing beyond gated items (rule 7) and facts only he holds.

1. **Intake.** Read everything in `docs/samples/`. If it holds nothing
   but its README, ask once whether there is material to add, then go
   on with whatever he answers.
2. **Stack memo** — `docs/research-stack.md`, chosen from this
   venture's requirements. Other ventures' stacks are candidates, never
   defaults; every load-bearing choice is checked against primary
   sources (rule 12). The moment it exists, message the builder and
   add a `TEAM.md` entry: the builder scaffolds on it, proves the build
   and deploy path, and wires auth and the platform shell the brief
   names while the spec matures. Features wait for the spec; the
   foundation does not.
3. **Research memos** in `docs/`: the domain; competitors' features and
   pricing.
4. **Spec v1** — every section filled, each decision with its
   rationale, a build order, and the items gated on the founder. Written
   so a blank-context agent can build from it (rule 24).
5. **Hand off** — a `TEAM.md` entry and a builder brief citing the spec.

Draft 2 and 4 on a `fable` sub-agent (`model: fable`) given the brief
and `docs/samples/` in full; review, edit, and own the result.

Done when the builder holds a stack and a brief. The founder then
reviews the spec as a whole and reacts; revise from that.
