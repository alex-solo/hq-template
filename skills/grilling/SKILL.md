---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea, one question at a time. Use when the user wants to stress-test their thinking, asks to be grilled, or opens a genuinely new area (a new venture, a new subsystem) where the specs are silent.
---

> Adapted from [mattpocock/skills](https://github.com/mattpocock/skills) (MIT).
> House note: once the grilling ends, decisions go where they live:
> venture decisions into the venture's spec (with changelog entry),
> portfolio decisions into hq's journal, an idea into `ideas.md` —
> never left only in the conversation.

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it. Keep the tree to yourself; the user sees one question at a time.

## One question per message

The **frontier** is every decision whose prerequisites are already settled. From it, ask the one question whose answer unblocks the most of the tree. Format:

```
**Q3 of about 8 — <short title>**

<the question, with the context needed to answer it in a sentence or two>

a) <suggested answer>
b) <suggested answer>
c) <suggested answer>

➡️ <letter> — <your recommendation, with a one-line why>
```

- Two to four suggested answers, lettered, so a reply can be "b", "b, but…", or free text.
- The count is an estimate; revise it as the tree grows or shrinks.
- Never two questions in one message, and never a question whose answer depends on one still open.

## Long answers

The user often dictates: a long answer that wanders, settles several branches at once, and answers questions you haven't asked yet. Before the next question, reply with:

```
What I took from that:
- <each decision it settled, in one line — including ones for later questions>
```

Mark every branch it settled as answered and never ask it. Flag only a real contradiction with an earlier answer, or a gap the answer opened, as the next question. If the answer was short and clear, skip the recap and ask the next question.

## Facts are yours

Finding _facts_ is your job, never the user's. When a question needs a fact from the environment (files, repos, tools, the web), dispatch a sub-agent to find it, and ask a question that doesn't depend on it while it runs. The _decisions_ are the user's: put each to them and wait.

## Done

The grilling is done when the frontier is empty: every branch visited, nothing silently assumed. Then show one summary of every decision, grouped by branch, and ask the user to confirm it. Do not act on it, or write it down as settled, until they do.
