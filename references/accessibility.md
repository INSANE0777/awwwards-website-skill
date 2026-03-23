# Accessibility & Reduced Motion
## ARIA patterns, focus management, prefers-reduced-motion, keyboard navigation

Awwwards Usability score is **30%**. Accessibility isn't optional — it's a scoring criterion.

---

## 1. PREFERS-REDUCED-MOTION

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

```javascript
// Check in JS
const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

// Skip animations for reduced-motion users
if (!prefersReducedMotion) {
  gsap.from('.hero-title', { opacity: 0, y: 50, duration: 1 });
  initLenis(); // smooth scroll
  initCursorEffects();
} else {
  // Show content immediately
  gsap.set('.hero-title', { opacity: 1, y: 0 });
}
```

---

## 2. FOCUS MANAGEMENT

```css
/* Visible focus styles (never remove outline without replacement) */
:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 3px;
  border-radius: 2px;
}

/* Remove outline for mouse users, keep for keyboard */
:focus:not(:focus-visible) { outline: none; }

/* Skip to content link */
.skip-link {
  position: fixed; top: -100%; left: 50%;
  transform: translateX(-50%);
  padding: 0.8rem 1.5rem;
  background: var(--accent); color: white;
  z-index: 10000; border-radius: 0 0 8px 8px;
  transition: top 0.3s ease;
}
.skip-link:focus { top: 0; }
```

```html
<a href="#main-content" class="skip-link">Skip to content</a>
<!-- ... nav ... -->
<main id="main-content" tabindex="-1">
```

---

## 3. ARIA PATTERNS

```html
<!-- Modal -->
<div class="modal" role="dialog" aria-modal="true" aria-labelledby="modalTitle">
  <h2 id="modalTitle">Title</h2>
  <button aria-label="Close" onclick="closeModal()">×</button>
</div>

<!-- Navigation -->
<nav aria-label="Main navigation">
  <a href="#home" aria-current="page">Home</a>
  <a href="#work">Work</a>
</nav>

<!-- Image -->
<img src="hero.jpg" alt="Dramatic sunset over mountains with golden light" />
<img src="divider.svg" alt="" role="presentation" /> <!-- decorative -->

<!-- Loading -->
<div class="loader" role="status" aria-label="Loading">
  <span class="sr-only">Loading...</span>
</div>

<!-- Hamburger -->
<button class="menu-trigger" aria-expanded="false" aria-controls="sideMenu" aria-label="Open menu">
```

```javascript
// Toggle aria-expanded
function toggleMenu(trigger, menu) {
  const isOpen = trigger.getAttribute('aria-expanded') === 'true';
  trigger.setAttribute('aria-expanded', !isOpen);
  menu.classList.toggle('open');
}
```

---

## 4. KEYBOARD NAVIGATION

```javascript
// Trap focus inside modal
function trapFocus(modal) {
  const focusable = modal.querySelectorAll('a, button, input, textarea, select, [tabindex]:not([tabindex="-1"])');
  const first = focusable[0];
  const last = focusable[focusable.length - 1];

  modal.addEventListener('keydown', e => {
    if (e.key === 'Tab') {
      if (e.shiftKey && document.activeElement === first) { e.preventDefault(); last.focus(); }
      else if (!e.shiftKey && document.activeElement === last) { e.preventDefault(); first.focus(); }
    }
    if (e.key === 'Escape') closeModal();
  });

  first.focus();
}
```

---

## 5. SCREEN READER UTILITIES

```css
.sr-only {
  position: absolute; width: 1px; height: 1px;
  padding: 0; margin: -1px; overflow: hidden;
  clip: rect(0, 0, 0, 0); white-space: nowrap;
  border: 0;
}
```

```html
<!-- Live region for dynamic content -->
<div aria-live="polite" aria-atomic="true" class="sr-only" id="announcer"></div>
```

```javascript
function announce(message) {
  document.getElementById('announcer').textContent = message;
}
```

---

## 6. COLOR CONTRAST

```
WCAG AA minimum ratios:
- Normal text:  4.5:1
- Large text:   3:1  (≥18px bold or ≥24px regular)
- UI components: 3:1
```

```css
/* Safe combinations: */
/* Dark bg + light text */
.dark  { background: #1a1a1a; color: #f5f5f5; } /* ✅ 16.5:1 */
/* Light bg + dark text */
.light { background: #fafafa; color: #1a1a1a; } /* ✅ 16.9:1 */
/* Danger: accent on bg */
.risky { background: #fafafa; color: #6366F1; } /* ⚠️ check ratio! */
```

---

## CHECKLIST
- [ ] Skip-to-content link present
- [ ] All images have alt text (empty for decorative)
- [ ] Focus styles visible for keyboard users
- [ ] `prefers-reduced-motion` respected
- [ ] Modals trap focus and close on ESC
- [ ] `aria-expanded` on toggles
- [ ] `aria-label` on icon-only buttons
- [ ] Color contrast ≥ 4.5:1 for text
- [ ] Form inputs have associated labels
- [ ] `aria-live` regions for dynamic content
