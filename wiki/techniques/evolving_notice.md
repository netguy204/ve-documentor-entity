---
title: The Evolving notice
description: Visual language for areas of the project that are implemented and useful but whose design isn't finalized. Sets reader expectations about stability without diminishing the value of what exists today.
created: 2026-04-26
updated: 2026-04-26
---

# The Evolving notice

A reusable visual element for the docs site. Placed at the top of a page
(immediately after the `<h2>` title, before the intro prose) when the
concept being described is implemented and useful but the design is not
finalized.

See: [[anti_defensive_framing]] for related — the Evolving notice is
*not* defensive framing because it answers a question the reader
genuinely should have ("can I rely on this?") with a frank answer.

## When to use

Apply when **all** of the following are true:

- The feature/concept is real and works today
- Operators are getting value from it as it stands
- The design is acknowledged as not-yet-final
- Significant rethinking is plausible (not just minor tweaks)
- Input would actually shape the next iteration

If only one or two apply, don't reach for this. A working stable feature
doesn't earn the notice; an experimental feature that doesn't work yet
earns a stronger warning.

## What it looks like

```html
<div class="evolving-notice">
  <span class="evolving-label">Evolving</span>
  <p>
    The friction log works today and is useful, but the design isn't
    finalized. The implementation may change, significant rethinking
    is possible, and input is welcome.
  </p>
</div>
```

Visual treatment (CSS in `global.css`):
- Left accent stripe (3px, `--accent`)
- Light surface background, full border, rounded right corners only
- Monospace uppercase label "EVOLVING" in accent color
- Small body text (13px) in muted color
- Constrained to `--max-prose` width
- Sits above page intro prose, with bottom margin to separate from body

The pattern is intentionally similar to `.docs-callout` and
`.decision-card` so it feels native to the site, but distinct enough
(narrower vertical padding, different label semantics) that it reads
as its own thing.

## Body content guidelines

The body should land three points in two or three sentences:

1. **It works today.** Don't undersell the present-day usefulness.
2. **The design isn't final.** Be specific about what may change
   (implementation, surface, naming, scope).
3. **Input is welcome.** Signal that operator feedback can shape the
   next iteration.

**Don't say:**
- "This feature is experimental." (Implies it might not work; misleading.)
- "Use at your own risk." (Defensive; the feature is supported.)
- "We may deprecate this." (Often untrue and scares readers off.)

**Do say:**
- "X works today and is useful, but the design isn't finalized."
- "The implementation may change, significant rethinking is possible, and input is welcome."

## Where it lives in the docs

Currently applied on:
- `/docs/friction/` (the friction log's design is acknowledged as
  not-yet-final)

Other candidates as the project's stability surface evolves:
- Any artifact type whose schema or workflow is openly being iterated on
- Specific commands or skills that are likely to change behavior

When adding to a new page, place the notice immediately after the page's
`<h2>` and before the first prose paragraph. Don't bury it inside a
section.
