---
name: tidy
description: The plumber's judgment sweep of every agent's steering files and memory — duplicates, contradictions, stale state, restated rules. Run when bin/tidy says a sweep is due and the founder agrees.
disable-model-invocation: true
---

The mechanical half is `bin/tidy`; this is the half that needs judgment.
Scope: `rules.md`, hq `CLAUDE.md` and charters, `ventures/CLAUDE.md`,
each venture's `CLAUDE.md`, `.claude/agents/`, `.claude/agent-memory/`,
and the tail of its `TEAM.md`; the designer's charter and memory; every
skill. Ventures are `team.conf` entries, never worktrees or deploy
checkouts.

1. Run `bin/tidy` and fix everything it prints.
2. Read every memory file. Each must hold a non-obvious lesson for its
   role in its venture, or a pointer — nothing that `rules.md`, a
   charter, a skill, a spec, or `TEAM.md` already says (rule 26).
   Translate any quote of the founder into plain language (rule 26).
   Delete restatements; delete project state (it belongs in specs and
   `TEAM.md`); delete anything written as fact that the founder only
   floated, and any guess about what he did or thinks. Keep the index
   in step.
3. Read the charters and `CLAUDE.md` files against `rules.md`: a
   sentence that restates a rule becomes a rule number or goes; a
   sentence that contradicts one is fixed toward the rule.
4. A general founder preference found in one venture's memory is a
   rule left in the wrong place (rule 8). Do not write it into
   `rules.md` yourself — list it for the founder.
5. Write today's date (`date +%F`) to `.team-state/last-sweep`.
6. Commit each repo with explicit paths and push. Record a removal in
   `decisions.md` only when it changes how the system works.

Done when `bin/tidy` prints nothing and every memory file passes step
2. Report once, in a few lines: what was removed, and any item from
step 4 that needs the founder. When nothing was found, say so in one
line.
