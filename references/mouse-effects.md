# Mouse & Cursor Effects
## Blend modes, magnetic, gooey, distortion, gallery, trailing, and more

All effects assume the default cursor is hidden. See `cursor-system.md` for base cursor setup.

---

## 1. BLEND MODE CURSOR

```css
.cursor-blend {
  position: fixed;
  width: 40px; height: 40px;
  border-radius: 50%;
  background: white;
  mix-blend-mode: difference;
  pointer-events: none;
  z-index: 9999;
  transform: translate(-50%, -50%);
}
```

```javascript
function initBlendCursor() {
  const cursor = document.createElement('div');
  cursor.className = 'cursor-blend';
  document.body.appendChild(cursor);

  const pos = { x: 0, y: 0 };
  document.addEventListener('mousemove', e => { pos.x = e.clientX; pos.y = e.clientY; });
  gsap.ticker.add(() => {
    gsap.set(cursor, { x: pos.x, y: pos.y });
  });

  // Scale up on interactive elements
  document.querySelectorAll('a, button, [data-cursor]').forEach(el => {
    el.addEventListener('mouseenter', () => gsap.to(cursor, { scale: 2.5, duration: 0.3 }));
    el.addEventListener('mouseleave', () => gsap.to(cursor, { scale: 1, duration: 0.3 }));
  });
}
```

---

## 2. MAGNETIC BUTTON

```javascript
function initMagneticButtons() {
  document.querySelectorAll('[data-magnetic]').forEach(btn => {
    const strength = parseFloat(btn.dataset.magnetic) || 0.3;

    btn.addEventListener('mousemove', (e) => {
      const rect = btn.getBoundingClientRect();
      const x = e.clientX - rect.left - rect.width / 2;
      const y = e.clientY - rect.top - rect.height / 2;

      gsap.to(btn, {
        x: x * strength,
        y: y * strength,
        duration: 0.4,
        ease: 'power2.out'
      });

      // Move inner text slightly more for depth
      const inner = btn.querySelector('span');
      if (inner) {
        gsap.to(inner, {
          x: x * strength * 0.5,
          y: y * strength * 0.5,
          duration: 0.4,
          ease: 'power2.out'
        });
      }
    });

    btn.addEventListener('mouseleave', () => {
      gsap.to(btn, { x: 0, y: 0, duration: 0.6, ease: 'elastic.out(1, 0.4)' });
      const inner = btn.querySelector('span');
      if (inner) gsap.to(inner, { x: 0, y: 0, duration: 0.6, ease: 'elastic.out(1, 0.4)' });
    });
  });
}
```

---

## 3. STICKY CURSOR (cursor sticks to element center)

```javascript
function initStickyCursor() {
  const cursor = document.querySelector('.cursor-dot');

  document.querySelectorAll('[data-sticky]').forEach(el => {
    el.addEventListener('mouseenter', () => {
      const rect = el.getBoundingClientRect();
      gsap.to(cursor, {
        x: rect.left + rect.width / 2,
        y: rect.top + rect.height / 2,
        width: rect.width + 20,
        height: rect.height + 20,
        borderRadius: '8px',
        duration: 0.3,
        ease: 'power3.out'
      });
    });

    el.addEventListener('mouseleave', () => {
      gsap.to(cursor, {
        width: 16, height: 16, borderRadius: '50%',
        duration: 0.3, ease: 'power3.out'
      });
    });
  });
}
```

---

## 4. TEXT GOOEY (mouse proximity distortion)

```html
<svg class="goo-filter-svg" style="position:absolute;width:0;height:0">
  <defs>
    <filter id="goo-text">
      <feGaussianBlur in="SourceGraphic" stdDeviation="6" result="blur" />
      <feColorMatrix in="blur" mode="matrix"
        values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 20 -9" result="goo" />
      <feComposite in="SourceGraphic" in2="goo" operator="atop" />
    </filter>
  </defs>
</svg>

<h1 class="gooey-text" style="filter: url(#goo-text)">GOOEY TEXT</h1>
```

