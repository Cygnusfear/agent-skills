---
name: 20-04-post-merge-hygiene
description: Post-merge cleanup — document implementation, write debrief, update docs, close tickets, clean worktrees and branches.
---

# 20.04 Post-Merge Hygiene (Clean Up)

## Instructions:

Now in `local main`:
- document the full (feature) implementation
- update all other relevant documentation
- **write a debrief** (ALWAYS — this is not optional, see below)
- close relevant `tk` tickets and ensure verification + Worktree are recorded
- clean up worktree and branches
- 🐲

## Debrief (MANDATORY)

Every merge gets a debrief. This is NOT a post-incident report — it's a full-fidelity account of the work that was done. Create a `tk` ticket tagged `debrief`.

Include:
- **What was the goal** — what were we trying to do
- **What actually happened** — the real sequence of events, not the happy path
- **Decisions made** — what choices came up, what did we pick, why
- **Problems hit** — what went wrong, what was harder than expected
- **What worked** — what went smoothly, what should we do again
- **What we'd do differently** — hindsight, not blame

Write with full fidelity. Don't sanitize, don't summarize into nothing. Future agents and humans need to understand what really happened here.
