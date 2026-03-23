# Dark Mode
## System preference detection, animated toggle, CSS variable swapping

---

## CSS VARIABLES APPROACH

```css
:root {
  color-scheme: light dark;

  /* Light (default) */
  --bg: #FAFAFA;
  --bg-elevated: #FFFFFF;
  --ink: #1A1A1A;
  --ink-muted: #666666;
  --border: rgba(0, 0, 0, 0.08);
  --shadow: rgba(0, 0, 0, 0.08);
  --accent: #6366F1;
}

[data-theme="dark"] {
  --bg: #0A0A0A;
  --bg-elevated: #141414;
  --ink: #F5F5F5;
  --ink-muted: #999999;
  --border: rgba(255, 255, 255, 0.08);
  --shadow: rgba(0, 0, 0, 0.4);
  --accent: #818CF8;
}

/* System preference auto-detect */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    --bg: #0A0A0A;
    --bg-elevated: #141414;
    --ink: #F5F5F5;
    --ink-muted: #999999;
    --border: rgba(255, 255, 255, 0.08);
    --shadow: rgba(0, 0, 0, 0.4);
    --accent: #818CF8;
  }
}

/* Smooth transition between modes */
body {
  background: var(--bg);
  color: var(--ink);
  transition: background-color 0.3s ease, color 0.3s ease;
}
```

---

## TOGGLE BUTTON

```html
<button class="theme-toggle" id="themeToggle" aria-label="Toggle dark mode">
  <svg class="sun-icon" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
    <circle cx="12" cy="12" r="5"/><line x1="12" y1="1" x2="12" y2="3"/>
    <line x1="12" y1="21" x2="12" y2="23"/><line x1="4.22" y1="4.22" x2="5.64" y2="5.64"/>
    <line x1="18.36" y1="18.36" x2="19.78" y2="19.78"/>
    <line x1="1" y1="12" x2="3" y2="12"/><line x1="21" y1="12" x2="23" y2="12"/>
    <line x1="4.22" y1="19.78" x2="5.64" y2="18.36"/>
    <line x1="18.36" y1="5.64" x2="19.78" y2="4.22"/>
  </svg>
  <svg class="moon-icon" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
    <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/>
  </svg>
</button>
```

```css
.theme-toggle {
  position: fixed; top: 2rem; right: 6rem; z-index: 100;
  width: 44px; height: 44px; border-radius: 50%;
  border: 1px solid var(--border);
  background: var(--bg-elevated); cursor: pointer;
  display: flex; align-items: center; justify-content: center;
  transition: transform 0.3s ease, border-color 0.3s ease;
}
.theme-toggle:hover { transform: scale(1.1); }

.sun-icon, .moon-icon { position: absolute; transition: opacity 0.3s, transform 0.3s; }
[data-theme="dark"] .sun-icon { opacity: 0; transform: rotate(90deg) scale(0); }
[data-theme="dark"] .moon-icon { opacity: 1; transform: rotate(0) scale(1); }
:root:not([data-theme="dark"]) .moon-icon { opacity: 0; transform: rotate(-90deg) scale(0); }
:root:not([data-theme="dark"]) .sun-icon { opacity: 1; }
```

---

## JAVASCRIPT

```javascript
function initDarkMode() {
  const toggle = document.getElementById('themeToggle');
  const root = document.documentElement;

  // Check saved preference, then system preference
  const saved = localStorage.getItem('theme');
  const systemDark = window.matchMedia('(prefers-color-scheme: dark)').matches;

  if (saved) {
    root.dataset.theme = saved;
  } else if (systemDark) {
    root.dataset.theme = 'dark';
  }

  toggle?.addEventListener('click', () => {
    const isDark = root.dataset.theme === 'dark';
    root.dataset.theme = isDark ? 'light' : 'dark';
    localStorage.setItem('theme', root.dataset.theme);
  });

  // Listen for system changes
  window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', e => {
    if (!localStorage.getItem('theme')) {
      root.dataset.theme = e.matches ? 'dark' : 'light';
    }
  });
}
```

---

## DARK MODE IMAGE HANDLING

```css
/* Reduce brightness of images in dark mode */
[data-theme="dark"] img:not([data-no-dim]) {
  filter: brightness(0.85);
}

/* Invert logos/icons */
[data-theme="dark"] .logo-light { display: none; }
[data-theme="dark"] .logo-dark  { display: block; }
:root:not([data-theme="dark"]) .logo-dark { display: none; }
```

---

## RULES
1. **Always respect system preference** as default
2. **Persist user choice** in `localStorage`
3. **Transition colors smoothly** — `transition: background 0.3s, color 0.3s`
4. **Test contrast ratios** in both modes — WCAG AA minimum (4.5:1)
5. **Avoid pure white (#fff) on dark** — use `#F5F5F5` or similar. Pure white glares
6. **Dim images slightly** in dark mode for visual comfort
