# Creative Accessibility (WCAG 2.1 AA)
## Making "Awe" accessible to everyone (Mandatory for 2025 Developer Award)

With the **European Accessibility Act (EAA) 2025**, accessibility is no longer a "nice-to-have". Judges now penalize sites that break basic usability.

---

## 1. THE "INCLUSIVE AWE" PROTOCOL

### Rule 1: Motion Management
Always honor `prefers-reduced-motion`.
```javascript
const mql = window.matchMedia('(prefers-reduced-motion: reduce)');
if (mql.matches) {
  // Disable heavy parallax, GSAP scrubs, and WebGL distortions
  gsap.ticker.fps(30); 
  // Simplify entry animations
}
```

### Rule 2: Keyboard Navigable Immersion
Ensure users can explore your 3D or panel-based site without a mouse.
- **Tab Order:** Logical flow through panels.
- **Focus States:** Custom, high-contrast focus rings (even on 3D objects).
- **Enter/Space:** Triggers the same "Signature Moment" as a click.

---

## 3. COLOR & CONTRAST IN "DARK MODE"
Dark themes often fail accessibility on text/background contrast.
- **Minimum:** 4.5:1 for body text.
- **Tip:** Use a `high-contrast-mode` toggle that swaps your `#080808` for a truer black/white if detected.

---

## 4. SCREEN READER HEROES (Aria-Labels)
Don't use `aria-hidden="true"` on everything just because it's "decorative".
- **Semantic Headings:** Even if they are split by SplitText, ensure they are readable as a single `<h1>`.
- **Image Alts:** "Abstract blue glass bubbles reflecting light" is better than "image 1".

---

## 5. ACCESSIBLE CUSTOM CURSORS
Custom cursors often hide the system cursor, which can be disorienting.
- **Mandate:** Always allow the user to toggle back to the system cursor.
- **Mandate:** Never hide the cursor if the user has specific accessibility settings enabled.

---

## 6. THE ACCESSIBILITY AUDIT (The Awwwards Way)
1. **Lighthouse Score:** Target 95+.
2. **Keyboard-Only Test:** Can you navigate the entire "Work" section using only `Tab` and `Enter`?
3. **Screen Reader Test:** Close your eyes. Does the site still make sense?
