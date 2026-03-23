# Panel-Based Interaction System
## (alanmenken.com / Museum-Gallery Style)

This is the complete reference for building a tall vertical panel system with hover expansion,
grayscale-to-color reveal, dynamic text, video-on-hover, and two-state transitions.

---

## CORE PANEL HTML STRUCTURE

```html
<div class="panels-container" data-hover="0">

  <!-- Side panels (left) -->
  <div class="hero-panel" data-index="1"
    onmouseenter="onPanelEnter(this, 1)"
    onmouseleave="onPanelLeave(this)">
    <img src="..." alt="" />
    <video loop muted playsinline>
      <source src="..." type="video/mp4" />
    </video>
  </div>

  <!-- Repeat for panels 2, 3 -->

  <!-- CENTER panel (visual anchor) -->
  <div class="hero-panel hero-panel--center" data-index="4"
    onmouseenter="onPanelEnter(this, 4)"
    onmouseleave="onPanelLeave(this)">
    <div class="center-glyph">M</div>
  </div>

  <!-- Side panels (right) -->
  <div class="hero-panel" data-index="5"
    onmouseenter="onPanelEnter(this, 5)"
    onmouseleave="onPanelLeave(this)">
    <img src="..." alt="" />
    <video loop muted playsinline>
      <source src="..." type="video/mp4" />
    </video>
  </div>

  <!-- Repeat for panels 6, 7 -->

</div>

<!-- Dynamic text block -->
<div class="text-block">
  <p class="text-label">Explore Song & Extra Material</p>
  <h2 class="text-title">Featured Work</h2>
  <p class="text-subtitle">Select a work to explore</p>
</div>
```

---

## CORE PANEL CSS

```css
/* === CONTAINER === */
.panels-container {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 14px;
  width: 100%;
  max-width: 1300px;
  margin: 0 auto;
}

/* === BASE PANEL === */
.hero-panel {
  position: relative;
  flex: 1;
  height: 72vh;
  min-width: 0;
  cursor: pointer;
  overflow: hidden;

  /* Grayscale default state */
  filter: grayscale(100%) contrast(0.7) brightness(0.95);
  opacity: 0.35;

  /* Smooth transitions — DO NOT CHANGE THESE */
  transition:
    flex 500ms cubic-bezier(0.2, 0.8, 0.2, 1),
    filter 450ms cubic-bezier(0.2, 0.8, 0.2, 1),
    opacity 400ms cubic-bezier(0.2, 0.8, 0.2, 1);
}

/* === CENTER PANEL (slightly wider at rest) === */
.hero-panel--center {
  flex: 1.4;
  background: radial-gradient(ellipse at 40% 25%, #8B2500 0%, #4A1000 50%, #1A0500 100%);
  display: flex;
  align-items: center;
  justify-content: center;
}

/* === DIMMING: when ANY panel is hovered, dim others === */
.panels-container:hover .hero-panel {
  opacity: 0.12;
  filter: grayscale(100%) contrast(0.55) brightness(0.85);
}

/* === ACTIVE: the hovered panel === */
.panels-container .hero-panel:hover {
  flex: 3.5;
  opacity: 1;
  filter: grayscale(25%) contrast(1.05) brightness(1.05);
}

/* === MEDIA LAYERS within panel === */
.hero-panel img,
.hero-panel video {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  pointer-events: none;
}

.hero-panel img {
  z-index: 2;
  opacity: 1;
  transition: opacity 300ms ease;
}

.hero-panel video {
  z-index: 3;
  opacity: 0;
  transition: opacity 300ms ease;
}

/* Reveal video, hide image on hover */
.hero-panel:hover video { opacity: 1; }
.hero-panel:hover img  { opacity: 0; }

/* === CENTER GLYPH === */
.center-glyph {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: clamp(5rem, 12vw, 10rem);
  font-weight: 400;
  color: #1A0500;
  letter-spacing: -0.03em;
  user-select: none;
  position: relative;
  z-index: 5;
}

/* === TEXT BLOCK === */
.text-block {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  margin-top: 2rem;
}

.text-label {
  font-family: 'Inter', sans-serif;
  font-size: 10px;
  font-weight: 500;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: #6B6B6B;
  margin-bottom: 1rem;
}

.text-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: clamp(2.5rem, 6vw, 5rem);
  font-weight: 300;
  color: #1A1A1A;
  letter-spacing: -0.02em;
  line-height: 1;
  /* Smooth opacity for dynamic text swap */
  transition: opacity 150ms ease;
}

.text-subtitle {
  font-family: 'Cormorant Garamond', serif;
  font-size: 13px;
  font-style: italic;
  color: #1A1A1A;
  letter-spacing: 0.15em;
  margin-top: 0.75rem;
  transition: opacity 150ms ease;
}
```

