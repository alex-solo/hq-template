# Ventures

@~/code/business/hq/rules.md

## Team protocol (every venture)

- Each venture is worked by `<venture>-analyst` (product and specs;
  never code, doc-scoped shell), `<venture>-builder` (code, tests,
  deploys, the pre-deploy audit) and `<venture>-designer` (design
  system, prototypes, design QA; bound by the venture's design
  constitution, briefed by the analyst; production code stays with the
  builder). Charters: the venture's `.claude/agents/`; the designer's
  is shared, in `hq/designer/`.
- **mentor** (`hq mentor`) serves every venture and belongs to none.
- Specs are the source of truth. `BACKLOG.md` is the one queue of
  what's next (rule 28). `TEAM.md` is the journal — recent work and
  cross-role state only; its tail loads at session start. Run
  `/handoff` before ending a work block.
- Hand off by message to the session's name when it is running; the
  `TEAM.md` entry is the durable fallback.
