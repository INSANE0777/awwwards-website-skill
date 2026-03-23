# SVG Animation Patterns
## Stroke drawing, path morphing, logo reveals, and icon animations

SVG animations are lightweight, scale perfectly, and score high on Awwwards Creativity.
They work without any JS library (CSS-only) or can be enhanced with GSAP.

---

## 1. STROKE DRAWING (line art reveal)

The most iconic SVG animation. Lines appear to draw themselves.

### CSS-Only Version
```css
.draw-path {
  stroke-dasharray: 1000;    /* total path length (approximate) */
  stroke-dashoffset: 1000;   /* start hidden */
  animation: drawLine 2s ease-out forwards;
}

@keyframes drawLine {
  to { stroke-dashoffset: 0; }
}
```

### Dynamic Version (auto-detects path length)
```javascript
function initStrokeDraw() {
  document.querySelectorAll('[data-draw]').forEach(path => {
    const length = path.getTotalLength();
    path.style.strokeDasharray = length;
    path.style.strokeDashoffset = length;

    gsap.to(path, {
      strokeDashoffset: 0,
      duration: parseFloat(path.dataset.draw) || 2,
      ease: 'power2.inOut',
      scrollTrigger: {
        trigger: path.closest('svg'),
        start: 'top 80%'
      }
    });
  });
}
```

```html
<!-- Usage: any SVG path -->
<svg viewBox="0 0 200 100">
  <path data-draw="2.5" d="M10,50 Q50,10 100,50 T190,50"
        fill="none" stroke="var(--accent)" stroke-width="2" />
</svg>
```

### Logo Reveal (stagger multiple paths)
```javascript
function revealLogo(svgSelector) {
  const paths = document.querySelectorAll(`${svgSelector} path`);

  paths.forEach(path => {
    const len = path.getTotalLength();
    gsap.set(path, { strokeDasharray: len, strokeDashoffset: len });
  });

  gsap.to(paths, {
    strokeDashoffset: 0,
    duration: 1.8,
    stagger: 0.15,
    ease: 'power3.inOut',
    delay: 0.5
  });
}
```

---

## 2. PATH MORPHING (shape transitions)

### CSS Clip-Path Morph
```css
.morph-shape {
  clip-path: circle(30% at 50% 50%);
  transition: clip-path 0.8s cubic-bezier(0.77, 0, 0.175, 1);
}
.morph-shape:hover {
  clip-path: circle(50% at 50% 50%);
}

/* Blob morph */
.blob {
  clip-path: polygon(30% 0%, 70% 0%, 100% 30%, 100% 70%, 70% 100%, 30% 100%, 0% 70%, 0% 30%);
  transition: clip-path 1s cubic-bezier(0.77, 0, 0.175, 1);
}
.blob:hover {
  clip-path: polygon(20% 5%, 80% 5%, 95% 20%, 95% 80%, 80% 95%, 20% 95%, 5% 80%, 5% 20%);
}
```

### GSAP MorphSVG (requires GSAP Club plugin)
```javascript
// If using GSAP Club GreenSock
gsap.to('#shape1', {
  morphSVG: '#shape2',
  duration: 1.2,
  ease: 'power2.inOut'
});
```

### Pure CSS Animated Blob (no JS)
```css
.animated-blob {
  border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%;
  animation: blobMorph 8s ease-in-out infinite;
}

@keyframes blobMorph {
  0%   { border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%; }
  25%  { border-radius: 30% 60% 70% 40% / 50% 60% 30% 60%; }
  50%  { border-radius: 50% 60% 30% 60% / 40% 70% 60% 30%; }
  75%  { border-radius: 40% 30% 60% 50% / 70% 40% 50% 60%; }
  100% { border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%; }
}
```

---

## 3. ICON ANIMATIONS

### Menu → X Toggle
```html
<button class="menu-toggle" aria-label="Menu">
  <svg viewBox="0 0 24 24" width="24" height="24">
    <line class="line-top" x1="4" y1="7" x2="20" y2="7" stroke="currentColor" stroke-width="2" />
    <line class="line-mid" x1="4" y1="12" x2="20" y2="12" stroke="currentColor" stroke-width="2" />
    <line class="line-bot" x1="4" y1="17" x2="20" y2="17" stroke="currentColor" stroke-width="2" />
  </svg>
</button>
```

```css
.menu-toggle svg line {
  transition: transform 0.3s ease, opacity 0.3s ease;
  transform-origin: center;
}
.menu-toggle.open .line-top { transform: rotate(45deg) translate(0, 5px); }
.menu-toggle.open .line-mid { opacity: 0; }
.menu-toggle.open .line-bot { transform: rotate(-45deg) translate(0, -5px); }
```

### Arrow Slide
```css
.arrow-link svg {
  transition: transform 0.4s cubic-bezier(0.2, 0.8, 0.2, 1);
}
.arrow-link:hover svg {
  transform: translateX(8px);
}
```

