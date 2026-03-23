# Advanced GSAP Recipes (2026)
## Pro-Tier Text & Layout Distortions

Beyond simple `from` and `to` tweens, these recipes create the "Signature Feel" of elite sites.

---

## 1. THE "EXPLODING" TEXT (SplitText + Physics2D)
Requires GSAP Club membership (or the 2025 Free Bundle).

```javascript
const split = new SplitText(".title", { type: "chars" });

gsap.to(split.chars, {
  duration: 2,
  physics2D: {
    velocity: "random(200, 400)",
    angle: "random(250, 290)",
    gravity: 500
  },
  stagger: 0.02
});
```

---

## 2. SMOOTH CLIP-PATH REVEAL
The "Curtain" effect used for high-end fashion portfolios.

```javascript
gsap.fromTo(".image-container", 
  { clipPath: "polygon(0 0, 0 0, 0 100%, 0% 100%)" },
  { 
    clipPath: "polygon(0 0, 100% 0, 100% 100%, 0 100%)",
    duration: 1.5,
    ease: "expo.inOut" 
  }
);
```

---

## 3. TEXT SCRAMBLE HOVER
Makes text look like it's "decoding".

```javascript
document.querySelector(".scramble").addEventListener("mouseenter", () => {
  gsap.to(".scramble", {
    duration: 1,
    scrambleText: {
      text: "DECODED VERSION",
      chars: "XO-!@#$%^&*()",
      revealDelay: 0.5,
      speed: 0.3
    }
  });
});
```

---

## 4. ELASTIC MOUSE-FOLLOW (The "Rubber" Container)
A container that deforms slightly toward the cursor.

```javascript
window.addEventListener("mousemove", (e) => {
  const x = e.clientX - window.innerWidth / 2;
  const y = e.clientY - window.innerHeight / 2;
  
  gsap.to(".elastic-box", {
    x: x * 0.05,
    y: y * 0.05,
    skewX: x * 0.01,
    rotate: y * 0.01,
    duration: 0.8,
    ease: "power2.out"
  });
});
```
