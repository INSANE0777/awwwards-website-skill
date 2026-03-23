# Modern CSS Features (2024-2025)
## Container queries, :has(), @layer, nesting, oklch(), and more

---

## 1. CONTAINER QUERIES

Components adapt to their container, not the viewport. Game-changer for reusable components.

```css
.card-wrapper { container-type: inline-size; container-name: card; }

.card { display: grid; gap: 1rem; }

@container card (min-width: 400px) {
  .card { grid-template-columns: 200px 1fr; }
}

@container card (min-width: 700px) {
  .card { grid-template-columns: 300px 1fr; }
  .card-title { font-size: 2rem; }
}
```

### Container query units
```css
.card-title {
  font-size: clamp(1rem, 5cqi, 2rem); /* cqi = container query inline */
}
```

---

## 2. :has() SELECTOR (parent selector)

Style parents based on children. No JS needed.

```css
/* Card with image gets different layout */
.card:has(img) { grid-template-rows: 200px 1fr; }
.card:not(:has(img)) { padding: 2rem; }

/* Form group with invalid input */
.form-group:has(input:invalid) { border-color: #D32F2F; }
.form-group:has(input:invalid) .form-label { color: #D32F2F; }

/* Section with video gets dark overlay */
section:has(video) { position: relative; color: white; }
section:has(video)::after {
  content: ''; position: absolute; inset: 0;
  background: rgba(0,0,0,0.4); z-index: 1;
}

/* Nav changes when page has scrolled (via JS class on body) */
body:has(.hero:not(.in-view)) .navbar { background: var(--bg); }

/* Dark mode via checkbox (no JS!) */
body:has(#dark-toggle:checked) { --bg: #0a0a0a; --ink: #f5f5f5; }
```

---

## 3. CASCADE LAYERS (@layer)

Control CSS specificity explicitly. No more `!important` wars.

```css
@layer reset, base, components, utilities;

@layer reset {
  *, *::before, *::after { box-sizing: border-box; margin: 0; }
}

@layer base {
  body { font-family: var(--font-body); color: var(--ink); background: var(--bg); }
  a { color: inherit; text-decoration: none; }
}

@layer components {
  .btn { padding: 1rem 2rem; border: 1px solid var(--ink); }
  .card { border-radius: 12px; overflow: hidden; }
}

@layer utilities {
  .sr-only { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0,0,0,0); }
  .text-center { text-align: center; }
}
```

---

## 4. CSS NESTING (native)

```css
.card {
  padding: 2rem;
  border-radius: 12px;

  & .title {
    font-size: 1.5rem;
    font-weight: 600;
  }

  & .body {
    color: var(--ink-muted);
  }

  &:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 30px var(--shadow);
  }

  @media (width < 768px) {
    padding: 1rem;
  }
}
```

---

## 5. OKLCH COLOR SPACE

Perceptually uniform — better than HSL for consistent palettes.

```css
:root {
  --accent: oklch(65% 0.25 260);       /* vibrant blue */
  --accent-light: oklch(80% 0.15 260); /* lighter, same hue */
  --accent-dark: oklch(45% 0.25 260);  /* darker, same hue */
}

/* Generate palette by adjusting L (lightness) only */
.btn-primary { background: oklch(65% 0.25 260); }
.btn-primary:hover { background: oklch(55% 0.25 260); }
```

### `color-mix()` for automatic shades
```css
:root {
  --accent: oklch(65% 0.25 260);
  --accent-50: color-mix(in oklch, var(--accent) 10%, white);
  --accent-100: color-mix(in oklch, var(--accent) 20%, white);
  --accent-900: color-mix(in oklch, var(--accent) 90%, black);
}
```

### `light-dark()` for automatic theming
```css
:root {
  color-scheme: light dark;
  --bg: light-dark(#fafafa, #0a0a0a);
  --ink: light-dark(#1a1a1a, #f5f5f5);
}
```

---

## 6. SUBGRID

Children align to parent grid tracks.

```css
.grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 2rem; }
.grid-item {
  display: grid;
  grid-template-rows: subgrid;
  grid-row: span 3; /* inherit 3 rows from parent */
}
/* All cards now have aligned titles, bodies, and footers */
```

---

## 7. @scope

Scope styles to a DOM subtree — no more BEM needed.

```css
@scope (.card) to (.card-footer) {
  /* Only applies inside .card but NOT inside .card-footer */
  p { color: var(--ink-muted); }
  a { text-decoration: underline; }
}
```

---

## 8. TRIGONOMETRIC FUNCTIONS

```css
/* Circular layout without JS */
.circle-item {
  --angle: calc(var(--i) * (360deg / var(--total)));
  transform: rotate(var(--angle)) translateX(150px) rotate(calc(-1 * var(--angle)));
}

/* Wavy text */
.wave-char {
  animation-delay: calc(sin(var(--i) * 0.5) * 0.3s);
}
```

---

## BROWSER SUPPORT CHEAT SHEET

| Feature | Chrome | Firefox | Safari |
|---|---|---|---|
| Container Queries | 105+ | 110+ | 16+ |
| `:has()` | 105+ | 121+ | 15.4+ |
| `@layer` | 99+ | 97+ | 15.4+ |
| Nesting | 120+ | 117+ | 17.2+ |
| `oklch()` | 111+ | 113+ | 15.4+ |
| Subgrid | 117+ | 71+ | 16+ |
| `color-mix()` | 111+ | 113+ | 16.2+ |
