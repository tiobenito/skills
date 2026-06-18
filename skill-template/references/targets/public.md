# Target: Public

A public, generic skill for anyone to install from a marketplace repo.

## Repo & account

| Field | Value |
|---|---|
| Marketplace repo | A public repo you own (e.g. `<your-username>/skills`) — ask which. |
| GitHub account | Your personal/public account. |
| Tooling | Whatever git tooling you normally use for that account. **Never mix accounts** — if you keep work and personal GitHub identities separate, confirm you're using the right one. |
| Marketplace id | The top-level `name` in `.claude-plugin/marketplace.json` — read it, don't assume. |
| Default branch | `main` |
| GitHub Pages | Optional — if enabled, HTML at the repo root is served at `<owner>.github.io/<repo>/<file>`. |

## plugin.json values

- `author`: `{ "name": "<your name or handle>" }`
- `homepage`: `https://github.com/<owner>/<repo>`
- `license`: `MIT` (or your choice)

## Strip policy — FULL

This skill goes to strangers. Strip aggressively to a generic, public template:

- Strip **all** personal names, contacts, and user-specific context.
- Strip **all** company/brand names and internal product names.
- Convert any hardcoded brand/design into a `design-system.md` fill-in template with a
  neutral default theme (the `visualization` template pattern).
- Replace work-specific examples with generic illustrative ones.
- Flag every MCP tool / CLI a stranger is unlikely to have; remove the feature or document
  the dependency clearly.
- Plus the always-strip list in `detect-personal.md` (secrets, paths, etc.).

## Deck

- Design system: **neutral template theme**.
- **Include the teaching preamble** (`guide-deck-outline.md` Part 1) if the audience may be
  new to skills.

## Publish

Standard sequence in `plugin-manifest-formats.md`. Branch first, validate with `--strict`,
stop before push, and hand the user the push command for the correct account. Smoke test:

```
/plugin marketplace add <owner>/<repo>
/plugin install <name>@<marketplace-id>
```
