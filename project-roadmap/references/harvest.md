# Harvest & infer — turning a project folder into a draft dashboard model

This is the heart of the skill. The goal: read what a project already has, infer as much
of the dashboard as possible, then ask the human ONLY about real gaps. Never start from a
blank template if the folder has content.

## Table of contents
- 1. What to read (harvest)
- 2. The dashboard model (what you're building toward)
- 3. Inference rules (docs → model)
- 4. Status mapping (project words → canonical vocabulary)
- 5. Adaptive gap-filling (how hard to push depends on folder richness)
- 6. The review checklist (confirm before rendering)

---

## 1. What to read (harvest)

Read these from the target project folder, in roughly this priority. Most well-organized
projects have several. Skip what's absent; don't invent.

| Source | What it gives the model |
|---|---|
| `STATUS.md` | Current health, workstreams, blockers → stage statuses + "where it's at" |
| `ROADMAP.md` / `PLAN.md` / any `*roadmap*` | Stage/phase structure, ordering, targets |
| `TASKS.md` | In-flight work → step statuses; assignees → owners |
| `IMPROVEMENTS.md` | Backlog → `next` / `later` steps |
| `DECISIONS.md` | Operating principles + strategic-decisions section |
| `BUGS.md` | Open bugs → `blocked`/watch items in stage Details |
| `SOP.md` / domain SOP | Stage definitions, the "how it works" narrative |
| `README.md` | Program one-liner, north-star candidate, audience |
| `CLAUDE.md` | Scope, goals, owners, vocabulary |
| Recent git log (`git log --oneline -30`) | What's actually shipping → corroborates step status |
| `goals/` or `2026/` docs | North-star metric + targets |

Also glance at any existing `state.md` / `index.html` in the folder — if present, this is a
`refresh`, not a `build` (see SKILL.md).

Read excerpts, not whole files where they're large. You're extracting structure, not summarizing.

### External sources (always check on refresh, check on build when available)

These sources often contain the most current signal and are not in the local folder. **For
refresh mode in particular, skip them only if there is genuinely no relevant repo or channel —
never skip by default.**

| Source | How to check | What it gives |
|---|---|---|
| GitHub PRs (merged, last 2 weeks) | Search the relevant repos for recently-updated PRs with keyword terms (project name, feature names, key collaborators). Run 2–3 targeted queries. Use whatever GitHub tooling you have (the `gh` CLI, a GitHub MCP server, or the web). | Recent merges → steps to mark `live`; open PRs → active `progress` steps; PR titles often reveal fixes and features not yet in any local doc |
| GitHub open PRs | Same search, filtered to open PRs | Blockers, in-flight work, platform changes that gate your stages |
| Chat (if accessible) | Search your team's Slack/Discord/etc. for project-relevant terms | Decisions made in chat, unblocking news, status updates that never made it into a file |
| Handover / session notes | Any running notes doc the team keeps | Open threads, pending actions, what happened last session that's not yet in project files |

**GitHub routing:** if you keep separate work and personal GitHub identities, use the right account's tooling for each repo so you never cross-push.

**What to do with what you find:** treat PR titles + bodies as the freshest source of truth
on step status. A merged PR that ships a feature → mark that step `live`. An open PR that
blocks staging → surface it as a `blocked` step with the PR number. Batch the findings into
the changelog (newest first, one row per meaningful cluster, not per individual PR).

---

## 2. The dashboard model

Build this structure in your head (or as notes) before writing any HTML:

```
program:
  title
  cadence                 # weekly / biweekly
  north_star:
    statement             # the one outcome sentence
    today / next / end    # the target progression
  vocab[]                 # OPTIONAL: {term, definition} — only if the project has shared terms
  stages[]:
    name
    metric: {current, goal, definition}   # or {deferred: true, activation_condition}
    owner                 # first-class — every stage has one
    pulse                 # one-line current status
    steps[]:
      name
      status              # canonical: live|progress|wire|next|later|blocked
      note
      owner               # optional, step-level
    now_executing         # the 1-2 active things
    details:              # the "Details" tab depth
      blocks[]: {heading, items[]}
  principles[]            # OPTIONAL: {name, explanation}
  foundations[]           # OPTIONAL cross-cutting: {name, note, status}
  owners_weeks[]          # {owner, items[]} — "who's doing what"
  changelog[]             # {date, change} — newest first
```

Every field should trace to a source ("status from STATUS.md line 12") OR be flagged as a gap
to ask about. Track a confidence per stage/step: `confident` (stated in a doc) vs `inferred`
(you guessed from indirect signal) vs `missing`.

---

## 3. Inference rules (docs → model)

- **Stages** come from the roadmap/phase headings, or the top-level workstreams in STATUS.md.
  If the project has no explicit stages, propose them from the natural phases in the work
  (e.g. "data → model → integration → launch") and mark them `inferred` for confirmation.
- **Steps** are the sub-items under each stage: pipeline rows in STATUS, checklist items,
  TASKS rows scoped to that stage.
- **Status**: map from the project's own words (section 4). When a doc says "shipped",
  "merged", "in prod" → `live`. "WIP", "building" → `progress`. Etc.
- **Owners**: pull from TASKS assignees, "Owner:" lines, or git authorship. Owners are rarely
  written down cleanly — expect this to be a common gap to ask about.
- **North star**: look in goals docs and the README intro. If there's a single headline metric,
  use it. If there are several, the north star is usually the most *outcome*-level one (what the
  program is ultimately for), not an input metric. If none exists, this is a gap — ask.
- **Changelog**: seed from DECISIONS.md dates and recent git history; the human refines.

**Do not hallucinate metrics.** If a stage has no number in any doc, leave the tile metric
blank and ask — never invent a percentage. (Standing rule — never write unverified metrics.)

---

## 4. Status mapping (project words → canonical vocabulary)

The dashboard uses six step states. Map whatever the project says into these, and CONFIRM the
mapping with the human (it's subjective):

| Canonical | Dot color | Project words that usually mean this |
|---|---|---|
| `live` | green | live, shipped, in prod, deployed, done, merged & released |
| `progress` | amber (pulsing) | in progress, WIP, building, active, in review |
| `wire` | amber | built not wired, vendor selected, ready, waiting, staged |
| `next` | grey | not started, up next, designed, spec'd |
| `later` | faded | backlog, future, gated, someday, post-launch |
| `blocked` | red | blocked, stuck, waiting on <external>, dependency |

When several project terms collapse to one canonical state, show the human the mapping table
you chose and let them correct it once. Keep the project's own word as the visible
`step-status` label (e.g. "Built, not wired") — the canonical state only drives the color.

---

## 5. Adaptive gap-filling

Decide interview depth from how much the harvest produced:

- **Rich folder** (STATUS + roadmap + TASKS present, most stages/steps/statuses extracted with
  `confident`): present the draft and ask only to *confirm* + fill the handful of `missing`
  fields (usually north-star targets and a couple of owners). Two or three questions, max.
- **Thin folder** (sparse docs, most fields `inferred` or `missing`): escalate to a proper
  interview. Walk the human through, in order: (1) north star + targets, (2) the stages and
  their order, (3) per stage: owner, the steps, each step's status, (4) this period's priorities.
  Batch related questions; don't ask one at a time.
- **In between**: confirm what's confident, interview the gaps.

State your read explicitly: *"Your folder had a clear STATUS.md and roadmap, so I've drafted
4 stages — I just need the north-star targets and two owners."* vs *"There wasn't much structure
to go on, so let's build it together — start with the one outcome this program is driving toward."*

Always batch gap questions using the AskUserQuestion tool where the options are knowable
(e.g. proposed status mappings, candidate north-star metrics), free-text otherwise.

---

## 6. The review checklist (confirm before rendering)

NEVER render HTML straight from inference. First show the human the draft model as a compact,
scannable checklist so they can catch a hallucinated stage or a wrong status before it becomes
a dashboard:

```
Here's what I pulled from the folder — confirm or correct:

NORTH STAR:  {{statement}}   ({{today}} → {{next}} → {{end}})   [source / GAP]
STAGES:
  1. {{name}} — owner {{owner}} — metric {{current}}/{{goal}}   [confident]
     steps: {{s1}} (live), {{s2}} (progress), {{s3}} (wire) ...
  2. {{name}} — DEFERRED until {{gate}}                          [inferred — confirm?]
  ...
PRINCIPLES: {{n}} pulled from DECISIONS.md
GAPS I need from you: {{north-star targets}}, {{Stage 3 owner}}, ...
```

Only after the human confirms/edits do you render `index.html` + `state.md`
(see references/components.md for the field→component mapping) and publish
(see references/publish.md).
