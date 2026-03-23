# Advanced Scroll Effects
## Parallax, horizontal scroll, zoom, sticky, 3D perspective, SVG mask, and more

All patterns use GSAP ScrollTrigger + Lenis. Register plugins first:
```javascript
gsap.registerPlugin(ScrollTrigger);
const lenis = new Lenis();
gsap.ticker.add(t => lenis.raf(t * 1000));
lenis.on('scroll', ScrollTrigger.update);
```

---

## 1. BACKGROUND IMAGE PARALLAX

```html
<section class="parallax-section">
  <div class="parallax-bg" data-speed="0.3"></div>
  <h2>Content Over Parallax</h2>
</section>
```

```css
.parallax-section { position: relative; height: 100vh; overflow: hidden; }
.parallax-bg {
  position: absolute; inset: -20% 0; /* extra height for movement range */
  background: url('hero.jpg') center/cover no-repeat;
  will-change: transform;
}
```

```javascript
function initParallax() {
  gsap.utils.toArray('[data-speed]').forEach(el => {
    const speed = parseFloat(el.dataset.speed);
    gsap.to(el, {
      yPercent: -30 * speed,
      ease: 'none',
      scrollTrigger: {
        trigger: el.parentElement,
        start: 'top bottom',
        end: 'bottom top',
        scrub: true
      }
    });
  });
}
```

---

## 2. TEXT PARALLAX

```javascript
function initTextParallax() {
  gsap.utils.toArray('.text-parallax').forEach(text => {
    gsap.to(text, {
      yPercent: -50,
      ease: 'none',
      scrollTrigger: {
        trigger: text,
        start: 'top bottom',
        end: 'bottom top',
        scrub: 0.5
      }
    });
  });
}
```

---

## 3. HORIZONTAL SCROLL SECTION

```html
<section class="h-scroll-wrap">
  <div class="h-scroll-track">
    <div class="h-panel">Panel 1</div>
    <div class="h-panel">Panel 2</div>
    <div class="h-panel">Panel 3</div>
    <div class="h-panel">Panel 4</div>
  </div>
</section>
```

```css
.h-scroll-wrap { overflow: hidden; }
.h-scroll-track { display: flex; width: fit-content; }
.h-panel { width: 100vw; height: 100vh; flex-shrink: 0; }
```

```javascript
function initHorizontalScroll() {
  const track = document.querySelector('.h-scroll-track');
  const panels = gsap.utils.toArray('.h-panel');

  gsap.to(track, {
    x: () => -(track.scrollWidth - window.innerWidth),
    ease: 'none',
    scrollTrigger: {
      trigger: '.h-scroll-wrap',
      start: 'top top',
      end: () => `+=${track.scrollWidth - window.innerWidth}`,
      pin: true,
      scrub: 1,
      invalidateOnRefresh: true
    }
  });
}
```

---

## 4. ZOOM PARALLAX

```html
<section class="zoom-section">
  <div class="zoom-layer" data-zoom="3">
    <img src="layer1.jpg" alt="" />
  </div>
  <div class="zoom-layer" data-zoom="2">
    <img src="layer2.jpg" alt="" />
  </div>
  <div class="zoom-layer" data-zoom="1.5">
    <img src="layer3.jpg" alt="" />
  </div>
</section>
```

```css
.zoom-section { position: relative; height: 300vh; }
.zoom-layer {
  position: sticky; top: 0; height: 100vh;
  display: flex; align-items: center; justify-content: center;
  overflow: hidden;
}
.zoom-layer img { width: 40vw; object-fit: cover; }
```

```javascript
function initZoomParallax() {
  gsap.utils.toArray('.zoom-layer').forEach(layer => {
    const zoom = parseFloat(layer.dataset.zoom);
    gsap.to(layer.querySelector('img'), {
      scale: zoom,
      ease: 'none',
      scrollTrigger: {
        trigger: layer,
        start: 'top top',
        end: 'bottom top',
        scrub: true,
        pin: false
      }
    });
  });
}
```

---

## 5. PARALLAX SCROLL (multi-layer depth)

