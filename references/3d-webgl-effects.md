# 3D & WebGL Effects
## Shaders, ripple, bulge, fluid, glass, 3D DOM, WebGL gallery, and more

Requires Three.js for most effects. CDN:
```html
<script type="importmap">
  { "imports": { "three": "https://unpkg.com/three@0.164.1/build/three.module.js" } }
</script>
```

---

## 1. RIPPLE SHADER (click/hover creates water ripple)

```javascript
import * as THREE from 'three';

const vertexShader = `
  varying vec2 vUv;
  void main() {
    vUv = uv;
    gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
  }
`;

const fragmentShader = `
  uniform sampler2D uTexture;
  uniform float uTime;
  uniform vec2 uMouse;
  uniform float uRipple;
  varying vec2 vUv;

  void main() {
    vec2 uv = vUv;
    float dist = distance(uv, uMouse);
    float ripple = sin(dist * 40.0 - uTime * 4.0) * uRipple;
    ripple *= smoothstep(0.4, 0.0, dist);
    uv += ripple * 0.02;
    gl_FragColor = texture2D(uTexture, uv);
  }
`;

function initRippleShader(containerId, imageUrl) {
  const container = document.getElementById(containerId);
  const scene = new THREE.Scene();
  const camera = new THREE.OrthographicCamera(-1, 1, 1, -1, 0, 1);
  const renderer = new THREE.WebGLRenderer({ alpha: true });
  renderer.setSize(container.offsetWidth, container.offsetHeight);
  container.appendChild(renderer.domElement);

  const uniforms = {
    uTexture: { value: new THREE.TextureLoader().load(imageUrl) },
    uTime: { value: 0 },
    uMouse: { value: new THREE.Vector2(0.5, 0.5) },
    uRipple: { value: 0 }
  };

  const mesh = new THREE.Mesh(
    new THREE.PlaneGeometry(2, 2),
    new THREE.ShaderMaterial({ vertexShader, fragmentShader, uniforms })
  );
  scene.add(mesh);

  container.addEventListener('mousemove', e => {
    const rect = container.getBoundingClientRect();
    uniforms.uMouse.value.set(
      (e.clientX - rect.left) / rect.width,
      1 - (e.clientY - rect.top) / rect.height
    );
    gsap.to(uniforms.uRipple, { value: 1, duration: 0.3 });
  });

  container.addEventListener('mouseleave', () => {
    gsap.to(uniforms.uRipple, { value: 0, duration: 1 });
  });

  function animate(t) {
    uniforms.uTime.value = t * 0.001;
    renderer.render(scene, camera);
    requestAnimationFrame(animate);
  }
  animate(0);
}
```

---

## 2. BULGE EFFECT (hover magnification)

```glsl
// Fragment shader for bulge
uniform sampler2D uTexture;
uniform vec2 uMouse;
uniform float uBulge;
varying vec2 vUv;

void main() {
  vec2 uv = vUv;
  vec2 center = uMouse;
  float dist = distance(uv, center);
  float radius = 0.3;

  if (dist < radius) {
    float strength = uBulge * (1.0 - dist / radius);
    vec2 dir = normalize(uv - center);
    uv -= dir * strength * 0.1;
  }

  gl_FragColor = texture2D(uTexture, uv);
}
```

---

## 3. 3D WAVE ON SCROLL

```javascript
function init3DWave(containerId) {
  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100);
  camera.position.z = 3;

  const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  document.getElementById(containerId).appendChild(renderer.domElement);

  const geometry = new THREE.PlaneGeometry(6, 4, 100, 100);
  const material = new THREE.MeshPhongMaterial({
    color: 0x6366f1,
    wireframe: true,
    transparent: true,
    opacity: 0.4
  });
  const plane = new THREE.Mesh(geometry, material);
  plane.rotation.x = -0.5;
  scene.add(plane);

  scene.add(new THREE.DirectionalLight(0xffffff, 1));
  scene.add(new THREE.AmbientLight(0x404040));

  let scrollProgress = 0;
  ScrollTrigger.create({
    start: 0, end: 'max', scrub: true,
    onUpdate: self => { scrollProgress = self.progress; }
  });

  function animate(t) {
    const positions = geometry.attributes.position.array;
    for (let i = 0; i < positions.length; i += 3) {
      positions[i + 2] = Math.sin(positions[i] * 2 + t * 0.001 + scrollProgress * 10) * 0.3
                       + Math.cos(positions[i + 1] * 2 + t * 0.0012) * 0.2;
    }
    geometry.attributes.position.needsUpdate = true;
    renderer.render(scene, camera);
    requestAnimationFrame(animate);
  }
  animate(0);
}
```

