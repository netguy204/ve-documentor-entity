---
title: Chunks own intent, not implementation
description: The refined definition of what chunks are for in ve — intent ownership rather than implementation tracking. Foundation for every doc that references chunks.
created: 2026-04-25
updated: 2026-04-25
---

# Chunks own intent, not implementation

The canonical definition of "what is a chunk for" in ve, refined by the
operator in `docs/chunks/intent_principles/` (in flight at the time of this
note; landing target: `docs/trunk/CHUNKS.md`). Every doc I write that
mentions chunks must conform to this model.

See: [[../identity]] for voice posture; [[ve_cli_inconsistencies]] for
related rough edges in the surface.

## The frame: engineering isn't about writing code

The deepest reason chunks exist: **engineering isn't about writing code.
It's about making architectural choices that can hold an implementation in
productive shape over time.** Chunks are the mechanism for those choices,
and so most of the human effort of engineering with ve goes into building
good chunks. The typing gets deferred to agents.

This thesis is the prologue to every chunk-related doc. When in doubt, the
question "is this work building a chunk or just typing?" sorts which side
of the human/agent line the work belongs on.

## The four principles

1. **Code owns implementation; chunks own intent; together they are the architecture.** Code says *how* the system works — mutable, refactorable, tactical. Chunks say *why* a piece of the system has the shape it has — the constraints, contracts, and boundaries that should outlive any particular implementation. Implementation without intent is code that future contributors will accidentally break. Intent without implementation is a wish.

2. **Chunks exist only for intent-bearing work.** Intent-less work — typos, dependency bumps, mechanical renames, one-off bug patches, performance tweaks that don't change shape, comment cleanup — bypasses the chunk system entirely. Just edit the code, commit, move on. The test is a single question: *does this code need to remember why it exists?* If yes, make a chunk. If no, don't. Over-chunking drowns the architectural signal in noise.

3. **A chunk's GOAL.md describes intent in present tense.** Written so it reads true at every status the chunk passes through. Goals describe how the system works (or, for FUTURE chunks, how it will work) — never what changed, never what we did, never the world as it was. Git owns the past. The chunk goal is an architectural record, not a change log.

4. **Status answers a single question — how much of the intent does this chunk own?** Each transition is an answer to that question, not a separate concept.

## Status taxonomy

| Status | Ownership |
|--------|-----------|
| `FUTURE` | Not yet owned. Queued for later. |
| `IMPLEMENTING` | Being taken into ownership. At most one per worktree. |
| `ACTIVE` | Fully owns the intent that governs the code. |
| `COMPOSITE` | Shares ownership with other chunks. Must be read alongside its co-owners. |
| `HISTORICAL` | No longer owns intent. The approach was replaced, the code was rolled back, or the intent was abandoned. |

**`COMPOSITE` replaces the older `SUPERSEDED`.** Audit of the existing
SUPERSEDED chunks showed the old name conflated two different things:
"this chunk's approach was replaced" (the new HISTORICAL case) vs "this
chunk shares intent ownership with peers" (the new COMPOSITE).

**`COMPOSITE` is always a refactoring opportunity.** Every `COMPOSITE`
chunk indicates that two real pieces of intent share the same code. The
architectural fix is one of three: split the code so each chunk owns its
own piece, split the chunks into smaller and more atomic intents, or
merge the chunks into a single coherent intent. (Resolution tooling for
`COMPOSITE` chunks is on the project roadmap; for now, treat each
`COMPOSITE` as a chunk to revisit.) Reaching for `COMPOSITE` for
typo-fix-like work is a separate antipattern: principle 2 was violated
upstream and the chunk should never have existed.

**`/chunk-complete` is the natural catch point for `COMPOSITE`.** When a
chunk completes, the skill scans for other chunks that govern the code
this chunk just touched. If the same code is now referenced by multiple
chunks, the skill surfaces the overlap and helps the operator decide on
a resolution. Catching ambiguity at completion is cheaper than
discovering it months later when someone refactors the shared code and
accidentally breaks one of the intents.

**`HISTORICAL` chunks are always safe to delete.** They don't constrain
the architecture. Some people keep them for archaeological interest, but
that is the only reason to. Deleting all `HISTORICAL` chunks in a project
would not affect what the codebase is allowed to do or how it is allowed
to change.

**No gradient between current and archaeological.** A chunk is either
fully current (`ACTIVE` or `COMPOSITE`: the intent it describes still
governs the code) or it is archaeologically interesting (`HISTORICAL`:
the intent no longer applies, and the chunk is safe to delete). There is
no "slightly stale," "partially current," or "drifting" middle ground.
Don't write language that implies one. "How current is this chunk" is a
binary, not a slider; "how much of the architecture does it own" is the
gradient worth describing.

## What this changes for documentation

When writing about chunks, **always** frame:
- A chunk as a record of intent, not a record of work.
- GOAL.md as an architectural record (present tense, ages well, evolves with the code).
- PLAN.md as a point-in-time briefing (discarded after completion).
- Status as positions on an ownership axis, not as lifecycle aging.
- The decision to chunk as a test, not a default. Every change does not earn a chunk.