```javascript
function initTextGooey() {
  const text = document.querySelector('.gooey-text');
  if (!text) return;

  const letters = text.textContent.split('');
  text.innerHTML = letters.map(l =>
    `<span class="goo-letter" style="display:inline-block">${l === ' ' ? '&nbsp;' : l}</span>`
  ).join('');

  document.addEventListener('mousemove', e => {
    text.querySelectorAll('.goo-letter').forEach(letter => {
      const rect = letter.getBoundingClientRect();
      const cx = rect.left + rect.width / 2;
      const cy = rect.top + rect.height / 2;
      const dist = Math.sqrt((e.clientX - cx) ** 2 + (e.clientY - cy) ** 2);
      const maxDist = 150;

      if (dist < maxDist) {
        const force = (1 - dist / maxDist) * 15;
        const angle = Math.atan2(e.clientY - cy, e.clientX - cx);
        gsap.to(letter, {
          x: Math.cos(angle) * force,
          y: Math.sin(angle) * force,
          duration: 0.3
        });
      } else {
        gsap.to(letter, { x: 0, y: 0, duration: 0.5 });
      }
    });
  });
}
```

---

## 5. GRADIENT MOUSE MOVE (background follows cursor)

```javascript
function initGradientMouseMove() {
  const section = document.querySelector('.gradient-section');
  if (!section) return;

  section.addEventListener('mousemove', e => {
    const rect = section.getBoundingClientRect();
    const x = ((e.clientX - rect.left) / rect.width) * 100;
    const y = ((e.clientY - rect.top) / rect.height) * 100;

    section.style.background = `radial-gradient(circle at ${x}% ${y}%, var(--accent-bg, rgba(99,102,241,0.15)), transparent 50%)`;
  });
}
```

---

## 6. MASK CURSOR EFFECT

```css
.mask-cursor-section {
  position: relative;
  overflow: hidden;
}
.mask-reveal {
  position: absolute; inset: 0;
  background: url('hidden-image.jpg') center/cover;
  clip-path: circle(80px at var(--mx, 50%) var(--my, 50%));
  transition: clip-path 0.1s ease;
  pointer-events: none;
}
```

```javascript
function initMaskCursor() {
  const section = document.querySelector('.mask-cursor-section');
  const reveal = section?.querySelector('.mask-reveal');
  if (!reveal) return;

  section.addEventListener('mousemove', e => {
    const rect = section.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;
    reveal.style.setProperty('--mx', x + 'px');
    reveal.style.setProperty('--my', y + 'px');
  });
}
```

---

## 7. MOUSE SCALE IMAGE GALLERY

```javascript
function initMouseScaleGallery() {
  document.querySelectorAll('.scale-gallery-item').forEach(item => {
    const img = item.querySelector('img');

    item.addEventListener('mousemove', e => {
      const rect = item.getBoundingClientRect();
      const x = (e.clientX - rect.left) / rect.width - 0.5;
      const y = (e.clientY - rect.top) / rect.height - 0.5;

      gsap.to(img, {
        scale: 1.15,
        x: x * 20,
        y: y * 20,
        rotateY: x * 8,
        rotateX: -y * 8,
        duration: 0.4,
        ease: 'power2.out'
      });
    });

    item.addEventListener('mouseleave', () => {
      gsap.to(img, { scale: 1, x: 0, y: 0, rotateY: 0, rotateX: 0, duration: 0.6 });
    });
  });
}
```

---

## 8. MOUSE IMAGE DISTORTION (hover tilt + scale)

```javascript
function initMouseDistortion() {
  document.querySelectorAll('[data-tilt]').forEach(el => {
    const intensity = parseFloat(el.dataset.tilt) || 15;

    el.addEventListener('mousemove', e => {
      const rect = el.getBoundingClientRect();
      const x = (e.clientX - rect.left) / rect.width;
      const y = (e.clientY - rect.top) / rect.height;

      gsap.to(el, {
        rotateX: (0.5 - y) * intensity,
        rotateY: (x - 0.5) * intensity,
        scale: 1.05,
        transformPerspective: 800,
        duration: 0.4,
        ease: 'power2.out'
      });
    });

    el.addEventListener('mouseleave', () => {
      gsap.to(el, { rotateX: 0, rotateY: 0, scale: 1, duration: 0.6 });
    });
  });
}
```

