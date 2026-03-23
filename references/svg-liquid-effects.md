# SVG Liquid & Gooey Effects
## The 2026 Standard for Organic Reveals

This module covers the "Awwwards SOTY" approach to liquid distortions using SVG filters and GSAP.

---

## 1. THE "GOOEY" FILTER (The Foundation)
To make two shapes look like they are melting into each other.

```html
<svg class="gooey-filter">
  <defs>
    <filter id="goo">
      <feGaussianBlur in="SourceGraphic" stdDeviation="10" result="blur" />
      <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 20 -10" result="goo" />
      <feComposite in="SourceGraphic" in2="goo" operator="atop"/>
    </filter>
  </defs>
</svg>
```

---

## 2. LIQUID TEXT DISTORTION (feTurbulence)
Creating a "Heat Haze" or "Underwater" effect on text.

```html
<svg>
  <filter id="liquid">
    <feTurbulence type="turbulence" baseFrequency="0.01 0.01" numOctaves="2" result="turbulence" />
    <feDisplacementMap in="SourceGraphic" in2="turbulence" scale="50" xChannelSelector="R" yChannelSelector="G" />
  </filter>
</svg>
```

### Animating the Liquid with GSAP:
```javascript
gsap.to("#liquid feTurbulence", {
  attr: { baseFrequency: "0.05 0.05" },
  duration: 5,
  repeat: -1,
  yoyo: true,
  ease: "none"
});
```

---

## 3. ORGANIC REVEAL RECIPE
1. Create a large SVG path covering an image.
2. Apply the `goo` filter to the path.
3. Scale the path from 0 to 1 using GSAP.
4. **Result:** The image "blobs" into existence rather than just fading in.

---

## 4. PERFORMANCE TIP (2026):
- Always set `color-interpolation-filters="sRGB"` on your SVG filters to avoid gamma-shift issues.
- Keep `numOctaves` below 3 for mobile performance.
