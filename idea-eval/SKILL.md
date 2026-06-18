---
name: idea-eval
description: "Idea evaluation pipeline — score, persist, and manage business ideas with a weighted 8-criteria rubric. Use when someone says 'validate this idea', 'score this idea', 'evaluate this idea', 'list ideas', 'view an idea', 'submit idea', 'add note', or presents a business idea to score. Supports listing, viewing, submitting, adding notes, quick scoring, deep framework analysis, and re-evaluation. Persistence is configurable (local files by default; optionally a GitHub repo). For general-purpose framework analysis without scoring/persistence, use idea-pressure-tester instead."
---

# Idea Evaluator

Evaluates business ideas through a structured pipeline, scores them on 8 weighted criteria, and persists results so you can track and revisit them over time.

## Setup — configure persistence (first run)

This skill stores evaluated ideas somewhere. Pick one of these and tell the user which is in effect:

- **Local files (default)** — write to `./ideas/<slug>/` in the current project (or a path the user names). Zero dependencies; works everywhere.
- **GitHub repo (optional)** — if the user wants ideas synced/shared, ask for a repo (`<your-org>/ideas`) and write under a `database/` prefix there. Use whatever GitHub tooling the user has configured for that account.

A `references/team-bios.md` file (used when scoring **Team Fit**) describes who would build these ideas. It ships as a fill-in template — **the user customizes it with their own team's skills and constraints before first use.** If it's still the template, ask the user about their team, or score Team Fit on stated info only.

## Modes

Determine mode from user input:

- **Quick mode** (default): "validate this idea", "score this", or presenting an idea
- **Deep mode**: "deep dive", "pressure test", "deep mode" — framework analysis
- **Score mode**: "re-eval", "re-score", "update score for [idea]"
- **List mode**: "list ideas", "show ideas", "what ideas do we have"
- **View mode**: "view [idea]", "show me [idea]", "details on [idea]"
- **Submit mode**: "submit idea", "new idea", "add idea"
- **Note mode**: "add note to [idea]", "note on [idea]"

---

## Quick Mode: 7-Stage Pipeline

Run stages sequentially. Between each stage, briefly share key findings and ask if the user wants to add context. Don't dump raw JSON — synthesize into readable insights.

### Stage 1: Intake

Extract from the user's raw idea description:

- **idea_name**: Short catchy name (2-4 words)
- **one_liner**: One sentence elevator pitch (max 15 words)
- **type**: consumer_app | smb_service | marketplace | saas | physical_product | other
- **problem**: The core problem being solved (1-2 sentences)
- **target_user**: Who specifically has this problem (be specific)
- **proposed_solution**: What the product/service does (1-2 sentences)
- **value_prop**: Why someone would pay for this vs alternatives
- **initial_revenue_model**: Best guess at how this makes money

Present the structured brief. Ask: "Does this capture your idea accurately? Anything to adjust?"

### Stage 2: Workshop

Think critically about the idea:

- **Sharpen the problem**: What's the real pain? How do people currently cope?
- **Key assumptions**: 3 things that must be true for this to work
- **Variations**: 2+ meaningfully different angles
- **Revenue models**: 2+ ways this could make money with price ranges
- **Biggest risk**: The single biggest reason this could fail (be specific)
- **Unfair advantage needed**: What would make this team specifically the right one?

Push the thinking — don't just validate.

### Stage 3: Thesis Fit (Scoring)

Read `references/pipeline/03-thesis-fit.md` for detailed scoring guidelines.

Score on 8 criteria (1-5 each). Reference `references/team-bios.md` when scoring Team Fit.

**Criteria & Weights:**
| Criterion | Weight |
|-----------|--------|
| Competition | 1.5x |
| Team Fit | 1.5x |
| Bootstrappability | 1.25x |
| Passive Potential | 1.25x |
| Side Project Viability | 1.25x |
| AI Leverage | 1.0x |
| Revenue Clarity | 1.0x |
| Market Timing | 0.75x |

**Composite score formula:**
```
raw = sum(score * weight for each criterion)
max = 5 * 9.5 = 47.5
base_score = (raw / max) * 100
```

