# Frameworkless Awwwards (Zero-Dependency Perfection)
## Achieving SOTY interactions using only raw Web APIs

While GSAP and Three.js are industry standards for complex choreographies, building an Awwwards-caliber site with **zero dependencies** demonstrates absolute technical mastery. It guarantees 100/100 Lighthouse scores, zero JS bundle bloat, and is the preferred approach for Brutalist, Neo-Brutalist, and highly Kinetic archetypes.

Here is the exact code to replace heavy libraries with pure browser APIs.

---

## 1. THE ZERO-LATENCY LERP CURSOR (Replaces GSAP quickTo)

GSAP's `quickTo` is great, but a native `requestAnimationFrame` loop with Linear Interpolation (LERP) is lighter and often smoother.

```javascript
const cursor = document.querySelector('.cursor');
let mouseX = window.innerWidth / 2;
let mouseY = window.innerHeight / 2;
let currentX = mouseX;
let currentY = mouseY;

// 1. Track raw mouse movement immediately
window.addEventListener('mousemove', (e) => {
    mouseX = e.clientX;
    mouseY = e.clientY;
});

// 2. Interpolate position smoothly on the hardware refresh rate
function renderCursor() {
    // The multiplier (0.15) controls the "weight/drag" of the cursor
    currentX += (mouseX - currentX) * 0.15;
    currentY += (mouseY - currentY) * 0.15;

    // Hardware accelerated translation
    cursor.style.transform = `translate3d(${currentX}px, ${currentY}px, 0)`;

    requestAnimationFrame(renderCursor);
}
requestAnimationFrame(renderCursor);
```

---

## 2. SCROLL REVEALS (Replaces ScrollTrigger)

Instead of loading the 40kb ScrollTrigger plugin just to fade elements up, use the native `IntersectionObserver`.

```css
/* CSS Setup */
.reveal-on-scroll {
  opacity: 0;
  transform: translateY(40px);
  transition: opacity 0.8s cubic-bezier(0.19, 1, 0.22, 1),
              transform 0.8s cubic-bezier(0.19, 1, 0.22, 1);
  will-change: opacity, transform;
}

.reveal-on-scroll.is-visible {
  opacity: 1;
  transform: translateY(0);
}
```

```javascript
/* JS Setup */
const observer = new IntersectionObserver((entries, obs) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.classList.add('is-visible');
            obs.unobserve(entry.target); // Unobserve to improve performance
        }
    });
}, {
    root: null,
    rootMargin: '0px 0px -10% 0px', // Triggers slightly before element enters viewport
    threshold: 0.1
});

document.querySelectorAll('.reveal-on-scroll').forEach(el => observer.observe(el));
```

---

## 3. INFINITE MARQUEE (Replaces GSAP Timeline)

CSS animations run on the browser's compositor thread, making them significantly cheaper than JS-driven tweens for continuous, non-interactive loops.

```css
.marquee-container {
  overflow: hidden;
  white-space: nowrap;
  display: flex;
}

.marquee-track {
  display: inline-block;
  /* Adjust seconds based on text length */
  animation: scroll-marquee 20s linear infinite;
  will-change: transform;
}

@keyframes scroll-marquee {
  0% { transform: translate3d(0, 0, 0); }
  100% { transform: translate3d(-50%, 0, 0); } /* Assuming content is duplicated inside track */
}
```

---

## 4. CSS GLITCH TYPOGRAPHY (Brutalist Signature)

Achieve a violent, stuttering typographic effect without canvas or JS by using `clip: rect` and text-shadow channel splitting.

```html
<!-- HTML Structure -->
<h1 class="glitch-text" data-text="BRUTAL">BRUTAL</h1>
```

```css
/* CSS Implementation */
.glitch-text {
  position: relative;
  display: inline-block;
}

.glitch-text::before,
.glitch-text::after {
  content: attr(data-text);
  position: absolute;
  top: 0; left: 0; width: 100%; height: 100%;
  background: var(--bg-color); /* Must match page background */
}

/* Red channel shift */
.glitch-text::before {
  left: 2px;
  text-shadow: -2px 0 #FF0000;
  clip: rect(44px, 450px, 56px, 0);
  animation: glitch-anim 3s infinite linear alternate-reverse;
}

/* Cyan/Green channel shift */
.glitch-text::after {
  left: -2px;
  text-shadow: -2px 0 #00FF41;
  clip: rect(44px, 450px, 56px, 0);
  animation: glitch-anim 3s infinite linear alternate-reverse;
  animation-delay: 1.5s;
}

@keyframes glitch-anim {
  0%   { clip: rect(10px, 9999px, 83px, 0); }
  20%  { clip: rect(61px, 9999px, 14px, 0); }
  40%  { clip: rect(98px, 9999px, 3px, 0); }
  60%  { clip: rect(21px, 9999px, 56px, 0); }
  80%  { clip: rect(42px, 9999px, 99px, 0); }
  100% { clip: rect(42px, 9999px, 99px, 0); }
}
```

---

## 5. INTERACTIVE PARTICLES (Replaces Three.js)

For 2D interactions (like a node network or swarming pixels), the native HTML5 `<canvas>` API is blistering fast if you avoid transparency (`alpha: false`).

```javascript
const canvas = document.getElementById('bg-canvas');
// alpha: false tells the browser the canvas is opaque, skipping expensive blending
const ctx = canvas.getContext('2d', { alpha: false });

let width = window.innerWidth;
let height = window.innerHeight;
canvas.width = width;
canvas.height = height;

// Simplified Render Loop
function animate() {
    // 1. Draw opaque background
    ctx.fillStyle = '#050505';
    ctx.fillRect(0, 0, width, height);

    // 2. Calculate Physics / Draw Elements
    ctx.fillStyle = '#00FF41';
    // ... loop through particles, update positions ...
    ctx.fillRect(x, y, 2, 2);

    requestAnimationFrame(animate);
}
animate();
```

> **The Perfectionist's Rule:** Only reach for GSAP when an animation requires timeline sequencing, scrubbing linked strictly to scroll position, or complex SVG morphing. Only reach for Three.js when the scene requires a true 3D camera, lighting, or WebGL shaders. For everything else, the browser is powerful enough on its own.