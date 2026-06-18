---
name: cc-level-up
description: Guided progression for your Claude setup, on any platform (Claude Code CLI, the desktop app, claude.ai, or other agent tools like Codex). Detects how far along your setup is across five capability dimensions, explains what's worth doing next, and builds it for you. Use when user says "level up my setup", "what should I improve", "help me set up Claude", "cc level up", "audit my setup", or "how do I get better at Claude". Also use when someone new wants help getting started.
---

# CC Level-Up

Help the user improve their Claude setup one step at a time, wherever they use Claude. Detect
where they are across five dimensions, explain what's possible, and build it for them.

## References

- `references/level-guide.md` — The five dimensions + the level ladder, in full
- `references/org-context.md` — Role-to-content mapping for personalization (customize for your org)
- `references/last-level-up.md` — State from the most recent run
- `references/templates/claude-md-starter.md` — Instructions-file creation guide + template
- `references/templates/skill-starter.md` — Skill creation guide + template
- `references/templates/reference-starter.md` — Reference doc template

Read `references/level-guide.md` before scoring and `references/org-context.md` before the build phase.

## Tone

Friendly, encouraging, conversational. You're a helpful colleague, not an auditor. Teach real
terms (instructions file, skill, reference doc, MCP/connector) but always explain them in plain
language first. Never use jargon without a one-sentence explanation.

## Platform — works anywhere

This skill is platform-agnostic. First, figure out where the user runs Claude, because it changes
the vocabulary, not the ideas:

| Platform | Instructions file | Skills | Tools/connectivity |
|---|---|---|---|
| **Claude Code (CLI)** | `CLAUDE.md` | `skills/` folder | MCP servers (`.mcp.json`), hooks |
| **Desktop app** | project instructions | project skills | connectors |
| **claude.ai (web)** | project instructions | project skills | connectors |
| **Codex / other agents** | `AGENTS.md` (or equivalent) | equivalent | MCP / integrations |

Detect the platform from what you can see (a `CLAUDE.md` + `.mcp.json` → CLI; an `AGENTS.md` →
Codex; etc.). If it's genuinely unclear, ask one quick question. Then use that platform's words
when you talk to the user, but score the same five dimensions for everyone.

## Performance Notes

- Max 3 recommendations per run. Do not overwhelm.
- Take your time on detection — read files, don't guess from names.
- Always show what you built before moving to the next thing.
- The user may not know what a "skill" or "MCP" is yet. Explain before asking if they want one.

---

## Step 1: Detect

Scan the user's setup and score it across the **five dimensions** (full rubric in
`references/level-guide.md`). Do this silently — don't narrate every file you read. Score each
dimension `absent · basic · solid · advanced`.

| Dimension | What to look at | absent → advanced |
|---|---|---|
| **Instructions** | the instructions file (CLAUDE.md / AGENTS.md / project instructions) | none → a role description → behavioral rules → scoped / subfolder instructions |
| **Organization** | folder + project structure, naming, cruft | messy/default → tidy → scoped projects → actively audited & maintained |
| **Tools & Connectivity** | MCP servers, connectors, integrations — by ANY method | connected to nothing → one thing → a couple → richly wired |
| **Skills** | reusable instruction sets | none → one or two → a few, with references → composed/templated |
| **Knowledge & Memory** | reference docs AND persistence across sessions | none → a reference doc → a knowledge library → memory that persists across sessions |

Also read `references/last-level-up.md` — if it has prior data, this is a returning user.

### Two foundational dimensions, then the rest

The ladder is anchored on **Instructions** and **Organization** — those are the floor. The other
three deepen on top. **Skill count is never a gate** — two strong skills can carry someone all the
way to Level 3. Don't withhold a level because they "only" have a skill or two.

### Tools & Connectivity — audit it directly

This one is crucial: a setup wired to nothing is far weaker than one connected to even a tool or
two, no matter how good the instructions are. So **audit connectivity explicitly**, and detect
where you can / ask where you can't:

- **Can detect** (CLI): read `.mcp.json`, settings, hook config.
- **Often can't detect** (web/desktop): you usually can't see a user's connectors from inside a
  session — so just ask: *"What have you connected Claude to — any connectors, MCP servers, or
  integrations?"* Don't guess.

**If they're connected to nothing, flag it loudly regardless of their other dimensions** — being
unconnected is the single biggest thing holding the setup back, and it becomes the #1 recommendation.

### Level assignment

Assign the highest level whose conditions are met (conditions are about *depth*, not skill counts):

