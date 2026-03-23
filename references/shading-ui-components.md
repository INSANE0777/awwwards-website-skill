# Shading UI Components
## Adding custom GLSL to ReactBits & 21st.dev Foundations

Pre-built components often use "Generic" shaders. To win Awwwards, you must inject **Brand-Specific Noise and Distortion**.

---

## 1. THE "UNIFORM" OVERRIDE
Most registries (like 21st.dev) expose shader uniforms as props. Use GSAP to animate these for a custom feel.

```javascript
// A component from 21st.dev
import { LiquidShader } from '@/components/v21/LiquidShader';

export function CustomHero() {
  const shaderRef = useRef();

  useGSAP(() => {
    // Animate the "Turbulence" of the pre-built shader
    gsap.to(shaderRef.current, {
      uNoiseScale: 5.0,
      uSpeed: 0.2,
      duration: 2,
      scrollTrigger: {
        scrub: true
      }
    });
  });

  return <LiquidShader ref={shaderRef} color="#ff0000" />;
}
```

---

## 2. INJECTING NOISE (The "Grain" Pass)
Take a standard ReactBits component and wrap it in a custom "Grain" filter to give it a cinematic, 2026 brutalist texture.

```css
.noise-overlay {
  position: relative;
}
.noise-overlay::after {
  content: "";
  position: absolute;
  inset: 0;
  background: url('/noise.png'); /* 3% opacity grain */
  mix-blend-mode: overlay;
  pointer-events: none;
}
```

---

## 3. POST-PROCESSING SYNC
If a UI component is in a `Canvas`, ensure it shares the same `EffectComposer` as your main WebGL scene to maintain visual consistency (e.g., shared Chromatic Aberration).

---

## 4. THE "SOTY" DIFFERENCE
- **Registry Version:** Smooth, clean, sterile.
- **Awwward-ified Version:** Jittery, textured, reactive, and physically weighted.