```javascript
function initLayeredParallax() {
  const layers = [
    { selector: '.bg-far',   speed: 0.1 },
    { selector: '.bg-mid',   speed: 0.3 },
    { selector: '.bg-near',  speed: 0.6 },
    { selector: '.fg',       speed: 1.0 }
  ];

  layers.forEach(({ selector, speed }) => {
    gsap.to(selector, {
      y: () => -(window.innerHeight * speed),
      ease: 'none',
      scrollTrigger: {
        trigger: '.parallax-container',
        start: 'top top',
        end: 'bottom top',
        scrub: true
      }
    });
  });
}
```

---

## 6. CARDS PARALLAX (stacked cards with scroll)

```html
<section class="cards-stack">
  <div class="stack-card" style="--i:0">Card 1</div>
  <div class="stack-card" style="--i:1">Card 2</div>
  <div class="stack-card" style="--i:2">Card 3</div>
</section>
```

```css
.cards-stack { position: relative; height: 300vh; }
.stack-card {
  position: sticky; top: calc(10vh + var(--i) * 30px);
  height: 70vh; margin: 0 auto;
  width: min(90vw, 800px);
  border-radius: 16px;
  background: var(--bg-elevated);
  box-shadow: 0 10px 40px var(--shadow);
  transform-origin: top center;
}
```

```javascript
function initCardsParallax() {
  gsap.utils.toArray('.stack-card').forEach((card, i) => {
    gsap.to(card, {
      scale: 1 - (i * 0.05),
      opacity: 1 - (i * 0.15),
      scrollTrigger: {
        trigger: card,
        start: 'top 10%',
        end: 'bottom top',
        scrub: true
      }
    });
  });
}
```

---

## 7. STACKED CARDS (reveal on scroll)

```javascript
function initStackedCards() {
  const cards = gsap.utils.toArray('.stacked-card');

  cards.forEach((card, i) => {
    gsap.fromTo(card,
      { y: 200, rotateX: -15, opacity: 0, scale: 0.9 },
      {
        y: 0, rotateX: 0, opacity: 1, scale: 1,
        duration: 1,
        ease: 'power3.out',
        scrollTrigger: {
          trigger: card,
          start: 'top 90%',
          end: 'top 40%',
          scrub: 1
        }
      }
    );
  });
}
```

---

## 8. 3D PERSPECTIVE SCROLL

```javascript
function init3DPerspective() {
  const sections = gsap.utils.toArray('.perspective-section');

  sections.forEach(section => {
    gsap.fromTo(section,
      { rotateX: 20, z: -200, opacity: 0.6, transformPerspective: 1000 },
      {
        rotateX: 0, z: 0, opacity: 1,
        ease: 'none',
        scrollTrigger: {
          trigger: section,
          start: 'top bottom',
          end: 'top 20%',
          scrub: 1
        }
      }
    );
  });
}
```

---

## 9. PERSPECTIVE SECTION TRANSITION

```javascript
function initPerspectiveTransition() {
  const sections = gsap.utils.toArray('.persp-section');

  sections.forEach((section, i) => {
    if (i === 0) return;
    ScrollTrigger.create({
      trigger: section,
      start: 'top bottom',
      end: 'top top',
      scrub: true,
      onUpdate: self => {
        const prev = sections[i - 1];
        gsap.set(prev, {
          scale: 1 - self.progress * 0.15,
          rotateX: self.progress * 5,
          opacity: 1 - self.progress * 0.3,
          transformOrigin: 'center bottom',
          transformPerspective: 1000
        });
      }
    });
  });
}
```

---

## 10. SVG MASK SCROLL

```html
<div class="mask-container">
  <svg viewBox="0 0 1 1" class="mask-svg">
    <defs>
      <mask id="scrollMask">
        <circle cx="0.5" cy="0.5" r="0" fill="white" class="mask-circle" />
      </mask>
    </defs>
    <image href="reveal-image.jpg" width="1" height="1" mask="url(#scrollMask)" />
  </svg>
</div>
```

```javascript
function initSVGMaskScroll() {
  gsap.to('.mask-circle', {
    attr: { r: 0.75 },
    ease: 'none',
    scrollTrigger: {
      trigger: '.mask-container',
      start: 'top center',
      end: 'bottom center',
      scrub: 1
    }
  });
}
```

---

