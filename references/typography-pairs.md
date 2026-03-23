# Typography Pairs for Awwwards-Level Sites
## Curated font combinations used in SOTY and SOTD sites

**Critical rule**: NEVER use Inter, Roboto, or Arial as a display/headline font.
These are only acceptable as invisible UI utility fonts (microtext, labels, form fields).

---

## PAIRING 1: Archival / Museum
**Feeling**: Timeless, literary, elegant restraint

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display / Hero | Cormorant Garamond | 300 Light or 400 | Let it breathe — massive size |
| Body / Editorial | Cormorant Garamond | 400 Regular | Italic for subtitles |
| Signature / Logo | Pinyon Script or Mrs Saint Delafield | 400 | Script logotype |
| Micro-labels | Inter | 500 Medium | Uppercase, wide tracking (0.3em) |

```css
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;1,400&family=Pinyon+Script&family=Inter:wght@400;500&display=swap');

.display { font-family: 'Cormorant Garamond', Georgia, serif; font-weight: 300; }
.signature { font-family: 'Pinyon Script', cursive; }
.micro { font-family: 'Inter', sans-serif; font-weight: 500; letter-spacing: 0.3em; text-transform: uppercase; font-size: 10px; }
```

---

## PAIRING 2: Bold Editorial / Magazine
**Feeling**: Confident, authoritative, directional

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display / Hero | Playfair Display | 700 Bold or 400 | High contrast strokes, dramatic |
| Subheadings | Playfair Display | 400 Regular Italic | Pairs with bold beautifully |
| Body | Libre Baskerville | 400 | Readable at small sizes |
| Micro-labels | Inter or DM Sans | 400 | Neutral contrast |

```css
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Libre+Baskerville:ital@0;1&family=Inter:wght@400&display=swap');
```

---

## PAIRING 3: Kinetic Agency / Studio
**Feeling**: Energetic, modern, technically sophisticated

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display | Bebas Neue | 400 | All-caps, condensed, impactful |
| OR Display | DM Serif Display | 400 | Contrast: unexpected elegance |
| Body | DM Sans | 300–400 | Clean, readable |
| Accent | DM Serif Display Italic | 400 | One word in italic for tension |

```css
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Serif+Display:ital@0;1&family=DM+Sans:wght@300;400&display=swap');
```

---

## PAIRING 4: Luxury / Fashion
**Feeling**: Refined, minimal, expensive

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display | Gilda Display | 400 Light | Hairline serifs, maximum elegance |
| OR Display | Cinzel | 400 | Roman inscriptions, classical |
| Body | Raleway | 300 Light | Wide, airy |
| Price/Details | Raleway | 200 Thin | Even thinner for premium feel |

```css
@import url('https://fonts.googleapis.com/css2?family=Gilda+Display&family=Cinzel:wght@400&family=Raleway:wght@200;300&display=swap');
```

---

## PAIRING 5: Creative / Experimental
**Feeling**: Unexpected, memorable, personal

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display | Space Grotesk | 700 | Geometric but with character |
| OR Display | Syne | 700–800 | Wide, contemporary, distinctive |
| Body | Syne | 400 | Same family, different weight |
| Accent | Syne Mono | 400 | Monospace accent for code-feel |

```css
@import url('https://fonts.googleapis.com/css2?family=Syne:wght@400;700;800&family=Syne+Mono&display=swap');
```

---

## TYPOGRAPHIC SCALE RULES

For Awwwards-level sites, the scale must be DRAMATIC. No gentle progression.

```css
/* ARCHIVAL / MUSEUM scale */
:root {
  --text-hero:     clamp(5rem, 12vw, 11rem);   /* One letter, a name */
  --text-display:  clamp(2.5rem, 5vw, 5rem);    /* Section titles */
  --text-headline: clamp(1.5rem, 3vw, 3rem);    /* Sub-titles */
  --text-body:     1rem;                         /* 16px */
  --text-caption:  0.875rem;                     /* 14px italic */
  --text-micro:    0.625rem;                     /* 10px uppercase labels */
}

/* KINETIC / AGENCY scale */
:root {
  --text-hero:     clamp(6rem, 18vw, 18rem);   /* MASSIVE — viewport-filling */
  --text-display:  clamp(2rem, 4vw, 4rem);
  --text-body:     1rem;
  --text-micro:    0.6875rem;                   /* 11px */
}
```

## TYPOGRAPHIC BEHAVIORS

### Large text that scrolls as background texture
```css
.bg-text {
  position: absolute;
  font-size: clamp(10rem, 25vw, 22rem);
  font-weight: 700;
  color: rgba(26, 26, 26, 0.04);
  letter-spacing: -0.05em;
  pointer-events: none;
  user-select: none;
  white-space: nowrap;
  overflow: hidden;
}
```

