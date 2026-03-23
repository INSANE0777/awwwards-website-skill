# Cursor System & Mouse-Reactive Design
## Complete implementation for Awwwards-level cursor effects

---

## BASIC CUSTOM CURSOR (always start here)

```html
<div class="c-dot"  aria-hidden="true"></div>
<div class="c-ring" aria-hidden="true"></div>
```

```css
body { cursor: none; }

.c-dot {
  position: fixed;
  top: 0; left: 0;
  width: 6px;
  height: 6px;
  background: #1A1A1A;
  border-radius: 50%;
  pointer-events: none;
  z-index: 10000;
  transform: translate(-50%, -50%);
}

.c-ring {
  position: fixed;
  top: 0; left: 0;
  width: 36px;
  height: 36px;
  border: 1px solid rgba(26, 26, 26, 0.45);
  border-radius: 50%;
  pointer-events: none;
  z-index: 10000;
  transform: translate(-50%, -50%);
}

/* Dark mode cursor */
@media (prefers-color-scheme: dark) {
  .c-dot  { background: #F2EFE9; }
  .c-ring { border-color: rgba(242, 239, 233, 0.45); }
}

/* Hide on touch devices */
@media (hover: none) {
  .c-dot, .c-ring { display: none; }
  body { cursor: auto; }
}
```

```javascript
function initCursor() {
  const dot  = document.querySelector('.c-dot');
  const ring = document.querySelector('.c-ring');
  if (!dot || !ring) return;

  // Position tracking
  window.addEventListener('mousemove', e => {
    gsap.to(dot,  { x: e.clientX, y: e.clientY, duration: 0 });
    gsap.to(ring, { x: e.clientX, y: e.clientY, duration: 0.18, ease: 'power2.out' });
  });

  // Grow on interactive elements
  const targets = document.querySelectorAll('a, button, [data-cursor], [data-magnetic]');
  targets.forEach(el => {
    el.addEventListener('mouseenter', () => {
      gsap.to(ring, { scale: 2.8, opacity: 0.5, duration: 0.3, ease: 'power2.out' });
      gsap.to(dot,  { scale: 0,               duration: 0.3 });
    });
    el.addEventListener('mouseleave', () => {
      gsap.to(ring, { scale: 1,   opacity: 1,   duration: 0.4, ease: 'power2.out' });
      gsap.to(dot,  { scale: 1,               duration: 0.3 });
    });
  });

  // Fade on window leave/enter
  document.addEventListener('mouseleave', () =>
    gsap.to([dot, ring], { opacity: 0, duration: 0.3 }));
  document.addEventListener('mouseenter', () =>
    gsap.to([dot, ring], { opacity: 1, duration: 0.3 }));
}
```

---

## CURSOR STATES (data-cursor system)

Add `data-cursor="type"` to any element for custom cursor states:

```javascript
const cursorStates = {
  drag:    { scale: 3.5, text: '⟷', opacity: 0.8 },
  view:    { scale: 4,   text: 'View', opacity: 0.9 },
  play:    { scale: 4,   text: '▶', opacity: 0.9 },
  close:   { scale: 2.5, text: '✕', opacity: 0.8 },
  explore: { scale: 3,   text: '', opacity: 0.6 }
};

function initAdvancedCursor() {
  const ring = document.querySelector('.c-ring');
  const label = document.createElement('span');
  label.className = 'c-label';
  label.style.cssText = `
    position: absolute; top: 50%; left: 50%;
    transform: translate(-50%, -50%);
    font-size: 9px; letter-spacing: 0.1em;
    text-transform: uppercase; white-space: nowrap;
    pointer-events: none;
  `;
  ring.appendChild(label);

  document.querySelectorAll('[data-cursor]').forEach(el => {
    const state = cursorStates[el.dataset.cursor];
    if (!state) return;

    el.addEventListener('mouseenter', () => {
      gsap.to(ring, { scale: state.scale, opacity: state.opacity, duration: 0.35 });
      label.textContent = state.text || '';
    });
    el.addEventListener('mouseleave', () => {
      gsap.to(ring, { scale: 1, opacity: 1, duration: 0.4 });
      label.textContent = '';
    });
  });
}
```

---

## MAGNETIC BUTTON EFFECT

```html
<button class="mag-btn" data-magnetic data-cursor="explore">
  <span>View Project</span>
</button>
```

```css
.mag-btn {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 16px 40px;
  border: 1px solid rgba(26,26,26,0.3);
  background: transparent;
  border-radius: 0; /* no rounded corners on premium sites */
  cursor: none;
  overflow: hidden;
  /* Will be moved by JS — don't set transform here */
}
```