### Checkmark Draw
```css
.checkmark-path {
  stroke-dasharray: 50;
  stroke-dashoffset: 50;
  transition: stroke-dashoffset 0.4s ease-out;
}
.checked .checkmark-path {
  stroke-dashoffset: 0;
}
```

---

## 4. TEXT PATH ANIMATION (text along a curve)

```html
<svg viewBox="0 0 500 200" class="text-path-svg">
  <defs>
    <path id="curve" d="M20,150 Q250,20 480,150" fill="none" />
  </defs>
  <text>
    <textPath href="#curve" class="curved-text">
      YOUR TEXT FOLLOWS THE CURVE
    </textPath>
  </text>
</svg>
```

```css
.curved-text {
  font-family: 'Bebas Neue', sans-serif;
  font-size: 28px;
  fill: var(--ink);
  letter-spacing: 0.15em;
}

/* Animate text position along path */
.text-path-svg textPath {
  animation: textSlide 15s linear infinite;
}

@keyframes textSlide {
  from { startOffset: 0%; }
  to   { startOffset: 100%; }
}
```

### Scroll-Driven Text Path
```javascript
gsap.to('.curved-text', {
  attr: { startOffset: '100%' },
  ease: 'none',
  scrollTrigger: {
    trigger: '.text-path-svg',
    start: 'top 80%',
    end: 'bottom 20%',
    scrub: 1
  }
});
```

---

## 5. ANIMATED MARQUEE (infinite scroll text)

```html
<div class="marquee" aria-hidden="true">
  <div class="marquee-track">
    <span>CREATIVE DEVELOPER — </span>
    <span>CREATIVE DEVELOPER — </span>
    <span>CREATIVE DEVELOPER — </span>
    <span>CREATIVE DEVELOPER — </span>
  </div>
</div>
```

```css
.marquee {
  overflow: hidden;
  white-space: nowrap;
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(3rem, 6vw, 8rem);
  color: var(--ink-muted);
  opacity: 0.15;
}

.marquee-track {
  display: inline-flex;
  animation: marqueeScroll 20s linear infinite;
}

.marquee-track span {
  flex-shrink: 0;
  padding-right: 1rem;
}

@keyframes marqueeScroll {
  from { transform: translateX(0); }
  to   { transform: translateX(-50%); }
}

/* Reverse direction on hover for playfulness */
.marquee:hover .marquee-track {
  animation-direction: reverse;
  animation-duration: 10s;
}
```

### GSAP Version (speed control + scroll influence)
```javascript
function initMarquee() {
  const tracks = document.querySelectorAll('.marquee-track');
  tracks.forEach(track => {
    const width = track.scrollWidth / 2;
    gsap.to(track, {
      x: -width,
      duration: 20,
      ease: 'none',
      repeat: -1,
      modifiers: {
        x: gsap.utils.unitize(x => parseFloat(x) % width)
      }
    });
  });
}
```

---

## 6. SVG FILTER EFFECTS

### Gooey Blobs (merge circles visually)
```html
<svg class="hidden-svg">
  <defs>
    <filter id="goo">
      <feGaussianBlur in="SourceGraphic" stdDeviation="10" result="blur" />
      <feColorMatrix in="blur" mode="matrix"
        values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 18 -7" result="goo" />
    </filter>
  </defs>
</svg>
```

```css
.goo-container { filter: url(#goo); }
.goo-blob {
  width: 60px; height: 60px;
  border-radius: 50%;
  background: var(--accent);
  position: absolute;
}
```

### Grainy Film Texture (SVG filter)
```html
<svg class="hidden-svg">
  <defs>
    <filter id="grain">
      <feTurbulence type="fractalNoise" baseFrequency="0.65" numOctaves="3" stitchTiles="stitch" />
      <feColorMatrix type="saturate" values="0" />
      <feBlend in="SourceGraphic" mode="multiply" />
    </filter>
  </defs>
</svg>
```

```css
.grain-overlay {
  position: fixed; inset: 0;
  filter: url(#grain);
  opacity: 0.04;
  pointer-events: none;
  z-index: 9998;
}
```

---

## BEST PRACTICES

1. **Always inline critical SVGs** — don't link external `.svg` files for animated elements
2. **Use `viewBox`** — never fixed `width`/`height` on animated SVGs (they need to scale)
3. **`getTotalLength()`** for stroke animations — don't guess dasharray values
4. **Reduced motion**: disable SVG animations too:
   ```css
   @media (prefers-reduced-motion: reduce) {
     .draw-path, .marquee-track, .animated-blob { animation: none !important; }
   }
   ```
5. **Decorative SVGs**: add `aria-hidden="true"` and `role="presentation"`
6. **Performance**: limit animated SVGs to 3-4 per page. Complex filters (goo, grain) are GPU-heavy
7. **Color**: use `currentColor` or CSS variables for stroke/fill — adapts to dark mode automatically
