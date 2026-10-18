---
name: idea-intake
description: The mentor's business grilling for an idea the founder wants to build — its shape first, then the questions that shape needs — ending in a complete ideas.md entry that `hq new` builds the venture from. Use when he brings an idea he means to start, or when `hq new` finds no entry for one.
---

Stage 1 of a new venture. Stage 2 is the plumber's `/new-venture`
(`hq new`): it reads the entry this writes and never asks what the
entry answers. Ask in the `grilling` style: one question per message,
lettered suggested answers, a recommendation, and the "what I took from
that" recap after a long dictated answer. Facts are yours: research
competitors, prices and communities before asking, on sub-agents.

## 1. Read first

- `ideas.md`: is this idea, or a near twin, already there, rejected
  ones included? Start from that entry; never re-ask what it records.
- `ventures.md`: the running ventures, for the comparison in step 3.
- Whatever he hands over: a dictated description, a chat export.

## 2. Shape first

The first question is always which kind of venture this is, because
the answer decides which questions follow:

- **a) For a market** — a product sold to customers he doesn't know yet.
- **b) For one known person** — a demo or commission for someone who
  wants it; a wider market may follow.
- **c) For himself** — a tool he will use; a market may follow.

A demo for one person that might open a market is b, with the market
recorded as a hypothesis to test, not a premise.

## 3. What each shape must settle

These are branches to cover, not a script: skip what his answers or
your research already settle, and ask in the order that unblocks most.

**For a market**
- The problem and who has it: the sharpest segment first.
- Evidence they already spend money or effort on it: your research,
  cited. His own stories count as leads, not evidence.
- What they use today: competitors, substitutes, their prices.
- Price, and how the first customers are reached (warm reach first).

**For one known person**
- Who it is for, their role, and what they want it for.
- What the demo must prove to them, and by when.
- What happens if it lands: they pay, they introduce others, it becomes
  a product, or goodwill only. Say which one makes it a venture.
- The wider market: a quick check that it exists and what would test
  it, no deep research yet.

**For himself**
- What he would use it for, and what he uses now.
- What makes it worth building over the free substitute.
- Whether others share the need (a quick check), and what would make
  it a product later.

**Every shape**
- Strongest case against.
- Kill criteria: what result, by when, ends it.
- Cost cap: monthly infrastructure against the target in `founder.md`,
  plus any one-off spend.
- Against the running ventures: what it shares with them (code,
  audience, channels) and what attention it takes. A comparison, not a
  ranking: judging still happens in a batch review unless he decides
  to start now.
- Source material that exists: chats, files, notes. Stage 2 puts them
  in the new repo.

Product questions (screens, scope) and the stack are not stage 1: the
analyst's kickoff asks them, from this entry. Product detail he
volunteers goes in the entry's Idea section, so nobody asks it again.

## 4. Write the entry

After he confirms the summary, write the entry in `ideas.md` in the
format your charter sets ("How ideas enter `ideas.md`"), with `shape:`
(market / one-person / himself) in its front matter beside `stage:` and
`origin:`; `stage: ready` once every branch is covered. Sections: Idea ·
Shape (for one person: who, and what happens if it lands) · Evidence ·
Strongest case against · Price and reach (for one person: the terms) ·
Kill criteria · Cost cap · Against the running ventures · Source
material. His position and your dissent both go on record, in plain
words, never quoted.

## 5. Hand on

Tell him the entry is ready and that `hq new <name>` builds the
venture from it. When the plumber reports the venture scaffolded, the
entry graduates: its kill criteria move into the venture's
`ventures.md` section, and its body is replaced by a Disposition line
pointing at the venture's `founding-brief.md`, which holds the rest.