---

## 9. TEXT DISPERSE EFFECT

```javascript
function initTextDisperse() {
  document.querySelectorAll('[data-disperse]').forEach(el => {
    const letters = el.textContent.split('');
    el.innerHTML = letters.map(l =>
      `<span style="display:inline-block">${l === ' ' ? '&nbsp;' : l}</span>`
    ).join('');

    el.addEventListener('mouseenter', () => {
      el.querySelectorAll('span').forEach(span => {
        gsap.to(span, {
          x: (Math.random() - 0.5) * 100,
          y: (Math.random() - 0.5) * 60,
          rotation: (Math.random() - 0.5) * 40,
          opacity: 0,
          duration: 0.5,
          ease: 'power3.out'
        });
      });
    });

    el.addEventListener('mouseleave', () => {
      el.querySelectorAll('span').forEach(span => {
        gsap.to(span, { x: 0, y: 0, rotation: 0, opacity: 1, duration: 0.6, ease: 'power3.out' });
      });
    });
  });
}
```

---

## 10. PIXEL CURSOR TRAILING

```javascript
function initPixelTrailing() {
  const trail = [];
  const TRAIL_COUNT = 20;

  for (let i = 0; i < TRAIL_COUNT; i++) {
    const dot = document.createElement('div');
    dot.style.cssText = `
      position: fixed; width: ${4 + i * 0.5}px; height: ${4 + i * 0.5}px;
      background: var(--accent); border-radius: 50%;
      pointer-events: none; z-index: 9998;
      opacity: ${1 - i / TRAIL_COUNT};
    `;
    document.body.appendChild(dot);
    trail.push({ el: dot, x: 0, y: 0 });
  }

  let mouse = { x: 0, y: 0 };
  document.addEventListener('mousemove', e => { mouse.x = e.clientX; mouse.y = e.clientY; });

  gsap.ticker.add(() => {
    trail.forEach((dot, i) => {
      const prev = i === 0 ? mouse : trail[i - 1];
      dot.x += (prev.x - dot.x) * (0.3 - i * 0.01);
      dot.y += (prev.y - dot.y) * (0.3 - i * 0.01);
      gsap.set(dot.el, { x: dot.x, y: dot.y });
    });
  });
}
```

---

## 11. CARTOON CURSOR TRAILING

```javascript
function initCartoonTrail() {
  const COUNT = 8;
  const colors = ['#FF6B6B', '#4ECDC4', '#45B7D1', '#96CEB4', '#FFEAA7', '#DDA0DD', '#98D8C8', '#F7DC6F'];

  document.addEventListener('mousemove', e => {
    const el = document.createElement('div');
    el.style.cssText = `
      position: fixed; left: ${e.clientX}px; top: ${e.clientY}px;
      width: 20px; height: 20px;
      background: ${colors[Math.floor(Math.random() * colors.length)]};
      border-radius: ${Math.random() > 0.5 ? '50%' : '4px'};
      pointer-events: none; z-index: 9998;
      transform: translate(-50%, -50%) scale(0);
    `;
    document.body.appendChild(el);

    gsap.to(el, {
      scale: 1 + Math.random(),
      rotation: Math.random() * 360,
      opacity: 0,
      x: (Math.random() - 0.5) * 60,
      y: (Math.random() - 0.5) * 60,
      duration: 0.8,
      ease: 'power2.out',
      onComplete: () => el.remove()
    });
  });
}
```

---

## 12. SVG BEZIER CURVE (mouse-driven)

```html
<svg class="bezier-svg" viewBox="0 0 1000 200" width="100%">
  <path class="bezier-path" d="M0,100 Q500,100 1000,100"
        fill="none" stroke="var(--accent)" stroke-width="2" />
</svg>
```

