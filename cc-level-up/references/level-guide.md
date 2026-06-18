# Level-Up Progression Guide

The full progression path for a Claude setup — on any platform (Claude Code CLI, the desktop app,
claude.ai, or other agent tools). Progress is measured across **five dimensions**; your **level** is
how far you've climbed across them, with Instructions and Organization as the floor and Knowledge &
Memory as the ceiling.

---

## The five dimensions

Each is scored `absent · basic · solid · advanced`.

### 1. Instructions (foundational)
The instructions file Claude loads every session — `CLAUDE.md`, `AGENTS.md`, or "project
instructions" depending on your platform.
- **basic:** a role description (who you are, what you do)
- **solid:** behavioral rules — "always/never" instructions, format and tone preferences, workflow rules
- **advanced:** scoped / subfolder instructions for different areas of work

### 2. Organization (foundational)
How your files, folders, and projects are structured. The one that quietly rots, so it gets
re-checked every run.
- **basic:** tidy — not a dumping ground
- **solid:** intentional, scoped projects/folders
- **advanced:** actively audited and maintained — cruft removed, structure still fits the work

### 3. Tools & Connectivity
What Claude is connected to — MCP servers, connectors, integrations — by *any* method. A setup
wired to nothing is far weaker than one connected to even a tool or two.
- **basic:** connected to one thing
- **solid:** a couple of things connected
- **advanced:** richly wired into the tools you actually use

### 4. Skills
Reusable instruction sets for tasks you repeat. **Quality over count** — two sharp skills beats ten
thin ones, and a small number of good skills is fine all the way up the ladder.
- **basic:** one or two skills
- **solid:** a few skills, some with their own reference files
- **advanced:** composed/templated skills that reference each other or chain

### 5. Knowledge & Memory (the ceiling)
Two related things: knowledge Claude can *look up*, and memory that *persists across sessions*.
- **basic:** a reference doc or two
- **solid:** a real knowledge library, and/or memory that carries context between sessions
- **advanced:** a maintained knowledge layer plus durable memory, feeding everything else

---

## The level ladder

Your level is the highest one whose conditions are met. Conditions are about **depth**, never about
how many skills you have.

### Level 0 — Getting Started
No real instructions file; the setup is essentially empty. **Next:** create your instructions file.

### Level 1 — Foundation
**The true minimum: Instructions ≥ basic AND Organization ≥ basic.** You have a real instructions
file with a role, and your files aren't a mess. Skills and tools are optional here.
**Next:** add behavioral rules; connect your first tool.

### Level 2 — Organized
Instructions ≥ solid (behavioral rules), Organization ≥ solid (intentional and scoped), and at
least one of {a tool connected, a skill in use}.
**Next:** connect a second tool; start auditing your organization; capture a repeated task as a skill.

### Level 3 — Connected & Scaling
Organization advanced (audited & maintained), Tools ≥ solid (a couple connected), Instructions
advanced (scoped/subfolder), and at least a skill or two in use. **You can sit here happily with
just two good skills.**
**Next:** start a knowledge library; set up memory that persists across sessions.

### Level 4 — Full System
Everything in Level 3, plus Knowledge & Memory ≥ solid — a knowledge library and/or persistent
memory — with all five dimensions reinforcing each other.
**Next:** maintenance. Keep auditing organization, prune stale knowledge, refine skills.

---

## The priority stack (what to coach first)

When recommending next steps, work foundation-up:

1. **Instructions** — the highest-leverage thing for anyone missing it
2. **Organization** — tidy and scope; keep re-checking it as the setup grows
3. **Tools & Connectivity** — connect to at least one or two things; if connected to *nothing*, this jumps to #1 regardless of level
4. **Skills** — capture repeated tasks; quality over count
5. **Knowledge & Memory** — reference library + cross-session memory; the top of the ladder

---

## How levels are detected

The skill scans what exists — the instructions file, folder/project structure, connected tools (or
asks, when it can't see them), skills, and reference/memory — and scores the five dimensions. You
don't declare your level; it's read from your setup. Connectivity in particular is often invisible
from inside a session, so the skill will simply ask what you've connected rather than guess.

## How often to run

Every 2–4 weeks is a good cadence. Each run re-scores you, celebrates progress, and suggests 2–3
concrete next steps.
