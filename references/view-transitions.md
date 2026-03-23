# View Transitions API
## Native browser page transitions for SPAs and MPAs

The View Transitions API provides smooth, animated transitions between DOM states — no GSAP required for basic transitions.

> **Support:** Chrome 111+, Edge 111+. Safari 18+. Firefox: behind flag.

---

## SPA VIEW TRANSITION (same-document)

```javascript
function navigateTo(newContent) {
  if (!document.startViewTransition) {
    // Fallback: just swap content
    document.getElementById('content').innerHTML = newContent;
    return;
  }

  document.startViewTransition(() => {
    document.getElementById('content').innerHTML = newContent;
  });
}
```

```css
/* Default crossfade works automatically. Customize: */
::view-transition-old(root) {
  animation: fadeOut 0.3s ease-in both;
}
::view-transition-new(root) {
  animation: fadeIn 0.3s ease-out both;
}
@keyframes fadeOut { to { opacity: 0; } }
@keyframes fadeIn  { from { opacity: 0; } }
```

---

## MPA VIEW TRANSITION (cross-document)

```css
/* Add to BOTH pages */
@view-transition { navigation: auto; }
```

```html
<!-- Page 1: list page -->
<a href="/detail.html">
  <img src="photo.jpg" style="view-transition-name: hero-image;" />
</a>

<!-- Page 2: detail page -->
<img src="photo.jpg" style="view-transition-name: hero-image;" />
```
The browser automatically animates the `hero-image` between pages!

---

## NAMED TRANSITIONS (element-level)

```css
.hero-title { view-transition-name: hero-title; }
.hero-image { view-transition-name: hero-image; }
.card-1     { view-transition-name: card-1; }
```

```css
/* Customize individual element transitions */
::view-transition-old(hero-image) {
  animation: shrinkOut 0.4s ease-in both;
}
::view-transition-new(hero-image) {
  animation: growIn 0.4s ease-out both;
}
@keyframes shrinkOut { to { transform: scale(0.8); opacity: 0; } }
@keyframes growIn { from { transform: scale(1.2); opacity: 0; } }
```

---

## SLIDE TRANSITION

```css
::view-transition-old(root) {
  animation: slideOut 0.4s cubic-bezier(0.77, 0, 0.175, 1) both;
}
::view-transition-new(root) {
  animation: slideIn 0.4s cubic-bezier(0.77, 0, 0.175, 1) both;
}
@keyframes slideOut { to { transform: translateX(-100%); } }
@keyframes slideIn { from { transform: translateX(100%); } }
```

---

## SCALE + CROSSFADE

```css
::view-transition-old(root) {
  animation: 0.3s ease both scaleDown;
}
::view-transition-new(root) {
  animation: 0.3s ease both scaleUp;
}
@keyframes scaleDown { to { transform: scale(0.9); opacity: 0; } }
@keyframes scaleUp { from { transform: scale(1.1); opacity: 0; } }
```

---

## WITH SPA ROUTER

```javascript
// Works with hash-based routing
window.addEventListener('hashchange', () => {
  if (document.startViewTransition) {
    document.startViewTransition(() => renderCurrentRoute());
  } else {
    renderCurrentRoute();
  }
});
```

---

## RULES
1. **`view-transition-name` must be unique** on the page at transition time
2. **Always provide fallback** — check `document.startViewTransition` exists
3. **Keep transitions <500ms** — longer feels laggy
4. **Named transitions** create shared-element animations (like Android/iOS)
5. **Combine with GSAP** for complex sequences the API can't handle alone
