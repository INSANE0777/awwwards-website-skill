# Noise, Grain & Texture Overlays
## Film grain, paper textures, halftone, noise backgrounds — the Awwwards signature

---

## 1. CINEMATIC FILM GRAIN (The Awwwards Standard)

**CRITICAL RULE:** Never use static grain. Static grain looks like a dirty monitor. Awwwards-level grain must be animated (typically 3-4 steps) and MUST use `mix-blend-mode: overlay` or `soft-light` to interact physically with the pixels beneath it.

```css
/* Add this to your main CSS file */
.noise-overlay {
  position: fixed;
  inset: -50%;
  width: 200vw;
  height: 200vh;
  pointer-events: none;
  z-index: 9999;
  opacity: 0.035; /* Adjust between 0.02 and 0.06 */
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 512 512' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.8' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  mix-blend-mode: overlay;
  animation: grainMove 0.3s steps(3) infinite;
}

@keyframes grainMove {
  0%   { transform: translate(0, 0); }
  33%  { transform: translate(-2%, 1%); }
  66%  { transform: translate(1%, -2%); }
  100% { transform: translate(-1%, 2%); }
}
```

---

## 3. PAPER / PARCHMENT TEXTURE

```css
.paper-bg {
  background-color: #F2EFE9;
  background-image:
    url("data:image/svg+xml,%3Csvg width='100' height='100' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='p'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.04' numOctaves='5'/%3E%3CfeColorMatrix type='saturate' values='0'/%3E%3C/filter%3E%3Crect width='100' height='100' filter='url(%23p)' opacity='0.03'/%3E%3C/svg%3E");
}
```

---

## 4. HALFTONE OVERLAY

```css
.halftone::after {
  content: ''; position: absolute; inset: 0;
  background-image: radial-gradient(circle, rgba(0,0,0,0.15) 1px, transparent 1px);
  background-size: 4px 4px;
  pointer-events: none; z-index: 1;
  mix-blend-mode: multiply;
}
```

---

## 5. GRADIENT NOISE (subtle banding fix)

```css
.smooth-gradient {
  background: linear-gradient(180deg, var(--bg) 0%, var(--bg-elevated) 100%);
  position: relative;
}
/* Add noise to prevent gradient banding */
.smooth-gradient::after {
  content: ''; position: absolute; inset: 0;
  background: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence baseFrequency='0.7' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  opacity: 0.02;
  mix-blend-mode: overlay;
  pointer-events: none;
}
```

---

## 6. CANVAS NOISE (dynamic, high-performance)

```javascript
function initCanvasNoise(opacity = 0.03) {
  const canvas = document.createElement('canvas');
  canvas.style.cssText = `position:fixed;inset:0;pointer-events:none;z-index:9999;opacity:${opacity};mix-blend-mode:overlay;`;
  canvas.width = 256; canvas.height = 256;
  document.body.appendChild(canvas);

  const ctx = canvas.getContext('2d');
  const imageData = ctx.createImageData(256, 256);
  const data = imageData.data;

  function render() {
    for (let i = 0; i < data.length; i += 4) {
      const v = Math.random() * 255;
      data[i] = data[i+1] = data[i+2] = v;
      data[i+3] = 255;
    }
    ctx.putImageData(imageData, 0, 0);
  }
  setInterval(render, 100); // Update 10fps for subtle movement
  render();
}
```

---

## WHICH TEXTURE FOR WHICH ARCHETYPE

| Archetype | Texture |
|---|---|
| Archival / Museum (01) | Paper + subtle grain |
| Cinematic (03) | Film grain (animated) |
| Luxury (04, 16) | Micro-grain, barely visible |
| Editorial (06) | Paper texture |
| Brutalist / Neo-Brutalism (14, 09) | Halftone |
| Retro (17) | Heavy grain + scanlines |
| Sci-fi (18) | Scanlines + subtle noise |
| Music Artist (29) | VHS grain + halftone |
| Crypto / Web3 (31) | Micro-grain on dark |
| Minimal SaaS (12, 13) | None or gradient noise only |
