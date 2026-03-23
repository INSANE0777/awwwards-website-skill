# Footer Templates for Awwwards-Level Sites
## Premium footer designs, animated marquees, newsletter CTAs, and social grids

Every site needs a footer. A generic one kills the score; a designed one elevates the whole experience.

---

## TEMPLATE 1: Editorial / Portfolio Footer

The "quiet authority" footer — minimal, typographic, no noise.

```html
<footer class="footer-editorial">
  <div class="footer-top">
    <div class="footer-col">
      <h3 class="footer-heading">Navigation</h3>
      <a href="#work" class="footer-link">Work</a>
      <a href="#about" class="footer-link">About</a>
      <a href="#contact" class="footer-link">Contact</a>
    </div>
    <div class="footer-col">
      <h3 class="footer-heading">Social</h3>
      <a href="#" class="footer-link" target="_blank">Instagram</a>
      <a href="#" class="footer-link" target="_blank">LinkedIn</a>
      <a href="#" class="footer-link" target="_blank">Dribbble</a>
    </div>
    <div class="footer-col footer-col--wide">
      <h3 class="footer-heading">Say Hello</h3>
      <a href="mailto:hello@example.com" class="footer-email">hello@example.com</a>
    </div>
  </div>
  <div class="footer-bottom">
    <span class="footer-copy">&copy; 2025 Brand Name</span>
    <span class="footer-credit">Designed with intention</span>
  </div>
</footer>
```

```css
.footer-editorial {
  padding: var(--space-section) var(--space-gutter);
  border-top: 1px solid var(--border);
}

.footer-top {
  display: grid;
  grid-template-columns: 1fr 1fr 2fr;
  gap: 3rem;
  margin-bottom: 4rem;
}

.footer-heading {
  font-family: inherit;
  font-size: 0.65rem;
  font-weight: 500;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--ink-muted);
  margin-bottom: 1.5rem;
}

.footer-link {
  display: block;
  font-size: 1rem;
  color: var(--ink);
  text-decoration: none;
  padding: 0.4rem 0;
  transition: opacity 0.3s ease;
}
.footer-link:hover { opacity: 0.5; }

.footer-email {
  font-size: clamp(1.5rem, 3vw, 2.5rem);
  font-weight: 300;
  color: var(--ink);
  text-decoration: none;
  border-bottom: 1px solid var(--border);
  padding-bottom: 0.3rem;
  transition: border-color 0.3s ease;
}
.footer-email:hover { border-color: var(--accent); }

.footer-bottom {
  display: flex;
  justify-content: space-between;
  font-size: 0.7rem;
  color: var(--ink-muted);
  letter-spacing: 0.1em;
}

@media (max-width: 768px) {
  .footer-top { grid-template-columns: 1fr; gap: 2rem; }
}
```

---

## TEMPLATE 2: Big CTA Footer

The "invitation" footer — makes contact feel like an event.

```html
<footer class="footer-cta">
  <div class="footer-cta-block">
    <p class="footer-eyebrow">Ready to start?</p>
    <h2 class="footer-headline">Let's build<br>something</h2>
    <a href="mailto:hello@example.com" class="footer-big-btn" data-magnetic>
      <span>Get in touch</span>
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M7 17L17 7M17 7H7M17 7V17"/>
      </svg>
    </a>
  </div>
  <div class="footer-info">
    <div class="footer-col">
      <span class="mono">Local Time</span>
      <span class="footer-clock" id="localTime">--:--</span>
    </div>
    <div class="footer-col">
      <span class="mono">Location</span>
      <span>San Diego, CA</span>
    </div>
    <div class="footer-col">
      <span class="mono">&copy; 2025</span>
      <span>All rights reserved</span>
    </div>
  </div>
</footer>
```

```css
.footer-cta {
  padding: var(--space-section) var(--space-gutter);
  text-align: center;
}

.footer-cta-block { margin-bottom: 6rem; }

.footer-eyebrow {
  font-size: 0.7rem;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: var(--ink-muted);
  margin-bottom: 1rem;
}

.footer-headline {
  font-size: clamp(3rem, 8vw, 8rem);
  line-height: 0.9;
  margin-bottom: 3rem;
}

.footer-big-btn {
  display: inline-flex;
  align-items: center;
  gap: 1rem;
  padding: 1.2rem 3rem;
  border: 1px solid var(--ink);
  color: var(--ink);
  text-decoration: none;
  font-size: 1rem;
  letter-spacing: 0.05em;
  transition: background 0.4s ease, color 0.4s ease;
}
.footer-big-btn:hover {
  background: var(--ink);
  color: var(--bg);
}
.footer-big-btn svg {
  transition: transform 0.4s cubic-bezier(0.2, 0.8, 0.2, 1);
}
.footer-big-btn:hover svg { transform: translate(4px, -4px); }

.footer-info {
  display: flex;
  justify-content: space-between;
  padding-top: 2rem;
  border-top: 1px solid var(--border);
  font-size: 0.85rem;
}

.footer-info .mono {
  display: block;
  font-size: 0.6rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--ink-muted);
  margin-bottom: 0.5rem;
}

@media (max-width: 768px) {
  .footer-info { flex-direction: column; gap: 1.5rem; }
}
```

```javascript
// Live clock
function initFooterClock() {
  const el = document.getElementById('localTime');
  if (!el) return;
  const update = () => {
    el.textContent = new Date().toLocaleTimeString('en-US', {
      hour: '2-digit', minute: '2-digit', hour12: false
    });
  };
  update();
  setInterval(update, 60000);
}
```