```javascript
function initMagneticButtons() {
  document.querySelectorAll('[data-magnetic]').forEach(el => {
    const inner = el.querySelector('span'); // optional inner element

    el.addEventListener('mousemove', e => {
      const r = el.getBoundingClientRect();
      const x = (e.clientX - r.left - r.width/2)  * 0.38;
      const y = (e.clientY - r.top  - r.height/2) * 0.38;

      gsap.to(el, { x, y, duration: 0.4, ease: 'power2.out' });
      if (inner) gsap.to(inner, { x: x * 0.2, y: y * 0.2, duration: 0.4 });
    });

    el.addEventListener('mouseleave', () => {
      gsap.to(el, { x: 0, y: 0, duration: 0.8, ease: 'elastic.out(1, 0.35)' });
      if (inner) gsap.to(inner, { x: 0, y: 0, duration: 0.8 });
    });
  });
}
```

---

## MOUSE-POSITION PARALLAX LAYERS

Creates depth by moving elements at different speeds relative to mouse position:

```html
<div data-parallax-mouse="0.02">background layer</div>
<div data-parallax-mouse="0.06">mid layer</div>
<div data-parallax-mouse="0.12">foreground layer</div>
```

```javascript
function initMouseParallax() {
  const layers = document.querySelectorAll('[data-parallax-mouse]');
  let mouseX = 0, mouseY = 0;
  let targetX = 0, targetY = 0;

  window.addEventListener('mousemove', e => {
    mouseX = (e.clientX / window.innerWidth)  - 0.5; // -0.5 to 0.5
    mouseY = (e.clientY / window.innerHeight) - 0.5;
  });

  gsap.ticker.add(() => {
    targetX += (mouseX - targetX) * 0.08; // lerp smoothing
    targetY += (mouseY - targetY) * 0.08;

    layers.forEach(el => {
      const speed = parseFloat(el.dataset.parallaxMouse);
      gsap.set(el, {
        x: targetX * window.innerWidth  * speed,
        y: targetY * window.innerHeight * speed
      });
    });
  });
}
```

---

## IMAGE HOVER DISTORTION (CSS + clip-path)

No WebGL needed for this effect:

```css
.hover-distort {
  position: relative;
  overflow: hidden;
}
.hover-distort img {
  transform: scale(1.08);
  transition: transform 0.8s cubic-bezier(0.2, 0.8, 0.2, 1),
              filter 0.8s ease;
  filter: grayscale(60%) brightness(0.9);
}
.hover-distort:hover img {
  transform: scale(1);
  filter: grayscale(0%) brightness(1);
}
.hover-distort::after {
  content: '';
  position: absolute;
  inset: 0;
  background: rgba(26, 26, 26, 0.2);
  transition: opacity 0.5s ease;
}
.hover-distort:hover::after { opacity: 0; }
```

---

## TRAILING CURSOR (multiple dots)

For a more organic cursor feel:

```javascript
function initTrailingCursor(count = 8) {
  const dots = [];

  for (let i = 0; i < count; i++) {
    const dot = document.createElement('div');
    dot.style.cssText = `
      position: fixed; pointer-events: none; z-index: 9999;
      width: ${6 - i * 0.5}px; height: ${6 - i * 0.5}px;
      background: rgba(26, 26, 26, ${0.8 - i * 0.1});
      border-radius: 50%; transform: translate(-50%, -50%);
    `;
    document.body.appendChild(dot);
    dots.push({ el: dot, x: 0, y: 0 });
  }

  let mouseX = 0, mouseY = 0;
  window.addEventListener('mousemove', e => { mouseX = e.clientX; mouseY = e.clientY; });

  gsap.ticker.add(() => {
    dots.forEach((dot, i) => {
      const prev = i === 0 ? { x: mouseX, y: mouseY } : dots[i - 1];
      dot.x += (prev.x - dot.x) * (0.35 - i * 0.03);
      dot.y += (prev.y - dot.y) * (0.35 - i * 0.03);
      gsap.set(dot.el, { x: dot.x, y: dot.y });
    });
  });
}
```

---

## CURSOR BEST PRACTICES

1. **Always hide on touch devices** — `@media (hover: none)`
2. **Never block pointer events** — `pointer-events: none` on cursor elements
3. **Use GSAP for position** (not CSS transitions) — GSAP is more reliable
4. **Dot tracks instantly** (duration: 0), ring trails slightly (duration: 0.18)
5. **Keep it minimal** — the cursor should enhance, not compete with content
6. **Test on all backgrounds** — cursor needs contrast on both light and dark areas
7. **Keyboard users**: cursor effects don't affect keyboard navigation
