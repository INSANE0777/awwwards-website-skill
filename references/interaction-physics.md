# Interaction Physics & Magnetic Elements
## Creating "Weight" and "Attraction" in the UI

In 2026, buttons shouldn't just hover; they should have **mass**.

---

## 1. THE MAGNETIC BUTTON
The button "pulls" the cursor toward its center when within a 100px radius.

```javascript
const magnets = document.querySelectorAll(".magnetic");

magnets.forEach((el) => {
  el.addEventListener("mousemove", (e) => {
    const rect = el.getBoundingClientRect();
    const x = e.clientX - (rect.left + rect.width / 2);
    const y = e.clientY - (rect.top + rect.height / 2);
    
    // Move the element slightly toward the mouse
    gsap.to(el, {
      x: x * 0.3,
      y: y * 0.3,
      duration: 0.4,
      ease: "power2.out"
    });
  });
  
  el.addEventListener("mouseleave", () => {
    gsap.to(el, { x: 0, y: 0, duration: 0.7, ease: "elastic.out(1, 0.3)" });
  });
});
```

---

## 2. INERTIA SCROLLING (Mental Model)
If you aren't using `Lenis`, you must simulate inertia.
- **Concept:** Every scroll event adds to a `velocity` variable. 
- **The Frame Loop:** Every frame, `currentY += velocity` and `velocity *= 0.95` (friction).

---

## 3. FRICTION-DRIVEN "DRAG"
For horizontal image carousels.

```javascript
const drag = { velocity: 0, currentX: 0 };

function update() {
  drag.currentX += drag.velocity;
  drag.velocity *= 0.92; // Friction
  
  gsap.set(".gallery", { x: drag.currentX });
  requestAnimationFrame(update);
}
```

---

## 4. THE "REBOUND" EFFECT
When reaching the top or bottom of a list, the list should "overshoot" and snap back.

```javascript
if (atBottom) {
  gsap.to(".list", { y: -50, duration: 0.2, yoyo: true, repeat: 1 });
}
```

---

## 5. WHY THIS WINS AWWWARDS:
- **Tactile Quality**: The site feels like a physical object.
- **User Satisfaction**: Magnetic buttons make the interface feel helpful and clever.
