# Immersive i18n (Internationalization)
## Handling Global Typography in WebGL/GSAP

A world-class site must be accessible in multiple languages without losing its aesthetic soul.

---

## 1. THE LAYOUT "EXPANSION" RULE
German can be 30% longer than English. Japanese can be 20% shorter.

- **GSAP Fix:** Use `width: "auto"` and `xPercent` rather than fixed pixel widths for text containers.
- **SplitText:** Always re-split text after a language switch to ensure characters are updated correctly.

---

## 2. RTL (ARABIC/HEBREW) IN 3D
When the locale is RTL:
- **Mirror the 3D Scene:** `camera.position.x *= -1` to flip the visual balance.
- **Text Direction:** Ensure your shader-based text rendering supports right-to-left glyph ordering.

---

## 3. CJK TYPOGRAPHY (CHINESE/JAPANESE/KOREAN)
- **Problem:** MSDF (Multi-channel Signed Distance Fields) fonts for WebGL can be massive for CJK sets (thousands of characters).
- **Solution:** Use **Dynamic Atlas Generation** or fallback to high-res Canvas-textured text for CJK locales to keep the initial load low.

---

## 4. DYNAMIC CONTENT SWITCHING
```javascript
i18next.on('languageChanged', (lng) => {
  // 1. Update DOM
  // 2. Clear Three.js Text Geometries
  // 3. Re-render 3D text with new strings
  // 4. Reset ScrollTrigger to account for new page heights
});
```
