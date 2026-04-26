---
title: Skill / CLI layering pattern
description: A core ve pattern. Slash commands (skills) are the operator-facing surface that shapes intent collaboratively. CLI commands instantiate templates and manipulate state. Docs should foreground skills, not CLIs.
created: 2026-04-25
updated: 2026-04-25
---

# Skill / CLI layering pattern

A core architectural pattern in the ve codebase, refined by the operator.
Every doc that mentions chunk creation, completion, or related lifecycle
operations should respect this layering.

See: [[chunk_intent_ownership]] for the broader chunk model;
[[plan_md_as_bridge]] for the related "agent thinking made visible" idea.

## The pattern

There are two layers in how operators work with ve:

1. **Skills** (slash commands like `/chunk-create`, `/chunk-complete`).
   The operator-facing surface. The skill is where vibey operator intent
   gets refined collaboratively with the agent in light of the code, the
   trunk goal, and the surrounding context. The skill is the architecture
   work.

2. **CLI commands** (`ve chunk create`, `ve chunk complete`, etc.).
   Plumbing. Instantiates templates, validates frontmatter, manipulates
   state on disk. The agent invokes these on behalf of the skill; the
   operator rarely calls them directly.

A skill typically wraps one or more CLI commands. `/chunk-create` calls
`ve chunk create` to scaffold the directory, then drives the agent to
fill in the templated `GOAL.md` collaboratively with the operator.

## Why it matters for documentation

Docs that lead with the CLI command (`$ ve chunk create my_chunk`) imply
the CLI is the primary path. It isn't. The operator's experience starts
with the skill; the CLI is what the skill executes.

Leading with the CLI makes the architecture work invisible. A reader
following CLI-first docs ends up with a chunk that has a templated
`GOAL.md` they have no idea how to fill in, missing the back-and-forth
that turns vibey intent into a clear architectural record.

## Don't say / Do say

**Don't say:**
- "`$ ve chunk create my_chunk`" as the first/primary creation command. (CLI-first ordering implies the CLI is the operator's tool. It isn't.)
- "The slash command refines the goal interactively (alternatively, you can run the CLI by hand)." (Frames the skill as a convenience over the CLI. The relationship is the other way around.)
- "The CLI command underlies the slash command." (True but uninformative; misses that the skill is the *substance* and the CLI is the *plumbing*.)
- "`/narrative-create my_initiative`", "`/chunk-create my_chunk`", or "`/investigation-create my_investigation`" as the operator-facing invocation. (The operator doesn't pick a name and run the skill. The actual workflow for all three: brain-dump the intent or question to the agent in prose, ask it to use the skill, iterate on the resulting `OVERVIEW.md` or `GOAL.md`. The skill name is internal vocabulary; the operator interface is conversation. The agent picks the directory name as part of running the skill.)

**Do say:**
- "Chunks are created with the `/chunk-create` skill, not the CLI directly."
- "The skill is where vibey intent gets refined into a clear architectural record."
- "Under the hood, the skill calls `ve chunk create` to instantiate the templates."
- "Skills are the operator-facing surface; CLI commands are plumbing that instantiates templates the agent then edits."
- "Brain-dump your intent to the agent and ask it to complete the templates." (The meta-pattern: agent decomposes operator-prose into template-structured output. Applies to skill-driven artifact creation *and* to filling in `docs/trunk/` after `ve init`. Whenever there are templates and the operator has rough intent, the brain-dump-to-agent flow is the right shape.)

## Where the pattern shows up

| Operation | Skill | CLI it invokes |
|-----------|-------|----------------|
| Create a chunk | `/chunk-create` | `ve chunk create` |
| Plan a chunk | `/chunk-plan` | (no direct CLI; PLAN.md was scaffolded at create time) |
| Implement a chunk | `/chunk-implement` | (agent edits files; no single CLI) |
| Complete a chunk | `/chunk-complete` | `ve chunk complete` |
| Create a narrative | `/narrative-create` | `ve narrative create` |
| Create an investigation | `/investigation-create` | `ve investigation create` |
| Discover a subsystem | `/subsystem-discover` | `ve subsystem discover` |
| Log friction | `/friction-log` | `ve friction log` |

Not every operation has both layers. State-introspection commands (e.g.,
`ve chunk list`, `ve orch ps`) are CLI-only because they don't shape
intent. Some skills (like `/chunk-plan` and `/chunk-implement`) do
substantive agent work without a corresponding CLI primitive — the chunk
directory was scaffolded by the create skill, so by plan/implement time
the templates already exist for the agent to edit.
