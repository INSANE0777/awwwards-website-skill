# Advanced Shader Blueprints (2025)
## High-end GLSL for SOTY-level visual drama

These blueprints focus on performance-optimized, high-fidelity effects that differentiate a "standard" site from a winner.

---

## 1. FBO (Frame Buffer Object) PARTICLES
Instead of moving 10,000 DOM elements (slow), we move 1,000,000 particles on the GPU (fast).

### The Concept:
1. Store particle positions in a texture.
2. Update positions in a "Simulation Shader".
3. Render particles in a "Display Shader" by reading the position texture.

```javascript
// Quick setup for FBO Particles using Three.js
// This creates a flow-field effect reactive to mouse
const simulationShader = `
  uniform sampler2D uPos;
  uniform vec2 uMouse;
  uniform float uTime;
  varying vec2 vUv;

  void main() {
    vec3 pos = texture2D(uPos, vUv).rgb;
    // Add curl noise or simple gravity toward mouse
    vec3 dir = vec3(uMouse, 0.0) - pos;
    pos += normalize(dir) * 0.01;
    gl_FragColor = vec4(pos, 1.0);
  }
`;
```

---

## 2. RAYMARCHING SPHERES (The "Metaball" Look)
Create organic, liquid-like shapes that merge and split without complex 3D models.

### Fragment Shader Snippet:
```glsl
float sdSphere(vec3 p, float s) { return length(p) - s; }

float map(vec3 p) {
  float d1 = sdSphere(p - vec3(sin(uTime), 0, 0), 1.0);
  float d2 = sdSphere(p - vec3(cos(uTime), 0, 0), 0.8);
  return smoothMin(d1, d2, 0.5); // The "Secret Sauce" for merging
}
```

---

## 3. NOISE-FIELD DISPLACEMENT (Cinematic Glass/Water)
Distort images or background text using a dynamic noise texture instead of static blur.

### Technique:
- Use **Simplex Noise** or **Worley Noise** to offset UV coordinates before sampling a texture.
- Animate the noise frequency over scroll to create "shimmer" or "shatter" transitions.

---

## 4. CHROMATIC ABERRATION ON MOTION
Subtle color splitting that only occurs during fast transitions or hovers.

```glsl
void main() {
  float shift = uVelocity * 0.05;
  float r = texture2D(uTexture, uv + vec2(shift, 0.0)).r;
  float g = texture2D(uTexture, uv).g;
  float b = texture2D(uTexture, uv - vec2(shift, 0.0)).b;
  gl_FragColor = vec4(r, g, b, 1.0);
}
```

---

## 5. VOLUMETRIC LIGHT (God Rays)
Add "Atmosphere" to simple dark scenes.

### Implementation:
1. Render a light source (circle) to a small off-screen buffer.
2. Apply a radial blur in the fragment shader.
3. Blend the result additively over your scene.

---

## 6. GPU-BASED TEXTURE TRANSITIONS
Cross-fading images using "Wind", "Glitch", or "Doorway" patterns.

- [See Gl-Transitions.com](https://gl-transitions.com/) for the math.
- Always use `uProgress` tied to a GSAP ScrollTrigger for manual scrubbing.
