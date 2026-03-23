# Scroll-Driven Narrative System
## GSAP ScrollTrigger + Lenis — Complete Implementation Guide

---

## SETUP (always first)

```javascript
gsap.registerPlugin(ScrollTrigger);

const lenis = new Lenis({
  duration: 1.4,
  easing: t => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
  smoothWheel: true,
  orientation: 'vertical'
});

// Critical: sync Lenis with ScrollTrigger
lenis.on('scroll', ScrollTrigger.update);
gsap.ticker.add(time => lenis.raf(time * 1000));
gsap.ticker.lagSmoothing(0);
```

---

## PATTERN 1: Pinned Section with Scrubbed Animation

```javascript
// The workhorse of scroll-driven design
const tl = gsap.timeline({
  scrollTrigger: {
    trigger: '.section-hero',
    start: 'top top',
    end: '+=200%',    // 2x the viewport height of scroll distance
    pin: true,        // pin the section while scrolling
    scrub: 1.2,       // smoothing factor (higher = more lag/smoothness)
    anticipatePin: 1, // prevent jump on pin
  }
});

tl.to('.hero-title', { opacity: 0, y: -100, duration: 0.4 })
  .to('.hero-image',  { scale: 1.2, duration: 1 }, 0)
  .from('.hero-sub',  { opacity: 0, y: 40, duration: 0.4 }, 0.6);
```

---

## PATTERN 2: Staggered Section Reveals (scroll enter)

```javascript
// Reusable reveal-on-scroll for all elements
function initRevealAnimations() {
  // Single elements
  gsap.utils.toArray('[data-reveal]').forEach(el => {
    const from = el.dataset.reveal; // 'bottom', 'left', 'right', 'fade'
    const props = {
      bottom: { opacity: 0, y: 60 },
      left:   { opacity: 0, x: -60 },
      right:  { opacity: 0, x: 60 },
      fade:   { opacity: 0 }
    }[from] || { opacity: 0, y: 40 };

    gsap.from(el, {
      ...props, duration: 1.1, ease: 'power3.out',
      scrollTrigger: {
        trigger: el,
        start: 'top 88%',
        toggleActions: 'play none none reverse'
      }
    });
  });

  // Staggered groups
  gsap.utils.toArray('[data-stagger-group]').forEach(group => {
    const items = group.querySelectorAll('[data-stagger-item]');
    gsap.from(items, {
      opacity: 0, y: 50, duration: 1.0,
      stagger: 0.1, ease: 'power3.out',
      scrollTrigger: { trigger: group, start: 'top 85%' }
    });
  });
}
```

---

## PATTERN 3: Text Line Reveals (clip-path style)

```javascript
function initLineReveals() {
  gsap.utils.toArray('[data-line-reveal]').forEach(el => {
    const split = new SplitType(el, { types: 'lines' });

    // Wrap each line in overflow:hidden container
    split.lines.forEach(line => {
      const wrap = document.createElement('div');
      wrap.style.cssText = 'overflow:hidden; display:block;';
      line.parentNode.insertBefore(wrap, line);
      wrap.appendChild(line);
    });

    gsap.from(split.lines, {
      yPercent: 110,
      duration: 1.0,
      stagger: 0.1,
      ease: 'power3.out',
      scrollTrigger: { trigger: el, start: 'top 88%' }
    });
  });
}
```

---

## PATTERN 4: Horizontal Scroll Section

```javascript
function initHorizontalScroll(containerSelector) {
  const container = document.querySelector(containerSelector);
  const panels = gsap.utils.toArray(`${containerSelector} .h-panel`);

  // Ensure the container has enough height for horizontal scroll
  gsap.to(panels, {
    xPercent: -100 * (panels.length - 1),
    ease: 'none',
    scrollTrigger: {
      trigger: container,
      pin: true,
      scrub: 1,
      snap: { snapTo: 1 / (panels.length - 1), duration: 0.3, ease: 'power1.inOut' },
      end: () => `+=${container.offsetWidth * (panels.length - 1)}`
    }
  });
}
```

---

## PATTERN 5: Counter / Number Scrub

```javascript
function initScrollCounter(el, start, end) {
  gsap.fromTo({ val: start }, { val: end },
    {
      duration: 1,
      ease: 'none',
      scrollTrigger: { trigger: el, start: 'top 80%', scrub: true },
      onUpdate: function() {
        el.textContent = Math.round(this.targets()[0].val).toLocaleString();
      }
    }
  );
}
```

---

## PATTERN 6: Parallax Layers (subtle, not exaggerated)

```javascript
function initParallax() {
  gsap.utils.toArray('[data-parallax]').forEach(el => {
    const speed = parseFloat(el.dataset.parallax) || 0.15; // 0.1–0.3 range
    gsap.to(el, {
      yPercent: -100 * speed,
      ease: 'none',
      scrollTrigger: {
        trigger: el.closest('section') || el,
        start: 'top bottom',
        end: 'bottom top',
        scrub: true
      }
    });
  });
}
```

---

## PATTERN 7: Camera Path in 3D (Three.js + ScrollTrigger)

