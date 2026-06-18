# {{PROGRAM_TITLE}} — Program State

Companion source-of-truth for `index.html`. Update this each {{CADENCE}} during review; the dashboard mirrors what's here.

**Last updated:** {{UPDATED_DATE}}
**Audience:** {{WHO_READS_THIS}}
**Cadence:** {{CADENCE}} review

---

## Sources

Checked automatically on every refresh. Add or remove rows as the project evolves.

**GitHub repos** (last 2 weeks of merged + open PRs):
| Repo | What to look for |
|---|---|
| `{{org}}/{{repo}}` | {{e.g. the area of the codebase this repo covers}} |

**Chat channels** (Slack, Discord, etc. — search for recent decisions + status signals):
| Channel | What to look for |
|---|---|
| `#{{channel-name}}` | {{e.g. status updates, blocker callouts, decisions}} |

---

## North Star

**{{NORTH_STAR_STATEMENT}}**

{{one or two sentences on what counts and at what level — e.g. shipment-level vs stop-level}}

- **Current:** {{value or "not yet measured"}}
- **Next target ({{period}}):** {{target}}
- **End-state:** {{target}}
- **Gating stage:** {{which stage paces the whole program}}

---

## Sub-metrics

| Stage | Metric | Current | Target |
|---|---|---|---|
| 1 — {{name}} | {{what it measures}} | {{value}} | {{goal}} |
| 2 — {{name}} | (deferred) | — | {{activation condition}} |
| 3 — {{name}} | {{what it measures}} | {{value}} | {{goal}} |

---

## Vocabulary (optional — delete if the program has no shared terms)

{{Glossary of domain terms the team keeps using. Keep it to the few terms that genuinely
need a shared definition; this is not a dictionary.}}

| Term | Definition |
|---|---|
| `{{term}}` | {{definition}} |

---

## Strategic decisions

{{Durable calls that shape the program. These map to the "Operating principles" panel.}}

- **{{Decision}}.** {{one line — what and why}}

---

## Stage 1 — {{Name}}

**Owner:** {{name}} ({{role, e.g. strategy / build / platform}}).

**Where it's at:** {{2-3 sentence narrative of the stage's current reality}}

### Pipeline

| Status | Component | Owner | Notes |
|---|---|---|---|
| {{status}} | {{step}} | {{owner}} | {{note}} |

### Push forward (next 2 weeks)
- {{action}}

### Blockers
- {{named dependency, or "none"}}

---

<!-- Repeat the Stage block for each stage. A deferred stage adds: -->
## Stage 2 — {{Name}} (DEFERRED)

**Owner:** {{name}}.

**Activation gate:** {{the condition that turns this stage on}}.

**Why deferred:** {{the reasoning}}

---

## This {{cadence}}'s top priorities ({{primary owner}})

1. {{priority}}

---

## Open decisions

- **{{decision}}** — {{status / who owns it}}

---

## Recent changes (rolling 4 weeks)

| Date | Change |
|---|---|
| {{Mon DD}} | {{what changed / what was decided}} |
