# Animation Library for Awwwards-Level Sites
## Ready-to-use GSAP + CSS animation recipes

---

## SETUP: CDN IMPORTS (for HTML artifacts)

```html
<!-- GSAP Core + Plugins (v3.12.7) -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.7/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.7/ScrollTrigger.min.js"></script>

<!-- Lenis smooth scroll (v1.1+) -->
<script src="https://unpkg.com/lenis@1.1.18/dist/lenis.min.js"></script>

<!-- SplitType (text splitting) -->
<script src="https://unpkg.com/split-type@0.3.4/umd/index.min.js"></script>

<!-- Matter.js (physics — optional, see physics-engines.md) -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/matter-js/0.20.0/matter.min.js"></script>

<!-- Google Fonts: editorial -->
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;1,400&family=Playfair+Display:wght@400;700&family=Pinyon+Script&family=Inter:wght@300;400;500&display=swap" rel="stylesheet"/>
```

---

## 1. PAGE ENTRANCE ANIMATIONS

### Cinematic Title Reveal (char by char)
```javascript
function revealTitle(selector) {
  const split = new SplitType(selector, { types: 'chars, words' });
  gsap.from(split.chars, {
    opacity: 0,
    y: 50,
    rotateX: -50,
    transformOrigin: "0% 50% -30px",
    duration: 1.1,
    stagger: 0.022,
    ease: "power4.out",
    delay: 0.3
  });
}
```

### Panel Cascade (staggered from center)
```javascript
function revealPanels(selector) {
  gsap.from(selector, {
    opacity: 0,
    scaleY: 0.85,
    transformOrigin: "bottom center",
    duration: 1.3,
    stagger: { each: 0.1, from: "center" },
    ease: "power3.out",
    delay: 0.2
  });
}
```

### Line-by-line text reveal
```javascript
function revealLines(selector, staggerDelay = 0.12) {
  const split = new SplitType(selector, { types: 'lines' });
  // Wrap lines for clip-path reveal
  split.lines.forEach(line => {
    const wrapper = document.createElement('div');
    wrapper.style.overflow = 'hidden';
    line.parentNode.insertBefore(wrapper, line);
    wrapper.appendChild(line);
  });
  gsap.from(split.lines, {
    y: '110%',
    duration: 1.0,
    stagger: staggerDelay,
    ease: "power3.out",
    delay: 0.5
  });
}
```

---

## 2. SCROLL-TRIGGERED ANIMATIONS

### Setup: Lenis + GSAP ScrollTrigger
```javascript
// Always initialize this first
const lenis = new Lenis({
  duration: 1.4,
  easing: t => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
  smoothWheel: true
});
lenis.on('scroll', ScrollTrigger.update);
gsap.ticker.add(time => lenis.raf(time * 1000));
gsap.ticker.lagSmoothing(0);
gsap.registerPlugin(ScrollTrigger);
```

### Pinned section with scrubbed animation
```javascript
gsap.timeline({
  scrollTrigger: {
    trigger: '.hero-section',
    start: 'top top',
    end: '+=150%',
    pin: true,
    scrub: 1.5,
    anticipatePin: 1
  }
})
.to('.hero-title', { opacity: 0, y: -80, duration: 1 })
.to('.hero-bg', { scale: 1.15, duration: 1 }, "<");
```

### Fade-in on scroll (reusable)
```javascript
document.querySelectorAll('[data-reveal]').forEach(el => {
  gsap.from(el, {
    opacity: 0,
    y: 40,
    duration: 1.1,
    ease: 'power3.out',
    scrollTrigger: {
      trigger: el,
      start: 'top 85%',
      toggleActions: 'play none none reverse'
    }
  });
});
```

### Horizontal scroll section
```javascript
const sections = gsap.utils.toArray('.h-panel');
gsap.to(sections, {
  xPercent: -100 * (sections.length - 1),
  ease: 'none',
  scrollTrigger: {
    trigger: '.h-scroll-container',
    pin: true,
    scrub: 1,
    snap: 1 / (sections.length - 1),
    end: () => `+=${document.querySelector('.h-scroll-container').offsetWidth}`
  }
});
```

