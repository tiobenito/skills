---
name: {{AGENT_NAME}}-manager
description: Use when reviewing {{AGENT_NAME}} — auditing whether it followed its process (deviations) or whether its plan can better hit its goal (improvements). Triggers include "review {{AGENT_NAME}}", "audit {{AGENT_NAME}} runs", "is {{AGENT_NAME}} hitting its goal". Takes an optional argument: deviations, improvements, or both. Not for setup — that's build-ai-manager.
allowed-tools: ["Read", "Write", "Edit", "Bash", "Glob", "Grep", "AskUserQuestion"]
---

# Manager — {{AGENT_NAME}}

Run the review method in **`./review-method.md`** against this agent, using its context in the
sibling files:

- `goal.md` — what to maximize + guardrails
- `data-sources.md` — the validated outcome metric, signal map, join recipe, connected sources
- `attention-list.md` — case types to always deep-dive
- `decision-log.md` — past decisions (consult before presenting; write after deciding)

**Follow `./review-method.md` as a strict procedure:** create a todo for each Run-Contract item, run
every step in order (if *both* reviews are chosen, **Deviations fully completes before
Improvements**), never silently skip (mark `SKIPPED — <reason>`), and declare the run `INCOMPLETE`
if any in-scope step is missing. Begin with its **Step 0 gate** (which review? which environment?).
