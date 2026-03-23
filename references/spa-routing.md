# SPA Routing & Page Transitions
## Single-file HTML routing with GSAP-driven transitions

Use this pattern for single-file HTML sites (the most common Awwwards artifact format).
For framework-based routing, see `framework-guide.md`.

---

## CORE SPA PATTERN (hash-based sections)

### HTML Structure
```html
<body>
  <nav>
    <button class="nav-link active" data-nav="home">Home</button>
    <button class="nav-link" data-nav="about">About</button>
    <button class="nav-link" data-nav="work">Work</button>
    <button class="nav-link" data-nav="contact">Contact</button>
  </nav>

  <main>
    <section id="home" class="page active"><!-- content --></section>
    <section id="about" class="page"><!-- content --></section>
    <section id="work" class="page"><!-- content --></section>
    <section id="contact" class="page"><!-- content --></section>
  </main>
</body>
```

### CSS
```css
.page {
  display: none;
  opacity: 0;
  min-height: 100vh;
}
.page.active {
  display: block;
  opacity: 1;
}
```

### JavaScript Router
```javascript
function navigateTo(sectionId) {
  const current = document.querySelector('.page.active');
  const target = document.getElementById(sectionId);
  if (!target || current === target) return;

  // 1. Exit animation
  gsap.to(current, {
    opacity: 0,
    y: -20,
    duration: 0.4,
    ease: 'power2.in',
    onComplete: () => {
      current.classList.remove('active');
      current.style.display = 'none';

      // 2. Reset target
      gsap.set(target, { opacity: 0, y: 20, display: 'block' });
      target.classList.add('active');

      // 3. Enter animation
      gsap.to(target, {
        opacity: 1,
        y: 0,
        duration: 0.6,
        ease: 'power3.out'
      });

      // 4. Update URL + nav state
      history.pushState(null, '', `#${sectionId}`);
      updateActiveNav(sectionId);

      // 5. Scroll to top
      window.scrollTo(0, 0);
    }
  });
}

function updateActiveNav(id) {
  document.querySelectorAll('.nav-link').forEach(link => {
    link.classList.toggle('active', link.dataset.nav === id);
  });
}

// Navigation click handler
document.querySelectorAll('.nav-link').forEach(link => {
  link.addEventListener('click', () => navigateTo(link.dataset.nav));
});

// Handle browser back/forward
window.addEventListener('popstate', () => {
  const hash = location.hash.replace('#', '') || 'home';
  navigateTo(hash);
});

// Handle initial hash on page load
window.addEventListener('load', () => {
  const hash = location.hash.replace('#', '') || 'home';
  const section = document.getElementById(hash);
  if (section) {
    document.querySelectorAll('.page').forEach(p => {
      p.classList.remove('active');
      p.style.display = 'none';
    });
    section.classList.add('active');
    section.style.display = 'block';
    gsap.set(section, { opacity: 1, y: 0 });
    updateActiveNav(hash);
  }
});
```

---

## PAGE TRANSITION VARIANTS

### Overlay Wipe (cinematic)
```javascript
// Create overlay element once
const overlay = document.createElement('div');
overlay.style.cssText = `
  position: fixed; inset: 0; z-index: 9999;
  background: var(--accent); pointer-events: none;
  transform: scaleY(0); transform-origin: bottom;
`;
document.body.appendChild(overlay);

