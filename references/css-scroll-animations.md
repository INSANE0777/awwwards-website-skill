# CSS Native Scroll-Driven Animations
## Zero-JS scroll animations at 60fps using `animation-timeline`

New CSS spec — no ScrollTrigger needed for many effects. 60fps, compositor-thread, butter-smooth.

> **Browser support:** Chrome 115+, Edge 115+, Firefox 110+ (flag). Safari: not yet. Use as progressive enhancement with GSAP fallback.

---

## CORE CONCEPT

```css
/* Link any CSS animation to scroll position */
.element {
  animation: fadeSlideIn linear both;
  animation-timeline: view();          /* triggers when element enters viewport */
  animation-range: entry 0% cover 40%; /* start → end of animation */
}

@keyframes fadeSlideIn {
  from { opacity: 0; transform: translateY(60px); }
  to   { opacity: 1; transform: translateY(0); }
}
```

---

## TIMELINE TYPES

### `scroll()` — tracks scroll container progress
```css
.progress-bar {
  animation: grow linear both;
  animation-timeline: scroll(root);  /* root = whole page */
}
@keyframes grow { from { scaleX: 0; } to { scaleX: 1; } }
```

### `view()` — tracks element visibility in viewport
```css
.reveal {
  animation: reveal linear both;
  animation-timeline: view();
  animation-range: entry 0% cover 30%;
}
```

---

## PATTERN 1: Fade-in on scroll (replaces ScrollTrigger)

```css
[data-reveal] {
  opacity: 0; transform: translateY(40px);
  animation: cssReveal 1s linear both;
  animation-timeline: view();
  animation-range: entry 10% cover 30%;
}
@keyframes cssReveal {
  to { opacity: 1; transform: translateY(0); }
}
```

---

## PATTERN 2: Parallax (pure CSS)

```css
.parallax-bg {
  animation: parallax linear both;
  animation-timeline: scroll();
}
@keyframes parallax {
  from { transform: translateY(-20%); }
  to   { transform: translateY(20%); }
}
```

---

## PATTERN 3: Horizontal progress bar

```css
.reading-progress {
  position: fixed; top: 0; left: 0; right: 0;
  height: 3px; background: var(--accent);
  transform-origin: left;
  animation: progressGrow linear both;
  animation-timeline: scroll(root);
}
@keyframes progressGrow { from { scaleX: 0; } to { scaleX: 1; } }
```

---

## PATTERN 4: Image scale on scroll

```css
.scale-image {
  animation: zoomIn linear both;
  animation-timeline: view();
  animation-range: entry 0% cover 50%;
}
@keyframes zoomIn {
  from { transform: scale(0.8); opacity: 0.6; }
  to   { transform: scale(1); opacity: 1; }
}
```

---

## PATTERN 5: Stagger without JS

```css
.card:nth-child(1) { animation-delay: 0ms; }
.card:nth-child(2) { animation-delay: 100ms; }
.card:nth-child(3) { animation-delay: 200ms; }
.card:nth-child(4) { animation-delay: 300ms; }
/* Each card uses the same view() timeline but starts later */
```

---

## PATTERN 6: Clip-path reveal

```css
.clip-reveal {
  animation: clipReveal linear both;
  animation-timeline: view();
  animation-range: entry 0% cover 50%;
}
@keyframes clipReveal {
  from { clip-path: inset(100% 0 0 0); }
  to   { clip-path: inset(0 0 0 0); }
}
```

---

## FALLBACK STRATEGY

```css
/* Use @supports to provide GSAP fallback */
@supports not (animation-timeline: view()) {
  [data-reveal] {
    /* GSAP will handle these via JS */
    opacity: 0;
    transform: translateY(40px);
  }
}
```

```javascript
// JS fallback only when CSS doesn't support it
if (!CSS.supports('animation-timeline', 'view()')) {
  gsap.utils.toArray('[data-reveal]').forEach(el => {
    gsap.from(el, {
      opacity: 0, y: 40,
      scrollTrigger: { trigger: el, start: 'top 85%' }
    });
  });
}
```

---

## WHEN TO USE CSS vs GSAP

| Use CSS `animation-timeline` | Use GSAP ScrollTrigger |
|---|---|
| Simple fade/slide reveals | Complex multi-step timelines |
| Parallax backgrounds | Pin + scrub effects |
| Progress bars | Horizontal scroll sections |
| Image scale on enter | Callback functions needed |
| When Safari support not needed | Cross-browser required now |