---

## 4. 3D GLASS EFFECT (glassmorphism with Three.js)

```javascript
function init3DGlass() {
  const material = new THREE.MeshPhysicalMaterial({
    color: 0xffffff,
    metalness: 0.0,
    roughness: 0.05,
    transmission: 0.95,  // glass transparency
    thickness: 0.5,
    ior: 1.5,  // index of refraction
    clearcoat: 1.0,
    clearcoatRoughness: 0.1
  });

  const geometry = new THREE.SphereGeometry(1, 64, 64);
  const sphere = new THREE.Mesh(geometry, material);
  scene.add(sphere);

  // Float animation
  gsap.to(sphere.position, {
    y: 0.3, duration: 2, yoyo: true, repeat: -1, ease: 'sine.inOut'
  });
  gsap.to(sphere.rotation, {
    y: Math.PI * 2, duration: 8, repeat: -1, ease: 'none'
  });
}
```

---

## 5. 3D PARALLAX LETTERS (text in 3D space)

```javascript
function init3DLetters(text) {
  const loader = new THREE.FontLoader();
  // Use a CDN font or bundled font
  const fontSize = 1.5;
  const spacing = 1.2;

  // Simple approach: use CSS3D or DOM-based 3D
  const container = document.querySelector('.letters-3d');
  text.split('').forEach((char, i) => {
    const el = document.createElement('span');
    el.textContent = char;
    el.style.cssText = `
      display: inline-block; font-size: 8rem; font-weight: 900;
      transform: perspective(800px) translateZ(${i * 10}px) rotateY(${i * 2}deg);
      transition: transform 0.5s ease;
    `;
    container.appendChild(el);
  });

  document.addEventListener('mousemove', e => {
    const x = (e.clientX / window.innerWidth - 0.5) * 2;
    const y = (e.clientY / window.innerHeight - 0.5) * 2;

    container.querySelectorAll('span').forEach((letter, i) => {
      gsap.to(letter, {
        rotateY: x * (5 + i * 2),
        rotateX: -y * (3 + i * 1.5),
        z: i * 10 + x * 20,
        duration: 0.5,
        ease: 'power2.out'
      });
    });
  });
}
```

---

## 6. 3D FLOAT EFFECT

```javascript
function init3DFloat(selector) {
  document.querySelectorAll(selector).forEach(el => {
    gsap.to(el, {
      y: -15,
      rotateX: 3,
      rotateY: -3,
      duration: 3,
      yoyo: true,
      repeat: -1,
      ease: 'sine.inOut',
      transformPerspective: 800
    });
  });
}
```

---

## 7. 3D CUBE (interactive rotating cube)

```javascript
function init3DCube(containerId) {
  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(50, 1, 0.1, 100);
  camera.position.z = 4;

  const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true });
  renderer.setSize(400, 400);
  document.getElementById(containerId).appendChild(renderer.domElement);

  const geometry = new THREE.BoxGeometry(1.5, 1.5, 1.5);
  const materials = Array(6).fill(null).map((_, i) =>
    new THREE.MeshStandardMaterial({
      color: new THREE.Color().setHSL(i / 6, 0.7, 0.5),
      metalness: 0.3, roughness: 0.4
    })
  );
  const cube = new THREE.Mesh(geometry, materials);
  scene.add(cube);

  scene.add(new THREE.DirectionalLight(0xffffff, 1.5));
  scene.add(new THREE.AmbientLight(0x404040, 0.5));

  let targetRotX = 0, targetRotY = 0;
  renderer.domElement.addEventListener('mousemove', e => {
    const rect = renderer.domElement.getBoundingClientRect();
    targetRotX = ((e.clientY - rect.top) / rect.height - 0.5) * Math.PI;
    targetRotY = ((e.clientX - rect.left) / rect.width - 0.5) * Math.PI;
  });

  function animate() {
    cube.rotation.x += (targetRotX - cube.rotation.x) * 0.05;
    cube.rotation.y += (targetRotY - cube.rotation.y) * 0.05;
    renderer.render(scene, camera);
    requestAnimationFrame(animate);
  }
  animate();
}
```

---

## 8. FLUID SHADER (animated gradient mesh)

