# Component catalog — mapping the model to the template

The three `assets/layout-*.html` files are working, self-contained examples — each filled with
sample content and embedding all three themes. Pick one (see `references/design.md`), copy it to
the project as `index.html`, set `data-theme`, and replace the sample content from the confirmed
model. The component map below is written for Layout A (Stages & Steps); Layouts B (board) and C
(timeline) use the same north-star header, owner chips, status states, and bottom panels but a
different middle section (columns / timeline nodes). The worked example baked into each layout
file is itself the reference — open `assets/layout-stages.html` in a browser to see the bar.

## Hard rules

- **Never touch the `<style>` block.** The palette + classes ARE the design. Only swap content.
- **Keep `index.html` and `state.md` in lockstep.** Same data, two forms. The markdown is the
  diff-friendly source of truth; the HTML is the shareable view.
- **The HTML is hand-maintained, all data inline.** No data file, no API calls, no build step,
  ~12 lines of vanilla JS for the tabs. This simplicity is intentional — keep it.
- **No unverified metrics.** Blank a tile rather than invent a number.

## Model → component map

| Model field | Component | Notes |
|---|---|---|
| `program.title` / `updated` / `cadence` | `<header>` | |
| `north_star` | `.north-star` | statement + 3-4 target columns (Today → … → End state) |
| `vocab[]` | `.vocab` section | OPTIONAL — delete the whole section if empty |
| `stages[].metric` | `.submetrics .tile` | one tile per stage; deferred stages use `.tile.deferred` + `.defer-tag` |
| `stage` | `.stage` panel | `.stage.deferred` for gated stages |
| `stage.owner` | `.owner` chip in `.stage-head` | first-class — show on every stage |
| `stage.pulse` | `.stage-pulse` | one-line status next to the stage title |
| `stage.steps[]` | `.sequence > .step` | horizontal cards; status class drives color |
| `step.status` | `.step.{live,progress,wire,next,later,blocked}` | see status table below |
| `step.owner` | `.owner` inside the step | optional; renders smaller/borderless |
| `stage.now_executing` + headline metric | `.stage-summary` | the "Now executing" line + big number |
| `stage.details.blocks[]` | `.tab-pane[data-pane=details] .detail-block` | the depth; two-column grid |
| `principles[]` | `.panel` "Operating principles" | |
| `foundations[]` | `.panel` + `.cross-item` + `.cross-pill` | optional cross-cutting work |
| `owners_weeks[]` | `.panel.dark` per owner | "{{Owner}}'s week" — the who's-doing-what view |
| `changelog[]` | `.panel.full-row .changelog` | newest first; drop rows > ~4 weeks |

## Step status classes (the colored dot)

| Class | Visual | Use for |
|---|---|---|
| `live` | solid green | in production, working |
| `progress` | pulsing amber | active build right now |
| `wire` | solid amber | built/selected but not yet activated |
| `next` | grey, dashed border | designed, not started, up next |
| `later` | faded, dashed | backlog / gated / future |
| `blocked` | red | named external dependency holding it |

The visible `.step-status` text keeps the project's own words ("Built, not wired",
"Vendor selected"); the class only sets the color. The last step in a sequence is often the
"ship" / done-definition card (`later` or `next`, labeled "Production gate").

## Stage anatomy (the IA that makes this work)

This zoom is the whole point — preserve it:
1. **Submetric tile** = the stage at a glance (number + goal + bar).
2. **Stage panel, Plan tab** = the stage as a left-to-right sequence of steps, each with a
   status dot, so anyone can see what's done / in-flight / blocked without reading prose.
3. **Stage panel, Details tab** = the depth — narrative, design calls, blockers, next 2 weeks.

A reader scans tiles → opens the one stage they care about → flips to Details only if they
want the why. Don't flatten this into one long list; the layering is what stakeholders liked.

## Sizing / taste guidance

- 3-6 stages is the sweet spot. More than ~6 and the submetrics grid gets cramped — consider
  whether some "stages" are really steps.
- 4-8 steps per stage. If a stage has 15 steps, they're probably sub-tasks; roll up.
- The `.stage-pulse` one-liner is high-value — it's the honest "where this really is" that a
  green dot can't convey ("Built, just not wired"). Write it candidly.
- Keep Details tabs to what a stakeholder would actually ask about. This is a roadmap, not a
  ticket tracker (Linear/Jira own tickets).
