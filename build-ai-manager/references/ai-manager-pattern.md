# The AI Manager pattern

## The one-paragraph pitch

An **AI manager** is a read-only layer that sits on top of an **AI employee** — any agent that
executes a defined process (an SOP, a set of rules, a workflow, a no-code automation). The employee
is deterministic and process-compliant by design; that's its strength and its blind spot. The
manager's only job is to keep asking one question: **where did the employee do its job but miss the
goal?** It never takes production action itself — it observes what happened, compares it to the
goal, ranks what matters, proposes a fix, and hands a human exactly two decisions to make.

## The employee / manager split

| | AI Employee | AI Manager |
|---|---|---|
| Optimizes for | process-compliance | the goal itself |
| Behavior | deterministic, bounded by the process as written | reasons over real outcomes, probabilistic |
| Blind spot | the process was written with limited foresight and doesn't update itself | none of its own — it exists to find the employee's blind spots |
| Output | does the work | a ranked list of where the work missed the goal, each with a proposed fix |

The failure mode this fixes: a process can be followed perfectly and still miss the goal, because
the process encoded yesterday's best guess about what would work. The manager is the mechanism that
closes that gap over time instead of leaving it to whoever happens to notice.

## The contract

```
IN:    a GOAL (one sentence) · guardrails (what it must never do) · the process/SOP · the code ·
       read access to what actually happened (data, logs, traces)

LOOP:  pull what happened  →  find where it missed the goal  →  rank by goal-impact (not raw count)  →
       attach a concrete fix to each finding  →  route it (code / data / human)  →  one report

OUT:   one report a human reads · a ranked, evidence-backed, fix-routed list of findings ·
       exactly two human taps: (1) which findings become work, (2) approve the resulting fix/write
```

## The test: manager, or just a dashboard?

Everything above compresses to two yes/no questions:

1. **Does it rank by goal-impact, not raw severity or row count?** (A structural fact affecting
   thousands of cases can matter less than a fixable lever affecting a dozen.)
2. **Does every finding carry a proposed fix, not just a description of the problem?**

If either is "no," it's an auditor or a dashboard — useful, but not a manager. Plenty of tools
already produce dashboards; the manager's job is to close the loop.

## Two implementation shapes

This generator produces the **agentic, on-demand** shape by default — a strict procedure Claude
runs live when called, reasoning about the evidence rather than executing pre-written checks. It
fits event-driven agents full of judgment calls (replies, escalations, quality of an LLM's output),
can discover failure modes nobody thought to check for in advance, and reaches deeper (it can read
the process and the code, not just the numbers).

The other valid shape is a **scripted, scheduled pipeline** — cheaper and deterministic, but limited
to whatever checks were pre-written, and better suited to high-frequency, numeric/statistical work
with a clean outcomes table. Building that shape is out of scope for this generator; if an agent
fits that profile better, treat this pattern doc as the spec and hand-build the pipeline instead,
keeping the same contract (goal-impact ranking, fix on every finding, two-tap governance).

## Governance: the two-tap ceiling

Every finding that survives ranking gets a **comfort tier** — can the manager confidently propose a
concrete fix (still only built on a human's go-ahead), or does it need a human's judgment first?
Either way, the manager never merges its own change, never fires a write, never files a ticket
unprompted. A **decision log** — what was accepted, rejected, and why — is permanent and consulted
before every future run, so a rejected finding never comes back to nag. This ceiling is deliberately
conservative; loosen it only after a system has proven itself over real runs.

## What's honestly still open

- **Delegated deep-research** for the highest-impact findings (a sub-agent reading the process and
  code in depth so the manager's own reasoning doesn't get diluted) is a natural extension, not
  something this v1 generator scaffolds automatically.
- **A "manager of managers"** — a coordination layer above several agents' managers — is a future
  idea, not part of this pattern yet.
- The manager is only as good as its read access. If a data source can't be verified live during
  setup, the review runs degraded on whatever it can reach and should say so explicitly rather than
  pretending to full coverage.

---

## Worked example: scripted pipeline

A prediction agent (forecasts an appointment time for each shipment and books it automatically)
had a daily Python pipeline auditing its own output — goal: *push an accurate prediction for every
eligible case, and have it stick.* It ran daily against the underlying data warehouse, checking
"what got pushed vs. eligible," rolling accuracy, and whether a pushed value later got overridden.
Findings ranked by `100 × lever × confidence × reach` — *lever* (how addressable/goal-moving this
*kind* of finding is) dominated the score on purpose, so a fixable dozen-row lever outranked a
structural fact affecting thousands of rows. Two follow-on tools consumed its output: one for a
human to triage findings, one that built and adversarially reviewed a fix before opening a gated PR.

## Worked example: agentic on-demand skill

A logistics company built a generator skill that interviewed the owner of a scheduling operation
(an agent that emails a facility, classifies the reply, and books or escalates), validated five
connected data sources live (a workflow engine, a data warehouse's change-log, the app's own API,
an error tracker, and a log/trace store), and scaffolded a standalone review skill for that specific
operation. The generated skill ran two reviews on demand: *Deviations* (did the agent follow its own
process?) and *Improvements* (run as an operations-manager, hypothesis-driven, until at least three
data-supported hypotheses backed a recommendation). Its first real run connected all five sources
and surfaced genuine findings — including one bug already fixed in production (confirmed via a
before/after check on live data) and a concrete opportunity to cut escalations by having the agent
answer with information it already held instead of asking a human.