## 11. TEXT GRADIENT SCROLL OPACITY

```html
<p class="gradient-text" data-splitting>
  Each word fades in with a gradient as you scroll down the page
</p>
```

```javascript
function initTextGradientScroll() {
  document.querySelectorAll('.gradient-text').forEach(el => {
    const words = el.textContent.split(' ');
    el.innerHTML = words.map(w => `<span class="gw">${w}</span>`).join(' ');

    gsap.from(el.querySelectorAll('.gw'), {
      opacity: 0.15,
      stagger: 0.05,
      ease: 'none',
      scrollTrigger: {
        trigger: el,
        start: 'top 80%',
        end: 'bottom 40%',
        scrub: 1
      }
    });
  });
}
```

---

## 12. SMOOTH PARALLAX SCROLL

```javascript
function initSmoothParallax() {
  gsap.utils.toArray('.smooth-parallax').forEach(el => {
    const depth = el.dataset.depth || 0.5;
    gsap.to(el, {
      y: () => (1 - depth) * ScrollTrigger.maxScroll(window) * -0.1,
      ease: 'none',
      scrollTrigger: { start: 0, end: 'max', scrub: 0.5 }
    });
  });
}
```

---

## 13. STICKY FOOTER (reveal beneath content)

```css
.main-content { position: relative; z-index: 2; background: var(--bg); }
.sticky-footer {
  position: fixed; bottom: 0; left: 0; right: 0;
  z-index: 1;
  min-height: 60vh;
}
/* Add margin-bottom on body = footer height */
body { margin-bottom: 60vh; }
```

---

## 14. VIDEO ON SCROLL (scrub video playback)

```html
<section class="video-scroll">
  <video id="scrollVideo" muted playsinline preload="auto">
    <source src="video.mp4" type="video/mp4" />
  </video>
</section>
```

```javascript
function initVideoScroll() {
  const video = document.getElementById('scrollVideo');

  video.addEventListener('loadedmetadata', () => {
    ScrollTrigger.create({
      trigger: '.video-scroll',
      start: 'top top',
      end: `+=${video.duration * 300}`,
      pin: true,
      scrub: 0.5,
      onUpdate: self => {
        video.currentTime = self.progress * video.duration;
      }
    });
  });

  // Pre-load frames
  video.play().then(() => video.pause()).catch(() => {});
}
```

---

## 15. INFINITE TEXT MOVE ON SCROLL

```html
<div class="infinite-text" aria-hidden="true">
  <div class="infinite-track">
    <span>SCROLL TO EXPLORE — </span>
    <span>SCROLL TO EXPLORE — </span>
    <span>SCROLL TO EXPLORE — </span>
    <span>SCROLL TO EXPLORE — </span>
  </div>
</div>
```

```javascript
function initInfiniteTextScroll() {
  const track = document.querySelector('.infinite-track');
  gsap.to(track, {
    xPercent: -50,
    ease: 'none',
    scrollTrigger: {
      trigger: '.infinite-text',
      start: 'top bottom',
      end: 'bottom top',
      scrub: 0.3
    }
  });
}
```

---

## 16. SVG PATH ON SCROLL (draw as you scroll)

```javascript
function initSVGPathScroll() {
  document.querySelectorAll('.scroll-draw-path').forEach(path => {
    const length = path.getTotalLength();
    gsap.set(path, { strokeDasharray: length, strokeDashoffset: length });

    gsap.to(path, {
      strokeDashoffset: 0,
      ease: 'none',
      scrollTrigger: {
        trigger: path.closest('svg'),
        start: 'top 60%',
        end: 'bottom 30%',
        scrub: 1
      }
    });
  });
}
```

---

## 17. MASK SECTION TRANSITION

```javascript
function initMaskSectionTransition() {
  const sections = gsap.utils.toArray('.mask-section');

  sections.forEach((section, i) => {
    if (i === 0) return;

    gsap.fromTo(section,
      { clipPath: 'circle(0% at 50% 50%)' },
      {
        clipPath: 'circle(150% at 50% 50%)',
        ease: 'none',
        scrollTrigger: {
          trigger: section,
          start: 'top bottom',
          end: 'top top',
          scrub: 1
        }
      }
    );
  });
}
```
