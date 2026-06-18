# Guide Deck Outline

The standard shape for a skill's guide deck. Pass this to the `visualization` skill so
every guide deck across a marketplace shares a structure. `visualization` handles all
HTML, styling, and the design system — this file is only the content spine.

The deck is a single-file HTML artifact saved as `<name>-guide.html` at the marketplace
repo root (GitHub Pages serves it at `<owner>.github.io/<repo>/<name>-guide.html`).

## Part 1 — Teaching preamble (optional)

Include this only when the audience is new to skills. Skip it when the recipient already
knows what a skill is.

Keep it short and general (not skill-specific):
- **What is a skill** — a folder of instructions Claude loads on demand.
- **The description is the trigger** — why the frontmatter `description` matters.
- **A skill is a folder, not a file** — `SKILL.md` plus `references/`.

## Part 2 — The five skill-specific sections (always)

1. **What this skill does** — the problem it solves, when it activates (the trigger
   phrases), and what the user gets out of it. One concrete "before / after."
2. **Install it** — the two commands, copy-pasteable:
   ```
   /plugin marketplace add <owner>/<repo>
   /plugin install <name>@<marketplace-id>
   ```
3. **Customize / set it up** — any first-run setup. If the skill ships a `design-system.md`
   or similar fill-in template, walk through customizing it here. If there is no setup,
   this section says so in one line ("Works out of the box — nothing to configure").
4. **A worked example** — one realistic end-to-end run: what the user types, what the
   skill does, what comes back. Show, don't just tell.
5. **Where to get help** — the marketplace repo link, how to report issues, how
   `learnings.md` lets the skill improve over time, and the `/plugin update` command.

## Style

- Defer all visual design to `visualization` and a neutral design system.
- One idea per slide; visual-first; short text. (`visualization` enforces this — let it.)
- The worked example is the most important slide. Make it concrete and specific.