```javascript
// In Three.js setup:
function initCameraScroll(camera, scene) {
  const cameraPath = [
    { pos: new THREE.Vector3(0, 0, 10), look: new THREE.Vector3(0, 0, 0) },
    { pos: new THREE.Vector3(5, 2, 6),  look: new THREE.Vector3(0, 0, 0) },
    { pos: new THREE.Vector3(-3, 4, 3), look: new THREE.Vector3(0, 1, 0) },
  ];

  ScrollTrigger.create({
    trigger: '.webgl-container',
    start: 'top top',
    end: '+=300%',
    pin: true,
    scrub: 1.5,
    onUpdate: (self) => {
      const p = self.progress;
      const segCount = cameraPath.length - 1;
      const seg = Math.min(Math.floor(p * segCount), segCount - 1);
      const t = (p * segCount) - seg;

      camera.position.lerpVectors(cameraPath[seg].pos, cameraPath[seg+1].pos, t);
      const lookTarget = new THREE.Vector3().lerpVectors(
        cameraPath[seg].look, cameraPath[seg+1].look, t
      );
      camera.lookAt(lookTarget);
    }
  });
}
```

---

## CINEMATIC CUSTOM EASINGS (GSAP CustomEase)

```javascript
// Register these once at the top of your script
// (requires GSAP CustomEase plugin)
if (typeof CustomEase !== 'undefined') {
  CustomEase.create('cinematicSilk',   '0.45, 0.05, 0.55, 0.95');
  CustomEase.create('cinematicSmooth', '0.25, 0.1, 0.25, 1');
  CustomEase.create('cinematicFlow',   '0.33, 0, 0.2, 1');
  CustomEase.create('dramaticIn',      '0.95, 0.05, 0.795, 0.035');
  CustomEase.create('dramaticOut',     '0.165, 0.84, 0.44, 1');
}
```

---

## SCROLL NARRATIVE STRUCTURE (editorial pattern)

Use this structure for content-rich scrolling experiences:

```
SECTION 1 — HOOK (100vh, pinned)
  - Hero title reveals character by character
  - Background slowly zooms or shifts
  - Mood is established

SECTION 2 — INTRODUCTION (100–200vh, scrubbed)
  - Content fades in on scroll
  - Supporting imagery parallax-offsets subtly
  - One key stat/fact revealed dramatically

SECTION 3 — DEPTH (200vh+, scroll-driven story)
  - Horizontal scroll OR stacked pinned sections
  - Each section adds one chapter to the narrative
  - Text + image in editorial composition

SECTION 4 — RESOLUTION (100vh)
  - Final statement, full viewport
  - CTA or navigation to next section
  - Often the most minimalist section
```

---

## PERFORMANCE NOTES

- Use `markers: false` in production (remove debug markers)
- Use `invalidateOnRefresh: true` when content height might change
- Kill ScrollTriggers on component unmount (React/Vue)
- `ScrollTrigger.refresh()` after dynamic content loads
- Avoid creating hundreds of ScrollTrigger instances — batch with `batch()`

```javascript
// Batch for many identical elements
ScrollTrigger.batch('[data-reveal]', {
  onEnter: els => gsap.from(els, {
    opacity: 0, y: 40, stagger: 0.1, duration: 1
  }),
  start: 'top 90%'
});
```

---

## PATTERN 8: Mobile Touch & Swipe Handling

```javascript
// Lenis handles smooth scroll on desktop. On mobile, use native scroll
// + optional touch swipe detection for custom gestures.

// Detect mobile and disable Lenis smooth scroll (native is faster on touch)
const isTouchDevice = () => window.matchMedia('(hover: none)').matches;

const lenis = new Lenis({
  duration: 1.4,
  smoothWheel: true,
  // On touch devices, native scroll feels better
  smoothTouch: false, // NEVER smooth touch — causes lag on iOS
});

// Touch swipe detection (for custom horizontal sections)
function initSwipeDetection(el, onSwipeLeft, onSwipeRight) {
  let startX = 0, startY = 0;

  el.addEventListener('touchstart', e => {
    startX = e.touches[0].clientX;
    startY = e.touches[0].clientY;
  }, { passive: true });

  el.addEventListener('touchend', e => {
    const dx = e.changedTouches[0].clientX - startX;
    const dy = e.changedTouches[0].clientY - startY;
    // Only trigger if horizontal swipe dominates
    if (Math.abs(dx) > Math.abs(dy) && Math.abs(dx) > 50) {
      dx < 0 ? onSwipeLeft() : onSwipeRight();
    }
  }, { passive: true });
}
```

---

## PATTERN 9: Scroll Snap (for section-by-section navigation)

```css
/* Full-page scroll snap — each section fills viewport */
.snap-container {
  height: 100vh;
  overflow-y: scroll;
  scroll-snap-type: y mandatory;
  /* Mandatory = hard snap. Use 'proximity' for soft snap */
}
.snap-section {
  height: 100vh;
  scroll-snap-align: start;
  scroll-snap-stop: always; /* Prevent scroll skipping sections */
}

/* Horizontal snap (for project galleries) */
.snap-gallery {
  display: flex;
  overflow-x: scroll;
  scroll-snap-type: x mandatory;
  -webkit-overflow-scrolling: touch; /* iOS momentum scroll */
}
.snap-gallery__item {
  min-width: 100vw;
  scroll-snap-align: start;
}
```

