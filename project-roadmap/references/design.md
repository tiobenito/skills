# Design choices — layout × theme

The dashboard has two independent choices. Offer BOTH to the user (with a recommendation),
then compose: pick a layout file, set its theme. They are orthogonal — any layout works in
any theme because every layout's colors come from CSS variables.

## Step 1 — pick a LAYOUT (structure)

Three self-contained templates in `assets/`. Each is a working, themeable example filled with
sample content — copy the chosen one to `<project>/index.html` and replace the content.

| Layout | File | Structure | Best for |
|---|---|---|---|
| **A · Stages & Steps** | `layout-stages.html` | North star → stage tiles → per-stage Plan/Details tabs with a horizontal step sequence | Phased / gated programs that move through stages. The richest; most drill-down. |
| **B · Now/Next/Later board** | `layout-board.html` | North star → metric strip → Shipped/Now/Next/Later columns of initiative cards | Continuous, non-linear work that doesn't form a clean pipeline. Scannable at a glance. |
| **C · Milestone timeline** | `layout-timeline.html` | North star → metric strip → vertical dated timeline with a "today" marker | Deadline-driven programs (launches, seasonal cutoffs) where dates are the spine. |

How to choose: if the work is **sequential phases** → A. If it's **parallel streams in flight** → B.
If it's **anchored to dates/deadlines** → C. When unsure, default to **A** — it carries the most
information and stakeholders respond to it. Show the user this menu; let them pick.

All three share: the north-star header, owners as first-class, the same 6 status states
(`live/progress/wire/next/later/blocked`), and the bottom panels (principles + changelog).
The harvest/infer model from `harvest.md` feeds all three — only the rendering differs.

## Step 2 — pick a THEME (palette)

Each layout file embeds all three themes as `[data-theme]` blocks. Switch by changing ONE
attribute on the `<html>` tag: `data-theme="cream" | "forest" | "midnight"`.
Nothing else changes — all component colors are variables.

| Theme | `data-theme` | Look | Best for |
|---|---|---|---|
| **Warm cream** | `cream` | Light, warm, teal + terracotta accents | Editorial / print-friendly. Default. |
| **Forest** | `forest` | Crisp white + deep green, mint accent | Data-forward, clean; pairs well with reports. |
| **Midnight** | `midnight` | Dark green surfaces, light text, mint accent | Screen-sharing / presentations. |

Recommend `cream` for a doc-style living tracker, `midnight` for something shown on a call,
`forest` for a clean data-forward look. The user decides — it's their dashboard.

## Composing

1. Copy the chosen layout file → `<project>/index.html`.
2. Set `<html data-theme="...">` to the chosen theme.
3. (Optional) delete the two unused `[data-theme]` blocks to keep the file lean — harmless to leave.
4. Replace the sample content with the confirmed model (see `components.md` for the field map;
   the component classes are identical across themes).
5. Render `state.md` from `assets/state-template.md` and host it (see `publish.md`).

All palette hex values live in the `[data-theme]` blocks at the top of each layout file —
edit them there if you want to match your own brand.