---

## TEMPLATE 3: Marquee Footer

The "kinetic" footer — text scrolls infinitely for energy and texture.

```html
<footer class="footer-marquee">
  <div class="marquee-band" aria-hidden="true">
    <div class="marquee-track">
      <span>LET'S WORK TOGETHER — </span>
      <span>LET'S WORK TOGETHER — </span>
      <span>LET'S WORK TOGETHER — </span>
      <span>LET'S WORK TOGETHER — </span>
    </div>
  </div>
  <div class="footer-grid">
    <div>
      <a href="mailto:hello@example.com" class="footer-email">hello@example.com</a>
    </div>
    <div class="footer-social-row">
      <a href="#" class="social-pill">IG</a>
      <a href="#" class="social-pill">TW</a>
      <a href="#" class="social-pill">LI</a>
      <a href="#" class="social-pill">GH</a>
    </div>
    <div class="footer-copy">&copy; 2025 Brand</div>
  </div>
</footer>
```

```css
.footer-marquee { padding-bottom: 2rem; }

.marquee-band {
  overflow: hidden;
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
  padding: 1.5rem 0;
  margin-bottom: 3rem;
}

.marquee-track {
  display: inline-flex;
  white-space: nowrap;
  animation: marquee 15s linear infinite;
  font-size: clamp(2rem, 5vw, 5rem);
  font-weight: 700;
  letter-spacing: -0.02em;
}

.marquee-track span { flex-shrink: 0; }

@keyframes marquee {
  0%   { transform: translateX(0); }
  100% { transform: translateX(-50%); }
}

/* Hover reversal */
.marquee-band:hover .marquee-track {
  animation-direction: reverse;
}

.footer-grid {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  padding: 0 var(--space-gutter);
}

.social-pill {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  border: 1px solid var(--border);
  color: var(--ink);
  text-decoration: none;
  font-size: 0.7rem;
  letter-spacing: 0.1em;
  transition: background 0.3s ease, color 0.3s ease;
}
.social-pill:hover {
  background: var(--ink);
  color: var(--bg);
}

.footer-social-row { display: flex; gap: 0.5rem; }

@media (max-width: 768px) {
  .footer-grid { flex-direction: column; align-items: flex-start; gap: 2rem; }
}
```

---

## TEMPLATE 4: Dark Immersive Footer

For sports, 3D, and cinematic themes.

```html
<footer class="footer-dark">
  <div class="footer-dark-inner">
    <h2 class="footer-cta-text" data-reveal="bottom">
      THE <span class="accent">NEXT CHAPTER</span><br>STARTS HERE.
    </h2>
    <a href="#contact" class="footer-action" data-magnetic>
      <span>Contact</span>
    </a>
  </div>
  <div class="footer-dark-bar">
    <span>&copy; 2025</span>
    <div class="footer-links-row">
      <a href="#">Privacy</a>
      <a href="#">Terms</a>
    </div>
    <span>↑ Back to top</span>
  </div>
</footer>
```

```css
.footer-dark {
  background: var(--bg-sunken, #050505);
  color: var(--ink, #FBFBFB);
  padding: 8rem var(--space-gutter) 2rem;
}

.footer-dark-inner { text-align: center; margin-bottom: 6rem; }

.footer-cta-text {
  font-size: clamp(2.5rem, 6vw, 6rem);
  line-height: 1;
  margin-bottom: 3rem;
}
.footer-cta-text .accent { color: var(--accent); }

.footer-action {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 120px; height: 120px;
  border-radius: 50%;
  border: 1px solid rgba(255,255,255,0.3);
  color: white;
  text-decoration: none;
  font-size: 0.85rem;
  letter-spacing: 0.1em;
  transition: background 0.4s, transform 0.4s;
}
.footer-action:hover {
  background: var(--accent);
  border-color: var(--accent);
  transform: scale(1.1);
}

.footer-dark-bar {
  display: flex;
  justify-content: space-between;
  font-size: 0.7rem;
  opacity: 0.5;
  padding-top: 2rem;
  border-top: 1px solid rgba(255,255,255,0.1);
}
.footer-dark-bar a {
  color: inherit;
  text-decoration: none;
  margin-left: 1.5rem;
}
.footer-dark-bar a:hover { opacity: 1; }

@media (max-width: 768px) {
  .footer-dark-bar { flex-direction: column; gap: 1rem; text-align: center; }
}
```

---

## CHOOSING THE RIGHT FOOTER

| Archetype | Best Template |
|---|---|
| Editorial, Portfolio, Archival (01, 06, 07, 11) | Template 1: Editorial |
| Agency, SaaS, Product (05, 12, 13, 22, 23) | Template 2: Big CTA |
| Kinetic, Playful, Brand (05, 09, 10, 20, 21) | Template 3: Marquee |
| Sports, Cinematic, 3D (03, 08, 16, 18, 25) | Template 4: Dark Immersive |
| Luxury, Fashion (04, 15, 26) | Template 1 or 4 (dark) |
| Brutalist (14) | Template 3 (stripped to text only) |

---

## FOOTER BEST PRACTICES

1. **Always include**: copyright, navigation links, contact method
2. **Real local time** shows attention to detail (Template 2)
3. **Marquee must duplicate** content 4x to loop seamlessly
4. **Touch targets** ≥ 44px for social links
5. **Back to top** on long-scrolling sites — use `lenis.scrollTo(0)`
6. **Dark footers** on light sites create visual closure
7. **Never center-align everything** — editorial footers need grid tension