### Vertical writing (for panel tabs)
```css
.vertical-text {
  writing-mode: vertical-rl;
  text-orientation: mixed;
  transform: rotate(180deg);
  font-size: 10px;
  letter-spacing: 0.15em;
  text-transform: uppercase;
}
```

### Tracked uppercase micro-label (always use this for category labels)
```css
.label {
  font-family: 'Inter', sans-serif;
  font-size: 10px;
  font-weight: 500;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: var(--ink-muted);
}
```

### Editorial italic subtitle
```css
.subtitle {
  font-family: 'Cormorant Garamond', serif;
  font-size: 13px;
  font-style: italic;
  letter-spacing: 0.1em;
  color: var(--ink);
  opacity: 0.8;
}
```

---

## PAIRING 6: Neo-Brutalism
**Feeling**: Bold, unapologetic, physically present

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display | Space Grotesk | 700–800 | Geometric with personality |
| OR Display | Syne | 800 | Wide, contemporary |
| Body | Space Grotesk | 400 | Same family, different weight |
| Code/Accent | Space Mono | 400 | Monospace for data/prices |

```css
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;700;800&family=Space+Mono&display=swap');
.display { font-family: 'Space Grotesk', sans-serif; font-weight: 800; letter-spacing: -0.02em; }
```

---

## PAIRING 7: Scifi / Futuristic
**Feeling**: Technical precision, edge-of-possibility, systematic

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display | Orbitron | 600–700 | Use sparingly — very sci-fi |
| OR Display | Space Grotesk | 600 | More versatile, less on-the-nose |
| Body | DM Sans | 300–400 | Clean, neutral |
| Data/Terminal | Space Mono or IBM Plex Mono | 400 | All data outputs |

```css
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@600;700&family=Space+Grotesk:wght@400;600&family=Space+Mono&display=swap');
.terminal { font-family: 'Space Mono', monospace; font-size: 12px; letter-spacing: 0.05em; }
```

---

## PAIRING 8: Sports / Athletic
**Feeling**: Power, speed, condensed urgency

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display | Bebas Neue | 400 | Condensed, all-caps — the sport poster font |
| Numbers/Stats | Barlow Condensed | 700–800 | Tabular, strong |
| Body | Barlow | 400–500 | Same family, readable |
| Labels | Barlow Condensed | 500 uppercase | Tight, wide-tracked |

```css
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Barlow+Condensed:wght@500;700;800&family=Barlow:wght@400;500&display=swap');
.stat-number { font-family: 'Barlow Condensed', sans-serif; font-weight: 800; font-variant-numeric: tabular-nums; }
```

---

## PAIRING 9: Academic / Research
**Feeling**: Rigor, credibility, longform readability

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display | EB Garamond | 400–500 | Scholarly authority |
| OR Display | Source Serif Pro | 600 | More modern academic |
| Body | Source Serif Pro | 400 | Generous line-height (1.75) |
| Data/Citations | IBM Plex Mono | 400 | Tables, footnotes, code |
| Labels | IBM Plex Sans | 500 | Clean, institutional |

```css
@import url('https://fonts.googleapis.com/css2?family=EB+Garamond:wght@400;500&family=Source+Serif+4:wght@400;600&family=IBM+Plex+Mono&family=IBM+Plex+Sans:wght@400;500&display=swap');
.body-text { font-family: 'Source Serif 4', serif; font-size: 18px; line-height: 1.75; }
```

---

## PAIRING 10: Retro / Nostalgic
**Feeling**: Warm, analog, time-stamped authenticity

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display | Abril Fatface | 400 | Slab serif poster — Victorian impact |
| OR Display | Playfair Display | 700 | More editorial retro |
| Body | Lora | 400 | Warm oldstyle reading font |
| Typewriter accent | Courier Prime | 400 | Typewriter feel for labels/quotes |

```css
@import url('https://fonts.googleapis.com/css2?family=Abril+Fatface&family=Lora:ital,wght@0,400;1,400&family=Courier+Prime&display=swap');
.typewriter { font-family: 'Courier Prime', monospace; letter-spacing: 0.05em; }
```

---

## PAIRING 11: Restaurant / Hospitality
**Feeling**: Warm refinement, occasion, culinary authority

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display | Canela | 300 Light Italic | THE restaurant font — elegant, warm |
| OR Display | Freight Display | 300 Italic | Similar warmth, editorial feel |
| Body | Lora | 400 | Warm, readable at menu sizes |
| Menu items | Lora | 400 Italic | Elegant item names |
| Prices/Details | Cormorant Garamond | 300 | Thin, refined |

