# Target: Personal

A skill just for you — packaged and versioned so it can be backed up, synced across
machines, or kept tidy. Not published to a public marketplace.

## Repo & account

| Field | Value |
|---|---|
| Destination repo | A personal repo you name — ask which. Common choice: a synced repo (e.g. a dotfiles or notes repo) so the skill lands as a real directory and rides `git pull` to other machines. |
| GitHub account | Your personal account. |
| Tooling | Whatever git tooling you normally use for that account. |
| Repo layout | Usually a plain skill directory committed in place — no `plugins/` wrapper, no `marketplace.json` — unless you explicitly want a personal marketplace. |

## plugin.json values

Only relevant if you want a marketplace-style package. Otherwise skip — a personal
skill is just a `SKILL.md` + `references/` directory.

## Strip policy — MINIMAL

It is your own skill. Do not genericize it:

- Keep names, brand, and context.
- Still apply the always-strip list in `detect-personal.md` — never commit secrets or
  tokens, even to a private repo. Absolute personal paths can stay if the repo is only
  ever used by you on the same machine; flag them if cross-machine sync is the goal
  (a `/Users/<name>/...` path may differ between machines).

## Deck

- Ask whether a deck is wanted at all — a personal skill rarely needs one.
- If yes: neutral design system, skip the teaching preamble.

## Publish

Branch first, commit the skill directory, stop before push, hand the user the push command.
No marketplace manifest unless asked for one.