- **Level 0 — Getting Started:** no real instructions file; setup is essentially empty.
- **Level 1 — Foundation:** Instructions ≥ basic (a real role description) **and** Organization ≥ basic (not a mess). This is the true minimum. Skills and tools optional here.
- **Level 2 — Organized:** Instructions ≥ solid (behavioral rules) **and** Organization ≥ solid (intentional, scoped) **and** at least one of {a tool connected, a skill in use}.
- **Level 3 — Connected & Scaling:** Organization advanced (audited & maintained) **and** Tools ≥ solid (a couple of things connected) **and** Instructions advanced (scoped/subfolder) **and** at least a skill or two in use (count doesn't matter beyond that).
- **Level 4 — Full System:** all of Level 3 **plus** Knowledge & Memory ≥ solid (a knowledge library and/or memory that persists across sessions), with the five dimensions reinforcing each other.

---

## Step 2: Present

Show the user their level and what's next. Keep it warm and concise.

### Format

> **Your setup: [Level Name]** (Level [N] of 4)
>
> [One sentence on what this level means — from level-guide.md]
>
> [A one-line read on the five dimensions — e.g. "Instructions and organization are solid; you're connected to nothing yet, and there's no memory layer."]

If this is a returning user, celebrate progress first ("Since last time: …").

Then show 2–3 recommendations as cards and offer to build them. **Order recommendations by the
priority stack below**, and always surface a zero-connectivity gap first.

### What to recommend — follow the priority stack

The progression to coach people through, foundation first:

1. **Instructions** — get a real instructions file with behavioral rules. The highest-leverage thing for anyone who lacks it.
2. **Organization** — tidy and scope folders/projects; and *keep auditing this* as their setup grows — it's the one that quietly rots.
3. **Tools & Connectivity** — connect Claude to at least one or two things (a connector, an MCP server, an integration). If they're connected to nothing, this jumps to #1 regardless of level.
4. **Skills** — capture a repeated task as a skill. Quality over count — a couple of sharp skills beats ten thin ones.
5. **Knowledge & Memory** — a reference library, and memory that persists across sessions. The top of the ladder.

Pick the 2–3 that move *this* user furthest given their dimension scores. Don't recommend a skill
to someone whose instructions file is empty; don't push memory on someone connected to nothing.

---

## Step 3: Build

For each recommendation the user accepts, follow the **Explain + Build** pattern.

### The Pattern

1. **Ask** 2–4 personalizing questions relevant to what you're building
   - Read the relevant template from `references/templates/` for question ideas
   - Read `references/org-context.md` if this is role-dependent content
2. **Explain** what you're about to create (1–2 sentences, plain language)
3. **Build** the file(s)
4. **Show** a summary of what was created (quote key parts, not the whole file)
5. **Explain** how it changes their experience going forward (1–2 sentences)

### Critical Rules

- **Never overwrite an existing instructions file or skill.** Read first. Add to it, or ask before replacing.
- **Always show what you created** before moving on. Don't batch-create silently.
- **Use the user's own words** from their answers. Don't over-polish into corporate speak.
- **Keep skills simple.** First skills should be 15–30 lines, not 100-line masterpieces.
- **Keep the instructions file minimal.** Start with 10–15 lines. Resist front-loading.
- **Explain file locations** and use the right name for their platform (CLAUDE.md / AGENTS.md / project instructions).

### What "building" looks like per dimension

- **Instructions** → read `references/templates/claude-md-starter.md` + `org-context.md`; create or extend the instructions file with a role and behavioral rules.
- **Organization** → audit folders/projects; propose a tidy structure; add subfolder/scoped instructions where they'd help. (This is partly a *review*, not just file creation.)
- **Tools & Connectivity** → explain MCP/connectors in plain language; point them to one or two worth connecting for their work; walk them through it (or hand them the exact steps if you can't do it for them).
- **Skills** → read `references/templates/skill-starter.md`; turn a repeated task into a small skill.
- **Knowledge & Memory** → read `references/templates/reference-starter.md`; start a reference library; set up a memory/session-notes habit so Claude carries context across sessions.

---

## Step 4: Save State

After building everything the user requested, update `references/last-level-up.md`.

```markdown
# Last Level-Up

**Date:** [today's date]
**Platform:** [CLI / desktop / web / Codex / other]
**Level detected:** [N] — [Level Name]
**Previous level:** [N from last run, or "First run"]
**Dimension scores:** Instructions [x] · Organization [x] · Tools [x] · Skills [x] · Knowledge/Memory [x]

## What Was Built
- [filename or change] — [one-line description]

## Recommendations Given
1. [recommendation title] — [accepted/declined/skipped]

## Suggested Next Run
[Date 2–4 weeks out] — by then you'll have used what we built and be ready for the next step.
```

### Closing Message

End encouraging and forward-looking:

> That's it for now! You're at **[Level Name]** with [brief summary]. Use what we built for a couple weeks, and when you're ready for more, just say "level up" again.
