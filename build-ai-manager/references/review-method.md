# Review method — used by every generated `<agent>-manager` skill

> A procedure, not a menu. Run every step, in order, using every connected data source. Never
> conclude from a single source. The most common way these reviews fail: skipping the Deviations
> pass entirely, or using only one data source and producing a shallow status tally while the other
> connected sources sit unqueried. The insight lives in the **merge** of sources plus the
> **invariant sweep**, not in any one source alone.

## How to run (non-negotiable)

1. **Create a todo per Run-Contract item;** check each off or mark `⚠️ SKIPPED — <reason>`.
2. **In order.** Both reviews → **Deviations first**.
3. **Use the real tool for every source in `data-sources.md`'s "Connected sources".** A source
   that's connected but **not queried makes the run INCOMPLETE.** One source's outcomes alone is a
   status tally, not a diagnosis.
3a. **Entity-scoped, never window-global.** First build the set of cases/runs in the window and
   extract their **shared identifiers** (whatever id/timestamp/key ties a case across sources — see
   `data-sources.md`). Then filter *every* evidence query to those identifiers. A global "everything
   in the window" pull is noise — it drags in unrelated work.
4. **Confirm against what actually ran** — the live/deployed version of the code or config, not a
   local draft or an in-progress branch.
5. **Novelty before filing** — a known stub/TODO, an open fix already in flight, or an existing
   ticket on the same issue ⇒ report **"known/in-flight,"** link it, don't duplicate.
6. Any in-scope contract section missing ⇒ end with **`INCOMPLETE — step <n> not done`**, not a
   conclusion.

## Run Contract — a complete run MUST produce, in this order

- **[0] Scope** — review(s) + environment (asked, never inferred).
- **[1] Inputs** — the goal, the metric, the process/SOP, and `data-sources.md`'s signal map.
- **[2] Tier-0 — build the run set first, then scan (ENTITY-SCOPED)** — each line a result or
  `SKIPPED — <reason>`:
  - **Run set + identifiers FIRST:** list the agent's runs/cases in the window; extract their
    shared identifiers. Every query below filters to these — **never a window-global dump.**
  - Status distribution + **stuck** cases (running past any expected SLA)
  - **Every other connected source's signal for these specific cases** (errors, non-2xx responses,
    flagged events — whatever each source tracks), grouped by case
  - **Invariant sweep:** check each case's log against **every rule the process/SOP states** — list
    each rule ✅ / ⚠️ / violated
- **[3] Runs selected** — the attention-list cases **+ a sample of clean/successful cases** (for
  catching silent failures and judging non-deterministic output).
- **[4] Deviations block** *(if in scope)* — per case: **reconstructed, time-ordered log** merged
  across sources · process-diff · **cause confirmed against the live code/config** · severity ·
  **evidence chain**. Or `SKIPPED — <reason>`.
- **[5] Improvements block** *(if in scope)* — operator-manager, **hypothesis-driven loop run until
  ≥3 data-SUPPORTED hypotheses** (refuted → retry, no confirmation-bias); each mapped to a
  lever-space intervention, sized, guardrail-checked, feasibility-scored; **top-5
  leverage-ranked** in the Improvements table; + a feedback-loop delta on prior accepted changes.
  Or `SKIPPED — <reason>`.
- **[6] Quality sample** — on clean cases: were any non-deterministic outputs (composed text,
  classification decisions, judgment calls) actually correct? Or `SKIPPED — <reason>`.
- **[7] Decision log + novelty** — drop already-decided findings; check for known stubs, in-flight
  fixes, existing tickets on the same signature → "known/in-flight," not duplicated.
- **[8] Ranked presentation** — **two separate tables**: **Deviations** (ranked by severity) and
  **Improvements** (ranked by goal-impact), each with columns **`Issue | Hypothesis | Evidence |
  Suggestion`** (Evidence = the multi-source chain).
- **[9] Self-audit** — restate [0]–[8]; **confirm every connected source was actually queried**;
  declare `COMPLETE` or `INCOMPLETE`.

---

## Step 0 — Gate (first, every time) → [0][1]

Ask **Deviations, Improvements, or both?** and **which environment?** (never infer). Load
`goal.md`, `data-sources.md`, `attention-list.md`, `decision-log.md`; read the full process/SOP.
Confirm `data-sources.md`'s connection coordinates are present. Don't proceed until answered; both
→ Deviations first.

## Tier-0 multi-source scan → [2]

Run every scan in `data-sources.md`'s tool map over **all** cases in the window. This is cheap and
exhaustive — it's the workhorse for the "succeeded but actually wrong" class. Every connected source
must be hit here at least once.

## Pick cases → [3]

Attention-list case types (`attention-list.md`) **+ a sample of clean, successful cases** — the
clean sample is how silent failures and the quality-judgment class get caught.

## Deviations — did the agent follow the plan? → [4]

Per selected case:
1. **Reconstruct actual behavior** — merge every connected source's record of the case into one
   **time-ordered** log; write it to a scratch file and search it rather than holding the full
   history in context.
2. **Compare against the full process/SOP.** Divergence → hypothesis.
3. **Confirm the cause** in the **live code/config** before asserting (correlation ≠ causation).
4. Group by root cause, score by **severity**, attach the **evidence chain**.

