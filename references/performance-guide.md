# Performance Guide for Developer Award (2025-2026)
## Lighthouse Optimization & 2026 Mandates

Awwwards 2026 officially integrates **Lighthouse Core Web Vitals** into the core judging score. Technical excellence is no longer optional for SOTD/SOTY consideration.

### Mandatory 2026 Targets:
- **LCP (Largest Contentful Paint):** < 1.0s (even with heavy 3D).
- **CLS (Cumulative Layout Shift):** 0.0 (any shift = point loss).
- **Overall Performance Score:** 90+ (Required for Developer Award).

The Developer Award requires high scores in performance, SEO, accessibility, and responsiveness.
Build for it from the start — it cannot be bolted on afterward.

---

## GPU-SAFE ANIMATION (most important rule)

Only animate properties that don't trigger layout reflow:

```
✅ SAFE (GPU-composited):
   opacity
   transform: translate(), scale(), rotate(), skew()
   filter: blur(), brightness(), saturate(), grayscale()

❌ NEVER ANIMATE (triggers layout/paint):
   width, height
   top, left, right, bottom
   margin, padding
   border-width
   font-size
   background-position (use transform instead)
```

```css
/* WRONG: */
.panel { transition: width 0.5s; }
.panel:hover { width: 300px; }

/* RIGHT: */
.panel { transition: flex 0.5s cubic-bezier(0.2, 0.8, 0.2, 1); }
.panel:hover { flex: 3.5; }
/* or use transform: scaleX() for true GPU compositing */
```

---

## WILL-CHANGE (use sparingly)

```css
/* Add BEFORE animation starts, remove AFTER it ends */
/* Only on elements that will definitely animate */
.hero-panel { will-change: transform, opacity; }
.animated-title { will-change: transform; }

/* NEVER on everything — it consumes GPU memory */
/* NEVER: * { will-change: transform; } */
```

---

## IMAGE OPTIMIZATION

```html
<!-- Lazy load below-fold images -->
<img src="hero.webp" alt="..." loading="eager" />      <!-- above fold -->
<img src="section.webp" alt="..." loading="lazy" />    <!-- below fold -->

<!-- Responsive images -->
<img
  src="image-800.webp"
  srcset="image-400.webp 400w, image-800.webp 800w, image-1200.webp 1200w"
  sizes="(max-width: 768px) 100vw, 50vw"
  alt="..."
  loading="lazy"
/>
```

---

## VIDEO OPTIMIZATION

```html
<!-- Always all 4 attributes -->
<video autoplay muted loop playsinline>
  <source src="hero.mp4" type="video/mp4" />
</video>

<!-- Don't preload off-screen videos -->
<video muted loop playsinline preload="none" data-autoplay>
  <source src="section.mp4" type="video/mp4" />
</video>
```

```javascript
// Intersection Observer for video autoplay
const videoObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    const video = entry.target;
    if (entry.isIntersecting) {
      video.play().catch(() => {}); // catch autoplay policy errors
    } else {
      video.pause();
    }
  });
}, { threshold: 0.25 });

document.querySelectorAll('[data-autoplay]').forEach(v => videoObserver.observe(v));
```

---

## FONT OPTIMIZATION

```html
<!-- Always preconnect to font origin -->
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />

<!-- Use display=swap to prevent invisible text during load -->
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@300;400&display=swap" rel="stylesheet" />

<!-- Preload critical fonts -->
<link rel="preload" href="/fonts/display.woff2" as="font" type="font/woff2" crossorigin />
```

---

## ACCESSIBILITY REQUIREMENTS

```html
<!-- 1. Skip link (REQUIRED for keyboard navigation) -->
<style>
.skip-link {
  position: absolute; top: -100%; left: 0;
  padding: 8px 16px; background: #1A1A1A; color: #F2EFE9;
  font-size: 14px; z-index: 10000;
  transition: top 0.2s;
}
.skip-link:focus { top: 0; }
</style>
<a href="#main-content" class="skip-link">Skip to main content</a>

<!-- 2. Semantic landmarks -->
<header role="banner">...</header>
<nav role="navigation" aria-label="Main navigation">...</nav>
<main id="main-content" role="main">...</main>
<footer role="contentinfo">...</footer>

<!-- 3. Interactive elements need labels -->
<button aria-label="Open menu">
  <span class="icon-menu" aria-hidden="true"></span>
</button>

<!-- 4. Images need alt text -->
<img src="hero.webp" alt="Alan Menken conducting at Carnegie Hall, 2023" />
<img src="decorative.webp" alt="" role="presentation" /> <!-- decorative -->

<!-- 5. Videos need captions for dialogue -->
<video>
  <source src="film.mp4" />
  <track kind="captions" src="film-captions.vtt" srclang="en" label="English" />
</video>
```

---

## REDUCED MOTION (always include)

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

```javascript
// Check in JS too (for GSAP animations)
const prefersReduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
if (prefersReduced) {
  gsap.globalTimeline.timeScale(100); // run all at 100x speed (instant)
  // OR: skip animation entirely, just show final state
}
```

---

## FOCUS STYLES (don't remove, enhance)

```css
/* Remove default ugly outline but replace with beautiful one */
:focus { outline: none; }
:focus-visible {
  outline: 2px solid #8B2500; /* brand color */
  outline-offset: 4px;
  border-radius: 2px;
}

/* For dark backgrounds */
.dark-section :focus-visible {
  outline-color: #F2EFE9;
}
```

---

## CORE WEB VITALS TARGETS

| Metric | Target | How to achieve |
|---|---|---|
| LCP (Largest Contentful Paint) | < 2.5s | Preload hero image/video, optimize size |
| FID / INP (Interactivity) | < 100ms | Avoid long JS tasks, defer non-critical |
| CLS (Layout Shift) | < 0.1 | Set explicit width/height on images/videos |
| FCP (First Contentful Paint) | < 1.8s | Minimal render-blocking resources |

```html
<!-- Prevent CLS: always set dimensions -->
<img src="hero.webp" width="1200" height="800" alt="..." />
<video width="1920" height="1080">...</video>

<!-- Preload hero image -->
<link rel="preload" href="hero.webp" as="image" />
```

---

## QUICK LIGHTHOUSE CHECKLIST

### Performance
- [ ] Hero image preloaded or inline
- [ ] Below-fold images lazy-loaded
- [ ] Fonts use `display=swap`
- [ ] Videos don't autoload off-screen
- [ ] No render-blocking CSS in `<body>`
- [ ] JS deferred or in `<body>` footer

### Accessibility (WCAG 2.1 AA)
- [ ] Skip link present
- [ ] All images have alt text
- [ ] Color contrast ≥ 4.5:1 for body text
- [ ] Interactive elements have labels
- [ ] Keyboard navigation works throughout
- [ ] Focus indicators visible
- [ ] `prefers-reduced-motion` respected
- [ ] Semantic HTML landmarks

### SEO
- [ ] `<title>` tag present and descriptive
- [ ] Meta description present
- [ ] Heading hierarchy logical (one `<h1>`)
- [ ] `lang` attribute on `<html>`
- [ ] Canonical URL set
- [ ] No broken links

### Mobile
- [ ] `<meta name="viewport" content="width=device-width, initial-scale=1">` present
- [ ] No horizontal scroll on any viewport width
- [ ] Touch targets ≥ 44×44px
- [ ] Text ≥ 16px for body (prevents iOS zoom)
- [ ] 3D/WebGL has fallback