```css
/* Canela not on Google Fonts — use Cormorant Garamond as accessible alternative */
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;1,300;1,400&family=Lora:ital,wght@0,400;1,400&display=swap');
.menu-item { font-family: 'Lora', serif; font-style: italic; }
```

---

## PAIRING 12: Organic / Handcrafted
**Feeling**: Warm, personal, made-by-hand intimacy

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display | Fraunces | 300–400 Italic | Optical variable, ink-trap quality |
| Handwritten accent | Caveat | 600 | For one key phrase only |
| Body | Lora | 400 | Warm, traditional reading |
| Labels | DM Sans | 300 | Neutral contrast without coldness |

```css
@import url('https://fonts.googleapis.com/css2?family=Fraunces:ital,wght@0,300;1,300;1,400&family=Caveat:wght@600&family=Lora:wght@400&family=DM+Sans:wght@300&display=swap');
.handwritten-accent { font-family: 'Caveat', cursive; font-weight: 600; font-size: 1.4em; }
```

---

## PAIRING 13: Gamified / Interactive
**Feeling**: Friendly, energetic, approachable, rewarding

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display | Nunito | 800–900 | Rounded, friendly, legible |
| OR Display | Poppins | 700–800 | Clean, geometric, youth |
| Body | Nunito | 400–500 | Same family, warm |
| Scores/Numbers | Nunito | 800 tabular | Badge numbers, progress |

```css
@import url('https://fonts.googleapis.com/css2?family=Nunito:wght@400;500;800;900&display=swap');
.score { font-family: 'Nunito', sans-serif; font-weight: 800; font-variant-numeric: tabular-nums; }
```

---

## PAIRING 14: Minimalist / Zen
**Feeling**: Absolute restraint, confidence through absence

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display | Helvetica Neue | 100 Ultra Light | Thin = expensive = silent confidence |
| OR Display | DM Sans | 200 ExtraLight | Google Fonts accessible equivalent |
| Body | DM Sans | 300 Light | Everything is the same family |
| ALL elements | DM Sans | 200–400 only | No bold, no italic — restraint is the style |

```css
@import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@200;300;400&display=swap');
/* In Zen archetype: the ONLY font property that changes is weight */
h1 { font-weight: 200; font-size: clamp(3rem, 8vw, 7rem); letter-spacing: -0.03em; }
p  { font-weight: 300; font-size: 1rem; line-height: 1.8; }
```

---

## PAIRING 15: Memphis / Pop Art
**Feeling**: Maximum personality, graphic energy, deliberate loudness

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display | Syne | 800 | Wide, geometric, bold |
| OR Display | Bebas Neue (stacked) | 400 | All-caps, graphic |
| Body | Space Grotesk | 400 | Personality without screaming |
| Accent mix | Any + Syne Mono | — | Mix fonts intentionally — it's the style |

```css
@import url('https://fonts.googleapis.com/css2?family=Syne:wght@400;700;800&family=Syne+Mono&display=swap');
/* Memphis rule: font size jumps are MASSIVE — 10px label next to 120px headline */
```

---

## PAIRING 16: Dark Luxury / Atmospheric
**Feeling**: Mystery, luminous elegance, premium darkness

| Role | Font | Weight | Notes |
|---|---|---|---|
| Display | Cormorant Garamond | 300 Light Italic | Luminous against dark — hairline serifs glow |
| OR Display | Cinzel | 400 | Roman gravitas — aristocratic |
| Body | Cormorant Garamond | 400 | Same family, slightly heavier |
| Micro-labels | Inter | 300–400, very muted (40% opacity) | Nearly invisible UI |

```css
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;1,300&family=Cinzel:wght@400&family=Inter:wght@300;400&display=swap');
.glow-title { font-family: 'Cormorant Garamond', serif; font-weight: 300; font-style: italic;
  text-shadow: 0 0 40px rgba(201, 168, 76, 0.5); }
```

---

## PAIRING 17: Motion / Generative Art
**Feeling**: Code proximity, minimal observer, the art speaks

| Role | Font | Weight | Notes |
|---|---|---|---|
| ALL text | DM Mono | 400 | Monospace only — observer in the studio |
| OR ALL text | Space Mono | 400 | Everything is code proximity |
| Labels | DM Mono | 400, 10px, muted | Almost invisible |

```css
@import url('https://fonts.googleapis.com/css2?family=DM+Mono:wght@400&display=swap');
/* In generative art: font is intentionally invisible — art is the visual */
* { font-family: 'DM Mono', monospace; font-size: 11px; color: rgba(26,26,26,0.5); }
```
