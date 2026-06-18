---
name: cc-level-up
description: Guided progression tool for improving your Claude setup. Detects your current level, explains what you can do next, and builds it for you. Designed for people new to Claude — especially non-technical users on claude.ai Projects. Use when user says "level up my setup", "what should I improve", "help me set up Claude", "cc level up", "how do I get better at Claude". Also use when someone is new to Claude and wants help getting started.
---

# CC Level-Up

Help the user improve their Claude setup one step at a time. Detect where they are, explain what's possible, and build it for them.

## References

- `references/level-guide.md` — Full progression path with level descriptions
- `references/org-context.md` — Role-to-content mapping for personalization (customize for your org)
- `references/last-level-up.md` — State from the most recent run
- `references/templates/claude-md-starter.md` — CLAUDE.md creation guide + template
- `references/templates/skill-starter.md` — Skill creation guide + template
- `references/templates/reference-starter.md` — Reference doc template

Read `references/org-context.md` before the build phase — it drives personalization.

## Tone

Friendly, encouraging, conversational. You're a helpful colleague, not an auditor. Teach real terms (CLAUDE.md, skill, reference doc) but always explain them in plain language first. Never use jargon without a one-sentence explanation.

**Platform assumption:** By default, assume the user is on claude.ai Projects (the web app), not the CLI. Do not mention hooks, auto-memory, MCP servers, or agents unless they reach Level 4. Use "project instructions" instead of "CLAUDE.md" in casual language (but use the real filename when creating files). If you know the user is on Claude Code (CLI), adjust accordingly.

## Performance Notes

- Max 3 recommendations per run. Do not overwhelm.
- Take your time on detection — read files, don't guess from names.
- Always show what you built before moving to the next thing.
- The user may not know what a "skill" is yet. Explain before asking if they want one.

---

## Step 1: Detect

Scan the user's project to determine their current level. Do this silently — don't narrate every file you're reading.

### What to Scan

| What | Where | What it tells you |
|------|-------|-------------------|
| CLAUDE.md | Project root | Exists? How many lines? Has behavioral rules? |
| Skills | `skills/` or similar folder | Count. Have frontmatter? Have references/? |
| Reference docs | `reference/` folder | Exists? How many docs? |
| Subfolder CLAUDE.md | Subdirectories | Any scoped instructions? |
| Folder organization | Project root | Beyond default course folders? |
| Previous run | `references/last-level-up.md` | Level last time? What was built? |

Use Glob and Read tools. Record findings internally.

### Level Assignment

Assign the highest level where ALL criteria are met:

**Level 0 — Getting Started**
- Has course folders (inbox/, processed/, outputs/) OR is a mostly empty project
- 0-1 skills
- No CLAUDE.md (or CLAUDE.md under 5 lines)

**Level 1 — Foundation**
- Has a CLAUDE.md with at least a role description (5+ lines)
- 2-3 skills with basic structure

**Level 2 — Established**
- CLAUDE.md has behavioral rules (not just a role description — includes "always/never" type instructions, format preferences, or workflow rules, 15+ lines)
- Reference folder with 2+ docs
- 4+ skills

**Level 3 — Power User**
- Well-organized folder structure (subfolders beyond course defaults)
- Skills have their own reference files or output templates
- Subfolder CLAUDE.md files OR evidence of progressive organization
- 6+ skills

**Level 4 — Graduate**
- Complete, well-organized system across all of the above
- Multiple refined skills with references
- Rich CLAUDE.md with clear sections
- Demonstrates system thinking (files reference each other, clear separation of concerns)

---

## Step 2: Present

Show the user their level and what's next. Keep it warm and concise.

### Format

Start with a greeting and their level:

> **Your setup: [Level Name]** (Level [N] of 4)
>
> [One sentence about what this level means — from level-guide.md]

If this is a returning user (last-level-up.md has previous data), celebrate progress:

> Since last time: [what changed — new skills added, CLAUDE.md grew, new reference docs]

Then show 2-3 recommendations as cards:

> **Here's what I'd suggest next:**
>
> **1. [Title]**
> [2-sentence plain-language description of what this is and why it helps]
>
> **2. [Title]**
> [2-sentence plain-language description]
>
> **3. [Title]** *(optional — only if relevant)*
> [2-sentence plain-language description]
>
> Which of these sounds useful? I can set any of them up for you right now.

### What to Recommend at Each Level

**Level 0 recommendations:**
1. Create your project instructions (CLAUDE.md) — "This is a file that tells Claude who you are and how you like to work. It loads automatically every time you start a conversation."
2. Build your first skill — "A skill is a reusable instruction set for a task you do repeatedly. Instead of explaining what you want every time, you just say 'use my [skill name]'."

**Level 1 recommendations:**
1. Start a reference library — "A reference folder is where you put docs Claude can look up — templates, glossaries, lists. Claude reads them when it needs context, so you don't have to paste them every time."
2. Add behavioral rules to your CLAUDE.md — "These are standing instructions like 'always use bullet points' or 'never round dollar amounts'. They shape every conversation without you having to repeat yourself."
3. Build another skill — "You've got [N] skills. Based on your role, [suggested skill] could save you time."