```javascript
// Detect which snap section is active
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const index = [...entry.target.parentElement.children].indexOf(entry.target);
      updateNavDots(index); // highlight nav indicator
    }
  });
}, { threshold: 0.6 });

document.querySelectorAll('.snap-section').forEach(s => observer.observe(s));
```

---

## PATTERN 10: Horizontal Scroll on Mobile (safe approach)

```css
/* Desktop: horizontal scroll section */
@media (min-width: 769px) {
  .h-scroll-track {
    display: flex;
    overflow: hidden; /* GSAP controls position */
  }
  .h-panel {
    min-width: 80vw;
    flex-shrink: 0;
  }
}

/* Mobile: convert to vertical stack — NEVER force horizontal on mobile */
@media (max-width: 768px) {
  .h-scroll-track {
    display: flex;
    flex-direction: column;
    overflow: visible; /* Remove horizontal scroll */
  }
  .h-panel {
    width: 100%;
    min-width: unset;
    margin-bottom: 2rem;
  }
  /* Kill the GSAP horizontal scroll on mobile */
}
```

```javascript
// Only init horizontal scroll on desktop
function initHorizontalScroll() {
  if (window.innerWidth <= 768) return; // Skip on mobile

  const panels = gsap.utils.toArray('.h-panel');
  gsap.to(panels, {
    xPercent: -100 * (panels.length - 1),
    ease: 'none',
    scrollTrigger: {
      trigger: '.h-scroll-track',
      pin: true, scrub: 1,
      snap: 1 / (panels.length - 1),
      end: () => `+=${document.querySelector('.h-scroll-track').offsetWidth}`
    }
  });
}

// Re-init on resize
let scrollInstance = null;
window.addEventListener('resize', () => {
  scrollInstance?.kill();
  ScrollTrigger.refresh();
  initHorizontalScroll();
});
initHorizontalScroll();
```

---

## PATTERN 11: Scroll Progress Indicator

```css
.scroll-progress {
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 3px;
  background: rgba(26,26,26,0.1);
  z-index: 1000;
}
.scroll-progress__bar {
  height: 100%;
  background: var(--accent);
  transform-origin: left;
  transform: scaleX(0);
}
```
```javascript
gsap.to('.scroll-progress__bar', {
  scaleX: 1,
  ease: 'none',
  scrollTrigger: { scrub: true, start: 'top top', end: 'bottom bottom' }
});
```

---

## PATTERN 12: Clip-Path Reveal on Scroll

```javascript
// Element reveals via expanding clip-path as user scrolls
function initClipReveals() {
  gsap.utils.toArray('[data-clip-reveal]').forEach(el => {
    gsap.fromTo(el,
      { clipPath: 'inset(20% 20% 20% 20%)' },
      {
        clipPath: 'inset(0% 0% 0% 0%)',
        duration: 1.2,
        ease: 'power3.out',
        scrollTrigger: {
          trigger: el,
          start: 'top 80%',
          toggleActions: 'play none none reverse'
        }
      }
    );
  });
}
```

---

## PATTERN 13: Staggered Grid Card Reveals

```javascript
// Cards in a grid reveal with offset delays based on position
function initGridReveals() {
  gsap.utils.toArray('.card-grid').forEach(grid => {
    const cards = grid.querySelectorAll('.card');
    const cols = getComputedStyle(grid).gridTemplateColumns.split(' ').length;

    cards.forEach((card, i) => {
      const row = Math.floor(i / cols);
      const col = i % cols;
      const delay = (row * 0.08) + (col * 0.06); // stagger by position

      gsap.from(card, {
        opacity: 0,
        y: 40,
        scale: 0.96,
        duration: 0.8,
        delay: delay,
        ease: 'power3.out',
        scrollTrigger: {
          trigger: grid,
          start: 'top 80%',
          toggleActions: 'play none none reverse'
        }
      });
    });
  });
}
```

---

## LENIS v2 MIGRATION NOTES

The Lenis package has moved from `@studio-freight/lenis` to `lenis`.

```html
<!-- OLD (v1.x — still works but deprecated) -->
<script src="https://cdn.jsdelivr.net/npm/@studio-freight/lenis@1.0.42/dist/lenis.min.js"></script>

<!-- NEW (v1.1+ — recommended) -->
<script src="https://unpkg.com/lenis@1.1.18/dist/lenis.min.js"></script>
```

**API differences (v1.1+):**
- Constructor is the same: `new Lenis({ ... })`
- Use `lenis.on('scroll', ScrollTrigger.update)` — unchanged
- Use `gsap.ticker.add(time => lenis.raf(time * 1000))` — unchanged
- **New**: `lenis.scrollTo(target, { immediate: true })` for instant scroll (useful after SPA navigation)
- **New**: `lenis.stop()` and `lenis.start()` to pause/resume (useful during modals/overlays)

