# Detect-Personal Pass

The core of Mode A. Scan the source skill for elements that should not ship as-is, propose
a generic replacement for each, and let the user confirm. Extend the categories below as
new patterns show up — this file is meant to grow.

## How to present findings

**Detect + propose, user confirms.** Do not dump one giant diff, and do not ask 40
separate questions.

Work **file by file** — `SKILL.md` first, then each file under `references/`, then
`examples/`. For each file, show all of its flags together as before/after pairs. The user
approves the file or adjusts specific items, then move to the next file.

## What to scan for

The proposed replacement depends on the target's strip policy (see the target file).
Two columns below: **always-strip** applies to every target; **target-dependent** is
governed by the target's strip policy.

### Always strip — every target, no exceptions

| Category | Flags | Replacement |
|---|---|---|
| Secrets / tokens | API keys, PATs, `GITHUB_PERSONAL_ACCESS_TOKEN*`, any token value | Remove. Never ship. If the skill needs one, document it as a named env var the user provides. |
| Account slugs | Private repo slugs, internal org names tied to credentials | Remove or replace with a `{placeholder}`. |
| Absolute personal paths | `/Users/<name>/...`, home-relative project paths | Relative path, or a `{placeholder}` the user fills in. |
| Machine-specific config | Hardcoded config dirs, local hostnames, `~/.config/...` paths | `{placeholder}` or a setup instruction. |

### Target-dependent — governed by the target's strip policy

| Category | Flags | Public (full strip) | Personal (minimal) |
|---|---|---|---|
| Personal names | Real people, contacts | Generic role ("the user", "your team") | Keep |
| Company / brand names | Employer or product names | Strip — make generic | Keep |
| Brand / design | Hardcoded hex palettes, fonts, logos | Convert to a fill-in template file (`design-system.md` pattern) | Keep |
| Work-specific examples | Sample content tied to a real ticket, deal, or workflow | Replace with a generic illustrative example | Keep |
| Environment references | MCP tools / CLIs that won't exist for the recipient (internal tools, custom rake tasks) | Flag — note as a dependency or remove the dependent feature | Keep |

When in doubt about a target-dependent item, ask. Never guess on something that could
leak private or work-internal content into a public skill.

## The `design-system.md` template pattern

When a skill hardcodes a brand, do not just delete the colors — that
breaks the skill. Convert them into a customizable reference file:

- Move the palette, fonts, and brand rules into `references/design-system.md`.
- Replace the hardcoded values with a neutral default theme.
- Add a first-run instruction to the SKILL.md: "If `design-system.md` still has the
  default theme, offer to set it up before building anything."

This is exactly how the `visualization` skill was templatized — the skill keeps working
out of the box and the user makes it theirs once.

## `learnings.md` triage

`learnings.md` is not blanket-emptied. Classify each entry:

- **Generic skill-improvement** — a craft lesson true for any user of the skill
  (e.g. "always add ORDER BY when a query has LIMIT"). **Keep it.**
- **Personal / contextual** — tied to the user, their company, or a one-off situation
  (e.g. "the user prefers their brand's dark palette"). **Strip it.**

Present a summary before changing anything:

> "6 learnings found — 4 generic and worth keeping, 2 personal. Here's the split: ..."

Let the user confirm or adjust. The shipped `learnings.md` carries the kept entries so the
recipient starts with real craft knowledge instead of a blank file.

## Output

A cleaned skill body the user has signed off on, ready to hand to `skill-creator` for
restructuring (Step 2 of SKILL.md).
