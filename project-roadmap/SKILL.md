---
name: project-roadmap
description: Point it at a project folder and it builds a living, single-page roadmap dashboard — it harvests the folder's existing docs (STATUS, roadmap, TASKS, DECISIONS, README, etc.), infers a draft structure (stages, steps, status, owners, north star), asks you only about real gaps, then renders a self-contained HTML dashboard + a state.md source-of-truth you can host anywhere. Use when someone says "build a roadmap", "make a project roadmap", "turn my project into a dashboard", "create a living roadmap", "roadmap for my project", or wants a visual project roadmap synced to a repo instead of a static text plan. Also use to refresh/update an existing roadmap. NOT for daily task dashboards, BI/metrics dashboards from a data pipeline, or deployed web apps.
argument-hint: "[project folder or name]"
---

# Project Roadmap

**Point it at a project folder and it works.** Give it a path (or a project name), and it reads
what's already there — STATUS, roadmap, TASKS, DECISIONS, README, git log — infers a draft
roadmap, asks you only about the genuine gaps, and renders a living dashboard that beats a static
text plan: a North Star metric, a row of stage tiles, and per-stage Plan/Details panels showing
each step's status and owner — synced to the folder and hosted at a stable URL the team can revisit.

The point of pointing it at a folder: you don't fill in a blank template. The skill harvests the
docs you already maintain and only interviews you for what's actually missing (usually a couple of
owners and the north-star targets). A folder with a decent STATUS.md and roadmap becomes a
dashboard in 2-3 questions.

## What makes this good (don't lose it)

The value is **information architecture + discipline**, not the HTML:
- **Zoom**: program → stage tile → step sequence → details. A reader scans, drills into the one
  stage they care about, and only opens Details for the why. Preserve this layering.
- **Honest status**: the colored dot + a candid one-line pulse ("Built, just not wired") tells
  the truth a green checkmark can't.
- **Living, not static**: a markdown source of truth (`state.md`) + a weekly update ritual +
  one stable URL. A template alone rots; the ritual is what keeps it true.

Reproduce those, and a sparse, ugly dashboard won't happen.

## Two modes

Check the target folder first. If it already has a `state.md` + `index.html` from this skill →
**refresh**. Otherwise → **build**.

---

## Mode: build

Follow in order. Steps 1-3 are the discovery-first core — do not skip to rendering.

1. **Locate the project folder.** Confirm the path with the user if ambiguous.

2. **Harvest + infer a draft model.** Read the folder's existing docs and build the dashboard
   model — stages, steps, statuses, owners, north star — with a source citation or a gap flag
   per field. **Read `references/harvest.md` for the exact file list, the model schema, the
   inference rules, and the no-hallucinated-metrics rule.** This is the substance of the skill.

3. **Capture sources (always ask, even if the folder is rich).** Before gap-filling or rendering,
   ask the owner two questions:
   - **GitHub repos:** "Which repos should I check for PRs on every refresh?" Accept a list.
   - **Chat channels (Slack/Discord/etc.):** "Which channels carry signals for this project?"
     Accept "none" if there isn't one.
   Write these into the `## Sources` table in `state.md` immediately — this is the memory
   that makes every future refresh self-sufficient. The owner should never have to answer
   these questions again.

4. **Confirm before rendering — adaptive.** Gauge folder richness and ask accordingly:
   - Rich folder → present the draft as a scannable checklist, confirm, fill the few gaps
     (usually north-star targets + a couple owners). 2-3 questions.
   - Thin folder → escalate to a guided interview (north star → stages → per-stage owner/steps/
     status → priorities), batching questions.
   Map the project's status words to the canonical 6 (`live/progress/wire/next/later/blocked`)
   and confirm the mapping. Details and the review-checklist format are in `references/harvest.md`.
   **Never render HTML from raw inference — the human signs off on the model first.**

5. **Offer layout + theme — let the user choose.** Present the menu (don't decide for them):
   - **Layout (structure):** A · Stages & Steps · B · Now/Next/Later board · C · Milestone timeline.
   - **Theme (palette):** cream · forest · midnight.
   Recommend a default (A + cream unless the work is clearly a board or timeline), but it's their
   dashboard. **Read `references/design.md` for what each is best for and how they compose.**

6. **Render.** Copy the chosen layout file (`assets/layout-stages.html` / `layout-board.html` /
   `layout-timeline.html`) → `<project>/index.html`, set `<html data-theme="...">` to the chosen
   theme, then replace the sample content from the confirmed model. Also render `state.md` from
   `assets/state-template.md`. Keep them in lockstep; keep the `<style>` block untouched. **Read
   `references/components.md` for the field→component map, step-status classes, and taste guidance.**

7. **Deliver + record the URL.** The dashboard is a self-contained `index.html` — host it
   wherever fits your stack (GitHub Pages, Netlify, Vercel, S3, an internal static host, or just
   open the file locally). **Read `references/publish.md` for hosting options and the stable-URL
   rule.** Once it has a stable URL, record that URL in the project's `README.md` and at the top
   of `state.md` so every future refresh updates the same link.

8. **Leave it maintainable.** Add (or update) a short "Stable URL + weekly refresh" note in the
   project's README so the owner can run the refresh without this skill, and tell the user the
   one command to update it.

---

## Mode: refresh

For an existing dashboard (the living-update loop):

1. **Re-harvest broadly** — project folder AND external sources in parallel:
   - Local files: `STATUS.md`, `TASKS.md`, `HANDOVER.md` or equivalents in the project folder
   - **Read `state.md` `## Sources` section first** — it lists the exact repos and chat
     channels captured at setup. Use these; don't guess. If `## Sources` is missing (older
     dashboard built before this feature), ask the owner for repos + channels and write them
     in before continuing.
   - **GitHub PRs** for each listed repo (merged + open, last 2 weeks). Open PRs = active
     blockers. Merged PRs = steps that moved to `live`. PR titles are often the freshest
     source of truth — use them.
   - **Chat channels** listed in Sources — search for project name + key decision terms to
     catch anything decided in chat but not yet in a file.
   - **See `references/harvest.md` § External sources** for the full protocol.

2. **Diff** the new reality against the current `state.md` — what changed in status, what
   shipped, new blockers, new steps.
3. **Propose the changes** to the user as a short diff. Don't silently overwrite. Don't invent metrics.
4. **Apply** confirmed changes to BOTH `state.md` and `index.html`, bump the "Last updated"
   date in both, and add a top row to the Recent changes changelog (batch PRs into dated
   clusters — one row per meaningful cluster, not per individual PR; drop rows > ~4 weeks old).
5. **Re-host** at the same stable URL so the shared link never changes. Confirm to the user
   that the URL is unchanged and they don't need to re-share it.

---

## Guardrails

- **No unverified metrics.** If no doc has the number, blank the tile and ask. Never invent.
- **One project = one dashboard** (v1). Portfolio rollups are out of scope.
- **This is a roadmap, not a ticket tracker.** Linear/Jira/GitHub Issues own individual tickets;
  this owns the program narrative. Keep Details to what a stakeholder would ask.
- **Out of scope for v1:** data-pipeline/BI dashboards and a "critique my dashboard" review mode.
  If the user wants live metrics wired in, note it as a future enhancement, don't build a pipeline.
- **Don't edit the template's `<style>`.** Swap content, not design.
