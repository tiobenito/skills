---
name: build-ai-manager
description: Use when someone has built an AI agent or automated workflow and wants an ongoing way to check whether it's actually hitting its goal, not just running without errors — e.g. "build me an AI manager for my support bot", "set up oversight for this workflow", "I want something that reviews my agent's output against a goal", "how do I know if my automation is actually working", or "create a manager for my agent". Takes the agent's name as an argument. This skill interviews the user and generates a second, standalone skill scoped to their specific agent, named "that-agent-manager". Not for running a review that's already set up — call the generated manager skill for that.
allowed-tools: ["Read", "Write", "Edit", "Bash", "Glob", "Grep", "AskUserQuestion"]
---

# Build AI Manager

## What this does

Most AI agents and automated workflows get built, shipped, and then nobody checks whether they're
actually working — only whether they're *running*. An agent can execute its process perfectly and
still miss the point of why it exists. This skill fixes that gap.

Run this **once per agent**. It interviews the owner, validates their data sources live, and
**generates a new, standalone skill** — a manager for that specific agent — that they call anytime
afterward to review it. This skill is the *generator*; the generated skill does the *reviewing*.

Read `references/ai-manager-pattern.md` before running this for the first time — it's the full
spec (the contract, the manager-vs-dashboard test, two real-world worked examples). The one-line
version: an AI manager is a read-only layer that finds where the agent **did its job but missed the
goal**, ranks findings by how much they'd move the goal (not by raw count), and attaches a concrete
fix to every finding — never executing anything itself.

## Guardrails (always)

- **Never ask for a key or secret in chat.** Credentials go in an env var the user already has, or
  an MCP/tool they authenticate through Claude.
- **Read-only** toward the agent's production systems. The only writes the generated skill ever
  makes are human-confirmed (a filed ticket, a code PR, a data correction) — never automatic.
- **Never infer which environment** (prod vs. staging, or whatever the user's equivalent is) — ask.

## The setup flow

Work through these in order. Draft what you can infer and ask the user to confirm; ask outright
what you can't infer. **One focused question at a time** — don't front-load a wall of questions.

1. **Understand the agent.** What does it do, and where does its logic live (code, a no-code
   workflow tool, a prompt/config file)? Is there a written process/SOP it's supposed to follow?
2. **Name the generated skill.** Ask for a short name; the generated skill will be
   `<name>-manager`.
3. **Goal + guardrails.** Ask what the agent should *maximize* — one sentence — and what it must
   *never* do. This is the single most important answer in the interview; everything else ranks
   against it.
4. **Find and verify every reachable data source, live.** Wherever "what actually happened" lives —
   a database, an API, application logs, an error tracker, a spreadsheet the agent writes to,
   whatever exists. For each one, run one trivial query/call to confirm it's actually reachable
   before trusting it. Report ✅/❌ per source and what a missing one costs the review.
5. **Define and validate the outcome metric, like a data analyst.** Ask what data best captures
   whether the goal was hit, and validate it against real data: does it exist, is it populated, does
   it mean what the user thinks, can it be joined back to a specific run/case? Show a sample. If the
   obvious metric turns out to be unreliable, say so and look for a better one together.
6. **Map the shared identifiers.** However cases/runs are tracked (an ID, a timestamp range, a
   filename pattern) — this is what lets evidence from different sources be tied to the same case
   later, instead of pulling a global, unscoped dump.
7. **Assemble the process/SOP.** Wherever the intended behavior is documented — a spec, a README, a
   prompt file, or "it's just in the code" if nothing else exists. Note the gap if there's no
   written process at all; that's itself worth flagging to the user.
8. **Build the attention list.** Seed defaults (errored, incomplete, stuck, escalated, needed a
   human, took unusually long) and ask what else matters specifically for this agent.
9. **Preview on real examples.** Pull a handful of real runs from the connected sources and narrate
   what a review would surface. Ask: "anything missing this should cover?" Loop back and adjust
   until the user's satisfied.
10. **Scaffold the skill** (see Scaffolding below). Run a quick end-to-end sanity check — list a
    few real runs, read the validated metric.
11. **Offer to run it.** Ask which review(s) to run first and hand off to the generated skill.

## Scaffolding (step 10)

Create `<name>-manager/` (a normal skill directory, next to this one or wherever the user keeps
their skills) with:

- **`SKILL.md`** — from `templates/generated-SKILL.md`, filled in with the agent name + goal.
- **`review-method.md`** — copy `references/review-method.md` **verbatim** (makes the generated
  skill self-contained; it doesn't depend on this generator skill still being installed).
- **The "what" files**, filled in from the interview:
  - `goal.md` — from `templates/goal.md` (the goal + guardrails)
  - `data-sources.md` — from `templates/data-sources.md` (the validated metric, every connected
    source, the shared identifiers, the join recipe)
  - `attention-list.md` — from `templates/attention-list.md`
  - `decision-log.md` — from `templates/decision-log.md`, empty except the entry format

**Before declaring done:** confirm `review-method.md` byte-matches the canonical copy in this
skill's `references/` — it carries the non-negotiable disciplines (multi-source, entity-scoped,
novelty-before-filing, the hypothesis loop, the two-table output). If it drifted while filling in
the templates, re-copy it clean.

## Reference

- `references/ai-manager-pattern.md` — the full pattern, read before the first run
- `references/review-method.md` — the procedure every generated skill runs (copied verbatim into
  each one)
- `templates/` — fill-in files for scaffolding a new instance