---

## 3. HOVER ANIMATIONS

### Magnetic button
```javascript
function initMagneticButtons() {
  document.querySelectorAll('[data-magnetic]').forEach(btn => {
    btn.addEventListener('mousemove', e => {
      const r = btn.getBoundingClientRect();
      const x = (e.clientX - r.left - r.width / 2)  * 0.35;
      const y = (e.clientY - r.top  - r.height / 2) * 0.35;
      gsap.to(btn, { x, y, duration: 0.4, ease: 'power2.out' });
    });
    btn.addEventListener('mouseleave', () => {
      gsap.to(btn, { x: 0, y: 0, duration: 0.8, ease: 'elastic.out(1, 0.4)' });
    });
  });
}
```

### Image reveal on hover (clip-path)
```css
.reveal-image {
  clip-path: inset(0 100% 0 0);
  transition: clip-path 0.7s cubic-bezier(0.77, 0, 0.175, 1);
}
.reveal-image-trigger:hover .reveal-image {
  clip-path: inset(0 0% 0 0);
}
```

### Text scramble on hover
```javascript
function scrambleText(el, finalText) {
  const chars = '!<>-_\\/[]{}—=+*^?#________';
  let frame = 0;
  const totalFrames = 20;
  const interval = setInterval(() => {
    el.textContent = finalText
      .split('')
      .map((char, i) => {
        if (i < Math.floor(frame / 2)) return finalText[i];
        return chars[Math.floor(Math.random() * chars.length)];
      })
      .join('');
    frame++;
    if (frame > totalFrames * 2) {
      clearInterval(interval);
      el.textContent = finalText;
    }
  }, 35);
}
```

---

## 4. PAGE TRANSITIONS

### Cinematic overlay transition
```javascript
// Create overlay element once
const overlay = document.createElement('div');
overlay.style.cssText = `
  position:fixed; inset:0; background:#1A0500; z-index:9999;
  pointer-events:none; opacity:0;
`;
document.body.appendChild(overlay);

function pageTransition(href) {
  // Fade in overlay
  gsap.to(overlay, {
    opacity: 1,
    duration: 0.7,
    ease: 'power2.inOut',
    onComplete: () => { window.location.href = href; }
  });
}

// On new page load, fade out overlay
window.addEventListener('load', () => {
  gsap.to(overlay, { opacity: 0, duration: 0.9, ease: 'power2.out', delay: 0.1 });
});
```

### Cross-fade between states (no routing)
```javascript
function crossfadeTo(hideEl, showEl, duration = 0.8) {
  gsap.to(hideEl, {
    opacity: 0,
    scale: 0.98,
    duration: duration * 0.6,
    ease: 'power2.inOut',
    onComplete: () => {
      hideEl.style.display = 'none';
      gsap.set(showEl, { opacity: 0, scale: 1.02, display: 'flex' });
      gsap.to(showEl, {
        opacity: 1,
        scale: 1,
        duration: duration,
        ease: 'power2.out'
      });
    }
  });
}
```

---

## 5. LOADER ANIMATIONS

### Number counter loader (Awwwards signature style)
```javascript
function initLoader(onComplete) {
  const loader = document.querySelector('.loader');
  const counter = document.querySelector('.loader-counter');
  let count = 0;

  const interval = setInterval(() => {
    count += Math.floor(Math.random() * 8) + 2;
    if (count >= 100) {
      count = 100;
      clearInterval(interval);
      // Exit loader
      gsap.to(loader, {
        opacity: 0,
        duration: 0.8,
        delay: 0.3,
        ease: 'power2.inOut',
        onComplete: () => {
          loader.style.display = 'none';
          onComplete?.();
        }
      });
    }
    counter.textContent = count;
  }, 60);
}
```

### Curtain reveal loader
```javascript
// Requires two elements: .curtain-top and .curtain-bottom
function curtainReveal(onComplete) {
  const tl = gsap.timeline({ onComplete });
  tl.to('.curtain-top',    { yPercent: -100, duration: 1.1, ease: 'power4.inOut' })
    .to('.curtain-bottom', { yPercent:  100, duration: 1.1, ease: 'power4.inOut' }, "<0.1");
}
```

