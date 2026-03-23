# Menu & Navigation Patterns
## Awwwards-level side menus, curved menus, sliding stairs, and creative navigation

---

## 1. AWWWARDS SIDE MENU (fullscreen overlay)

```html
<button class="menu-trigger" id="menuTrigger">
  <span class="trigger-bar"></span>
  <span class="trigger-bar"></span>
</button>

<nav class="side-menu" id="sideMenu">
  <div class="menu-bg"></div>
  <div class="menu-content">
    <div class="menu-links">
      <a href="#" class="menu-link" data-index="01">Home</a>
      <a href="#" class="menu-link" data-index="02">Work</a>
      <a href="#" class="menu-link" data-index="03">About</a>
      <a href="#" class="menu-link" data-index="04">Contact</a>
    </div>
    <div class="menu-footer">
      <span class="mono">hello@studio.com</span>
      <div class="menu-socials">
        <a href="#">IG</a>
        <a href="#">TW</a>
        <a href="#">LI</a>
      </div>
    </div>
  </div>
</nav>
```

```css
.menu-trigger {
  position: fixed; top: 2rem; right: 2rem; z-index: 1002;
  width: 48px; height: 48px;
  display: flex; flex-direction: column; justify-content: center; align-items: center;
  gap: 6px; background: none; border: none; cursor: pointer;
  mix-blend-mode: difference;
}
.trigger-bar {
  width: 32px; height: 2px; background: white;
  transition: transform 0.4s ease, gap 0.4s ease;
}
.menu-trigger.open .trigger-bar:first-child { transform: rotate(45deg) translateY(4px); }
.menu-trigger.open .trigger-bar:last-child { transform: rotate(-45deg) translateY(-4px); }

.side-menu {
  position: fixed; inset: 0; z-index: 1001;
  pointer-events: none;
}
.side-menu.open { pointer-events: all; }

.menu-bg {
  position: absolute; inset: 0;
  background: var(--ink);
  clip-path: circle(0% at calc(100% - 3rem) 3rem);
  transition: clip-path 0.8s cubic-bezier(0.77, 0, 0.175, 1);
}
.side-menu.open .menu-bg {
  clip-path: circle(150% at calc(100% - 3rem) 3rem);
}

.menu-content {
  position: relative; z-index: 1;
  height: 100%; display: flex; flex-direction: column;
  justify-content: center; padding: 4rem;
}

.menu-link {
  display: block; font-size: clamp(3rem, 6vw, 5rem);
  font-weight: 700; color: var(--bg);
  text-decoration: none; line-height: 1.2;
  opacity: 0; transform: translateY(40px);
  transition: opacity 0.1s, transform 0.1s;
}
.menu-link::before {
  content: attr(data-index);
  font-size: 0.7rem; letter-spacing: 0.2em;
  vertical-align: super; margin-right: 1rem;
  opacity: 0.4;
}
.menu-link:hover { opacity: 0.5; }
```

```javascript
function initSideMenu() {
  const trigger = document.getElementById('menuTrigger');
  const menu = document.getElementById('sideMenu');
  const links = menu.querySelectorAll('.menu-link');

  trigger.addEventListener('click', () => {
    const isOpen = trigger.classList.toggle('open');
    menu.classList.toggle('open');
    document.body.style.overflow = isOpen ? 'hidden' : '';

    if (isOpen) {
      gsap.fromTo(links,
        { opacity: 0, y: 40 },
        { opacity: 1, y: 0, stagger: 0.08, duration: 0.6, delay: 0.3, ease: 'power3.out' }
      );
    } else {
      gsap.to(links, { opacity: 0, y: -20, stagger: 0.03, duration: 0.3 });
    }
  });

  // Close on link click
  links.forEach(link => {
    link.addEventListener('click', () => {
      trigger.click();
    });
  });
}
```

---

## 2. CURVED MENU (arc-shaped navigation)

```css
.curved-menu {
  position: fixed; inset: 0; z-index: 1001;
  display: flex; flex-direction: column;
  align-items: center; justify-content: center;
  background: var(--ink);
  clip-path: circle(0% at 50% 50%);
  transition: clip-path 0.8s cubic-bezier(0.77, 0, 0.175, 1);
}
.curved-menu.open {
  clip-path: circle(100% at 50% 50%);
}

.curved-link {
  font-size: clamp(2rem, 5vw, 4rem);
  color: var(--bg);
  text-decoration: none;
  padding: 0.5rem 2rem;
  display: block;
  position: relative;
}

/* Curved underline */
.curved-link::after {
  content: ''; position: absolute;
  bottom: 0; left: 50%;
  width: 0; height: 3px;
  background: var(--accent);
  border-radius: 2px;
  transform: translateX(-50%);
  transition: width 0.4s cubic-bezier(0.77, 0, 0.175, 1);
}
.curved-link:hover::after { width: 80%; }
```