**Modifiers (applied after):**
- Market growing: +3
- Market shrinking: -5
- Has moat potential (not "none"): +2

Cap at 100, floor at 0.

**Verdicts:** STRONG (75-100), PROMISING (60-74), MEH (45-59), PASS (0-44)

Present scores as a table with reasoning. Show the composite score and verdict prominently.

### Stage 4: Market Research

Analyze:
- **Competitors**: 3-5 (include substitutes like spreadsheets, manual processes)
- **Market size**: TAM, SAM, target niche (don't inflate — use ranges if uncertain)
- **Trends**: Growing/stable/shrinking with evidence, tailwinds, headwinds
- **Distribution channels**: Prioritize low-cost channels (SEO, communities, word-of-mouth)
- **Moat potential**: network_effects | data | brand | switching_costs | none

### Stage 5: Feasibility

Focus on NON-TECHNICAL blockers. Assume the team can build anything. The real question is:
- Does this require a big marketing budget, sales team, regulatory approvals, physical infrastructure, or ongoing operational burden that doesn't fit a side project?
- What are the real blockers and how severe?
- Cost to operate at modest scale?
- Is this actually feasible at 4-8 hrs/week?

### Stage 6: Monetization

Be realistic — this is a side project:
- Primary model with specific pricing
- Unit economics at 100, 500, and 1000 users
- Time to first dollar (be honest — most side projects take months)
- Revenue ceiling with side-project effort
- Pricing psychology for the target user

### Stage 7: Summary & Persist

Generate an executive summary covering:
1. One-paragraph verdict (is this worth building?)
2. Top 3 strengths
3. Top 3 concerns
4. Recommended next steps (specific and actionable)
5. Who should lead this and why

Then **persist results** to the configured store (see Setup):

1. Build the full `idea.json` (see schema below)
2. Create an empty `notes.json`: `{"notes": []}`
3. Save a history snapshot: `history/YYYY-MM-DD-initial.json`
4. Update a `RANKINGS.md` — insert the new idea in score order, update timestamp

**Local store:** write all files under `./ideas/<slug>/`.
**GitHub store (if configured):** push the same files under `database/ideas/<slug>/` and update `database/RANKINGS.md`. Always save locally first as a durable backup, then push — if the push fails, tell the user the files are safe locally.

**slug** = lowercase, hyphenated (e.g. "boat-storage", "voice-reservation-bot"). Used as the folder name and the `id` field.

---

## List Mode: Browse All Ideas

Read all idea folders from the configured store. For each slug, load `idea.json` and `notes.json`.

Display a markdown table sorted by composite_score (descending), drafts at the bottom:

```
| # | Idea | Score | Verdict | Status | Notes | Type |
|---|------|-------|---------|--------|-------|------|
| 1 | Example One | 72 | PROMISING | evaluated | 3 | consumer_app |
| 2 | Example Two | 65 | PROMISING | evaluated | 1 | smb_service |
```

After the table: "Use `view <slug>` to see details, or `submit` to add a new idea."

---

## View Mode: Idea Details

Takes a slug. Load `idea.json` and `notes.json` from the store. Display in sections:

**1. Header**
```
## Example One — "one-liner here" (PROMISING - 72)
Status: evaluated | Type: consumer_app | Effort: solo
Submitted by: <name> | Created: YYYY-MM-DD
```

**2. Brief** — all 5 fields from the brief object
**3. Scores** — table with all 8 criteria, score, weight, reasoning
**4. Summary** — full summary text if present
**5. Notes** — chronological: `[date] author (type): "text"`
**6. Evaluation History** — if any re-evaluations exist

If slug not found: "No idea found with slug '<slug>'. Use `list` to see all ideas."

---

## Submit Mode: Create New Idea

Prompt for: **Name** (2-4 words), **One-liner** (max 15 words), **Submitter**.

Generate slug: `name.toLowerCase().replace(/[^a-z0-9]+/g, "-").replace(/(^-|-$)/g, "")`

Create a draft `idea.json` (status `draft`, scores all 0) and empty `notes.json`, save to the store.

After saving: "Created '<name>' as a draft. Score it anytime, or `view <slug>` to see it."

---

## Note Mode: Add Note to Idea

Takes a slug. Load existing `notes.json`. Prompt for: **Text**, **Type** (validation/concern/pivot/general), **Author**.

Append a note object (`id`, `author`, `date`, `text`, `type`) to the array and save.

After saving: "Note added to <idea name>. It now has <N> notes total."

---

## Deep Mode: Framework Analysis

Use the 9 innovation frameworks in `references/frameworks/` for thorough analysis.

### Process

1. **Discovery**: Ask questions to understand the idea, goal, and stage
2. **Framework Selection**: Recommend 1-3 frameworks

**Selection Matrix:**
- Validate worth pursuing → 5 Whys, JTBD, or Assumption Testing
- Identify blind spots → Pre-mortem, Fishbone
- Refine to make actionable → Pareto, Design Thinking
- Test core assumptions → Assumption Testing + First Principles
- New venture (comprehensive) → Pre-mortem + Assumption Testing + JTBD

3. **Apply Framework**: Load from `references/frameworks/` and follow conversationally
4. **Synthesize**: Key insights, validated/invalidated assumptions, next steps
5. **Offer to Score**: "Want me to run the scoring pipeline on this now?"

**Framework files** (in `references/frameworks/`): framework-five-whys, framework-first-principles, framework-pre-mortem, framework-jtbd, framework-assumption-testing, framework-value-proposition-canvas, framework-fishbone, framework-pareto, framework-design-thinking.

---

## Score Mode: Re-evaluate

1. Load existing `idea.json` from the store
2. Show current scores and verdict
3. Ask: "What's changed? What new information do you have?"
4. Re-run stages 3-7 with the new context
5. Save updated `idea.json`; snapshot the previous version to `history/YYYY-MM-DD-re-eval.json`
6. Update `evaluation_history` and `RANKINGS.md`
7. Show the score delta: "Score changed from X to Y (+/- Z)"

---

## idea.json schema

```json
{
  "id": "<slug>",
  "name": "<name>",
  "one_liner": "<one_liner>",
  "submitter": "<submitter>",
  "created": "YYYY-MM-DD",
  "updated": "YYYY-MM-DD",
  "status": "draft | evaluated",
  "type": "consumer_app | smb_service | marketplace | saas | physical_product | other",
  "effort_level": "solo | pair | team",
  "tags": [],
  "brief": { "problem": "", "target_user": "", "proposed_solution": "", "value_prop": "", "initial_revenue_model": "" },
  "scores": {
    "ai_leverage": { "score": 0, "reasoning": "" },
    "competition": { "score": 0, "reasoning": "" },
    "bootstrappability": { "score": 0, "reasoning": "" },
    "revenue_clarity": { "score": 0, "reasoning": "" },
    "passive_potential": { "score": 0, "reasoning": "" },
    "team_fit": { "score": 0, "reasoning": "" },
    "side_project_viability": { "score": 0, "reasoning": "" },
    "market_timing": { "score": 0, "reasoning": "" }
  },
  "composite_score": 0,
  "verdict": null,
  "summary": "",
  "evaluation_history": []
}
```

---

## Team Context

This rubric is tuned for a small team building AI-powered side projects: a few people, limited hours per week (4-8 each), bootstrap-only, optimizing for passive or semi-passive income. Two tracks:
1. **Short-term: Consumer apps** — quick-to-ship AI tools, SEO/virality
2. **Long-term: Niche SMB services** — capture a small cohort of SMBs, become their trusted tech partner

**Key principle:** for the SMB track, the relationship is the moat, not any single product.

Customize `references/team-bios.md` to reflect your own team — it drives the Team Fit score.

---

## Tone & Style

- Be direct and honest — don't sugarcoat scores
- Push back on weak ideas respectfully
- Use data and reasoning, not vibes
- Celebrate genuine strengths
- Use tables for scores, prose for insights