---

## 6. CURSOR SYSTEM

```css
/* Base cursor styles */
.cursor-dot {
  position: fixed;
  top: 0; left: 0;
  width: 6px; height: 6px;
  background: #1A1A1A;
  border-radius: 50%;
  pointer-events: none;
  z-index: 10000;
  transform: translate(-50%, -50%);
}
.cursor-ring {
  position: fixed;
  top: 0; left: 0;
  width: 36px; height: 36px;
  border: 1px solid rgba(26,26,26,0.4);
  border-radius: 50%;
  pointer-events: none;
  z-index: 10000;
  transform: translate(-50%, -50%);
}
body { cursor: none; }
```

```javascript
function initCursor() {
  const dot  = document.querySelector('.cursor-dot');
  const ring = document.querySelector('.cursor-ring');

  window.addEventListener('mousemove', e => {
    gsap.to(dot,  { x: e.clientX, y: e.clientY, duration: 0 });
    gsap.to(ring, { x: e.clientX, y: e.clientY, duration: 0.2, ease: 'power2.out' });
  });

  // Enlarge on interactive elements
  const interactives = document.querySelectorAll('a, button, [data-cursor-grow]');
  interactives.forEach(el => {
    el.addEventListener('mouseenter', () => {
      gsap.to(ring, { scale: 2.5, opacity: 0.6, duration: 0.3, ease: 'power2.out' });
      gsap.to(dot,  { scale: 0,   duration: 0.3 });
    });
    el.addEventListener('mouseleave', () => {
      gsap.to(ring, { scale: 1, opacity: 1, duration: 0.4, ease: 'power2.out' });
      gsap.to(dot,  { scale: 1, duration: 0.3 });
    });
  });

  // Hide on leave
  document.addEventListener('mouseleave', () => {
    gsap.to([dot, ring], { opacity: 0, duration: 0.3 });
  });
  document.addEventListener('mouseenter', () => {
    gsap.to([dot, ring], { opacity: 1, duration: 0.3 });
  });
}
```

---

## 7. EASING CHEATSHEET

For Awwwards-level sites, always use these. Never use `linear` or default `ease`.

| Purpose | GSAP Ease | Duration |
|---|---|---|
| Elegant text reveal | `power4.out` | 1.0–1.4s |
| Panel hover/expand | `cubic-bezier(0.2, 0.8, 0.2, 1)` | 0.5–0.6s |
| Page exit | `power2.inOut` | 0.6–0.9s |
| Page entrance | `power3.out` | 0.8–1.2s |
| Elastic/magnetic return | `elastic.out(1, 0.4)` | 0.8s |
| Cinematic scrub | `none` (ScrollTrigger scrub) | — |
| Image reveal | `cubic-bezier(0.77, 0, 0.175, 1)` | 0.7s |
| Loader exit | `power4.inOut` | 1.1s |

**Key rule**: The slower and smoother the animation, the more premium it feels. If something looks "fast" — slow it down.

---

## REACT / NEXT.JS VERSIONS (useGSAP hook)

All recipes above are vanilla JS. Below are the React equivalents using `useGSAP` — the official hook that handles cleanup automatically on unmount.

### Setup
```bash
npm install gsap @gsap/react
```
```javascript
// In your root layout or _app.tsx — register once
import { useGSAP } from '@gsap/react'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
gsap.registerPlugin(ScrollTrigger, useGSAP)
```

---

### React: Staggered Panel Reveal
```jsx
import { useRef } from 'react'
import { useGSAP } from '@gsap/react'
import gsap from 'gsap'

export function HeroPanels() {
  const container = useRef(null)

  useGSAP(() => {
    gsap.from('.hero-panel', {
      opacity: 0, scaleY: 0.85, transformOrigin: 'bottom center',
      duration: 1.3, stagger: { each: 0.1, from: 'center' },
      ease: 'power3.out', delay: 0.2
    })
  }, { scope: container }) // scope: container limits selectors to this component

  return (
    <div ref={container} className="panels-container">
      {panels.map((p, i) => <div key={i} className="hero-panel">{/* ... */}</div>)}
    </div>
  )
}
```