## Improvements — act as the operations manager → [5]

**You are the operations manager accountable for the goal (`goal.md`) under its guardrails, using
all available context.** Don't report — find the highest-leverage changes that move the goal, and
**prove each with data.** Only after Deviations (when running both).

**Hypothesis-driven loop — run until you have ≥3 hypotheses the data SUPPORTS:**
1. **Theorize like an operator** — a specific, falsifiable claim about *why* the goal is missed or
   *what* would move it. Source ideas from **discriminator analysis** (what separates goal-HIT from
   goal-MISS cases) and **frontier benchmarking** (what the best-performing segment does
   differently).
2. **Test it with a real query** on an **adequate sample** — the last ~100 cases or a meaningful
   window, **not just the most recent case** (small samples mislead) → state **SUPPORTED** (with the
   numbers, the sample size, and confidence) or **REFUTED**.
3. **If REFUTED, discard it and form a new hypothesis — repeat.** Never stop at a refutation; never
   twist data to fit — a hypothesis is "supported" only on genuine evidence, and refuted ones get
   logged too (no confirmation-bias, no tautologies). If you can't reach 3 supported hypotheses in
   the connected data, say so and report the missing data — that gap is itself a finding.

From the **≥3 supported hypotheses**, build the findings: map each to an intervention (a change to
the process, the code, an agent decision rule, a data correction, or an upstream fix), size the
opportunity (baseline → addressable volume → expected lift), check the **guardrail budget**, and
score **feasibility + effort** (must be realistically buildable). Rank by **leverage** (Reach ×
Impact × Confidence ÷ Effort × feasibility); present the **top 5** in the Improvements table (§[8]).
Guardrail judgments made without supporting data are first-principles and labeled as judgment, not
fact. **Feedback loop:** re-measure previously-accepted suggestions (referenced from
`decision-log.md`) and report the metric delta.

## Quality sample → [6]

On the clean sample, judge the agent's non-deterministic outputs and decisions: was composed
text correct and on-policy? Was a classification/parsing decision accurate? Was an escalation to a
human warranted (or wrongly withheld)? These are the failures no status code or log line reveals on
its own.

## Novelty + decision log → [7]

Drop fingerprints already in `decision-log.md`. Also scan for known stubs/TODOs in the code, changes
already in flight, or existing tickets on the same signature → report "known/in-flight" and link,
never duplicate.

## Present + decide → [8]

Present survivors in **two separate tables**, one row per finding, ranked (Deviations by
**severity**, Improvements by **goal-impact**):

**Deviations** — *did the agent follow the plan?*

| Issue | Hypothesis | Evidence | Suggestion |
|---|---|---|---|

**Improvements** — *can the plan do better on the goal?*

| Issue | Hypothesis | Evidence | Suggestion |
|---|---|---|---|

Then the human **accepts / rejects (reason) / defers** each row. Never self-act. On accept: a
code fix → open an issue/PR through whatever the user's normal process is; a process/SOP change →
draft the edit, file a ticket only if it warrants one. Append every decision to `decision-log.md`.

## Self-audit before finishing → [9]

Restate [0]–[8], mark each ✅ or ⚠️ `SKIPPED — <reason>`, **explicitly confirm each connected source
was queried**. If any in-scope section is missing → `INCOMPLETE — step <n> not done`.

---

## Finding fields (one row per finding; rendered in the [8] tables)

Every finding is captured as exactly four fields — the table columns:
1. **Issue** — the deviation (Deviations) or opportunity (Improvements) in one line, with a
   **severity / impact** tag.
2. **Hypothesis** — the suspected mechanism/root cause; marked **confirmed** (against the live code,
   with a specific reference) or **hypothesis** (+ its open question). Correlation ≠ causation.
3. **Evidence** — the **multi-source chain**: what each connected source showed, tied together by
   the shared identifier, plus the specific process/SOP rule or metric it violates.
4. **Suggestion** — the grounded fix (if any), or **known/in-flight → link**, or **none — needs
   `<open item>`**.

## Finding taxonomy

- **(a)** plan goal-misalignment *(Improvements)* · **(b)** guardrail-vs-goal tuning *(Improvements)*
- **(c)** conformance gap — the process says it, the code doesn't *(Deviations)* · **(d)** absent
  primitive *(Improvements)*

## Disciplines (non-negotiable)

- **Multi-source or it's incomplete** — query every connected source; conclude from the merge, not
  one source.
- **Entity-scoped, not window-global** — build the case set + shared identifiers first; filter every
  evidence query to them. A global dump is noise.
- **Time-ordered merge** — order from one source alone misleads.
- **Hypothesis until confirmed** — correlation ≠ causation.
- **Live/deployed reality, not a draft** — confirm against what actually ran; record the reference.
- **Invariant sweep every run** — check the run against every rule the process states, not a
  favorite few.
- **Novelty before filing** — known/in-flight ⇒ link, don't duplicate.
- **Cluster by root cause; calibrated confidence with explicit open items.**
- **Hypothesis-driven Improvements** — theorize → query → verdict; keep going until **≥3
  data-supported hypotheses**; refuted → try another; never confirmation-bias, never a tautology.