```javascript
function initBezierCurve() {
  const path = document.querySelector('.bezier-path');
  const svg = document.querySelector('.bezier-svg');

  svg.addEventListener('mousemove', e => {
    const rect = svg.getBoundingClientRect();
    const x = ((e.clientX - rect.left) / rect.width) * 1000;
    const y = ((e.clientY - rect.top) / rect.height) * 200;
    path.setAttribute('d', `M0,100 Q${x},${y} 1000,100`);
  });

  svg.addEventListener('mouseleave', () => {
    gsap.to({ val: 0 }, {
      val: 1,
      duration: 0.6,
      ease: 'elastic.out(1, 0.3)',
      onUpdate: function() {
        const progress = this.targets()[0].val;
        const currentD = path.getAttribute('d');
        // Animate back to flat
        path.setAttribute('d', `M0,100 Q500,100 1000,100`);
      }
    });
  });
}
```

---

## 13. PAINT REVEAL (brush cursor reveals content)

```css
.paint-container { position: relative; overflow: hidden; }
.paint-canvas {
  position: absolute; inset: 0; z-index: 2;
  cursor: crosshair;
}
.paint-hidden { position: relative; z-index: 1; }
```

```javascript
function initPaintReveal() {
  const container = document.querySelector('.paint-container');
  const canvas = document.createElement('canvas');
  canvas.className = 'paint-canvas';
  container.prepend(canvas);

  const ctx = canvas.getContext('2d');
  canvas.width = container.offsetWidth;
  canvas.height = container.offsetHeight;

  // Fill with covering color
  ctx.fillStyle = getComputedStyle(document.body).getPropertyValue('--bg') || '#1a1a1a';
  ctx.fillRect(0, 0, canvas.width, canvas.height);
  ctx.globalCompositeOperation = 'destination-out';

  let painting = false;
  canvas.addEventListener('mousedown', () => painting = true);
  canvas.addEventListener('mouseup', () => painting = false);
  canvas.addEventListener('mousemove', e => {
    if (!painting) return;
    const rect = canvas.getBoundingClientRect();
    ctx.beginPath();
    ctx.arc(e.clientX - rect.left, e.clientY - rect.top, 40, 0, Math.PI * 2);
    ctx.fill();
  });
}
```

---

## 14. SPLIT VIGNETTE

```javascript
function initSplitVignette() {
  const section = document.querySelector('.vignette-section');
  const left = section?.querySelector('.vignette-left');
  const right = section?.querySelector('.vignette-right');
  if (!left || !right) return;

  section.addEventListener('mousemove', e => {
    const rect = section.getBoundingClientRect();
    const x = (e.clientX - rect.left) / rect.width;

    gsap.to(left, { width: `${x * 100}%`, duration: 0.3 });
    gsap.to(right, { width: `${(1 - x) * 100}%`, duration: 0.3 });
  });
}
```

---

## 15. CREATIVE BUTTONS

```css
/* Sliding fill */
.btn-slide {
  position: relative; overflow: hidden;
  padding: 1rem 2.5rem; border: 1px solid var(--ink);
  background: transparent; color: var(--ink);
}
.btn-slide::before {
  content: ''; position: absolute; inset: 0;
  background: var(--ink); transform: scaleX(0);
  transform-origin: right; transition: transform 0.5s cubic-bezier(0.77, 0, 0.175, 1);
}
.btn-slide:hover::before { transform: scaleX(1); transform-origin: left; }
.btn-slide:hover { color: var(--bg); }
.btn-slide span { position: relative; z-index: 1; }

/* Circle expand */
.btn-circle {
  position: relative; overflow: hidden;
  padding: 1rem 2.5rem;
}
.btn-circle::before {
  content: ''; position: absolute;
  width: 0; height: 0;
  border-radius: 50%;
  background: var(--accent);
  top: var(--y, 50%); left: var(--x, 50%);
  transform: translate(-50%, -50%);
  transition: width 0.6s, height 0.6s;
}
.btn-circle:hover::before { width: 300px; height: 300px; }
```

```javascript
// Track mouse position for circle-expand origin
document.querySelectorAll('.btn-circle').forEach(btn => {
  btn.addEventListener('mousemove', e => {
    const rect = btn.getBoundingClientRect();
    btn.style.setProperty('--x', `${e.clientX - rect.left}px`);
    btn.style.setProperty('--y', `${e.clientY - rect.top}px`);
  });
});
```
