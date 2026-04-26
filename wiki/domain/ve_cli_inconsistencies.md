---
title: ve CLI inconsistencies
description: Rough edges noticed in the ve CLI surface while writing docs — for later cleanup or design review.
created: 2026-04-25
updated: 2026-04-25
---

# ve CLI inconsistencies

A running ledger of rough edges in the `ve` CLI surface, noticed while
writing the marketing-site docs (quickstart, chunks, narratives,
investigations, friction, orchestrator). Not a complaint list — these are
candidates for design review or for documenting explicitly so users aren't
surprised. See [[../identity]].

When a pattern accumulates here, the project's own
[friction log](../../../docs/trunk/FRICTION.md) is the right place to file it
formally. This page is the working notebook.

## Verbs for "create a new artifact" are inconsistent

Different artifact types use different verbs to mean the same thing:

| Artifact | Creation verb |
|----------|---------------|
| Chunk | `ve chunk create` (alias: `start`) |
| Narrative | `ve narrative create` |
| Investigation | `ve investigation create` |
| Subsystem | `ve subsystem discover` |
| Friction entry | `ve friction log` |

`ve subsystem discover` even has a help description that reads "Create a new
subsystem." — the verb in the command name disagrees with the description.

The argument for `discover` is conceptual: subsystems are observed, not
designed up-front. `log` is conceptual too: friction is recorded, not
created. The argument against is practical — agents and operators have to
remember which verb each artifact uses, and `--help` is the only way to
recover it. A consistent `create` (with the conceptual nuance documented in
the help body) would be cheaper to use.

## `ve chunk start` is an unmarked alias of `create`

The SPEC says `start` is "deprecated, same behavior." The CLI help for
`ve chunk` lists `start` as an active command without any deprecation note
("Create a new chunk (or multiple chunks)." appears under both `create` and
`start`). Either the alias should announce its deprecation in `--help`, or
the SPEC should drop the deprecation language.

## `ve chunk validate` does double duty

One command serves two distinct validations:
- "Is this chunk ready for completion?" (default)
- "Is this chunk ready for orchestrator injection?" (`--injectable`)

The two checks have different success criteria (e.g., injectable requires
PLAN.md populated). A flag on a single command works, but `validate` and
`validate-injectable` (or two subcommands) would be more discoverable. As
documented today, the difference between the two checks is buried in the
flag's help text.

## Two paths into the orchestrator: `ve orch inject` and `ve orch work-unit create`

`ve orch inject` is the documented primary path. `ve orch work-unit create`
exists as well and presumably does something similar (or lower-level). The
relationship between them isn't surfaced in `--help` for either. Either one
should be the obvious entry point and the other should be hidden or removed,
or both should explain when to use which.

`ve orch ps` being an alias of `ve orch work-unit list` is a similar
two-paths-to-one-thing pattern, but at least it's documented as an alias.

## Chunk lifecycle has no PLANNED status

The lifecycle described to users is **Goal → Plan → Implement → Complete**
(four phases). The status enum is **FUTURE → IMPLEMENTING → ACTIVE**
(plus terminal SUPERSEDED / HISTORICAL). There's no status that means
"plan written, implementation not started." A chunk with a thoroughly
written PLAN.md and no code looks identical to one with no plan at all.

This may be intentional (the operator/agent doesn't need a separate state)
but it makes the four-phase pedagogy a half-truth. Worth either expanding
the enum or revising how phases are taught.

## `ve chunk list` filter modes don't compose

Three orthogonal filter mechanisms live on `list`:
- `--status FUTURE,ACTIVE` (canonical, multi-valued)
- `--future`, `--active`, `--implementing` (shortcuts for single statuses)
- `--current`, `--last-active`, `--recent` (semantic queries, mutually
  exclusive with the above)

The shortcuts duplicate `--status`. The semantic queries don't compose with
the others. Users have to internalize which flag-family they're in. A
single, composable filter (e.g., `--status` plus `--limit 1 --order recent`)
would be smaller surface.

## Friction entry format disagreement across the project's own docs

Three different ways the friction entry format is described:

1. `docs/trunk/ARTIFACTS.md` says `### FXXX: YYYY-MM-DD [theme-id] Title`
2. The inline guidance in `docs/trunk/FRICTION.md` says
   `### YYYY-MM-DD [theme-id] Title` (no F-prefix)
3. The actual entries in `FRICTION.md` use the F-prefix form (`F001:`,
   `F002:` …)

The actual format wins — but the inline guidance, which is the first thing
an agent reads when appending an entry, is wrong. Worth fixing in the
project's own docs.

## Five chunk-maintenance commands at the top level

`ve chunk` lists fifteen subcommands. Five of them
(`overlap`, `cluster`, `cluster-list`, `cluster-rename`, `suggest-prefix`)
are project-hygiene operations rather than parts of the create / plan /
implement / complete loop. They drown out the lifecycle commands in
`--help`. A `ve chunk maintenance ...` (or `ve chunk hygiene ...`)
subgroup would keep `ve chunk --help` focused on what most users do most
of the time.

## `# Narrative:` and `# Investigation:` backreferences are forbidden but not enforced

Trunk docs say only `# Chunk:` and `# Subsystem:` backreferences are valid
in source code. Agents repeatedly try to add narrative or investigation
backreferences anyway (because it's intuitive — those artifacts produced
the work). The prohibition is documentation-only; nothing in `ve validate`
flags a forbidden backreference. Either enforce it in validation, or
relax the rule.

## Asymmetric directory layout for related artifact types

| Artifact | Layout |
|----------|--------|
| Chunk | `docs/chunks/<name>/` (directory with GOAL + PLAN) |
| Narrative | `docs/narratives/<name>/` (directory with OVERVIEW) |
| Investigation | `docs/investigations/<name>/` (directory with OVERVIEW) |
| Subsystem | `docs/subsystems/<name>/` (directory with OVERVIEW) |
| Friction | `docs/trunk/FRICTION.md` (single file, with frontmatter for themes and proposed_chunks) |

Friction is the odd one out: a single accumulating file rather than a
directory of artifacts. There's a reasonable design argument for both
shapes — friction entries are short and many, while chunks/narratives are
long and few — but the asymmetry is worth being explicit about in the docs
rather than making readers infer it.

## `ve chunk create` accepts ticket as positional or flag

In single-chunk mode: `ve chunk create my_feature TICKET-123`.
In multi-chunk mode: `ve chunk create chunk_a chunk_b --ticket TICKET-123`.

The SPEC notes a "two args, second contains a dash" heuristic to distinguish
single-chunk-with-ticket from two-chunk batch. This is fragile. A consistent
`--ticket` flag in both modes (with the positional form deprecated) would
be safer.
