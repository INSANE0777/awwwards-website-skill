# Three.js Immersive System
## WebGL + GSAP + GLSL for Awwwards-Level 3D Experiences

This file covers Three.js as the primary surface — the Lusion/Igloo Inc tier of web experience.

---

## CORE SETUP (HTML artifact with CDN)

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
```

```javascript
class WebGLScene {
  constructor(container) {
    this.container = container;
    this.sizes = { w: window.innerWidth, h: window.innerHeight };
    this.init();
  }

  init() {
    // Renderer
    this.renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
    this.renderer.setSize(this.sizes.w, this.sizes.h);
    this.renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    this.container.appendChild(this.renderer.domElement);

    // Scene + Camera
    this.scene = new THREE.Scene();
    this.camera = new THREE.PerspectiveCamera(75, this.sizes.w / this.sizes.h, 0.1, 100);
    this.camera.position.set(0, 0, 5);

    // Resize handler
    window.addEventListener('resize', () => this.resize());

    // Render loop via GSAP ticker (syncs with Lenis)
    gsap.ticker.add(() => this.renderer.render(this.scene, this.camera));
  }

  resize() {
    this.sizes = { w: window.innerWidth, h: window.innerHeight };
    this.camera.aspect = this.sizes.w / this.sizes.h;
    this.camera.updateProjectionMatrix();
    this.renderer.setSize(this.sizes.w, this.sizes.h);
    this.renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  }
}
```

---

## CANVAS POSITIONING (WebGL behind HTML)

```css
.webgl-canvas {
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
  z-index: 0;
  pointer-events: none;
}
.html-content {
  position: relative;
  z-index: 10;
}
/* Blend WebGL with HTML text */
.overlay-text {
  mix-blend-mode: difference;
  color: white;
}
```

---

## GLSL SHADERS — KEY PATTERNS

### Displacement / Distortion Shader
```glsl
/* vertex.glsl */
uniform float uTime;
uniform float uDistortion;
varying vec2 vUv;

void main() {
  vUv = uv;
  vec3 pos = position;
  pos.z += sin(pos.x * 3.0 + uTime) * uDistortion;
  pos.z += cos(pos.y * 2.5 + uTime * 0.8) * uDistortion * 0.5;
  gl_Position = projectionMatrix * modelViewMatrix * vec4(pos, 1.0);
}
```
```glsl
/* fragment.glsl */
uniform sampler2D uTexture;
uniform float uProgress;
uniform vec2 uMouse;
varying vec2 vUv;

void main() {
  // Mouse displacement effect
  vec2 mouse = (uMouse - 0.5) * 0.08;
  vec2 uv = vUv + mouse * (1.0 - uProgress);
  vec4 color = texture2D(uTexture, uv);
  gl_FragColor = color;
}
```

### Image Transition Shader (crossfade with distortion)
```glsl
uniform sampler2D uTexture1;
uniform sampler2D uTexture2;
uniform sampler2D uDisp;
uniform float uProgress; /* 0→1 animated by GSAP */
varying vec2 vUv;

void main() {
  vec4 disp = texture2D(uDisp, vUv);
  float d = disp.r * 0.3;
  vec4 c1 = texture2D(uTexture1, vUv + vec2(d * uProgress, 0.0));
  vec4 c2 = texture2D(uTexture2, vUv - vec2(d * (1.0 - uProgress), 0.0));
  gl_FragColor = mix(c1, c2, uProgress);
}
```

### Grayscale + Color Reveal
```glsl
uniform sampler2D uTexture;
uniform float uSaturation; /* 0=grayscale, 1=full color — animated by GSAP */
varying vec2 vUv;

