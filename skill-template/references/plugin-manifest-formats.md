# Plugin & Marketplace Manifest Formats

The packaging layer. A Claude Code plugin marketplace is a git repo; each installable
skill is a *plugin* inside it. This file documents the file shapes and the staging
sequence. Repo paths and the GitHub account come from where the user chose to publish (SKILL.md Step 4).

## Repo layout

```
<marketplace-repo>/
├── .claude-plugin/
│   └── marketplace.json          ← catalogs every plugin
├── plugins/
│   └── <name>/
│       ├── .claude-plugin/
│       │   └── plugin.json        ← this plugin's manifest
│       └── skills/
│           └── <name>/
│               ├── SKILL.md
│               ├── references/    ← if the skill has them
│               ├── examples/      ← if the skill has them
│               └── learnings.md   ← triaged, not blank
├── README.md                     ← human-facing skill list
└── <name>-guide.html             ← optional guide deck, repo root
```

`<name>` is identical everywhere: the plugin dir, the skill dir, the skill's frontmatter
`name`, and the `marketplace.json` entry. Hyphen-case, lowercase, digits and hyphens only.

## `plugins/<name>/.claude-plugin/plugin.json`

```json
{
  "name": "<name>",
  "description": "One-line description. Match the marketplace.json entry.",
  "version": "1.0.0",
  "author": { "name": "<your name or handle>" },
  "homepage": "<repo homepage URL>",
  "license": "MIT",
  "keywords": ["<3-6 keywords>"],
  "skills": "./skills"
}
```

New skills start at `version` `1.0.0`. A re-publish of an existing skill bumps it
(patch for fixes, minor for new behavior).

## `.claude-plugin/marketplace.json`

Append one object to the `plugins` array — do not rewrite the file:

```json
{
  "name": "<name>",
  "source": "./plugins/<name>",
  "description": "One-line description. Match plugin.json."
}
```

The top-level `name` of this file (e.g. `aistudycamp-skills`) is the marketplace id used
in the install command — read it, do not assume it.

## `README.md`

Add a `### <name>` subsection under the existing "Skills" heading: one paragraph on what
the skill does and when it activates, plus the install command. If a guide deck was
built, link it. Match the tone and structure of the entries already there.

## Staging sequence

The skill itself was already built in a staging directory (SKILL.md Step 2). This
sequence moves it into the marketplace repo.

1. **Branch first.** In the marketplace repo: `git checkout -b add-skill-<name>` *before*
   writing anything to it. An abandoned run must never leave changes on `main`.
2. Move the staged skill into `plugins/<name>/skills/<name>/`, and write
   `plugins/<name>/.claude-plugin/plugin.json`.
3. Append the `marketplace.json` entry.
4. Update `README.md`.
5. Place `<name>-guide.html` at the repo root (only if a deck was built).
6. **Validate** with `--strict`, on both paths — the command validates whichever manifest
   the path resolves to:
   - `claude plugin validate <marketplace-repo> --strict` → checks `marketplace.json`
   - `claude plugin validate <marketplace-repo>/plugins/<name> --strict` → checks `plugin.json`

   `--strict` fails on unrecognized fields and missing metadata. Fix and re-run until clean.
7. Commit to the branch. Show the user `git diff --stat` and the exact push command for
   the destination account. **Stop — do not push.**
8. Print the install commands for the post-push smoke test:
   ```
   /plugin marketplace add <owner>/<repo>      # first time only
   /plugin install <name>@<marketplace-id>
   ```

## Notes

- `claude plugin validate` and `claude plugin validate --strict` are real commands.
  `claude plugin tag` exists too, for cutting a `{name}--v{version}` release tag.
- Installed plugin files land in `~/.claude/plugins/cache/`. A user who customizes a
  template file (e.g. `design-system.md`) edits the cached copy; a later `/plugin update`
  would reset it. Acceptable for install-once use — but mention it in the guide deck.
