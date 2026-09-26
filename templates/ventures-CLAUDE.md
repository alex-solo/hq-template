# Ventures

@~/code/business/hq/rules.md

## Team protocol (every venture)

- Each venture is worked by `<venture>-analyst` (product and specs;
  never code, doc-scoped shell) and `<venture>-builder` (code, tests,
  deploys, the pre-deploy audit). Charters: the venture's
  `.claude/agents/`.
- **mentor** (`hq mentor`) and **designer** (`hq designer`) serve every
  venture and belong to none. The designer is bound by the venture's
  design constitution; briefs come from the analyst; production code
  stays with the builder.
- Specs are the source of truth. `TEAM.md` carries work in flight and
  cross-role state only; its tail loads at session start. Run
  `/handoff` before ending a work block.
- Hand off by message to the session's name when it is running; the
  `TEAM.md` entry is the durable fallback.