---

## DYNAMIC TEXT JAVASCRIPT

```javascript
// Data map: panel index → title/subtitle
const panelData = {
  0: { title: "Featured Work",        subtitle: "Select a work to explore" },
  1: { title: "Panel One Title",      subtitle: "Category · Description" },
  2: { title: "Panel Two Title",      subtitle: "Category · Description" },
  // ... add all panels
};

const titleEl    = document.querySelector('.text-title');
const subtitleEl = document.querySelector('.text-subtitle');
const container  = document.querySelector('.panels-container');

function updateText(index) {
  // Fade out
  titleEl.style.opacity    = 0;
  subtitleEl.style.opacity = 0;

  setTimeout(() => {
    titleEl.textContent    = panelData[index].title;
    subtitleEl.textContent = panelData[index].subtitle;
    // Fade in
    titleEl.style.opacity    = 1;
    subtitleEl.style.opacity = 1;
  }, 120);
}

// Observe data-hover attribute changes
const observer = new MutationObserver(() => {
  const current = container.getAttribute('data-hover');
  updateText(current);
});
observer.observe(container, { attributes: true, attributeFilter: ['data-hover'] });

// Initialize
updateText(0);

function onPanelEnter(el, index) {
  el.querySelector('video')?.play();
  container.setAttribute('data-hover', index);
}

function onPanelLeave(el) {
  const video = el.querySelector('video');
  if (video) { video.pause(); video.currentTime = 0; }
  container.setAttribute('data-hover', '0');
}
```

---

## TWO-STATE SYSTEM (Overview ↔ Detail)

```javascript
// State tracking
let currentState = 'overview'; // 'overview' | 'detail'

const overviewSection = document.getElementById('overview-section');
const detailSection   = document.getElementById('detail-section');
const overviewPanels  = document.querySelectorAll('.hero-panel');
const titleEl         = document.querySelector('.locked-title');
const subtitleEl      = document.querySelector('.locked-subtitle');

// === TRANSITION: Overview → Detail ===
function transitionToDetail() {
  if (currentState === 'detail') return;
  currentState = 'detail';

  // 1. Fade out title text
  gsap.to([titleEl, subtitleEl], { opacity: 0, duration: 0.4 });

  // 2. Stagger-exit panels (randomized, non-mechanical)
  overviewPanels.forEach(panel => {
    gsap.to(panel, {
      opacity: 0,
      scale: 0.9 + Math.random() * 0.05,
      duration: 0.65 + Math.random() * 0.2,
      delay: 0.04 + Math.random() * 0.28,
      ease: 'power2.inOut'
    });
  });

  // 3. After exit completes, swap sections
  setTimeout(() => {
    overviewSection.style.display = 'none';
    detailSection.style.opacity   = 0;
    detailSection.style.transform = 'scale(0.97)';
    detailSection.style.display   = 'flex';

    // 4. Fade in detail section
    gsap.to(detailSection, {
      opacity: 1,
      scale: 1,
      duration: 0.9,
      ease: 'power2.out'
    });
  }, 700); // Wait for panel exit
}

// === TRANSITION: Detail → Overview ===
function transitionToOverview() {
  if (currentState === 'overview') return;
  currentState = 'overview';

  // 1. Fade out detail section
  gsap.to(detailSection, {
    opacity: 0,
    scale: 0.97,
    duration: 0.5,
    ease: 'power2.inOut',
    onComplete: () => {
      detailSection.style.display = 'none';
      overviewSection.style.display = 'flex';

      // Reset panels to pre-animation state first
      overviewPanels.forEach(panel => {
        gsap.set(panel, { opacity: 0, scale: 0.92 });
      });

      // 2. Stagger-reveal panels
      overviewPanels.forEach(panel => {
        gsap.to(panel, {
          opacity: 0.35, // original opacity
          scale: 1,
          duration: 0.8 + Math.random() * 0.3,
          delay: 0.05 + Math.random() * 0.3,
          ease: 'power3.out'
        });
      });

      // 3. Restore title text
      setTimeout(() => {
        gsap.to([titleEl, subtitleEl], { opacity: 1, duration: 0.6 });
      }, 300);
    }
  });
}
```

---

