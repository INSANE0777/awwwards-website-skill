# WebGL Post-Processing (2026)
## Cinematic Polish: Chromatic Aberration & Motion Blur

A 2D site with WebGL post-processing is the "secret sauce" for the Awwwards Honorable Mention bracket.

---

## 1. CHROMATIC ABERRATION (Color Fringing)
Simulates a high-end lens distortion where colors split at the edges.

**Vertex Shader:**
```glsl
varying vec2 vUv;
void main() {
  vUv = uv;
  gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
}
```

**Fragment Shader Snippet:**
```glsl
uniform sampler2D tDiffuse;
uniform float uOffset;
varying vec2 vUv;

void main() {
  // Offset the red and blue channels based on the uOffset
  float r = texture2D(tDiffuse, vUv + uOffset).r;
  float g = texture2D(tDiffuse, vUv).g;
  float b = texture2D(tDiffuse, vUv - uOffset).b;
  
  gl_FragColor = vec4(r, g, b, 1.0);
}
```

---

## 2. MOTION BLUR (Gaussian Blur via Velocity)
Applying blur only when the "Camera" (scroll) is moving.

```javascript
// On scroll
const scrollSpeed = currentScrollVelocity();
composer.pass.uniforms.uBlurStrength.value = scrollSpeed * 0.001;
```

---

## 3. THE "LO-FI" PIXELATION DISSOLVE
The modern "Brutalist" transition.
1. Render current page to Texture A.
2. Render next page to Texture B.
3. Use a shader to "Pixelate" Texture A until it's 1x1, then reveal Texture B from a pixelated state.

---

## 4. PERFORMANCE MANDATE:
- Post-processing is expensive. Always use `EffectComposer` from Three.js and **downscale the render target** to 0.5x if FPS drops below 55 (Auto-optimization).
