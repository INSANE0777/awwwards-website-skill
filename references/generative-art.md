# Generative Art Patterns
## Canvas, p5.js, procedural graphics, and creative coding for web

---

## CDN
```html
<script src="https://cdn.jsdelivr.net/npm/p5@1.9.4/lib/p5.min.js"></script>
```

---

## 1. PERLIN NOISE FLOW FIELD (Canvas)

```javascript
function initFlowField(canvasId) {
  const canvas = document.getElementById(canvasId);
  const ctx = canvas.getContext('2d');
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;

  const scale = 20;
  const cols = Math.ceil(canvas.width / scale);
  const rows = Math.ceil(canvas.height / scale);
  let zOff = 0;

  const particles = Array.from({ length: 500 }, () => ({
    x: Math.random() * canvas.width,
    y: Math.random() * canvas.height,
    prevX: 0, prevY: 0
  }));
  particles.forEach(p => { p.prevX = p.x; p.prevY = p.y; });

  // Simple Perlin noise approximation
  function noise(x, y, z) {
    return (Math.sin(x * 1.5 + z) * Math.cos(y * 1.5 + z) + 1) / 2;
  }

  ctx.fillStyle = 'rgba(10, 10, 10, 1)';
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  function animate() {
    ctx.fillStyle = 'rgba(10, 10, 10, 0.02)';
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    particles.forEach(p => {
      const col = Math.floor(p.x / scale);
      const row = Math.floor(p.y / scale);
      const angle = noise(col * 0.1, row * 0.1, zOff) * Math.PI * 4;

      p.prevX = p.x;
      p.prevY = p.y;
      p.x += Math.cos(angle) * 1.5;
      p.y += Math.sin(angle) * 1.5;

      // Wrap around
      if (p.x < 0 || p.x > canvas.width || p.y < 0 || p.y > canvas.height) {
        p.x = Math.random() * canvas.width;
        p.y = Math.random() * canvas.height;
        p.prevX = p.x; p.prevY = p.y;
      }

      ctx.beginPath();
      ctx.moveTo(p.prevX, p.prevY);
      ctx.lineTo(p.x, p.y);
      ctx.strokeStyle = `hsla(${(angle / Math.PI) * 180 + 200}, 70%, 60%, 0.3)`;
      ctx.lineWidth = 0.8;
      ctx.stroke();
    });

    zOff += 0.003;
    requestAnimationFrame(animate);
  }
  animate();
}
```

---

## 2. CIRCLE PACKING

```javascript
function initCirclePacking(canvasId) {
  const canvas = document.getElementById(canvasId);
  const ctx = canvas.getContext('2d');
  canvas.width = canvas.height = Math.min(window.innerWidth, window.innerHeight);

  const circles = [];
  const colors = ['#6366F1', '#8B5CF6', '#06B6D4', '#10B981', '#F59E0B'];

  function addCircle() {
    for (let attempt = 0; attempt < 100; attempt++) {
      const x = Math.random() * canvas.width;
      const y = Math.random() * canvas.height;
      let valid = true;
      let minDist = Infinity;

      for (const c of circles) {
        const d = Math.hypot(x - c.x, y - c.y) - c.r;
        if (d < 2) { valid = false; break; }
        minDist = Math.min(minDist, d);
      }

      if (valid) {
        circles.push({
          x, y,
          r: 2,
          maxR: Math.min(minDist, 50 + Math.random() * 30),
          color: colors[Math.floor(Math.random() * colors.length)]
        });
        return true;
      }
    }
    return false;
  }

  function draw() {
    ctx.fillStyle = '#0a0a0a';
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    let growing = false;
    circles.forEach(c => {
      if (c.r < c.maxR) { c.r += 0.5; growing = true; }
      ctx.beginPath();
      ctx.arc(c.x, c.y, c.r, 0, Math.PI * 2);
      ctx.fillStyle = c.color + '20';
      ctx.fill();
      ctx.strokeStyle = c.color;
      ctx.lineWidth = 1;
      ctx.stroke();
    });

    addCircle();
    if (growing || circles.length < 300) requestAnimationFrame(draw);
  }
  draw();
}
```

---

## 3. WAVE PATTERN

```javascript
function initWavePattern(canvasId) {
  const canvas = document.getElementById(canvasId);
  const ctx = canvas.getContext('2d');
  canvas.width = window.innerWidth;
  canvas.height = 300;

  function draw(t) {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    for (let wave = 0; wave < 5; wave++) {
      ctx.beginPath();
      const amplitude = 30 - wave * 5;
      const frequency = 0.01 + wave * 0.002;
      const speed = t * 0.001 * (1 + wave * 0.3);

      for (let x = 0; x <= canvas.width; x++) {
        const y = canvas.height / 2 + Math.sin(x * frequency + speed) * amplitude;
        x === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
      }

      ctx.strokeStyle = `hsla(${240 + wave * 20}, 70%, 60%, ${0.3 - wave * 0.05})`;
      ctx.lineWidth = 2;
      ctx.stroke();
    }

    requestAnimationFrame(draw);
  }
  draw(0);
}
```

---

## 4. DOT MATRIX / GRID

```javascript
function initDotMatrix(canvasId) {
  const canvas = document.getElementById(canvasId);
  const ctx = canvas.getContext('2d');
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;

  const spacing = 20;
  let mouse = { x: -1000, y: -1000 };
  canvas.addEventListener('mousemove', e => { mouse.x = e.clientX; mouse.y = e.clientY; });

  function draw() {
    ctx.fillStyle = '#0a0a0a';
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    for (let x = spacing; x < canvas.width; x += spacing) {
      for (let y = spacing; y < canvas.height; y += spacing) {
        const dist = Math.hypot(mouse.x - x, mouse.y - y);
        const size = dist < 200 ? 3 + (200 - dist) / 200 * 4 : 1.5;
        const alpha = dist < 200 ? 0.8 : 0.15;

        ctx.beginPath();
        ctx.arc(x, y, size, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(99, 102, 241, ${alpha})`;
        ctx.fill();
      }
    }

    requestAnimationFrame(draw);
  }
  draw();
}
```

---

## 5. MESH GRADIENT (animated)

```css
.mesh-gradient {
  background:
    radial-gradient(at 20% 30%, hsla(260, 80%, 50%, 0.4) 0%, transparent 60%),
    radial-gradient(at 80% 20%, hsla(200, 80%, 50%, 0.3) 0%, transparent 50%),
    radial-gradient(at 50% 80%, hsla(300, 70%, 50%, 0.3) 0%, transparent 60%),
    radial-gradient(at 70% 60%, hsla(170, 70%, 40%, 0.2) 0%, transparent 50%);
  background-color: #0a0a0a;
  animation: meshMove 15s ease-in-out infinite alternate;
}

@keyframes meshMove {
  0%   { background-position: 0% 0%, 100% 0%, 50% 100%, 70% 60%; }
  100% { background-position: 100% 100%, 0% 100%, 70% 0%, 30% 40%; }
}
```

---

## USE CASES BY ARCHETYPE

| Archetype | Pattern |
|---|---|
| AI / Tech (27) | Dot matrix, flow field |
| Generative (25) | Circle packing, flow field |
| Crypto / Web3 (31) | Mesh gradient, dot matrix |
| Music (29) | Wave pattern (audio-reactive) |
| Sci-fi (18) | Dot matrix, particles |
| Minimal (19) | Subtle wave or single dots |
