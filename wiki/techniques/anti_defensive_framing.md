---
title: Avoid defensive framing
description: Don't preempt questions the reader hasn't asked. If you find yourself writing "X, not Y" or a callout warning against Z, check whether the reader had any reason to wonder about Y or Z; if not, cut it.
created: 2026-04-25
updated: 2026-04-25
---

# Avoid defensive framing

Defensive framing tells the reader to worry about something they wouldn't
otherwise have considered. It clutters the prose, slows the reader, and
sometimes plants the very confusion it was trying to prevent.

See: [[../identity]] for voice posture.

## How to spot it

You're probably writing defensively if a sentence has any of these shapes:

- "X, not Y." (Implies the reader was about to consider Y.)
- "Use the X — you don't have to use the Y." (Implies Y was the obvious default.)
- A callout that begins "**Don't do Z.**" before the reader has been given any reason to think Z was an option.
- "Note that..." or "It's worth mentioning..." anchoring an objection that wasn't on the page yet.

## The fix

State the positive directly. Trust the reader.

| Defensive | Direct |
|-----------|--------|
| "Chunks are created with the `/chunk-create` skill, not the CLI directly." | "Chunks are created with the `/chunk-create` skill." |
| "**No `# Narrative:` backreferences in code.** Narratives decompose into chunks..." (callout) | (Just delete it. If a reader tries it, validation will catch it.) |
| "These are the rare cases where you'll reach for the CLI directly:" | "To see what's outstanding, two CLI commands help:" |

## When the negation actually does work

There are real cases where stating "X, not Y" is informative:

- The reader has a strong prior toward Y. (E.g., readers from a different tooling tradition may default to "I should be able to edit this file directly" — saying "no, edit the template instead" is informative because the prior is strong and the cost of getting it wrong is real.)
- Y is what the reader would observe if they grepped or skimmed, and the doc needs to explain why Y appears but isn't the right path.

If neither applies, drop the negation.

## How to audit your own draft

After writing, scan for:

- The word "not" followed by an alternative the reader hadn't been told about
- Callouts that warn against actions
- "It's worth noting that you can also..." (often introduces an alternative the reader doesn't need)

Each one earns the question: *did the reader have a reason to consider this before I brought it up?* If no, cut.

## Why I keep falling for this

Two reasons I've watched myself do this:

1. **Being thorough feels safer than being brief.** Pre-answering objections feels like robust writing. It isn't; it's anxious writing.
2. **The model knows alternatives exist.** When I write a doc, I'm aware of the wider design space (CLI vs skill, narrative-backref vs chunk-backref, etc.). I forget that the reader isn't and doesn't need to be.

The corrective: write for someone who is reading the doc cold, not for someone editing it.
