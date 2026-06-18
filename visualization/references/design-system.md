# Design System

The default is a neutral dark theme. Customize this file for your own brand — swap colors once and every visualization inherits them.

---

## Default — Neutral Dark Theme

```css
:root {
  --bg:      #0f1923;   /* page background */
  --surface: #1a2d3d;   /* card / panel background */
  --surface2:#213547;   /* slightly lighter surface */
  --border:  #2a4055;   /* borders, dividers */
  --text:    #e8f0f7;   /* primary text */
  --text2:   #8fa8be;   /* secondary / muted text */
  --accent:  #3b82f6;   /* primary accent */
  --accent2: #60a5fa;   /* lighter accent */
  --green:   #22c55e;   /* success / after / done */
  --orange:  #f59e0b;   /* warning / pending */
  --red:     #ef4444;   /* error / before / danger */
  --blue:    #3b82f6;   /* info / neutral callout */
}
```

### Typography

```css
@import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&family=Playfair+Display:wght@500;600&family=JetBrains+Mono:wght@400;500&display=swap');

font-family: 'DM Sans', system-ui, sans-serif;       /* body + UI */
font-family: 'Playfair Display', Georgia, serif;     /* editorial headlines */
font-family: 'JetBrains Mono', monospace;            /* code + numbers */
```

Type scale: h1 56px / h2 38px / body 16–20px / labels 12px uppercase

### Brand rules

- Swap `--accent` / `--accent2` to your brand color
- Progress bars: solid accent, 3–4px
- Footer brand label: `"[your brand] · <name>"` in 11px uppercase
- No gradients on backgrounds; border-radius ≤ 12px on cards

---

## Light Theme Alternative

```css
:root {
  --bg:      #F8F5EF;
  --surface: #FFFFFF;
  --surface2:#F0EBE3;
  --border:  rgba(28,25,23,0.12);
  --text:    #1C1917;
  --text2:   #57534E;
  --accent:  #0D9488;   /* teal — swap to your brand color */
  --accent2: #0D9488;
  --green:   #16A34A;
  --orange:  #D97706;
  --red:     #DC2626;
  --blue:    #2563EB;
}
```

---

## Shared semantic rules (both themes)

- Green = success / confirmed / "after"
- Orange = warning / pending
- Red = error / danger / "before"
- Blue = info / neutral call-out

Keep meanings consistent within a visualization.
