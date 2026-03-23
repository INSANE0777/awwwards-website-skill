# Responsive & Mobile-First Design
## Breakpoints, fluid typography, touch handling, and iOS gotchas

Awwwards judges mobile as part of Usability (30%). A site that breaks on mobile cannot win.

---

## BREAKPOINT SYSTEM

```css
/* Mobile-first approach — design for small screens, enhance upward */

/* Base: 0–479px (small phones) */
:root {
  --text-hero:    clamp(2.5rem, 12vw, 4rem);
  --text-display: clamp(1.5rem, 8vw, 2.5rem);
  --text-body:    clamp(1rem, 4vw, 1.125rem);
  --space-section: clamp(4rem, 10vw, 8rem);
  --space-gutter:  clamp(1rem, 4vw, 2rem);
}

/* Tablet: 768px+ */
@media (min-width: 768px) {
  :root {
    --text-hero:    clamp(4rem, 8vw, 8rem);
    --text-display: clamp(2rem, 4vw, 3.5rem);
    --space-gutter:  2rem;
  }
}

/* Desktop: 1024px+ */
@media (min-width: 1024px) {
  :root {
    --text-hero:    clamp(5rem, 7vw, 12rem);
    --text-display: clamp(2.5rem, 3.5vw, 5rem);
    --space-gutter:  3rem;
  }
}

/* Large desktop: 1440px+ */
@media (min-width: 1440px) {
  .container { max-width: 1400px; margin: 0 auto; }
}
```

---

## FLUID TYPOGRAPHY FORMULAS

```css
/* The clamp() function: clamp(minimum, preferred, maximum) */

/* Body text: never below 16px (prevents iOS zoom), scales with viewport */
body { font-size: clamp(1rem, 2.5vw, 1.25rem); }

/* Hero: dramatic on desktop, readable on mobile */
.hero-title { font-size: clamp(3rem, 10vw + 1rem, 12rem); }

/* Section headers */
.section-title { font-size: clamp(2rem, 5vw, 5rem); }

/* Captions / labels */
.mono { font-size: clamp(0.625rem, 1.5vw, 0.75rem); }
```

**Key rule**: Body text ≥16px always. Below 16px, iOS Safari auto-zooms on input focus.

---

## NAVIGATION — MOBILE PATTERNS

### Hamburger (most common)
```css
/* Desktop: horizontal nav */
.nav-links {
  display: flex;
  gap: 2rem;
}

/* Mobile: hide links, show toggle */
@media (max-width: 768px) {
  .nav-links { display: none; }
  .nav-toggle { display: flex; }

  .nav-links.open {
    display: flex;
    flex-direction: column;
    position: fixed;
    inset: 0;
    background: var(--bg);
    z-index: 999;
    align-items: center;
    justify-content: center;
    gap: 2rem;
  }
}
```

### Bottom Tab Bar (app-like)
```css
@media (max-width: 768px) {
  .nav {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: space-around;
    padding: 0.5rem 0;
    background: var(--bg);
    border-top: 1px solid var(--border);
    z-index: 100;
    /* Safe area for notched phones */
    padding-bottom: env(safe-area-inset-bottom, 0.5rem);
  }
}
```

---

## LAYOUT CONVERSION PATTERNS

### Grid → Stack
```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2rem;
}

@media (max-width: 768px) {
  .grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }
}
```

### Horizontal Scroll → Vertical Stack
```css
/* Desktop: horizontal scroll section */
@media (min-width: 769px) {
  .h-scroll-track {
    display: flex;
    overflow: hidden;
  }
  .h-panel { min-width: 80vw; flex-shrink: 0; }
}

/* Mobile: stack vertically */
@media (max-width: 768px) {
  .h-scroll-track {
    display: flex;
    flex-direction: column;
  }
  .h-panel {
    width: 100%;
    min-width: unset;
    margin-bottom: 2rem;
  }
}
```