function navigateWithWipe(sectionId) {
  const current = document.querySelector('.page.active');
  const target = document.getElementById(sectionId);
  if (!target || current === target) return;

  const tl = gsap.timeline();

  // Wipe in from bottom
  tl.to(overlay, {
    scaleY: 1,
    duration: 0.5,
    ease: 'power4.inOut'
  })
  // Swap content while covered
  .call(() => {
    current.classList.remove('active');
    current.style.display = 'none';
    gsap.set(target, { display: 'block', opacity: 1 });
    target.classList.add('active');
    history.pushState(null, '', `#${sectionId}`);
    updateActiveNav(sectionId);
    window.scrollTo(0, 0);
  })
  // Wipe out to top
  .set(overlay, { transformOrigin: 'top' })
  .to(overlay, {
    scaleY: 0,
    duration: 0.5,
    ease: 'power4.inOut'
  })
  .set(overlay, { transformOrigin: 'bottom' });
}
```

### Curtain Split
```javascript
// Requires two curtain elements
function navigateWithCurtain(sectionId) {
  const current = document.querySelector('.page.active');
  const target = document.getElementById(sectionId);
  if (!target || current === target) return;

  const tl = gsap.timeline();
  tl.to('.curtain-top', { yPercent: 0, duration: 0.5, ease: 'power4.inOut' })
    .to('.curtain-bottom', { yPercent: 0, duration: 0.5, ease: 'power4.inOut' }, '<0.05')
    .call(() => {
      current.classList.remove('active');
      current.style.display = 'none';
      gsap.set(target, { display: 'block', opacity: 1 });
      target.classList.add('active');
      history.pushState(null, '', `#${sectionId}`);
      updateActiveNav(sectionId);
      window.scrollTo(0, 0);
    })
    .to('.curtain-top', { yPercent: -100, duration: 0.5, ease: 'power4.inOut' })
    .to('.curtain-bottom', { yPercent: 100, duration: 0.5, ease: 'power4.inOut' }, '<0.05');
}
```

### Crossfade (simple, elegant)
```javascript
function navigateWithFade(sectionId) {
  const current = document.querySelector('.page.active');
  const target = document.getElementById(sectionId);
  if (!target || current === target) return;

  gsap.to(current, {
    opacity: 0,
    duration: 0.3,
    onComplete: () => {
      current.classList.remove('active');
      current.style.display = 'none';
      target.style.display = 'block';
      target.classList.add('active');
      gsap.fromTo(target, { opacity: 0 }, { opacity: 1, duration: 0.5 });
      history.pushState(null, '', `#${sectionId}`);
      updateActiveNav(sectionId);
      window.scrollTo(0, 0);
    }
  });
}
```

---

## MOBILE HAMBURGER MENU

### HTML
```html
<button class="nav-toggle" aria-label="Toggle menu" aria-expanded="false">
  <span class="bar"></span>
  <span class="bar"></span>
  <span class="bar"></span>
</button>

<div class="mobile-menu">
  <button class="nav-link" data-nav="home">Home</button>
  <button class="nav-link" data-nav="about">About</button>
  <button class="nav-link" data-nav="work">Work</button>
  <button class="nav-link" data-nav="contact">Contact</button>
</div>
```

### CSS
```css
.nav-toggle {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
  z-index: 1001;
}
.nav-toggle .bar {
  width: 28px;
  height: 2px;
  background: var(--ink);
  transition: transform 0.3s ease, opacity 0.3s ease;
}
/* X state */
.nav-toggle.open .bar:nth-child(1) { transform: rotate(45deg) translate(5px, 5px); }
.nav-toggle.open .bar:nth-child(2) { opacity: 0; }
.nav-toggle.open .bar:nth-child(3) { transform: rotate(-45deg) translate(5px, -5px); }

.mobile-menu {
  position: fixed;
  inset: 0;
  background: var(--bg);
  z-index: 1000;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 2rem;
  clip-path: circle(0% at calc(100% - 40px) 40px);
  transition: clip-path 0.6s cubic-bezier(0.77, 0, 0.175, 1);
}
.mobile-menu.open {
  clip-path: circle(150% at calc(100% - 40px) 40px);
}

@media (max-width: 768px) {
  .nav-links { display: none; }
  .nav-toggle { display: flex; }
}
```

### JavaScript
```javascript
function initMobileMenu() {
  const toggle = document.querySelector('.nav-toggle');
  const menu = document.querySelector('.mobile-menu');

  toggle?.addEventListener('click', () => {
    const isOpen = toggle.classList.toggle('open');
    menu.classList.toggle('open');
    toggle.setAttribute('aria-expanded', isOpen);
    document.body.style.overflow = isOpen ? 'hidden' : '';
  });

  // Close menu after navigation
  menu?.querySelectorAll('.nav-link').forEach(link => {
    link.addEventListener('click', () => {
      toggle.classList.remove('open');
      menu.classList.remove('open');
      toggle.setAttribute('aria-expanded', 'false');
      document.body.style.overflow = '';
    });
  });
}
```

---

## BEST PRACTICES

1. **Use `<button>` for nav links** — not `<a>` when routing is JS-only (accessibility)
2. **Always update URL** — `history.pushState()` so browser back/forward works
3. **Scroll to top** after transition — `window.scrollTo(0, 0)` or `lenis.scrollTo(0, { immediate: true })`
4. **Re-initialize animations** in the target section after transition (ScrollTrigger, counters)
5. **Transition duration**: 0.3–0.6s for crossfade, 0.5–1.0s for wipe/curtain
6. **Kill ScrollTriggers** from previous section to prevent memory leaks
7. **Test on mobile** — transitions should feel snappy, not sluggish
