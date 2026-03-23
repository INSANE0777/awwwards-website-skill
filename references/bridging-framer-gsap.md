# Bridging Framer Motion & GSAP
## Using the best of both worlds in a single Awwwards site

Most modern UI kits (Magic UI, Aceternity) use **Framer Motion**. However, SOTY sites often require **GSAP** for its timeline precision and performance.

---

## 1. WHEN TO USE WHICH
- **Framer Motion:** Simple entry/exit transitions, layout-id based morphing, and "Shadcn" style components.
- **GSAP:** Complex scroll-driven scenes, multi-step timelines, WebGL uniform updates, and SVG morphing.

---

## 2. THE "LIFECYCLE" CONFLICT
Avoid animating the same property with both libraries. It causes "Flash of Unstyled Motion" (FOUM).

**Safe Pattern:**
- Use Framer Motion for the **Parent Container** entry (`initial`, `animate`).
- Use GSAP for the **Inner Elements** and interaction loops.

---

## 3. SYNCING GSAP WITH FRAMER STATE
You can use a React `ref` to let GSAP "take over" a Framer-initialized element.

```javascript
const boxRef = useRef(null);

// Framer handles the initial fade-in
<motion.div 
  ref={boxRef}
  initial={{ opacity: 0 }}
  animate={{ opacity: 1 }}
>
  {/* GSAP handles the persistent interaction */}
  <button onMouseMove={(e) => {
    gsap.to(boxRef.current, { x: e.clientX * 0.1 });
  }}>
    Magnetic
  </button>
</motion.div>
```

---

## 4. ANIMATING "MOTION VALUES"
Framer's `useMotionValue` can be tweened via GSAP for ultra-smooth hybrid animations.

```javascript
const opacity = useMotionValue(0);

useGSAP(() => {
  gsap.to(opacity, { 
    set: 1, // Custom setter
    duration: 1, 
    scrollTrigger: ".trigger" 
  });
});
```