```glsl
// Fragment shader
uniform float uTime;
varying vec2 vUv;

vec3 palette(float t) {
  vec3 a = vec3(0.5, 0.5, 0.5);
  vec3 b = vec3(0.5, 0.5, 0.5);
  vec3 c = vec3(1.0, 1.0, 1.0);
  vec3 d = vec3(0.263, 0.416, 0.557);
  return a + b * cos(6.28318 * (c * t + d));
}

void main() {
  vec2 uv = vUv * 2.0 - 1.0;
  float d = length(uv);
  vec3 col = palette(d + uTime * 0.2);
  d = sin(d * 8.0 + uTime) / 8.0;
  d = abs(d);
  d = 0.02 / d;
  col *= d;
  gl_FragColor = vec4(col, 0.6);
}
```

---

## 9. PIXEL SHADER (pixelation effect)

```glsl
uniform sampler2D uTexture;
uniform float uPixelSize;
varying vec2 vUv;

void main() {
  vec2 uv = vUv;
  float ps = uPixelSize;
  uv = floor(uv * ps) / ps;
  gl_FragColor = texture2D(uTexture, uv);
}
```

```javascript
// Animate pixel size on scroll
const uniforms = { uPixelSize: { value: 200.0 } };
gsap.to(uniforms.uPixelSize, {
  value: 4.0,
  ease: 'none',
  scrollTrigger: {
    trigger: '.pixel-section',
    start: 'top top',
    end: 'bottom top',
    scrub: 1
  }
});
```

---

## 10. WEBGL IMAGE GALLERY (hover distortion)

```javascript
function initWebGLGallery() {
  const images = document.querySelectorAll('.gl-gallery img');
  const canvas = document.createElement('canvas');
  canvas.style.cssText = 'position:fixed;inset:0;pointer-events:none;z-index:100;';
  document.body.appendChild(canvas);

  const scene = new THREE.Scene();
  const camera = new THREE.OrthographicCamera(-1, 1, 1, -1, 0, 1);
  const renderer = new THREE.WebGLRenderer({ canvas, alpha: true });
  renderer.setSize(window.innerWidth, window.innerHeight);

  // ... Create plane mesh with distortion shader per image
  // Texture swaps on hover, with wave distortion transition
}
```

---

## 11. 3D PERSPECTIVE GALLERY

```css
.perspective-gallery {
  display: flex; gap: 1rem;
  perspective: 1200px;
  transform-style: preserve-3d;
}
.pg-item {
  width: 300px; height: 400px;
  transform: rotateY(var(--ry, 0deg)) translateZ(var(--tz, 0px));
  transition: transform 0.5s ease;
}
.pg-item:nth-child(1) { --ry: 15deg; --tz: -50px; }
.pg-item:nth-child(2) { --ry: 5deg; --tz: -20px; }
.pg-item:nth-child(3) { --ry: 0deg; --tz: 0px; }
.pg-item:nth-child(4) { --ry: -5deg; --tz: -20px; }
.pg-item:nth-child(5) { --ry: -15deg; --tz: -50px; }

.pg-item:hover { --ry: 0deg; --tz: 60px; }
```

---

## 12. 3D DOM POSITIONING (CSS perspective transforms)

```javascript
function init3DCards() {
  document.querySelectorAll('.card-3d').forEach((card, i) => {
    gsap.set(card, {
      transformPerspective: 1000,
      z: i * -30,
      rotateY: (i - 2) * 8,
      transformOrigin: 'center center'
    });

    card.addEventListener('mouseenter', () => {
      gsap.to(card, { z: 100, rotateY: 0, scale: 1.1, duration: 0.5 });
    });
    card.addEventListener('mouseleave', () => {
      gsap.to(card, { z: i * -30, rotateY: (i - 2) * 8, scale: 1, duration: 0.5 });
    });
  });
}
```

---

## 13. CREATIVE 404 PAGE

```javascript
function init404() {
  const text = document.querySelector('.error-404');
  const letters = text.textContent.split('');
  text.innerHTML = letters.map(l =>
    `<span style="display:inline-block;font-size:20vw;font-weight:900">${l}</span>`
  ).join('');

  // Letters react to mouse with physics-like repulsion
  document.addEventListener('mousemove', e => {
    text.querySelectorAll('span').forEach(span => {
      const rect = span.getBoundingClientRect();
      const cx = rect.left + rect.width / 2;
      const cy = rect.top + rect.height / 2;
      const dx = e.clientX - cx;
      const dy = e.clientY - cy;
      const dist = Math.sqrt(dx * dx + dy * dy);

      if (dist < 200) {
        const force = (200 - dist) / 200 * 30;
        gsap.to(span, {
          x: -dx / dist * force,
          y: -dy / dist * force,
          rotation: (Math.random() - 0.5) * 20,
          duration: 0.3
        });
      } else {
        gsap.to(span, { x: 0, y: 0, rotation: 0, duration: 0.6 });
      }
    });
  });
}
```
