---
title: The leverage model — time vs tokens
description: ve assumes the operator's time is more valuable than tokens and their judgment is what makes their time valuable. The orchestrator is the mechanism for converting articulated architecture into shipped code in parallel.
created: 2026-04-26
updated: 2026-04-26
---

# The leverage model — time vs tokens

The economic and cognitive premise behind ve, articulated by the operator.
Every doc that touches the orchestrator, the chunk lifecycle's parallel
mode, or the "who this is for" framing should respect this model.

See: [[chunk_intent_ownership]] for what chunks actually own;
[[skill_cli_layering]] for how operator intent gets shaped.

## The core trade

Operator time is more valuable than tokens. Operator judgment is what
makes that time valuable. If you can spend tokens to remove the parts
of your work that aren't judgment, you almost certainly should.

Concrete framing the operator uses (verified pricing as of 2026-04):

- **Claude Pro: $17/month.** For light vibing where the operator stays
  fully in the loop. Not the price point ve is designed for.
- **Claude Max minimum: $100/month.** Real Max access; entry-level Max.
- **Claude Max economical tier: $200/month.** This is the price point
  **ve is designed around** — a tech-lead-grade brain working at full
  planning capacity, with throughput equivalent of six or seven decent
  engineers.
- Higher architectural ambition can plausibly consume more than one Max
  plan in a month. The cost ceiling is not fixed at $200; the leverage
  scales with it.
- If $200/mo scares you, ve is not for you. Use the free tiers, do
  light vibing, stay fully in the loop. That is a real and valid mode;
  it is just not the one ve is designed for.

**DON'T hallucinate prices.** I once wrote "$50/mo for Claude Max"
which was wrong. Always cite from this list or verify externally.

## The orchestrator as team-of-engineers

The natural metaphor: the orchestrator is a team of decent engineers
**who understand your codebase**. The architecture is staked out by
chunks discoverable through code backreferences; a new chunk that
arrives with just a defined goal can be reliably implemented by a team
that already understands what it's building on. You hand them chunks;
they do the work in dedicated workspaces (worktrees off the active
branch) and send each chunk back to you the moment it's done. Each
chunk lands as its own commit on the active branch, which makes the
work easy to read chunk-by-chunk afterward.

What the orchestrator is converting: your articulated architecture
(chunks with clear goals, dependencies declared via `depends_on`) into
shipped code in parallel. The judgment was already in the chunks; the
typing happens in the worktrees.

### Orthogonality with the entity system

Codebase understanding (what makes the orchestrator's engineers competent
on your project) is owned by the **chunk architecture**. Personal
preferences and working-style training (how an engineer collaborates
with you specifically across sessions) is owned by the **entity system**.
The two are orthogonal — never conflate them in docs.

**Don't say:**
- "The orchestrator is a team of decent engineers you've already trained." (Implies the orchestrator carries personal preferences across runs. It doesn't; that's the entity system. Conflates two orthogonal axes.)

**Do say:**
- "The orchestrator is a team of decent engineers who understand your codebase."
- "Chunks make the codebase legible to anyone reading it; that's what the orchestrator's agents lean on."
- "Working with the orchestrator at full tilt feels like having a highly competent team bring your architecture vision into reality at unbelievable speed, with you serving just the role of providing judgment." (Operator-validated framing for the felt experience; lands well as a closer.)

## What this changes for documentation

**Don't say:**
- "ve makes coding faster." (Generic and shallow. The actual claim is more specific: ve makes *judgment* the bottleneck instead of *typing*, and pays tokens to handle the typing.)
- "The orchestrator runs chunks in the background." (True but flat. Doesn't capture the leverage point: parallel execution of work you've already architected.)
- "ve is for AI-assisted development." (Too broad. Free-tier vibe coding is also AI-assisted development; ve is specifically for the mode where you're paying tokens to externalize judgment at scale.)

**Do say:**
- "The orchestrator is a team of decent engineers you've already trained."
- "The leverage shows up when you understand a large swath of your architecture and can articulate it as interrelated chunks."
- "Engineering isn't about writing code; it's about making architectural choices that hold the implementation in productive shape over time."
- "If you're reading every line that gets written, you're moving slow enough at low enough leverage that the free tiers are good enough."

## The engineering-manager working mode

When the operator is using the orchestrator as designed, the day-to-day
working pattern is:

- Hand off large bodies of work to the orchestrator
- Don't look at the code until it goes to PR
- At PR time, switch into engineering-manager mode: ensure the intent
  is being maintained in broad strokes, leave comments where it isn't
- Sometimes nitpick (lots of comments, then tell the agent to resolve
  them all); most of the time accept distance from the code in
  exchange for the leverage
- QA the result: start it up, run the feature, confirm it actually
  makes sense. For data-pipeline-shaped work, spawn a separate agent
  to test invariants under a range of scenarios and verify the tests
  it writes are valid
- Across investigation, debugging, and test-writing: let the agent
  lead while you verify the methodology is sound. The operator's
  contribution is judgment about *how* the work is being done, not
  doing the work directly.

This is the natural consequence of "judgment > typing." The operator's
contribution is the architectural intent (the chunks) and the
gate-keeping at PR time. The implementation in between is tokens.

Don't write docs that imply the operator is reviewing code line-by-line
during implementation. That would defeat the leverage point. The reader
of these docs should expect to see code at PR time, with the orchestrator
having handled everything in between.

## Greenfield and legacy parity

The Quickstart steps work the same on a brand-new project and on a
legacy codebase being retrofitted. Operator has personally
onboarded legacy projects via these steps and become productive
immediately.

The only difference is **when the orchestrator becomes safe**:

- **Greenfield:** immediately. The architecture is reified from day
  one; agents picking up new chunks have everything they need.
- **Legacy:** after a few chunks of scaffolding. A clean trunk plus
  a handful of chunks that stake out the parts of the architecture
  the agent will reason about. Once that's in place, the orchestrator
  works just as well on the legacy project.

**Don't say:**
- "ve is best on greenfield projects" or "ve is hard to retrofit."
  (Wrong; legacy retrofit is a first-class path.)

**Do say:**
- "These steps work the same on a brand-new project and on a legacy
  codebase you're retrofitting."
- "Greenfield reaches the orchestrator immediately; legacy needs a
  few chunks of scaffolding first."

## Where this shows up across docs

- **Chunks page** (lifecycle intro): "Engineering isn't about writing code... most of the human effort of engineering with ve goes into building good ones. The boring typing gets deferred to agents."
- **Who-this-is-for page**: the explicit $50/$200 framing, the IC-vs-architect distinction, the bridge to /docs/chunks/ and /docs/orchestrator/.
- **Orchestrator page** (intro + mental model): the team-of-engineers metaphor, parallel execution, "while you sleep."
- **Narratives page** (workflow step 5): the segue into the orchestrator as the parallel-no-review path.

When refining any of these, the leverage frame is the connective tissue.
Don't let one of them drift toward generic "AI-assisted productivity"
language; that's the cliché the framing is trying to escape.
