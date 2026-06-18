# Claude Code Skills

A small collection of installable [Claude Code](https://claude.com/claude-code) skills I built and use. Each one packages a workflow I do repeatedly into a reusable instruction set — drop it in, trigger it by describing what you want, and Claude follows the playbook.

## What's a skill?

A skill is a folder with a `SKILL.md` (instructions + a `description` that tells Claude when to activate it) and optional `references/` for deeper detail Claude loads on demand. You install one and then just talk normally — "visualize this doc," "score this idea" — and the matching skill kicks in.

## The skills

| Skill | What it does | Trigger it by saying |
|-------|--------------|----------------------|
| **[visualization](visualization/)** | Builds polished, single-file HTML visualizations — slide decks, diagrams, training material, or turning a wall of text into a scannable, sectioned page. Zero dependencies, opens in any browser. | "visualize this", "make a deck", "turn this doc into something readable" |
| **[cc-level-up](cc-level-up/)** | Guided progression for your Claude setup. Detects your current level, explains what's next, and builds it for you. Designed for people new to Claude. | "level up my setup", "what should I improve", "help me set up Claude" |
| **[skill-template](skill-template/)** | Packages an existing skill into a clean, marketplace-ready template and stages it to a repo for publishing — strips internal references, builds an optional guide deck, stops before the push. | "templatize this skill", "make this skill installable" |
| **[idea-eval](idea-eval/)** | Evaluates a business idea through a 7-stage pipeline and scores it on a weighted 8-criteria rubric. Lists, views, and re-scores ideas; persists to local files or a GitHub repo. | "score this idea", "validate this idea", "list ideas" |
| **[roadmap-dashboard](roadmap-dashboard/)** | Turns a project folder into a living, single-page HTML roadmap dashboard — harvests your existing docs, infers stages/steps/owners/status, confirms with you, then renders a self-contained dashboard you host at one stable URL and refresh weekly. Three layouts, three themes. | "build a roadmap dashboard", "turn my project into a dashboard", "create a living roadmap" |

## Install

These are plain skill folders. To use one, copy it into your skills directory:

- **Claude Code (CLI):** copy the folder into `~/.claude/skills/` (global) or `.claude/skills/` (per-project)
- **claude.ai Projects:** add the skill folder to your project files

Then start a conversation and trigger it with one of the phrases above.

## Customizing

A couple of these ship with a fill-in template so they work for you out of the box and get better once personalized:

- **visualization** → edit `references/design-system.md` to use your own brand colors and fonts
- **cc-level-up** → edit `references/org-context.md` with your team's real roles
- **idea-eval** → edit `references/team-bios.md` with your actual team (drives the Team Fit score)
- **roadmap-dashboard** → pick a theme (`cream`/`forest`/`midnight`) or edit the `[data-theme]` blocks in the layout files to match your brand

Claude will offer to walk you through setup the first time, or you can edit the files directly.

## License

MIT
