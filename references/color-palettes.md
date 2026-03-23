# Color Palette System for Awwwards-Level Sites
## Curated palettes, token naming, dark mode, and contrast rules

---

## TOKEN NAMING CONVENTION (use in every project)

```css
:root {
  /* Surface tokens — background layers */
  --bg:          #F2EFE9;   /* page background */
  --bg-elevated: #FAFAF8;   /* cards, modals */
  --bg-sunken:   #E8E5DF;   /* recessed areas */

  /* Ink tokens — text and foreground */
  --ink:         #1A1A1A;   /* primary text */
  --ink-muted:   #6B6B6B;   /* secondary text */
  --ink-faint:   #A0A0A0;   /* tertiary/disabled text */

  /* Accent tokens — brand and interaction */
  --accent:      #8B2500;   /* primary brand color */
  --accent-soft: #B8532E;   /* hover states */
  --accent-bg:   rgba(139, 37, 0, 0.08); /* tinted backgrounds */

  /* Utility */
  --border:      rgba(26, 26, 26, 0.12);
  --shadow:      rgba(26, 26, 26, 0.06);
}
```

**Rule**: Never hardcode hex values in component CSS. Always reference tokens.

---

## PALETTES BY ARCHETYPE CATEGORY

### Editorial / Archival (Archetypes 01, 06, 07, 11, 17)
```css
/* Warm Parchment (recommended default) */
--bg: #F2EFE9;  --ink: #1A1A1A;  --accent: #8B2500;  --ink-muted: #6B6B6B;

/* Cool Ivory */
--bg: #F5F5F0;  --ink: #2D2D2D;  --accent: #3D5A80;  --ink-muted: #7A7A7A;

/* Museum Slate */
--bg: #EAEAE5;  --ink: #1C1C1C;  --accent: #6B4E3D;  --ink-muted: #8A8A8A;
```

### Sports / Athletic (Archetypes 09, 21)
```css
/* Ferrari Rosso */
--bg: #050505;  --ink: #FBFBFB;  --accent: #D40000;  --accent-soft: #FFF200;

/* Neon Energy */
--bg: #0A0A0A;  --ink: #F0F0F0;  --accent: #00FF87;  --accent-soft: #00D1FF;

/* Bold Racing */
--bg: #111111;  --ink: #FFFFFF;  --accent: #FF6B00;  --accent-soft: #FFB800;
```

### SaaS / Product (Archetypes 12, 13, 22, 23)
```css
/* Clean SaaS */
--bg: #FFFFFF;  --ink: #1A1A2E;  --accent: #6366F1;  --accent-soft: #818CF8;

/* Dark SaaS */
--bg: #0F0F23;  --ink: #E2E8F0;  --accent: #7C3AED;  --accent-soft: #A78BFA;

/* Warm SaaS */
--bg: #FEFCE8;  --ink: #1C1917;  --accent: #EA580C;  --accent-soft: #F97316;
```

### Luxury / Fashion (Archetypes 04, 15, 26)
```css
/* Black Gold */
--bg: #0D0A08;  --ink: #F2EFE9;  --accent: #C4900D;  --ink-muted: #8A8A8A;

/* Marble */
--bg: #F8F6F3;  --ink: #1A1A1A;  --accent: #8B6914;  --ink-muted: #999999;

/* Midnight Navy */
--bg: #0A1628;  --ink: #E8E6E1;  --accent: #C49B63;  --ink-muted: #7A8B9E;
```

### Brutalist / Raw (Archetypes 14, 20)
```css
/* True Brutalist */
--bg: #FFFFFF;  --ink: #000000;  --accent: #FF0000;  --ink-muted: #666666;

/* Inverted */
--bg: #000000;  --ink: #FFFFFF;  --accent: #00FF00;  --ink-muted: #999999;
```

### Kinetic / Playful (Archetypes 05, 10)
```css
/* Saturated Editorial (DontBoardMe style) */
--bg: #F2F0E6;  --ink: #1A1A1A;  --accent: #E33529;  --ink-muted: #888888;

/* Candy Pop */
--bg: #FFF8F0;  --ink: #2D1B4E;  --accent: #FF3366;  --accent-soft: #FF9933;

/* Electric */
--bg: #F0F0F0;  --ink: #111111;  --accent: #0066FF;  --accent-soft: #FF3300;
```

### Nature / Organic (Archetype 16)
```css
/* Forest */
--bg: #F5F2ED;  --ink: #2D3B2D;  --accent: #4A7C59;  --ink-muted: #8B9A8B;

/* Ocean */
--bg: #F0F4F8;  --ink: #1B2838;  --accent: #0077B6;  --ink-muted: #6B7B8D;
```

---

## DARK MODE IMPLEMENTATION

```css
/* Light theme (default) */
:root {
  --bg: #F2EFE9;
  --bg-elevated: #FAFAF8;
  --ink: #1A1A1A;
  --ink-muted: #6B6B6B;
  --accent: #8B2500;
  --border: rgba(26, 26, 26, 0.12);
}

/* Dark theme */
[data-theme="dark"] {
  --bg: #0D0A08;
  --bg-elevated: #1A1714;
  --ink: #F2EFE9;
  --ink-muted: #8A8A8A;
  --accent: #C4900D; /* often a warmer/lighter accent for dark */
  --border: rgba(242, 239, 233, 0.12);
}

/* Respect system preference */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    --bg: #0D0A08;
    --bg-elevated: #1A1714;
    --ink: #F2EFE9;
    --ink-muted: #8A8A8A;
    --accent: #C4900D;
    --border: rgba(242, 239, 233, 0.12);
  }
}
```

---

## CONTRAST RULES (WCAG AA)

| Combination | Minimum ratio | How to check |
|---|---|---|
| Body text on background | **4.5:1** | `--ink` on `--bg` |
| Large text (≥24px / 18px bold) | **3:1** | `--accent` on `--bg` |
| Interactive elements | **3:1** | Buttons, links |
| Disabled text | No minimum | But should be visually distinct |

### Quick contrast checks
```
✅ #1A1A1A on #F2EFE9 = 15.3:1 (excellent)
✅ #8B2500 on #F2EFE9 = 5.8:1  (passes AA)
✅ #F2EFE9 on #0D0A08 = 15.1:1 (excellent)
⚠️ #6B6B6B on #F2EFE9 = 4.7:1  (barely passes — OK for body)
❌ #A0A0A0 on #F2EFE9 = 2.5:1  (fails — use for decorative only)
```

---

## CRITICAL RULES

1. **Never use pure `#FFFFFF` or `#000000`** unless archetype explicitly demands it (only 14 Brutalist Raw)
2. **Max 2 colors + 1 accent** per palette. Exception: Archetype 20 Memphis (multiple intentional)
3. **Accent should appear sparingly** — CTAs, active states, one hero moment. Never flood the page
4. **Dark mode accent often differs** from light mode — lighter/warmer tones read better on dark backgrounds
5. **Test on real screens** — colors look different on MacBook vs external monitor vs phone
6. **Named tokens, not hex** — every color in CSS references a variable, never a hardcoded value
