# Page Transitions & Section Effects
## Pixel, stairs, mask, parallel, SVG morph, perspective, and more

---

## 1. PIXEL TRANSITION

```javascript
function pixelTransition(onMidpoint) {
  const overlay = document.createElement('canvas');
  overlay.style.cssText = 'position:fixed;inset:0;z-index:10000;pointer-events:none;';
  document.body.appendChild(overlay);
  const ctx = overlay.getContext('2d');
  const w = overlay.width = window.innerWidth;
  const h = overlay.height = window.innerHeight;

  const pixelSize = 20;
  const cols = Math.ceil(w / pixelSize);
  const rows = Math.ceil(h / pixelSize);
  const total = cols * rows;
  const cells = Array.from({ length: total }, (_, i) => ({
    x: (i % cols) * pixelSize,
    y: Math.floor(i / cols) * pixelSize,
    delay: Math.random()
  }));
  cells.sort((a, b) => a.delay - b.delay);

  // Fill in
  let filled = 0;
  const fillInterval = setInterval(() => {
    const batch = Math.ceil(total / 30);
    for (let i = 0; i < batch && filled < total; i++, filled++) {
      ctx.fillStyle = getComputedStyle(document.body).getPropertyValue('--ink') || '#1a1a1a';
      ctx.fillRect(cells[filled].x, cells[filled].y, pixelSize, pixelSize);
    }
    if (filled >= total) {
      clearInterval(fillInterval);
      onMidpoint?.();

      // Clear out
      let cleared = 0;
      const clearInterval2 = setInterval(() => {
        for (let i = 0; i < batch && cleared < total; i++, cleared++) {
          ctx.clearRect(cells[cleared].x, cells[cleared].y, pixelSize, pixelSize);
        }
        if (cleared >= total) {
          clearInterval(clearInterval2);
          overlay.remove();
        }
      }, 16);
    }
  }, 16);
}
```

---

## 2. STAIRS TRANSITION

```html
<div class="stairs-overlay" id="stairsOverlay">
  <div class="stair"></div><div class="stair"></div><div class="stair"></div>
  <div class="stair"></div><div class="stair"></div><div class="stair"></div>
  <div class="stair"></div><div class="stair"></div><div class="stair"></div>
  <div class="stair"></div>
</div>
```

```css
.stairs-overlay {
  position: fixed; inset: 0; z-index: 10000;
  display: flex; pointer-events: none;
}
.stair {
  flex: 1; height: 100%;
  background: var(--ink);
  transform: scaleY(0);
  transform-origin: bottom;
}
```

```javascript
function stairsTransition(onMidpoint) {
  const stairs = document.querySelectorAll('.stair');
  const tl = gsap.timeline();

  // Stairs fill from bottom
  tl.to(stairs, {
    scaleY: 1,
    duration: 0.4,
    stagger: 0.05,
    ease: 'power4.inOut'
  })
  .call(() => onMidpoint?.())
  // Stairs clear from top
  .set(stairs, { transformOrigin: 'top' })
  .to(stairs, {
    scaleY: 0,
    duration: 0.4,
    stagger: 0.05,
    ease: 'power4.inOut'
  })
  .set(stairs, { transformOrigin: 'bottom' });
}
```

---

## 3. MASK SECTION TRANSITION (expanding shape)

```javascript
function maskSectionTransition(fromSection, toSection) {
  const tl = gsap.timeline();

  tl.fromTo(toSection,
    { clipPath: 'circle(0% at 50% 100%)' },
    {
      clipPath: 'circle(150% at 50% 100%)',
      duration: 1.2,
      ease: 'power4.inOut'
    }
  )
  .set(fromSection, { display: 'none' }, 0.6);
}

// Variant: polygon wipe
function polygonWipe(from, to) {
  gsap.fromTo(to,
    { clipPath: 'polygon(0 100%, 100% 100%, 100% 100%, 0 100%)' },
    {
      clipPath: 'polygon(0 0%, 100% 0%, 100% 100%, 0 100%)',
      duration: 0.8,
      ease: 'power3.inOut',
      onStart: () => { to.style.display = 'block'; },
      onComplete: () => { from.style.display = 'none'; }
    }
  );
}
```

---

## 4. PARALLEL PAGE TRANSITION (dual sliding panels)

```html
<div class="parallel-overlay">
  <div class="parallel-left"></div>
  <div class="parallel-right"></div>
</div>
```

```css
.parallel-overlay {
  position: fixed; inset: 0; z-index: 10000;
  display: flex; pointer-events: none;
}
.parallel-left, .parallel-right {
  width: 50%; height: 100%;
  background: var(--ink);
}
.parallel-left { transform: translateY(100%); }
.parallel-right { transform: translateY(-100%); }
```

