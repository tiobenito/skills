---
name: skill-template
description: Package a Claude Code skill into a clean, marketplace-ready template. Use when the user says "publish this skill", "templatize this skill", "ship this skill to the marketplace", "turn this skill into a template", "make this skill installable", or wants to take an existing skill (or a workflow idea) and prepare it as an installable plugin. Orchestrates skill-creator and visualization; does not replace them.
---

# Skill Template

Turn a skill (or a workflow idea) into a clean, **marketplace-ready template** — a generic,
packaged skill anyone can install. The deliverable is always a template. Publishing is the
last step, and this skill stops before the push so the user decides where it goes.

This skill **orchestrates**, it does not duplicate:
- `skill-creator` — builds and restructures the skill itself.
- `visualization` — builds the optional guide deck.

Keep this lean. Detailed formats live in `references/` — read each one when its step calls for it.

## Step 1 — Pick the mode

| Mode | Input | Path |
|---|---|---|
| **A — Templatize** (primary) | A skill that already exists | Heavy: read it, strip it, restructure it |
| **B — New from request** (secondary) | A description of a workflow | Light: build it fresh, then a safety check |

Most runs are Mode A — the user has already built and used a skill and now wants to share
it. Default to Mode A unless the user is describing something that does not exist yet.

## Step 2 — Build the skill body

Build the skill into a **staging directory** (e.g. `/tmp/skill-template-<name>/`) — never
directly into the destination repo. Step 4 branches the repo and moves the finished skill in.
This keeps "no writes to the destination before the branch" literally true, and an abandoned
run leaves only a throwaway temp dir.

### Mode A — Templatize an existing skill

1. Read the whole source skill — `SKILL.md`, every file in `references/`, `examples/`,
   and `learnings.md`.
2. Run the **detect-personal pass**: read `references/detect-personal.md` and follow it. It
   strips personal/internal references and converts hardcoded brand into a fill-in template,
   so the skill works for a stranger out of the box.
3. Triage `learnings.md` (also covered in `references/detect-personal.md`) — keep generic
   skill-improvement entries, strip personal ones, confirm the split with the user.
4. Hand the cleaned, confirmed body to **`skill-creator`** to restructure into a proper
   skill (lean SKILL.md, references split out, frontmatter, naming) — written into the
   staging directory.

### Mode B — Build a new skill from a request

1. Hand the workflow description to **`skill-creator`** to build the skill from scratch,
   into the staging directory.
2. Run the detect-personal pass as a **light safety check only** — `skill-creator` writes
   generic skills by default, so this just catches accidental personal references
   (`/Users/...` paths, real names, secrets). Do not run the full Mode A strip machinery.

## Step 3 — Guide deck (always ask)

Always ask the user whether this skill needs a guide deck. Never skip silently.

Carry a recommendation into the question: **suggest a deck when the skill has install-time
setup the user must be walked through** (e.g. a `design-system.md` to customize). A skill
that just works on install can be covered by a README section instead — but the user decides.

If yes:
1. Read `references/guide-deck-outline.md` for the standard 5-section outline.
2. Hand off to the **`visualization`** skill — do not build HTML directly. Pass it the
   outline and a neutral design system.
3. Output: `<name>-guide.html`, built into the staging directory. Step 4 places it at the
   destination repo root.

## Step 4 — Stage to the destination

Ask the user where this should go — a marketplace repo they own, a personal repo, or just a
local folder. Then read `references/plugin-manifest-formats.md` and:

1. **Branch first.** Create a feature branch in the destination repo (`add-skill-<name>`)
   *before any writes to that repo* — an abandoned run must never dirty `main`.
2. Move the staged skill into `plugins/<name>/skills/<name>/`, create the plugin manifest,
   place the guide deck (if any) at the repo root, update the marketplace manifest, and
   update the README — the exact file list is in `references/plugin-manifest-formats.md`.
3. **Validate** with `--strict`, on **both** the marketplace root (checks `marketplace.json`)
   and the new plugin dir `plugins/<name>` (checks `plugin.json`). Fix and re-run on failure.
4. **Stop.** Commit to the branch, show the user `git diff --stat` and the exact push
   command, and stop. Do **not** push. Do **not** touch `main`.
5. Print the install command so the user can smoke-test after pushing.

## Hard rules

- **Never push.** Stage to a branch, hand the user the push command, stop.
- **Never cross-push.** If you keep separate work and personal GitHub identities, never use
  one account's tooling on the other's repo.
- **Never ship secrets.** Tokens, env var names, and account slugs are stripped, always.
- **Orchestrate, don't reimplement.** `skill-creator` builds skills; `visualization` builds
  decks. This skill sequences them and handles the packaging layer.
