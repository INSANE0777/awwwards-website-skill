# Particle Systems
## GPGPU particles, instanced meshes, dreamy clouds, and canvas particles

---

## 1. CANVAS PARTICLES (lightweight, no Three.js)

```javascript
function initCanvasParticles(canvasId) {
  const canvas = document.getElementById(canvasId);
  const ctx = canvas.getContext('2d');
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;

  const particles = Array.from({ length: 200 }, () => ({
    x: Math.random() * canvas.width,
    y: Math.random() * canvas.height,
    vx: (Math.random() - 0.5) * 0.5,
    vy: (Math.random() - 0.5) * 0.5,
    size: Math.random() * 2 + 0.5,
    opacity: Math.random() * 0.5 + 0.1
  }));

  function animate() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    particles.forEach(p => {
      p.x += p.vx;
      p.y += p.vy;
      if (p.x < 0 || p.x > canvas.width) p.vx *= -1;
      if (p.y < 0 || p.y > canvas.height) p.vy *= -1;

      ctx.beginPath();
      ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
      ctx.fillStyle = `rgba(255, 255, 255, ${p.opacity})`;
      ctx.fill();
    });

    // Draw connections
    particles.forEach((a, i) => {
      particles.slice(i + 1).forEach(b => {
        const dist = Math.hypot(a.x - b.x, a.y - b.y);
        if (dist < 120) {
          ctx.beginPath();
          ctx.moveTo(a.x, a.y);
          ctx.lineTo(b.x, b.y);
          ctx.strokeStyle = `rgba(255,255,255,${0.1 * (1 - dist / 120)})`;
          ctx.stroke();
        }
      });
    });

    requestAnimationFrame(animate);
  }
  animate();
}
```

---

## 2. MOUSE-REACTIVE CANVAS PARTICLES

```javascript
function initMouseParticles(canvasId) {
  const canvas = document.getElementById(canvasId);
  const ctx = canvas.getContext('2d');
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;

  let mouse = { x: canvas.width / 2, y: canvas.height / 2 };
  canvas.addEventListener('mousemove', e => { mouse.x = e.clientX; mouse.y = e.clientY; });

  const particles = Array.from({ length: 150 }, () => ({
    x: Math.random() * canvas.width,
    y: Math.random() * canvas.height,
    baseX: 0, baseY: 0,
    size: Math.random() * 3 + 1,
    density: Math.random() * 30 + 1
  }));
  particles.forEach(p => { p.baseX = p.x; p.baseY = p.y; });

  function animate() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    particles.forEach(p => {
      const dx = mouse.x - p.x;
      const dy = mouse.y - p.y;
      const dist = Math.sqrt(dx * dx + dy * dy);

      if (dist < 150) {
        const force = (150 - dist) / 150;
        p.x -= (dx / dist) * force * p.density * 0.5;
        p.y -= (dy / dist) * force * p.density * 0.5;
      } else {
        // Return to base
        p.x += (p.baseX - p.x) * 0.05;
        p.y += (p.baseY - p.y) * 0.05;
      }

      ctx.beginPath();
      ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
      ctx.fillStyle = 'rgba(99, 102, 241, 0.6)';
      ctx.fill();
    });

    requestAnimationFrame(animate);
  }
  animate();
}
```

---

## 3. THREE.JS INSTANCED PARTICLES

```javascript
import * as THREE from 'three';

function initInstancedParticles(container) {
  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100);
  camera.position.z = 5;

  const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  container.appendChild(renderer.domElement);

  const COUNT = 5000;
  const geometry = new THREE.BufferGeometry();
  const positions = new Float32Array(COUNT * 3);
  const colors = new Float32Array(COUNT * 3);

  for (let i = 0; i < COUNT; i++) {
    positions[i * 3]     = (Math.random() - 0.5) * 10;
    positions[i * 3 + 1] = (Math.random() - 0.5) * 10;
    positions[i * 3 + 2] = (Math.random() - 0.5) * 10;
    colors[i * 3]     = Math.random() * 0.5 + 0.3;
    colors[i * 3 + 1] = Math.random() * 0.3 + 0.2;
    colors[i * 3 + 2] = Math.random() * 0.8 + 0.2;
  }

  geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));
  geometry.setAttribute('color', new THREE.BufferAttribute(colors, 3));

  const material = new THREE.PointsMaterial({
    size: 0.02, vertexColors: true,
    transparent: true, opacity: 0.8,
    blending: THREE.AdditiveBlending, depthWrite: false
  });

  const points = new THREE.Points(geometry, material);
  scene.add(points);

  function animate(t) {
    points.rotation.y = t * 0.0001;
    points.rotation.x = t * 0.00005;

    const pos = geometry.attributes.position.array;
    for (let i = 0; i < COUNT; i++) {
      pos[i * 3 + 1] += Math.sin(t * 0.001 + i * 0.1) * 0.001;
    }
    geometry.attributes.position.needsUpdate = true;

    renderer.render(scene, camera);
    requestAnimationFrame(animate);
  }
  animate(0);
}
```

---

## 4. DREAMY FLOATING PARTICLES (CSS-only)

```html
<div class="particle-field" aria-hidden="true">
  <span class="particle" style="--x:10%;--y:20%;--d:8s;--s:3px"></span>
  <span class="particle" style="--x:30%;--y:60%;--d:12s;--s:2px"></span>
  <span class="particle" style="--x:50%;--y:40%;--d:10s;--s:4px"></span>
  <span class="particle" style="--x:70%;--y:80%;--d:15s;--s:2px"></span>
  <span class="particle" style="--x:90%;--y:30%;--d:9s;--s:3px"></span>
</div>
```

```css
.particle-field { position: fixed; inset: 0; pointer-events: none; z-index: -1; }
.particle {
  position: absolute;
  left: var(--x); top: var(--y);
  width: var(--s); height: var(--s);
  background: var(--accent);
  border-radius: 50%;
  opacity: 0.4;
  animation: floatParticle var(--d) ease-in-out infinite;
}
@keyframes floatParticle {
  0%, 100% { transform: translateY(0) translateX(0); opacity: 0.2; }
  50%      { transform: translateY(-30px) translateX(15px); opacity: 0.6; }
}
```

---

## 5. CONFETTI BURST

```javascript
function confettiBurst(x, y) {
  const colors = ['#FF6B6B','#4ECDC4','#45B7D1','#96CEB4','#FFEAA7','#DDA0DD'];
  for (let i = 0; i < 50; i++) {
    const el = document.createElement('div');
    el.style.cssText = `
      position:fixed;left:${x}px;top:${y}px;width:8px;height:8px;
      background:${colors[i % colors.length]};
      border-radius:${Math.random()>0.5?'50%':'2px'};
      pointer-events:none;z-index:10000;
    `;
    document.body.appendChild(el);
    gsap.to(el, {
      x: (Math.random()-0.5) * 400,
      y: (Math.random()-0.5) * 400 - 200,
      rotation: Math.random() * 720,
      opacity: 0, scale: 0,
      duration: 1 + Math.random(),
      ease: 'power3.out',
      onComplete: () => el.remove()
    });
  }
}
```
