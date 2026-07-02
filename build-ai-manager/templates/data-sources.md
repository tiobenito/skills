# Data sources — {{AGENT_NAME}}

## The outcome metric

**{{METRIC_NAME}}** — {{METRIC_DEFINITION}}

Validated: {{HOW_VALIDATED — e.g. "sampled 20 real cases, confirmed the field is populated and
matches manual review"}}. Lives in: {{WHERE_THE_METRIC_LIVES}}.

## Shared identifiers

The key(s) that tie one case together across every source below: {{SHARED_IDENTIFIERS}}.

## Connected sources

| Source | How it connects | What it unlocks | Status |
|---|---|---|---|
| {{SOURCE_1_NAME}} | {{how — CLI / API / MCP / file}} | {{what it shows}} | ✅ / ❌ |
| {{SOURCE_2_NAME}} | | | |
| {{SOURCE_3_NAME}} | | | |

A review must query **every row marked ✅** — a connected-but-unqueried source makes the run
INCOMPLETE (see `review-method.md`). A row marked ❌ degrades the review; say so explicitly rather
than silently skipping it.

## Join recipe

How to pull a specific case's evidence from each source using the shared identifier(s) above:
{{JOIN_RECIPE — e.g. "query source 1 by case_id, then filter source 2's log by the same id within
the case's time window"}}.