**Don't say:**
- Open the user-facing status table with "ownership" language. (The ownership axis is a synthesis on top of the chunk-intent-code relationship; readers need that relationship made concrete first. Use "intent governs code" framing in the table; layer the ownership-axis insight on top in a follow-up paragraph.)
- "A chunk is a unit of implementation work." (Sounds like a ticket. A chunk is a piece of architecture work bound to implementation that survives the implementation changing.)
- "The code that resulted." (Frames the chunk as tracking past output rather than maintaining a living architectural record.)
- "Every change is a chunk." (Violates principle 2.)
- "The chunk records what we did." (Violates principle 3 — git records what we did.)
- "SUPERSEDED" (Replaced by COMPOSITE.)
- "ACTIVE describes recently-merged work." (The "recently-merged" hedge is gone.)
- "`COMPOSITE` and `HISTORICAL` are honest states." (Feel-good and uninformative — doesn't tell the reader what to do. Use "degenerate, resolvable, realistic states" instead: technical, precise, and points at the path forward.)
- "`$ ve chunk create my_chunk`" as the operator-facing primary command. (Inverts the layering. Lead with `/chunk-create`; mention `ve chunk create` only as the CLI the skill invokes underneath. See [[skill_cli_layering]].)
- "When the agent writes a non-trivial new file or function, it adds a backreference." (Vague size-based heuristic. The criterion is the chunk-intent-code relationship: would changing this code violate what the chunk asserts? If yes, backreference. Size and novelty are irrelevant.)
- "Chunk it versus commit it." (Misleading: we commit either way. The actual contrast is between recording architectural intent in a chunk and writing the code without that record. Use "chunk it versus vibe it.")
- "The narrative is a living document; keep `OVERVIEW.md` in sync with the code as it evolves." (Wrong. Narratives are scaffolding for generating chunks. Once the chunks exist, they own the architecture; the narrative drifts and that's expected. The operator does not maintain narratives.)
- `## Problem` as a GOAL.md section header. (The label invites past-tense narrative: "Users are bounced...", "the middleware doesn't use them." Replace with `## Behavior` and write present-tense intent: "Users remain logged in until they explicitly log out.")
- "Surfaces as 401, not 500." (Implicit reference to a past-broken state. Drop the "not X" and just describe how the system behaves: "Surfaces as 401.")

**Do say:**
- "A chunk is a piece of architecture work bound to implementation." (Captures both halves: it's architecture, and it's connected to the code it governs.)
- "Maintains the architectural record as the code evolves underneath it." (The chunk is the constant; implementation is the variable.)
- "Does this code need to remember why it exists?" (The intent test.)
- "Code owns how, chunks own why." (The principle in five words.)
- "Status answers: how much of the intent does this chunk own?"
- GOAL.md is "an architectural record," not a change log.
- GOAL.md sections that describe the system in present tense: `## Behavior`, `## Success Criteria`, `## Constraints`, `## Out of Scope`. Each reads true at every status the chunk passes through.
- "`COMPOSITE` and `HISTORICAL` are degenerate, resolvable, realistic states." (Each adjective does work: *degenerate* = not the canonical clean form; *resolvable* = there's an explicit path forward; *realistic* = they actually happen, don't pretend they don't.)
- "When the agent writes code that the chunk's intent governs, it adds a backreference." (Precise: ties the criterion to the chunk-intent-code triangle. The test is whether changing the code would violate what the chunk asserts.)
- "Chunk it versus vibe it." (Captures the real contrast: both paths end in committed code; only one records architectural intent. Connects to the project's "vibe coding vs vibe engineering" framing.)
- "Narratives are scaffolding for generating chunks; the chunks are what stake out the architecture." (Captures the asymmetric maintenance: chunks endure and get maintained; narratives drift and that's fine.)
- "If you don't yet know the intent, you're not ready to write a chunk; that's what investigations are for." (The principle linking intent-as-prerequisite to investigation-as-discovery. Agents are good at interviewing ambitious-but-confused engineers until an articulable intent surfaces.)

## PRD / TDD orientation

For readers familiar with traditional product/engineering docs, the
operator's PRD/TDD comparison framing is:

| | PRD | TDD | GOAL.md | PLAN.md |
|---|-----|-----|---------|---------|
| Answers | What to build and why | How to build it | Why the code is the way it is | How to get from here to the goal |
| Lifespan | Project lifetime | Project lifetime | Evolves with the code | Discarded after completion |

GOAL.md is the architectural-intent slot that PRDs and TDDs together
don't quite fill. PLAN.md is the implementation-bridge that disappears
after the bridge is crossed.

## Where this lives in the project

- In flight: `docs/chunks/intent_principles/GOAL.md` (status: IMPLEMENTING; narrative: intent_ownership)
- Landing target: `docs/trunk/CHUNKS.md` (canonical statement, ~one screen, declarative)
- Cross-references: SPEC.md, ARTIFACTS.md, GOAL.md.jinja2 template all updated to match the new taxonomy
- Out of scope (separate chunks): migrating the 12 existing SUPERSEDED chunks; backfilling ACTIVE chunks for retrospective tells; behavioral changes to /chunk-create and /chunk-complete