### Panels → Cards
```css
/* Desktop: side-by-side panels */
.panels-container {
  display: flex;
  gap: 14px;
}
.hero-panel { flex: 1; height: 72vh; }

/* Mobile: full-width cards */
@media (max-width: 768px) {
  .panels-container {
    flex-direction: column;
    height: auto;
  }
  .hero-panel {
    width: 100%;
    height: 35vh;
    flex: none !important;
  }
  /* Disable hover-expand on touch */
  .panels-container .hero-panel:hover { flex: none; }
}
```

---

## iOS-SPECIFIC GOTCHAS

### 1. 100vh Bug
```css
/* iOS Safari: 100vh includes the address bar. Use dvh instead */
.hero {
  height: 100vh; /* fallback */
  height: 100dvh; /* dynamic viewport height — respects address bar */
}

/* Or use CSS custom property set by JS */
:root { --vh: 1vh; }
.hero { height: calc(var(--vh, 1vh) * 100); }
```

```javascript
// Set --vh custom property (fallback for older browsers)
function setVH() {
  document.documentElement.style.setProperty('--vh', `${window.innerHeight * 0.01}px`);
}
setVH();
window.addEventListener('resize', setVH);
```

### 2. Safe Area Insets (notched phones)
```css
/* Padding for notched devices (iPhone X+) */
body {
  padding-top: env(safe-area-inset-top);
  padding-bottom: env(safe-area-inset-bottom);
  padding-left: env(safe-area-inset-left);
  padding-right: env(safe-area-inset-right);
}

/* Or on specific elements */
.nav { padding-top: max(1rem, env(safe-area-inset-top)); }
.footer { padding-bottom: max(1rem, env(safe-area-inset-bottom)); }
```

### 3. Overscroll Bounce
```css
/* Prevent rubber-band scroll on iOS */
html {
  overscroll-behavior: none;
}
```

### 4. Input Zoom Prevention
```css
/* iOS zooms on inputs with font < 16px */
input, select, textarea {
  font-size: 16px !important; /* or clamp(16px, ...) */
}
```

---

## TOUCH TARGET SIZING

```css
/* Minimum 44×44px for all interactive elements (WCAG 2.5.5) */
button, a, .nav-link, .social-icon {
  min-width: 44px;
  min-height: 44px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

/* Increase spacing between small targets */
.social-icons { gap: 0.5rem; }
```

---

## CURSOR MANAGEMENT

```css
/* Hide custom cursor on touch devices — ALWAYS */
@media (hover: none) {
  .c-dot, .c-ring, .cursor-dot, .cursor-ring { display: none !important; }
  body { cursor: auto !important; }
}
```

```javascript
// Skip cursor initialization on touch
function initCursor() {
  if (window.matchMedia('(hover: none)').matches) return;
  // ... cursor code
}
```

---

## LENIS ON MOBILE

```javascript
const lenis = new Lenis({
  smoothWheel: true,
  smoothTouch: false,   // NEVER smooth touch — causes lag on iOS
  touchMultiplier: 2,   // tweak if scroll feels too slow on touch
});
```

---

## RESPONSIVE TESTING CHECKLIST

- [ ] **320px** — Small phones (iPhone SE). Nothing overflows.
- [ ] **375px** — Standard phones (iPhone 12/13). Primary test target.
- [ ] **428px** — Large phones (iPhone 14 Pro Max).
- [ ] **768px** — Tablets. Grid collapses correctly.
- [ ] **1024px** — Small laptops. Desktop layout kicks in.
- [ ] **1440px** — Standard desktop. Max-width container active.
- [ ] **1920px** — Large screens. Nothing stretches absurdly.
- [ ] **Landscape mobile** — Hero still readable, nothing cut off.
- [ ] **No horizontal scroll** on ANY viewport width.
- [ ] **Touch targets** ≥ 44×44px.
- [ ] Custom cursor hidden on touch devices.
- [ ] Hover-only interactions have touch fallbacks.
- [ ] 3D/WebGL has graceful fallback.
