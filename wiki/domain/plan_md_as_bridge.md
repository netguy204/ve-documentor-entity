---
title: PLAN.md as literate-programming bridge
description: PLAN.md is a prose bridge between two states of the codebase. Its primary value is verification — it reveals whether the goal constrained the solution tightly enough.
created: 2026-04-25
updated: 2026-04-25
---

# PLAN.md as literate-programming bridge

The conceptual model for PLAN.md, refined by the operator. Every doc that
references planning should conform to this framing.

See: [[chunk_intent_ownership]] for the broader chunk model;
[[../identity]] for voice posture.

## What it is

A literate-programming bridge between two states: the codebase as it
stands when the chunk is created, and the codebase that satisfies the
goal. The plan walks the path from one to the other in prose, explaining
each step's reasoning rather than just listing what to do.

"Literate programming" is the operative word. The plan is a narrative
about a transformation, not a TODO list. Each step has reasoning;
references to existing code paths, prior decisions, and subsystem
invariants are inline rather than implied.

## Why it exists: verification

The primary value of PLAN.md is **verification, not instruction.** When
the agent writes the plan, it's thinking through the goal in a form a
human reviewer can read.

Two reads of a plan:

- **Plan goes somewhere surprising** → the goal didn't constrain the
  solution tightly enough. The plan reveals the gap before any code gets
  written. Tighten the goal; regenerate the plan.
- **Plan produces no surprises** → the goal was specific enough. Proceed
  to implementation.

This is the feedback loop on goal quality. Without PLAN.md, an
underspecified goal silently becomes code that solved a problem you didn't
have. With it, the misalignment surfaces at the cheapest moment to fix.

## Why it doesn't outlive the chunk

PLAN.md is point-in-time. The "from-state" it bridges (the codebase as
it stood when the chunk was created) drifts the moment any other chunk
ships. The plan's narrative becomes stale by the second day. Once the
chunk reaches `ACTIVE`, the plan has done its job; the architectural
record lives in GOAL.md, which is maintained, while PLAN.md is left as
archaeology.

This is why the chunk lifecycle has only one document that evolves
(GOAL.md) and one that doesn't (PLAN.md).

## What this changes for documentation

**Don't say:**
- "PLAN.md is briefing notes for the agent." (Inverts the direction. The plan is the agent's *output*, not its *input*.)
- "The plan tells the agent how to implement the goal." (Frames the agent as a worker reading a script. The agent generates the plan; the human reviews it.)
- "PLAN.md surfaces what the agent needs to know before writing code." (Same inversion.)

**Do say:**
- "PLAN.md is a literate-programming bridge between two states of the codebase."
- "The plan is how the agent thinks through the goal in a form a human can verify."
- "Its value is verification: a surprising plan means the goal was underspecified."
- "Reading the plan tells you whether the goal constrained the solution tightly enough."

## How this connects to the goal

GOAL.md and PLAN.md are not parallel artifacts. They have asymmetric roles:

- GOAL.md owns intent. Present tense. Maintained as code evolves.
- PLAN.md owns the bridge. Past-state to goal-state. Discarded after crossing.

A weak plan often points to a weak goal. The fix is upstream: refine the
goal, regenerate the plan. Don't try to make the plan more specific while
leaving the goal vague.