---

### React: Cinematic Text Reveal
```jsx
import { useRef } from 'react'
import { useGSAP } from '@gsap/react'
import gsap from 'gsap'
import SplitType from 'split-type'

export function RevealTitle({ children }: { children: string }) {
  const ref = useRef<HTMLHeadingElement>(null)

  useGSAP(() => {
    if (!ref.current) return
    const split = new SplitType(ref.current, { types: 'chars' })
    gsap.from(split.chars, {
      opacity: 0, y: 60, rotateX: -45,
      transformOrigin: '0% 50% -20px',
      duration: 1.1, stagger: 0.025, ease: 'power4.out', delay: 0.3
    })
    return () => split.revert() // cleanup
  }, { scope: ref })

  return <h1 ref={ref}>{children}</h1>
}
```

---

### React: ScrollTrigger Reveal on Scroll
```jsx
import { useRef } from 'react'
import { useGSAP } from '@gsap/react'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

export function ScrollReveal({ children, className = '' }: { children: React.ReactNode, className?: string }) {
  const ref = useRef<HTMLDivElement>(null)

  useGSAP(() => {
    gsap.from(ref.current, {
      opacity: 0, y: 40, duration: 1.1, ease: 'power3.out',
      scrollTrigger: {
        trigger: ref.current,
        start: 'top 88%',
        toggleActions: 'play none none reverse'
      }
    })
  }, { scope: ref })

  return <div ref={ref} className={className}>{children}</div>
}

// Usage: wrap any section
// <ScrollReveal><MySection /></ScrollReveal>
```

---

### React: Lenis smooth scroll setup (Next.js App Router)
```tsx
// components/SmoothScroll.tsx
'use client'
import { useEffect } from 'react'
import Lenis from '@studio-freight/lenis'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import gsap from 'gsap'

export function SmoothScroll({ children }: { children: React.ReactNode }) {
  useEffect(() => {
    const lenis = new Lenis({
      duration: 1.4,
      easing: t => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
      smoothWheel: true,
      smoothTouch: false,
    })

    lenis.on('scroll', ScrollTrigger.update)
    gsap.ticker.add(time => lenis.raf(time * 1000))
    gsap.ticker.lagSmoothing(0)

    return () => {
      lenis.destroy()
      gsap.ticker.remove(time => lenis.raf(time * 1000))
    }
  }, [])

  return <>{children}</>
}

// In app/layout.tsx:
// <SmoothScroll>{children}</SmoothScroll>
```

---

### React: Magnetic Button
```tsx
import { useRef } from 'react'
import { useGSAP } from '@gsap/react'
import gsap from 'gsap'

export function MagneticButton({ children, className = '' }: { children: React.ReactNode, className?: string }) {
  const ref = useRef<HTMLButtonElement>(null)

  useGSAP(() => {
    const el = ref.current
    if (!el) return

    const onMove = (e: MouseEvent) => {
      const r = el.getBoundingClientRect()
      gsap.to(el, {
        x: (e.clientX - r.left - r.width/2)  * 0.38,
        y: (e.clientY - r.top  - r.height/2) * 0.38,
        duration: 0.4, ease: 'power2.out'
      })
    }
    const onLeave = () => gsap.to(el, { x: 0, y: 0, duration: 0.8, ease: 'elastic.out(1, 0.35)' })

    el.addEventListener('mousemove', onMove)
    el.addEventListener('mouseleave', onLeave)
    return () => { el.removeEventListener('mousemove', onMove); el.removeEventListener('mouseleave', onLeave) }
  }, { scope: ref })

  return <button ref={ref} className={className}>{children}</button>
}
```

---

