---
name: xref
description: Cross-reference audit — use ONLY after identity changes (path moves, session/command renames, reversed documented semantics), scaled to blast radius; never for normal multi-file work
---

Run this after any change that alters an identity or a behavior other
files might reference: a path move, a rename (session, command, file,
concept), a command's semantics changing, a template edit. The goal:
zero files left contradicting the new reality.

**Scale to blast radius.** This is not a fixed ceremony: a renamed
function with three call sites needs one grep-and-fix and you are done —
steps 3–5 apply only when commands, docs, or templates were actually
part of the change. Reserve the full procedure for restructures (moves,
roster renames, command-surface changes). Normal multi-file work
(features, fixes, doc edits) should never invoke this skill at all.

## 1. Derive the hunt list from the change itself

List every OLD token the change retired — don't guess from memory, read
the actual diff (`git diff` / `git log -p` of the change): old paths
(every historical form, not just the latest), old names, old command
invocations, old flag syntax, and short phrases stating the old behavior
(e.g. "plus the mentor", "launches everything").

## 2. Grep the whole estate for every old token

Scope: both business repos (hq + all ventures), `~/.zshrc`, and the
Claude project memory dirs (`~/.claude/projects/*/memory/`). Exclude
`.git/`. For each hit, one of exactly two outcomes:
- **update it**, or
- **consciously exempt it** — journal/changelog/TEAM.md *entries* record
  history and must NOT be rewritten to match the present. Headers and
  standing text in those same files are not exempt.

A grep that comes back empty on filenames alone proves nothing — match
on file *content*, and beware filters that exclude by path when you
meant to exclude by content (a real mistake from the audit this skill
was born from).

## 3. Docs-vs-behavior parity

If a command or script changed, verify three things agree: the script's
actual behavior, its `--help`/dispatcher text, and every doc describing
it (README, CLAUDE.md sections, agent charters, skill files). Read them
side by side — don't assume a doc is right because it was written today.

## 4. Templates get executed, not eyeballed

If anything that *generates* files changed (scaffold templates, heredocs),
generate a real throwaway output, read the generated files, then remove
the throwaway completely — including registry side effects (roster
lines, ventures.md entries). Escaping bugs (`$VAR` swallowed by a quoting
layer) are invisible in the template source and obvious in the output.

## 5. Contradiction sweep

For the concept that changed, grep its *current* keywords too, and read
a few lines around each hit: is any surrounding statement now false,
even if no old token appears? (A sentence can contradict new behavior
without containing any renamed string.)

## 6. Close

Fix everything found, re-run the greps until clean, syntax-check any
touched scripts (`bash -n`, `jq -e`), then commit — audit fixes in their
own commit(s) with a message saying what was reconciled. Report what was
found and fixed versus already-clean.
