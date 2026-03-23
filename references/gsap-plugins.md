# GSAP Premium Plugins Reference
## DrawSVG, MorphSVG, SplitText, MotionPath, ScrollSmoother, Observer

All plugins are now **FREE** (sponsored by Webflow). Register via CDN:
```html
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.7/dist/gsap.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.7/dist/ScrollTrigger.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.7/dist/ScrollSmoother.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.7/dist/SplitText.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.7/dist/DrawSVGPlugin.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.7/dist/MorphSVGPlugin.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.7/dist/MotionPathPlugin.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.7/dist/Observer.min.js"></script>
```

```javascript
gsap.registerPlugin(ScrollTrigger, ScrollSmoother, SplitText, DrawSVGPlugin, MorphSVGPlugin, MotionPathPlugin, Observer);
```

---

## 1. SplitText — Character/Word/Line Animation

```javascript
// Basic split + animate
const split = new SplitText('.hero-title', { type: 'chars, words, lines' });

gsap.from(split.chars, {
  opacity: 0, y: 50, rotateX: -90,
  stagger: 0.02, duration: 0.8,
  ease: 'back.out(1.7)'
});

// Auto-resplit on resize (v3.13+)
const split2 = new SplitText('.paragraph', {
  type: 'lines',
  autoSplit: true,     // re-splits on resize/font-load
  mask: 'lines'        // built-in overflow:hidden mask
});

gsap.from(split2.lines, {
  yPercent: 100,       // reveals from behind mask
  stagger: 0.1,
  scrollTrigger: { trigger: '.paragraph', start: 'top 80%' }
});

// Cleanup
split.revert(); // restores original HTML
```

---

## 2. DrawSVG — Stroke Animation

```javascript
// Draw from 0% to 100%
gsap.from('.draw-path', {
  drawSVG: '0%',         // animate FROM 0% to full
  duration: 2,
  ease: 'power2.inOut',
  scrollTrigger: { trigger: '.draw-path', start: 'top 70%' }
});

// Draw from center outward
gsap.from('.center-draw', { drawSVG: '50% 50%', duration: 1.5 });

// Animated dash moving along path
gsap.fromTo('.dash-path',
  { drawSVG: '0% 10%' },
  { drawSVG: '90% 100%', duration: 2, repeat: -1, ease: 'none' }
);

// Live update (responsive)
gsap.from('.responsive-path', { drawSVG: '0%', duration: 2, live: true });
```

---

## 3. MorphSVG — Shape Morphing

```javascript
// Morph one shape into another
gsap.to('.shape-a', {
  morphSVG: '.shape-b',
  duration: 1.5,
  ease: 'power2.inOut'
});

// Morph circle to star
gsap.to('#circle', { morphSVG: '#star', duration: 1, yoyo: true, repeat: -1 });

// Convert basic shapes to paths first
MorphSVGPlugin.convertToPath('circle, rect, ellipse, polygon');

// Fine-tune morphing
gsap.to('.shape', {
  morphSVG: { shape: '.target', map: 'size', shapeIndex: 'auto' },
  duration: 2
});
```

---

## 4. MotionPath — Animate Along a Path

```javascript
gsap.to('.element', {
  motionPath: {
    path: '#curve-path',
    autoRotate: true,     // element rotates to follow curve
    align: '#curve-path',
    alignOrigin: [0.5, 0.5]
  },
  duration: 3,
  ease: 'power1.inOut'
});

// Array-based path (no SVG needed)
gsap.to('.dot', {
  motionPath: [
    { x: 100, y: 0 },
    { x: 200, y: 100 },
    { x: 300, y: 50 },
    { x: 400, y: 0 }
  ],
  duration: 2
});

// Scroll-driven path animation
gsap.to('.traveler', {
  motionPath: { path: '#scroll-path', autoRotate: true },
  ease: 'none',
  scrollTrigger: { trigger: '.path-section', start: 'top top', end: 'bottom bottom', scrub: 1 }
});
```

---

## 5. ScrollSmoother — Butter-Smooth Scrolling

```html
<div id="smooth-wrapper">
  <div id="smooth-content">
    <!-- ALL page content -->
  </div>
</div>
```

```javascript
const smoother = ScrollSmoother.create({
  wrapper: '#smooth-wrapper',
  content: '#smooth-content',
  smooth: 1.5,          // seconds to catch up
  effects: true,        // enables data-speed and data-lag
  smoothTouch: 0.1      // subtle on mobile
});
```

```html
<!-- Parallax via attributes -->
<img data-speed="0.5" src="bg.jpg" />     <!-- moves at half speed -->
<h2 data-speed="1.2">Title</h2>           <!-- moves faster -->
<div data-lag="0.5">Delayed element</div> <!-- follows with delay -->
```

```javascript
// Scroll to element
smoother.scrollTo('.section-3', true, 'center center');

// Pause/resume
smoother.paused(true);
smoother.paused(false);
```

---

## 6. Observer — Unified Input Handling

```javascript
Observer.create({
  type: 'wheel, touch, pointer',
  onUp: () => goToPrevSection(),
  onDown: () => goToNextSection(),
  tolerance: 10,
  preventDefault: true
});

// Drag-based gallery
Observer.create({
  target: '.gallery',
  type: 'touch, pointer',
  onDrag: self => {
    gsap.to('.gallery-track', { x: `+=${self.deltaX}`, duration: 0.3 });
  }
});
```

---

## COMBINING PLUGINS

```javascript
// SplitText + ScrollTrigger + ScrollSmoother
const split = new SplitText('.reveal-text', { type: 'lines', mask: 'lines' });
gsap.from(split.lines, {
  yPercent: 100, stagger: 0.1,
  scrollTrigger: { trigger: '.reveal-text', start: 'top 85%' }
});

// DrawSVG + ScrollTrigger
gsap.from('.scroll-draw', {
  drawSVG: '0%',
  scrollTrigger: { trigger: '.scroll-draw', start: 'top 70%', end: 'bottom 30%', scrub: 1 }
});

// MotionPath + ScrollTrigger
gsap.to('.scroll-traveler', {
  motionPath: { path: '#journey', autoRotate: true },
  scrollTrigger: { trigger: '.journey-section', scrub: 1 }
});
```
