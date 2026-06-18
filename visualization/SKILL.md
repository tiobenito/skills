---
name: visualization
description: "Create polished, single-file HTML visualizations for one-off artifacts — teaching materials, slide decks, architecture diagrams, interactive explanations, before/after comparisons, and demo decks. Zero dependencies, opens in any browser. Use when user says 'visualize this', 'make a presentation', 'build slides', 'make a diagram', 'create a visual', 'explain this visually', 'make an interactive diagram', 'create a training deck', 'build a training deck', 'make a curriculum deck', 'build a curriculum deck', 'make an HTML file', 'build the HTML', 'create an HTML', 'make this readable', 'make this easier to read', 'summarize this doc', 'turn this into sections', 'I don't want to read a wall of text', 'visualize this doc', or any request to build a standalone HTML artifact OR to turn a long document / wall of text into a readable, sectioned HTML page. Do NOT use for production web applications or deployable UIs."
---

# Visualization

Create single-file HTML visualizations — presentations, diagrams, dashboards, training materials — that look polished and professional. Zero dependencies, open in any browser.

## Format Selection

| Format | When to Use | Navigation |
|--------|-------------|------------|
| **Slides** | Presenting to an audience, training, demos | Arrow keys, dots, progress bar |
| **Tabbed Dashboard** | Technical reference, multi-view exploration | Click tabs, scrollable content |
| **Single Page** | One concept, a diagram, a comparison | Scroll only |
| **Doc Transform** | "Make this long doc / wall of text readable" | Scroll, section dividers |

## Doc Transform

Trigger when someone hands over a long doc and asks to "visualize this," "make it easier to read," or "break it into sections." Two sub-modes:

- **Summarize** — distill to decisions/takeaways. One card per item, lead with the answer.
- **Reformat (keep it all)** — preserve full content but impose structure: cards, headers, scannable sub-blocks.

**The card-per-item pattern is the workhorse.** Each item becomes a `.card` with a colored left-edge stripe, a mono tag + title, a one-line "the point," then labeled sub-blocks. Proven sub-blocks: **Scenario**, **What happens now** (orange/red callout), **Options** (2-col grid), **Before/After** (red/green top borders), **Recommendation** (green callout).

**Rules that make these land:**
- **Large body text** — 18–20px, line-height ~1.6
- **Lead with the conclusion** in each card
- **Examples become callouts, never inline prose**
- **Color = meaning, every time** — orange/red = current/broken/before, green = recommended/after/done, blue = info
- **Single page with section dividers** unless 6+ heavy items (then tabs)

## Design System

**Always read `references/design-system.md` first.** It has the token set and palette.

If `references/design-system.md` still uses the default theme, offer to customize it for the user's brand before building anything.

### Quick reference (default neutral dark)

```css
:root {
  --bg:#0f1923; --surface:#1a2d3d; --surface2:#213547; --border:#2a4055;
  --text:#e8f0f7; --text2:#8fa8be;
  --accent:#3b82f6; --accent2:#60a5fa;
  --green:#22c55e; --orange:#f59e0b; --red:#ef4444; --blue:#3b82f6;
}
```

Fonts: DM Sans (body) · Playfair Display (editorial headlines) · JetBrains Mono (code/numbers)

### Core principles

1. **Single file** — All CSS and JS inline. No external dependencies except Google Fonts.
2. **Color-coded semantics** — consistent throughout.
3. **Generous whitespace** — 60-80px padding, 20-24px gaps.
4. **Smooth transitions** — 0.3-0.45s ease on interactive elements.

## Building Blocks

### Cards & Grids
- **Stat cards** — Big number + label. `font-size: 48px` JetBrains Mono.
- **Compare cards** — Before/After with colored top borders (red/green). Side-by-side grid.
- **Classification cards** — Color-coded top stripe per category. 2x2 grid.

### Flow & Process
- **Flow diagrams** — Horizontal nodes with arrow connectors.
- **Timelines** — Vertical line with numbered circle steps.

### Data Display
- **Status badges** — Pill-shaped, color-coded.
- **Tables** — Light background mockup style.

### Interactive
- **Tabs** — Button row with active underline.
- **Speaker notes** — Toggle with 'n' key (slides only).

## Slide Presentation Template

Always include: progress bar (fixed top), navigation footer (brand label left, dot nav center, slide count right), keyboard navigation (arrow keys + space, 'n' for notes), fade + translateX transitions.

See `references/design-patterns.md` → "Slide Navigation JavaScript" for the exact JS.

## Workflow

1. Clarify content — what information? who's the audience? slides or dashboard?
2. Outline sections — draft titles before writing HTML
3. Build incrementally — structure first, then components
4. Keep text minimal on slides
5. Save to `_temp/` or ask where

## Quality Checklist

- [ ] Opens in browser with no errors
- [ ] All text readable (contrast, size, spacing)
- [ ] Navigation works
- [ ] Consistent color coding