## NAVIGATION ICONS (below text block)

```html
<div class="state-nav">
  <button class="state-btn" id="btn-detail" onclick="transitionToDetail()" aria-label="View detail">
    <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
      <rect x="0.5" y="0.5" width="7" height="19" rx="0" stroke="#1A1A1A" stroke-width="1"/>
      <rect x="12.5" y="0.5" width="7" height="19" rx="0" stroke="#1A1A1A" stroke-width="1"/>
    </svg>
  </button>
  <button class="state-btn" id="btn-overview" onclick="transitionToOverview()" aria-label="View overview">
    <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
      <rect x="0.5" y="0.5" width="19" height="19" rx="0" stroke="#1A1A1A" stroke-width="1"/>
      <line x1="7" y1="0" x2="7" y2="20" stroke="#1A1A1A" stroke-width="1"/>
      <line x1="13" y1="0" x2="13" y2="20" stroke="#1A1A1A" stroke-width="1"/>
    </svg>
  </button>
</div>
```

```css
.state-nav {
  display: flex;
  gap: 16px;
  margin-top: 2rem;
  justify-content: center;
}
.state-btn {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  border: 1px solid rgba(26,26,26,0.2);
  background: transparent;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.3s ease, border-color 0.3s ease;
}
.state-btn:hover {
  background: rgba(26,26,26,0.06);
  border-color: rgba(26,26,26,0.5);
}
```

---

## NAVIGATION (Top bar)

```html
<nav class="site-nav">
  <div class="nav-left">
    <!-- Minimal monochrome logo mark -->
    <span class="nav-icon material-symbols-outlined">notifications</span>
  </div>
  <div class="nav-center">
    <h1 class="site-signature">Artist Name</h1>
  </div>
  <div class="nav-right">
    <span class="nav-icon material-symbols-outlined">menu</span>
  </div>
</nav>
```

```css
.site-nav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 2rem 3rem;
  z-index: 100;
  /* No background — floats over content */
}

.site-signature {
  font-family: 'Pinyon Script', cursive;
  font-size: 4rem;
  line-height: 1;
  color: #1A1A1A;
  font-weight: 400;
  user-select: none;
}

.nav-icon {
  font-size: 1.5rem;
  cursor: pointer;
  color: #1A1A1A;
  opacity: 0.7;
  transition: opacity 0.3s ease;
}
.nav-icon:hover { opacity: 1; }
```

---

## SECONDARY DETAIL SECTION (Multi-Slice Video)

```css
/* The secondary section uses the same gallery width and panel gap */
:root {
  --gallery-width: 1100px;
  --panel-gap: 18px;
  --total-panels: 6;
  --panel-width: calc((1100px - (5 * 18px)) / 6);
}

.detail-gallery {
  height: 70vh;
  display: flex;
  align-items: center;
  gap: var(--panel-gap);
  width: var(--gallery-width);
  margin: 0 auto;
}

.detail-panel {
  position: relative;
  overflow: hidden;
  height: 100%;
  flex: 0 0 var(--panel-width);
}

/* Vertical offsets create cinematic stagger — DO NOT EQUALIZE */
.detail-panel:nth-child(1) { height: 75%; transform: translateY(-10px); }
.detail-panel:nth-child(2) { height: 85%; transform: translateY(20px); }
.detail-panel:nth-child(3) { height: 72%; transform: translateY(-30px); }
.detail-panel:nth-child(4) { height: 78%; transform: translateY(10px); }
.detail-panel:nth-child(5) {
  height: 92%;
  background: white;
  z-index: 20;
  box-shadow: 0 20px 60px rgba(0,0,0,0.08);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  padding: 40px 0;
}
.detail-panel:nth-child(6) { height: 74%; transform: translateY(-15px); }
```

---

## FOOTER AREA

```html
<footer class="site-footer">
  <div class="footer-left">
    <a href="#" class="footer-link">See all works</a>
  </div>
  <div class="footer-center">
    <!-- Optional: play/equalizer icons -->
  </div>
  <div class="footer-right">
    <!-- Small icons or empty -->
  </div>
</footer>
```

```css
.site-footer {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  padding: 2rem 3rem;
  z-index: 100;
  /* No border, blends into background */
}

.footer-link {
  font-family: 'Inter', sans-serif;
  font-size: 10px;
  font-weight: 500;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: #1A1A1A;
  text-decoration: none;
  opacity: 0.6;
  transition: opacity 0.3s ease;
}
.footer-link:hover { opacity: 1; }
```