```javascript
function parallelTransition(onMidpoint) {
  const tl = gsap.timeline();

  tl.to('.parallel-left', { y: 0, duration: 0.6, ease: 'power4.inOut' })
    .to('.parallel-right', { y: 0, duration: 0.6, ease: 'power4.inOut' }, '<0.1')
    .call(() => onMidpoint?.())
    .to('.parallel-left', { y: '-100%', duration: 0.6, ease: 'power4.inOut' }, '+=0.2')
    .to('.parallel-right', { y: '100%', duration: 0.6, ease: 'power4.inOut' }, '<0.1')
    .set(['.parallel-left', '.parallel-right'], { y: i => i === 0 ? '100%' : '-100%' });
}
```

---

## 5. SVG MORPH TRANSITION

```javascript
function svgMorphTransition() {
  const overlay = document.querySelector('.morph-overlay');
  const path = overlay.querySelector('path');

  const tl = gsap.timeline();
  tl.to(path, {
    attr: { d: 'M 0 0 Q 50 0 100 0 L 100 100 Q 50 100 0 100 Z' }, // flat top
    duration: 0.4, ease: 'power3.in'
  })
  .to(path, {
    attr: { d: 'M 0 0 Q 50 50 100 0 L 100 100 Q 50 100 0 100 Z' }, // bulge
    duration: 0.3, ease: 'power3.out'
  })
  .to(path, {
    attr: { d: 'M 0 0 Q 50 0 100 0 L 100 100 Q 50 100 0 100 Z' }, // flat again
    duration: 0.4, ease: 'power3.inOut'
  });
}
```

---

## 6. SVG CURVE LOADING

```html
<svg viewBox="0 0 100 100" class="svg-curve-loader">
  <path d="M0,50 Q25,50 50,50 T100,50" stroke="var(--accent)" fill="none"
        stroke-width="2" class="curve-path" />
</svg>
```

```javascript
function initCurveLoader() {
  gsap.to('.curve-path', {
    attr: { d: 'M0,50 Q25,20 50,50 T100,50' },
    duration: 0.6,
    yoyo: true,
    repeat: -1,
    ease: 'sine.inOut'
  });
}
```

---

## 7. PERSPECTIVE SECTION TRANSITION (sections slide behind)

```javascript
function initPerspectiveSections() {
  const sections = gsap.utils.toArray('.persp-transition');

  sections.forEach((section, i) => {
    gsap.set(section, { transformOrigin: 'top center', transformPerspective: 1200 });

    ScrollTrigger.create({
      trigger: section,
      start: 'top top',
      end: 'bottom top',
      pin: true,
      pinSpacing: false,
      scrub: true,
      onUpdate: self => {
        gsap.set(section, {
          scale: 1 - self.progress * 0.1,
          rotateX: self.progress * 3,
          y: -self.progress * 50,
          filter: `brightness(${1 - self.progress * 0.3})`
        });
      }
    });
  });
}
```

---

## 8. GOOEY SECTION TRANSITION (blob merge)

```html
<svg style="position:absolute;width:0;height:0">
  <filter id="goo-transition">
    <feGaussianBlur in="SourceGraphic" stdDeviation="12" />
    <feColorMatrix values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 22 -8" />
  </filter>
</svg>
```

```javascript
function gooeyTransition(onMidpoint) {
  const blob = document.createElement('div');
  blob.style.cssText = `
    position: fixed; z-index: 10000;
    width: 50px; height: 50px;
    background: var(--accent);
    border-radius: 50%;
    top: 50%; left: 50%;
    transform: translate(-50%, -50%) scale(0);
    filter: url(#goo-transition);
  `;
  document.body.appendChild(blob);

  gsap.to(blob, {
    scale: 60,
    duration: 0.8,
    ease: 'power4.in',
    onComplete: () => {
      onMidpoint?.();
      gsap.to(blob, {
        scale: 0, duration: 0.6, ease: 'power4.out',
        onComplete: () => blob.remove()
      });
    }
  });
}
```

---

## CHOOSING TRANSITIONS

| Site type | Best transition |
|---|---|
| Editorial / Portfolio | Mask (circle expand) or Crossfade |
| Kinetic / Agency | Stairs or Parallel |
| Cinematic / 3D | Perspective or Gooey |
| Brutalist | Pixel |
| Luxury | SVG Morph (slow, elegant) |
| SaaS / Product | Crossfade (simple, fast) |
