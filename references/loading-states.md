# Loading States & Skeleton Screens
## Branded loaders, shimmer effects, progressive content, and transition states

The loader is the first thing users see. It sets the emotional register for the entire experience.

---

## PATTERN 1: Number Counter Loader (Awwwards classic)

```html
<div class="loader" id="loader">
  <div class="loader-count">0</div>
  <div class="loader-bar">
    <div class="loader-fill"></div>
  </div>
</div>
```

```css
.loader {
  position: fixed;
  inset: 0;
  background: var(--bg);
  z-index: 10000;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.loader-count {
  font-size: clamp(5rem, 15vw, 12rem);
  font-weight: 700;
  line-height: 1;
  color: var(--ink);
}

.loader-bar {
  width: 200px;
  height: 2px;
  background: var(--border);
  margin-top: 2rem;
  overflow: hidden;
}

.loader-fill {
  height: 100%;
  width: 0%;
  background: var(--accent);
  transition: width 0.1s linear;
}
```

```javascript
function initLoader(onComplete) {
  const counter = document.querySelector('.loader-count');
  const fill = document.querySelector('.loader-fill');
  const loader = document.querySelector('.loader');
  let count = 0;

  const interval = setInterval(() => {
    count += Math.floor(Math.random() * 8) + 2;
    if (count >= 100) {
      count = 100;
      clearInterval(interval);
      fill.style.width = '100%';
      counter.textContent = '100';

      gsap.to(loader, {
        yPercent: -100,
        duration: 0.8,
        delay: 0.4,
        ease: 'power4.inOut',
        onComplete: () => {
          loader.style.display = 'none';
          onComplete?.();
        }
      });
    }
    counter.textContent = count;
    fill.style.width = count + '%';
  }, 50);
}
```

---

## PATTERN 2: Word Reveal Loader

```html
<div class="loader" id="loader">
  <div class="loader-words">
    <span class="loader-word active">Creating</span>
    <span class="loader-word">Designing</span>
    <span class="loader-word">Building</span>
    <span class="loader-word">Launching</span>
  </div>
</div>
```

```css
.loader-words {
  position: relative;
  height: 1.2em;
  overflow: hidden;
  font-size: clamp(2rem, 5vw, 4rem);
  font-weight: 300;
}

.loader-word {
  display: block;
  position: absolute;
  opacity: 0;
  transform: translateY(100%);
}

.loader-word.active {
  opacity: 1;
  transform: translateY(0);
}
```

```javascript
function initWordLoader(onComplete) {
  const words = document.querySelectorAll('.loader-word');
  let index = 0;

  const cycle = setInterval(() => {
    const current = words[index];
    const next = words[(index + 1) % words.length];

    gsap.to(current, { yPercent: -100, opacity: 0, duration: 0.5, ease: 'power2.in' });
    gsap.fromTo(next,
      { yPercent: 100, opacity: 0 },
      { yPercent: 0, opacity: 1, duration: 0.5, ease: 'power2.out', delay: 0.1 }
    );

    current.classList.remove('active');
    next.classList.add('active');
    index = (index + 1) % words.length;

    if (index === 0) {
      clearInterval(cycle);
      setTimeout(() => {
        gsap.to('#loader', {
          opacity: 0,
          duration: 0.6,
          onComplete: () => {
            document.getElementById('loader').style.display = 'none';
            onComplete?.();
          }
        });
      }, 500);
    }
  }, 600);
}
```

---

## PATTERN 3: Skeleton Shimmer (content loading)

```html
<div class="skeleton-card">
  <div class="skeleton skeleton-image"></div>
  <div class="skeleton skeleton-title"></div>
  <div class="skeleton skeleton-text"></div>
  <div class="skeleton skeleton-text short"></div>
</div>
```

```css
.skeleton {
  background: linear-gradient(
    90deg,
    var(--bg-sunken) 25%,
    var(--bg-elevated) 50%,
    var(--bg-sunken) 75%
  );
  background-size: 200% 100%;
  animation: shimmer 1.5s ease-in-out infinite;
  border-radius: 4px;
}

@keyframes shimmer {
  0%   { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}

.skeleton-image {
  width: 100%;
  height: 200px;
  margin-bottom: 1rem;
}

.skeleton-title {
  width: 60%;
  height: 1.5rem;
  margin-bottom: 0.8rem;
}

.skeleton-text {
  width: 100%;
  height: 0.9rem;
  margin-bottom: 0.5rem;
}

.skeleton-text.short { width: 40%; }
```

---

## PATTERN 4: Circular Progress Loader

```html
<div class="loader" id="loader">
  <svg class="progress-ring" viewBox="0 0 120 120">
    <circle class="progress-bg" cx="60" cy="60" r="54" />
    <circle class="progress-fill" cx="60" cy="60" r="54" />
  </svg>
  <span class="progress-text">0%</span>
</div>
```

```css
.progress-ring {
  width: 120px;
  height: 120px;
  transform: rotate(-90deg);
}

.progress-bg {
  fill: none;
  stroke: var(--border);
  stroke-width: 3;
}

.progress-fill {
  fill: none;
  stroke: var(--accent);
  stroke-width: 3;
  stroke-linecap: round;
  stroke-dasharray: 339.292; /* 2πr = 2 × π × 54 */
  stroke-dashoffset: 339.292;
  transition: stroke-dashoffset 0.1s linear;
}

.progress-text {
  position: absolute;
  font-size: 0.85rem;
  font-weight: 500;
  letter-spacing: 0.1em;
}
```

---

## CHOOSING THE RIGHT LOADER

| Archetype | Loader Pattern |
|---|---|
| Editorial, Portfolio (01, 06, 07, 11) | Pattern 1: Number counter |
| Agency, Creative (05, 10) | Pattern 2: Word reveal |
| SaaS, Product (12, 13, 22, 23) | Pattern 3: Skeleton (inline) |
| Cinematic, 3D (03, 08, 16, 18) | Pattern 1 or custom branded |
| Luxury (04, 15, 26) | Pattern 4: Circular progress |
| Brutalist (14) | No loader (intentional — instant load) |
| Kinetic (05, 09, 21) | Custom physics-based (see `physics-engines.md`) |

---

## RULES

1. **Loaders must be ≤3 seconds** — longer kills bounce rate
2. **Show real progress** when possible (not fake percentages)
3. **Brand the loader** — use your accent color, font, or a signature element
4. **Exit animation matters** — slide up, fade, or curtain reveal, never just `display: none`
5. **Skip on return visits** — use `sessionStorage` to skip loader for repeat visitors:
   ```javascript
   if (sessionStorage.getItem('visited')) {
     document.getElementById('loader')?.remove();
   } else {
     sessionStorage.setItem('visited', 'true');
     initLoader();
   }
   ```
6. **`prefers-reduced-motion`** — skip animated loaders, show content immediately
