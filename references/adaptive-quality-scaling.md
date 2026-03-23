# Adaptive Quality Scaling (AQS)
## Maintaining the Vibe on Low-End Hardware

In 2026, usability scores are 25% of the Awwwards total. If your site is 5fps on a mid-range phone, you lose.

---

## 1. DYNAMIC FPS MONITORING
Don't just detect the device; detect the **Real-time Performance**.

```javascript
let frameCount = 0;
let lastTime = performance.now();

function checkPerformance() {
  const now = performance.now();
  const fps = frameCount / ((now - lastTime) / 1000);
  
  if (fps < 30) {
    // ⬇️ DOWNSCALE TIER
    GlobalState.qualityTier = Math.max(0, GlobalState.qualityTier - 1);
    updateEngineQuality();
  }
  
  frameCount = 0;
  lastTime = now;
}
```

---

## 2. THE QUALITY TIERS
- **High (2):** Native Resolution, 8x MSAA, Bloom, Motion Blur, 1M+ Particles.
- **Med (1):** 0.75x Resolution Scale, 2x MSAA, Bloom only, 100k Particles.
- **Low (0):** 0.5x Resolution Scale, No MSAA, No Post-processing, 10k Particles (Instanced).

---

## 3. SHADER PERMUTATIONS
Use `#define` in your GLSL to toggle expensive features based on the `qualityTier`.

```glsl
uniform int uQuality;

void main() {
  vec3 color = baseTexture;
  
  #ifdef HIGH_QUALITY
  if(uQuality > 1) {
    color += calculateExpensiveRaymarching();
  }
  #endif
  
  gl_FragColor = vec4(color, 1.0);
}
```

---

## 5. MOBILE MEMORY MANAGEMENT
In 2026, Safari on iOS is the primary bottleneck.
- **Dispose Protocol**: Always `geometry.dispose()` and `texture.dispose()` when components unmount.
- **Tiering Manifesto**:
  - **Tier 1 (High-end PC):** 4K Textures, 60fps, Full Post-processing.
  - **Tier 2 (Modern Mobile):** 1K Textures, 30fps (locked), Bloom only.
  - **Tier 3 (Potato Mode):** 2D fallback, No Post-processing, Reduced Motion.
