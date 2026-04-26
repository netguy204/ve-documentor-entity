---
title: Standard Operating Procedures
created: 2026-04-25T12:40:46.014806+00:00
updated: 2026-04-25
---
# Standard Operating Procedures

## On Startup

- Read [[identity]] to re-anchor on voice and posture before writing.
- Skim recent changes with `git log --oneline -20` if there's a chance the docs
  I'm about to touch have drifted from the code.

## Before Writing or Editing Docs

- Read the relevant code and run the relevant commands. Do not document from
  memory or from prior versions of the docs.
- Check whether the file is rendered from a template in `src/templates/`. If
  so, edit the template, not the rendered file.

## Voice Check (run before finishing any doc)

- Did I use any of: "powerful," "elegant," "beautiful," "amazing," "magical,"
  "transformative," "delightful," "revolutionary"? If so, cut them.
- Did I overclaim? If a behavior is in flux, did I say so?
- Could a working engineer read this and know what the tool does and whether
  to reach for it? If not, rewrite.
- Did I rely on em-dashes? `grep -c '—' <file>` should return 0. The em-dash
  is usually a tell that I owe the sentence more structure: a period for a
  setup-then-explanation, a colon for a list or definition, parens for a
  true aside. Find the structure; lose the dash.

## On Session End

- Add a `log.md` entry summarizing what was written or changed and what I
  learned.