### React: Dark Mode (Gap 10)
```tsx
// hooks/useDarkMode.ts
'use client'
import { useState, useEffect } from 'react'

export function useDarkMode() {
  const [theme, setTheme] = useState<'light' | 'dark'>('light')

  useEffect(() => {
    // Check saved preference, then system preference
    const saved = localStorage.getItem('theme') as 'light' | 'dark' | null
    const system = window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light'
    const initial = saved || system
    setTheme(initial)
    document.documentElement.setAttribute('data-theme', initial)
  }, [])

  const toggle = () => {
    const next = theme === 'dark' ? 'light' : 'dark'
    setTheme(next)
    document.documentElement.setAttribute('data-theme', next)
    localStorage.setItem('theme', next)
  }

  return { theme, toggle }
}

// Usage in any component:
// const { theme, toggle } = useDarkMode()
// <button onClick={toggle}>{theme === 'dark' ? '☀️' : '🌙'}</button>
```

---

## 8. STAGGERED CARD REVEALS (pricing, services, portfolios)

```javascript
// Reveal cards one by one as a group enters viewport
function initCardReveals() {
  gsap.utils.toArray('[data-card-group]').forEach(group => {
    const cards = group.querySelectorAll('.card, .price-card, .service-card');
    gsap.from(cards, {
      opacity: 0,
      y: 60,
      scale: 0.95,
      duration: 0.8,
      stagger: 0.12,
      ease: 'power3.out',
      scrollTrigger: {
        trigger: group,
        start: 'top 80%',
        toggleActions: 'play none none reverse'
      }
    });
  });
}
```

---

## 9. STATS COUNTER ANIMATION

```javascript
// Animate numbers from 0 to target value
function initStatsCounters() {
  document.querySelectorAll('[data-count]').forEach(el => {
    const target = parseInt(el.dataset.count, 10);
    const suffix = el.dataset.suffix || '';
    const prefix = el.dataset.prefix || '';

    gsap.from({ val: 0 }, {
      val: target,
      duration: 2,
      ease: 'power2.out',
      scrollTrigger: {
        trigger: el,
        start: 'top 85%',
        toggleActions: 'play none none none'
      },
      onUpdate: function() {
        el.textContent = prefix + Math.round(this.targets()[0].val).toLocaleString() + suffix;
      }
    });
  });
}
```

```html
<!-- Usage -->
<span data-count="247" data-suffix="+">0</span>
<span data-count="12" data-prefix="$" data-suffix="M">0</span>
```

---

## 10. IMAGE MASK REVEAL (circle or rectangle wipe)

```css
/* Circle reveal */
.mask-circle {
  clip-path: circle(0% at 50% 50%);
  transition: clip-path 1.2s cubic-bezier(0.77, 0, 0.175, 1);
}
.mask-circle.revealed {
  clip-path: circle(75% at 50% 50%);
}

/* Rectangle wipe (left to right) */
.mask-wipe {
  clip-path: inset(0 100% 0 0);
  transition: clip-path 0.9s cubic-bezier(0.77, 0, 0.175, 1);
}
.mask-wipe.revealed {
  clip-path: inset(0 0% 0 0);
}

/* Diagonal reveal */
.mask-diagonal {
  clip-path: polygon(0 0, 0 0, 0 100%, 0 100%);
  transition: clip-path 1s cubic-bezier(0.77, 0, 0.175, 1);
}
.mask-diagonal.revealed {
  clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%);
}
```

```javascript
// Trigger with ScrollTrigger
function initMaskReveals() {
  document.querySelectorAll('[data-mask]').forEach(el => {
    ScrollTrigger.create({
      trigger: el,
      start: 'top 80%',
      onEnter: () => el.classList.add('revealed'),
      onLeaveBack: () => el.classList.remove('revealed')
    });
  });
}
```

---

## 11. CLIP-PATH SECTION WIPE (hero → content)

```javascript
function initSectionWipe() {
  gsap.to('.hero-section', {
    clipPath: 'inset(0 0 100% 0)',
    ease: 'none',
    scrollTrigger: {
      trigger: '.hero-section',
      start: 'top top',
      end: '+=100%',
      pin: true,
      scrub: 1
    }
  });
}
```

