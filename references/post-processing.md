# Post-Processing Effects
## Bloom, chromatic aberration, film grain, glitch, vignette for Three.js

```javascript
import { EffectComposer } from 'three/addons/postprocessing/EffectComposer.js';
import { RenderPass } from 'three/addons/postprocessing/RenderPass.js';
import { UnrealBloomPass } from 'three/addons/postprocessing/UnrealBloomPass.js';
import { ShaderPass } from 'three/addons/postprocessing/ShaderPass.js';
```

---

## SETUP

```javascript
function initPostProcessing(renderer, scene, camera) {
  const composer = new EffectComposer(renderer);
  composer.addPass(new RenderPass(scene, camera));
  return composer;
}

// In animation loop: use composer.render() instead of renderer.render()
```

---

## 1. BLOOM (glow)

```javascript
const bloom = new UnrealBloomPass(
  new THREE.Vector2(window.innerWidth, window.innerHeight),
  0.8,   // strength
  0.4,   // radius
  0.85   // threshold
);
composer.addPass(bloom);
```

---

## 2. CHROMATIC ABERRATION

```javascript
const chromaticShader = {
  uniforms: {
    tDiffuse: { value: null },
    uOffset: { value: 0.003 }
  },
  vertexShader: `varying vec2 vUv; void main() { vUv = uv; gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0); }`,
  fragmentShader: `
    uniform sampler2D tDiffuse;
    uniform float uOffset;
    varying vec2 vUv;
    void main() {
      float r = texture2D(tDiffuse, vUv + vec2(uOffset, 0.0)).r;
      float g = texture2D(tDiffuse, vUv).g;
      float b = texture2D(tDiffuse, vUv - vec2(uOffset, 0.0)).b;
      gl_FragColor = vec4(r, g, b, 1.0);
    }
  `
};
composer.addPass(new ShaderPass(chromaticShader));
```

---

## 3. FILM GRAIN (CSS-only version)

```css
.grain-overlay {
  position: fixed; inset: 0; z-index: 9999;
  pointer-events: none;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  opacity: 0.04;
  mix-blend-mode: overlay;
  animation: grainShift 0.5s steps(4) infinite;
}

@keyframes grainShift {
  0%   { transform: translate(0, 0); }
  25%  { transform: translate(-2%, 2%); }
  50%  { transform: translate(2%, -1%); }
  75%  { transform: translate(-1%, -2%); }
  100% { transform: translate(1%, 1%); }
}
```

```html
<div class="grain-overlay" aria-hidden="true"></div>
```

---

## 4. FILM GRAIN (Three.js shader)

```javascript
const grainShader = {
  uniforms: { tDiffuse: { value: null }, uTime: { value: 0 }, uAmount: { value: 0.05 } },
  vertexShader: `varying vec2 vUv; void main() { vUv = uv; gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0); }`,
  fragmentShader: `
    uniform sampler2D tDiffuse;
    uniform float uTime;
    uniform float uAmount;
    varying vec2 vUv;
    float random(vec2 co) { return fract(sin(dot(co, vec2(12.9898, 78.233))) * 43758.5453); }
    void main() {
      vec4 color = texture2D(tDiffuse, vUv);
      float grain = random(vUv + uTime) * uAmount;
      gl_FragColor = vec4(color.rgb + grain, color.a);
    }
  `
};
const grainPass = new ShaderPass(grainShader);
composer.addPass(grainPass);
// In loop: grainPass.uniforms.uTime.value = time;
```

---

## 5. GLITCH EFFECT

```javascript
const glitchShader = {
  uniforms: {
    tDiffuse: { value: null },
    uGlitch: { value: 0 },
    uTime: { value: 0 }
  },
  fragmentShader: `
    uniform sampler2D tDiffuse;
    uniform float uGlitch;
    uniform float uTime;
    varying vec2 vUv;
    float random(vec2 co) { return fract(sin(dot(co, vec2(12.9898,78.233))) * 43758.5453); }
    void main() {
      vec2 uv = vUv;
      if (uGlitch > 0.5) {
        float noise = random(vec2(floor(uv.y * 40.0), uTime));
        if (noise > 0.9) uv.x += (random(vec2(uTime)) - 0.5) * 0.1;
      }
      float r = texture2D(tDiffuse, uv + vec2(uGlitch * 0.01, 0.0)).r;
      float g = texture2D(tDiffuse, uv).g;
      float b = texture2D(tDiffuse, uv - vec2(uGlitch * 0.01, 0.0)).b;
      gl_FragColor = vec4(r, g, b, 1.0);
    }
  `
};

// Trigger glitch randomly
setInterval(() => {
  if (Math.random() > 0.9) {
    gsap.to(glitchPass.uniforms.uGlitch, { value: 1, duration: 0.05,
      onComplete: () => gsap.to(glitchPass.uniforms.uGlitch, { value: 0, duration: 0.1 })
    });
  }
}, 200);
```

---

## 6. VIGNETTE

```javascript
const vignetteShader = {
  uniforms: { tDiffuse: { value: null }, uDarkness: { value: 0.5 }, uOffset: { value: 1.0 } },
  fragmentShader: `
    uniform sampler2D tDiffuse;
    uniform float uDarkness;
    uniform float uOffset;
    varying vec2 vUv;
    void main() {
      vec4 color = texture2D(tDiffuse, vUv);
      vec2 center = vUv - 0.5;
      float dist = length(center);
      color.rgb *= smoothstep(0.8, uOffset * 0.4, dist * (uDarkness + uOffset));
      gl_FragColor = color;
    }
  `
};
```

---

## CSS-ONLY POST-PROCESSING

```css
/* Vignette overlay */
.vignette::after {
  content: ''; position: fixed; inset: 0;
  background: radial-gradient(ellipse at center, transparent 60%, rgba(0,0,0,0.4));
  pointer-events: none; z-index: 9998;
}

/* Scanlines */
.scanlines::after {
  content: ''; position: fixed; inset: 0;
  background: repeating-linear-gradient(
    0deg, transparent, transparent 2px, rgba(0,0,0,0.03) 2px, rgba(0,0,0,0.03) 4px
  );
  pointer-events: none; z-index: 9998;
}
```
