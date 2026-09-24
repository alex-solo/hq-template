---
name: handoff
description: Append a role-tagged handoff entry to TEAM.md before ending a work block — what was decided/done, what's in flight, what other roles must know
---

Append one entry to `TEAM.md` (at the bottom, newest last) in this exact
format, then confirm to the user in one line:

```
## YYYY-MM-DD — <role>
- Did/decided: <the substance of this work block, compressed; cite spec
  sections or commits rather than restating them>
- In flight: <anything started but not finished, or "nothing">
- Other roles should know: <cross-role facts only — decisions that change
  another session's work, gotchas, state that lives outside the repo;
  or "nothing">
```

Rules:
- `<role>` is this session's role: builder, analyst, designer, mentor,
  or plumber.
- No filler. If the block produced nothing worth handing off, say so and
  write nothing.
- Don't duplicate what specs/commits already record — reference them.
- If the entry contains an action for another role and that session is
  running, also send it a short message (SendMessage) pointing at the entry.