void main() {
  vec4 color = texture2D(uTexture, vUv);
  float gray = dot(color.rgb, vec3(0.299, 0.587, 0.114));
  vec3 result = mix(vec3(gray), color.rgb, uSaturation);
  gl_FragColor = vec4(result, color.a);
}
```

---

## SCROLL-DRIVEN CAMERA PATH

```javascript
function initScrollCamera(scene) {
  const positions = [
    [0, 0, 8],
    [3, 1, 5],
    [-2, -1, 6],
    [0, 2, 4]
  ];

  let progress = 0;
  ScrollTrigger.create({
    trigger: '.webgl-scroll',
    start: 'top top',
    end: '+=400%',
    pin: true,
    scrub: 2,
    onUpdate: (self) => {
      progress = self.progress;
      const seg = Math.floor(progress * (positions.length - 1));
      const t = (progress * (positions.length - 1)) - seg;
      const curr = positions[Math.min(seg, positions.length - 1)];
      const next = positions[Math.min(seg + 1, positions.length - 1)];

      scene.camera.position.set(
        curr[0] + (next[0] - curr[0]) * t,
        curr[1] + (next[1] - curr[1]) * t,
        curr[2] + (next[2] - curr[2]) * t
      );
    }
  });
}
```

---

## MOUSE PARALLAX (3D scene reacts to cursor)

```javascript
function initMouseParallax(scene) {
  const mouse = new THREE.Vector2();
  const target = new THREE.Vector2();

  window.addEventListener('mousemove', e => {
    mouse.x = (e.clientX / window.innerWidth)  * 2 - 1;
    mouse.y = -(e.clientY / window.innerHeight) * 2 + 1;
  });

  // In render loop / gsap ticker:
  gsap.ticker.add(() => {
    target.lerp(mouse, 0.05); // smoothing
    scene.rotation.y = target.x * 0.15;
    scene.rotation.x = target.y * 0.1;
  });
}
```

---

## PARTICLES / POINTS SYSTEM

```javascript
function createParticles(count = 2000) {
  const geometry = new THREE.BufferGeometry();
  const positions = new Float32Array(count * 3);
  const colors    = new Float32Array(count * 3);

  for (let i = 0; i < count; i++) {
    positions[i*3]   = (Math.random() - 0.5) * 20;
    positions[i*3+1] = (Math.random() - 0.5) * 20;
    positions[i*3+2] = (Math.random() - 0.5) * 20;
    colors[i*3]   = Math.random();
    colors[i*3+1] = Math.random();
    colors[i*3+2] = Math.random();
  }

  geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));
  geometry.setAttribute('color',    new THREE.BufferAttribute(colors, 3));

  const material = new THREE.PointsMaterial({
    size: 0.05,
    vertexColors: true,
    transparent: true,
    opacity: 0.8,
    sizeAttenuation: true
  });

  return new THREE.Points(geometry, material);
}
```

---

## MOBILE FALLBACK PATTERN

```javascript
function shouldUseWebGL() {
  // Detect low-power or mobile devices
  const canvas = document.createElement('canvas');
  const gl = canvas.getContext('webgl') || canvas.getContext('experimental-webgl');
  if (!gl) return false;

  const isMobile = /iPhone|iPad|Android/i.test(navigator.userAgent);
  const lowMemory = navigator.deviceMemory && navigator.deviceMemory < 2;
  return !isMobile && !lowMemory;
}

if (shouldUseWebGL()) {
  initWebGLScene();
} else {
  // Show video or image fallback
  document.querySelector('.webgl-fallback').style.display = 'block';
  document.querySelector('.webgl-canvas').style.display = 'none';
}
```

---

## SHADER MATERIAL IN THREE.JS

```javascript
const material = new THREE.ShaderMaterial({
  uniforms: {
    uTime:       { value: 0 },
    uTexture:    { value: texture },
    uProgress:   { value: 0 },
    uMouse:      { value: new THREE.Vector2(0.5, 0.5) },
    uResolution: { value: new THREE.Vector2(window.innerWidth, window.innerHeight) }
  },
  vertexShader:   vertexGLSL,   // string
  fragmentShader: fragmentGLSL, // string
  transparent: true
});

// Update time uniform in render loop
gsap.ticker.add((time) => {
  material.uniforms.uTime.value = time;
});

// Animate progress with GSAP
gsap.to(material.uniforms.uProgress, {
  value: 1, duration: 1.4, ease: 'power3.inOut'
});
```

---

## AWWWARDS 3D CHECKLIST

- [ ] Render loop via `gsap.ticker` (not `requestAnimationFrame` directly — syncs with Lenis)
- [ ] Pixel ratio capped at 2 (`Math.min(devicePixelRatio, 2)`)
- [ ] Resize handler updates camera aspect + renderer size
- [ ] Mobile fallback exists (video or image)
- [ ] WebGL context loss handled: `WEBGL_lose_context`
- [ ] Custom GLSL shader for at least one effect (not just default materials)
- [ ] Camera moves are driven by scroll or mouse (not static)
- [ ] Performance: use `renderer.dispose()` and `geometry.dispose()` on cleanup