```javascript
function initCurvedMenu() {
  const links = document.querySelectorAll('.curved-link');

  // Arrange links in a subtle arc
  links.forEach((link, i) => {
    const total = links.length;
    const center = (total - 1) / 2;
    const offset = (i - center) * 3; // degrees from center

    gsap.set(link, {
      rotation: offset,
      transformOrigin: 'center 500px' // large radius = subtle curve
    });
  });
}
```

---

## 3. SLIDING STAIRS MENU

```html
<nav class="stairs-menu" id="stairsMenu">
  <a href="#" class="stair-link"><span class="stair-bg"></span><span class="stair-text">Home</span></a>
  <a href="#" class="stair-link"><span class="stair-bg"></span><span class="stair-text">Work</span></a>
  <a href="#" class="stair-link"><span class="stair-bg"></span><span class="stair-text">About</span></a>
  <a href="#" class="stair-link"><span class="stair-bg"></span><span class="stair-text">Contact</span></a>
</nav>
```

```css
.stairs-menu {
  position: fixed; inset: 0; z-index: 1001;
  display: flex; flex-direction: column;
  pointer-events: none;
}
.stairs-menu.open { pointer-events: all; }

.stair-link {
  flex: 1; position: relative;
  display: flex; align-items: center;
  padding-left: 4rem; text-decoration: none;
  overflow: hidden;
}

.stair-bg {
  position: absolute; inset: 0;
  background: var(--ink);
  transform: translateX(-100%);
}

.stair-text {
  position: relative; z-index: 1;
  font-size: clamp(2rem, 4vw, 3.5rem);
  font-weight: 700; color: var(--bg);
  opacity: 0; transform: translateY(20px);
}
```

```javascript
function initStairsMenu() {
  const trigger = document.getElementById('menuTrigger');
  const menu = document.getElementById('stairsMenu');
  const bgs = menu.querySelectorAll('.stair-bg');
  const texts = menu.querySelectorAll('.stair-text');

  trigger.addEventListener('click', () => {
    const isOpen = trigger.classList.toggle('open');
    menu.classList.toggle('open');

    if (isOpen) {
      // Stagger stair backgrounds from left
      gsap.to(bgs, {
        x: 0,
        duration: 0.5,
        stagger: 0.08,
        ease: 'power4.out'
      });
      gsap.to(texts, {
        opacity: 1, y: 0,
        duration: 0.4,
        stagger: 0.08,
        delay: 0.3,
        ease: 'power3.out'
      });
    } else {
      gsap.to(texts, { opacity: 0, y: -10, stagger: 0.03, duration: 0.2 });
      gsap.to(bgs, {
        x: '100%', duration: 0.5,
        stagger: 0.05, delay: 0.1,
        ease: 'power4.in'
      });
    }
  });
}
```

---

## 4. NAVIGATION MENU (sliding panel with preview)

```css
.nav-panel {
  position: fixed; top: 0; right: -50vw;
  width: 50vw; height: 100vh;
  background: var(--bg-elevated);
  z-index: 1001;
  transition: right 0.6s cubic-bezier(0.77, 0, 0.175, 1);
  display: flex;
}
.nav-panel.open { right: 0; }

.nav-panel-links {
  flex: 1; display: flex; flex-direction: column;
  justify-content: center; padding: 4rem;
}
.nav-panel-preview {
  flex: 1; position: relative;
  overflow: hidden;
}
.nav-panel-preview img {
  position: absolute; inset: 0;
  width: 100%; height: 100%; object-fit: cover;
  opacity: 0; transition: opacity 0.4s ease;
}
.nav-panel-preview img.active { opacity: 1; }
```

```javascript
function initNavPanel() {
  const links = document.querySelectorAll('.nav-panel .nav-link');
  const images = document.querySelectorAll('.nav-panel-preview img');

  links.forEach((link, i) => {
    link.addEventListener('mouseenter', () => {
      images.forEach(img => img.classList.remove('active'));
      images[i]?.classList.add('active');
    });
  });
}
```

---

## CHOOSING THE RIGHT MENU

| Site type | Menu pattern |
|---|---|
| Portfolio / Agency | Side Menu (fullscreen overlay) |
| Editorial / Magazine | Navigation Panel (half-screen with preview) |
| Kinetic / Creative | Sliding Stairs |
| Luxury / Fashion | Curved Menu |
| SaaS / Product | Standard top nav (no fancy menu needed) |
| Mobile (all types) | Side Menu or Stairs |

---

## BEST PRACTICES

1. **`mix-blend-mode: difference`** on the trigger — makes it visible on any background
2. **Lock body scroll** when menu is open: `body.style.overflow = 'hidden'`
3. **Stagger link reveals** — never show all links simultaneously
4. **Close on link click** — always wire up auto-close after navigation
5. **ESC key** should close the menu:
   ```javascript
   document.addEventListener('keydown', e => {
     if (e.key === 'Escape' && menu.classList.contains('open')) trigger.click();
   });
   ```
6. **Touch-friendly** — ensure trigger is ≥44px and menu links have ample padding
7. **Disable scroll-based animations** while menu is open to prevent conflicts