**Level 2 recommendations:**
1. Refine your skills — "Some of your skills could be sharper. [Specific suggestion — e.g., 'Your status report skill could include an output template so the format is consistent every time.']"
2. Organize with subfolder instructions — "You can add a CLAUDE.md inside a subfolder to give Claude context that's specific to that area. Good for [specific suggestion based on their folders]."
3. Add output templates to skills — "An output template inside a skill means Claude produces the same format every time. Great for reports, summaries, or anything with a consistent structure."

**Level 3 recommendations:**
1. Session notes habit — "Right now, each conversation starts fresh. A session notes file lets Claude pick up where you left off — what you were working on, decisions made, next steps."
2. Advanced skill patterns — "Your skills can reference other files, chain together, or include example outputs. [Specific suggestion based on their skills.]"
3. System maintenance — "Your setup is mature enough to benefit from periodic review. Things get stale — old references, outdated rules, skills you've outgrown."

**Level 4 recommendations:**
1. Mention the CLI — "There's a whole other level if you want it. Claude Code (the CLI version) gives you hooks that run automatically, persistent memory across sessions, MCP servers that connect Claude to your tools, and agent teams that work in parallel. It's a bigger learning curve, but the payoff is huge."
2. Suggest cc-audit — "If you move to the CLI, there's an audit skill that scores your entire setup and gives you a detailed report card. Good for power users who want to optimize."

---

## Step 3: Build

For each recommendation the user accepts, follow the **Explain + Build** pattern.

### The Pattern

1. **Ask** 2-4 personalizing questions relevant to what you're building
   - Read the relevant template from `references/templates/` for question ideas
   - Read `references/org-context.md` if this is role-dependent content
2. **Explain** what you're about to create (1-2 sentences, plain language)
3. **Build** the file(s)
4. **Show** a summary of what was created (quote key parts, not the whole file)
5. **Explain** how it changes their experience going forward (1-2 sentences)

### Critical Rules

- **Never overwrite an existing CLAUDE.md.** Read it first. Add to it, or ask before replacing content.
- **Never overwrite existing skills.** Suggest improvements or create new ones alongside.
- **Always show what you created** before moving to the next recommendation. Don't batch-create silently.
- **Use the user's own words** from their answers when writing content. Don't over-polish into corporate speak.
- **Keep skills simple.** First skills should be 15-30 lines, not 100-line masterpieces.
- **Keep CLAUDE.md minimal.** Start with 10-15 lines. Users can always add more. Resist the urge to front-load.
- **Explain file locations.** Tell the user where the file lives and why it's there: "I created this in your skills folder — that's where Claude looks for reusable instructions."

### Building a CLAUDE.md (Level 0)

1. Ask: role, primary use case, any format preferences
2. Read `references/templates/claude-md-starter.md` for the template
3. Read `references/org-context.md` for role-specific flavor
4. Create the file at the project root
5. Show what you wrote, explain that it loads automatically every session

### Building a Skill (Level 0-1)

1. Ask: what task to speed up, how they do it now, what good output looks like
2. Read `references/templates/skill-starter.md` for structure
3. Read `references/org-context.md` for role-appropriate suggestions
4. Create the skill folder and SKILL.md
5. Show the skill, explain how to invoke it ("just say 'use my [skill name]'")

### Building a Reference Library (Level 1)

1. Ask: what docs they reference often, what they wish Claude already knew
2. Read `references/templates/reference-starter.md` for structure
3. Create `reference/` folder and 2-3 starter docs
4. Show what was created, explain that Claude reads these when relevant

### Refining Skills (Level 2)

1. Read existing skills to identify improvement opportunities
2. Suggest specific changes: add output templates, split multi-purpose skills, add reference files
3. Ask before making changes — explain what you'd change and why
4. Make the edits, show the diff

### Adding Behavioral Rules (Level 1-2)

1. Ask: things they always want, things they never want, format preferences
2. Read existing CLAUDE.md
3. Add a "How I Like Things" or "Rules" section
4. Show what was added

---

## Step 4: Save State

After building everything the user requested, update `references/last-level-up.md`.

### What to Save

```markdown
# Last Level-Up

**Date:** [today's date]
**Level detected:** [N] — [Level Name]
**Previous level:** [N from last run, or "First run"]

## What Was Built

- [filename] — [one-line description of what it does]
- [filename] — [one-line description]

## Recommendations Given

1. [recommendation title] — [accepted/declined/skipped]
2. [recommendation title] — [accepted/declined/skipped]

## Suggested Next Run

[Date 2-4 weeks from now] — by then you'll have used what we built and be ready for the next step.
```

### Closing Message

End with something encouraging and forward-looking:

> That's it for now! You're at **[Level Name]** with [brief summary of what they have]. Use what we built for a couple weeks, and when you're ready for more, just say "level up" again.
