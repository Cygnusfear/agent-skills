---
name: 20-05-debrief
description: Write a full-fidelity debrief after completing development work. Use after any merge, feature completion, or significant work session.
---

# 20.05 Debrief

Write a debrief after every significant piece of development work. This is NOT an incident report — it's a full-fidelity account of what happened during the work so future agents and humans understand the real story.

## When to Write

- After every merge (triggered by `20.04`)
- After completing a feature or significant refactor
- After any work session where decisions were made, problems were hit, or lessons were learned

**There is no threshold.** Don't skip because "nothing went wrong" or "it was straightforward." Straightforward work still has decisions, context, and lessons worth capturing.

## Create the Debrief

Create a `tk` ticket tagged `debrief` with the template below filled in.

## Template

```markdown
# Debrief: <title>

**Date:** <date>
**Branch/Feature:** <branch or feature name>
**Duration:** <rough time spent>

## Goal

What were we trying to accomplish? One or two sentences.

## What Actually Happened

Factual replay of the work in sequence. Include:
- What was tried first
- What changed along the way
- Key timestamps or commits if relevant
- Links, quotes, error messages — concrete evidence

This section is calm and factual. No analysis yet.

## Decisions Made

For each significant choice:
- **Decision:** What we chose
- **Alternatives considered:** What else was on the table
- **Why:** The reasoning at the time
- **In hindsight:** Would we choose the same? Why or why not?

## Problems Hit

What went wrong or was harder than expected:
- What the problem was
- How it manifested (error messages, unexpected behavior, confusion)
- How it was resolved
- How long it took vs how long it should have taken

## What Worked

What went smoothly:
- Tools, patterns, or approaches that were effective
- Decisions that paid off
- Things to repeat next time

## What We'd Do Differently

Hindsight improvements — not blame, but honest reflection:
- Process changes
- Different technical approaches
- Better preparation or research
- Things that were over-engineered or under-engineered

## One Binding Improvement

Pick exactly ONE concrete, actionable improvement. Not five aspirational ideas — one thing that will actually get done.

- **What:** <specific change>
- **Who:** <person or agent responsible>
- **When:** <deadline or trigger>
```

## Rules

1. **Write with full fidelity.** Don't sanitize, don't summarize into nothing. The goal is that someone reading this in 3 months understands what really happened.
2. **Blameless.** Describe what happened and why, not whose fault it was. People (and agents) make decisions under uncertainty — document the context, not the blame.
3. **One binding improvement.** Many postmortems produce a list of 10 action items that nobody does. Pick one. It must actually happen. If you're motivated to do more, great — but only one is required.
4. **Do it now.** Don't defer "until later" — memory fades fast. The best debrief is written immediately after the work while context is fresh.
5. **Include the ugly parts.** Dead ends, wrong approaches, wasted time, confusion — these are the most valuable parts for future readers. A debrief that only describes success is useless.
