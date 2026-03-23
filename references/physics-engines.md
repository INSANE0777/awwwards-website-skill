# Physics Engines for Awwwards-Level Interactivity
## Matter.js (2D) & Cannon-es (3D) — Setup, Patterns, Performance

Use physics engines when you need: bouncing elements, draggable objects, falling/scattering animations, ball-bounce loaders, interactive toys, or gravity-driven layouts.

---

## MATTER.JS (2D Physics)

### CDN Setup
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/matter-js/0.20.0/matter.min.js"></script>
```

### Core Setup
```javascript
const { Engine, Render, Runner, Bodies, Composite, Mouse, MouseConstraint, Events } = Matter;

function initPhysics(containerId) {
  const container = document.getElementById(containerId);
  const w = container.offsetWidth;
  const h = container.offsetHeight;

  // Engine
  const engine = Engine.create({ gravity: { x: 0, y: 1.5 } });

  // Renderer
  const render = Render.create({
    element: container,
    engine: engine,
    options: {
      width: w,
      height: h,
      wireframes: false,
      background: 'transparent',
      pixelRatio: Math.min(window.devicePixelRatio, 2)
    }
  });

  // Walls (invisible boundaries)
  const walls = [
    Bodies.rectangle(w / 2, h + 25, w, 50, { isStatic: true, render: { visible: false } }), // floor
    Bodies.rectangle(-25, h / 2, 50, h, { isStatic: true, render: { visible: false } }),     // left
    Bodies.rectangle(w + 25, h / 2, 50, h, { isStatic: true, render: { visible: false } }),  // right
    Bodies.rectangle(w / 2, -25, w, 50, { isStatic: true, render: { visible: false } }),     // ceiling
  ];
  Composite.add(engine.world, walls);

  // Mouse interaction
  const mouse = Mouse.create(render.canvas);
  const mouseConstraint = MouseConstraint.create(engine, {
    mouse: mouse,
    constraint: { stiffness: 0.2, render: { visible: false } }
  });
  Composite.add(engine.world, mouseConstraint);
  render.mouse = mouse;

  // Run
  const runner = Runner.create();
  Runner.run(runner, engine);
  Render.run(render);

  return { engine, render, runner };
}
```

---

## PATTERN 1: Bouncing Ball Loader (DontBoardMe style)

```javascript
function createBallLoader(engine, w, h) {
  // Create interactive ball
  const ball = Bodies.circle(w / 2, h / 3, 40, {
    restitution: 0.85,  // bounciness
    friction: 0.05,
    density: 0.002,
    render: {
      fillStyle: '#E33529',
      strokeStyle: '#C42D22',
      lineWidth: 2
    }
  });
  Composite.add(engine.world, ball);

  // Track bounce count
  let bounces = 0;
  const TARGET = 5;

  Events.on(engine, 'collisionStart', (event) => {
    event.pairs.forEach(pair => {
      if (pair.bodyA === ball || pair.bodyB === ball) {
        bounces++;
        if (bounces >= TARGET) {
          dismissLoader();
        }
      }
    });
  });

  return ball;
}
```

---

## PATTERN 2: Falling Letters

```javascript
function createFallingText(engine, text, w, h) {
  const letters = text.split('');
  const spacing = w / (letters.length + 1);

  letters.forEach((char, i) => {
    const x = spacing * (i + 1) + (Math.random() - 0.5) * 20;
    const y = -50 - Math.random() * 200; // start above viewport

    const body = Bodies.rectangle(x, y, 40, 50, {
      restitution: 0.4,
      friction: 0.3,
      angle: (Math.random() - 0.5) * 0.5,
      render: {
        fillStyle: 'transparent',
        // Use custom renderer for text (see below)
      },
      label: char
    });
    Composite.add(engine.world, body);
  });
}

// Custom text rendering (after Render.run)
Events.on(render, 'afterRender', () => {
  const ctx = render.context;
  const bodies = Composite.allBodies(engine.world);

  bodies.forEach(body => {
    if (body.label && body.label.length === 1) {
      ctx.save();
      ctx.translate(body.position.x, body.position.y);
      ctx.rotate(body.angle);
      ctx.font = 'bold 36px "Bebas Neue", sans-serif';
      ctx.fillStyle = '#1A1A1A';
      ctx.textAlign = 'center';
      ctx.textBaseline = 'middle';
      ctx.fillText(body.label, 0, 0);
      ctx.restore();
    }
  });
});
```

---

## PATTERN 3: Draggable Stickers / Cards

```javascript
function createDraggableCards(engine, items, w, h) {
  items.forEach((item, i) => {
    const x = (w / (items.length + 1)) * (i + 1);
    const y = h / 2 + (Math.random() - 0.5) * 100;

    const card = Bodies.rectangle(x, y, 120, 80, {
      restitution: 0.3,
      friction: 0.8,
      angle: (Math.random() - 0.5) * 0.3,
      render: {
        fillStyle: '#FAFAF8',
        strokeStyle: '#E8E5DF',
        lineWidth: 1
      }
    });
    Composite.add(engine.world, card);
  });
}
```

---

## CANNON-ES (3D Physics for Three.js)

### CDN/Import
```html
<script type="module">
  import * as CANNON from 'https://cdn.jsdelivr.net/npm/cannon-es@0.20.0/+esm';
</script>
```

### Basic Integration with Three.js
```javascript
// Physics world
const world = new CANNON.World({ gravity: new CANNON.Vec3(0, -9.82, 0) });

// Create physics body for each Three.js mesh
function addPhysicsBody(mesh, mass = 1) {
  const shape = new CANNON.Box(
    new CANNON.Vec3(0.5, 0.5, 0.5) // half-extents
  );
  const body = new CANNON.Body({ mass, shape });
  body.position.copy(mesh.position);
  world.addBody(body);
  return body;
}

// Sync in render loop
gsap.ticker.add(() => {
  world.step(1 / 60);
  // Copy physics positions to Three.js meshes
  meshes.forEach((mesh, i) => {
    mesh.position.copy(bodies[i].position);
    mesh.quaternion.copy(bodies[i].quaternion);
  });
});
```

---

## PERFORMANCE OPTIMIZATION

### Body Count Limits
| Device | Max bodies | Notes |
|---|---|---|
| Desktop | 100–200 | Smooth at 60fps |
| Mobile | 30–50 | Reduce or disable on touch |
| Low-end | 10–20 | Consider static fallback |

### Sleep Optimization
```javascript
// Enable sleeping (bodies that stop moving go to sleep)
const engine = Engine.create({
  enableSleeping: true,
  gravity: { x: 0, y: 1.5 }
});

// Custom sleep threshold
body.sleepThreshold = 30; // frames of near-zero velocity before sleep
```

### Cleanup
```javascript
function destroyPhysics(engine, render, runner) {
  Render.stop(render);
  Runner.stop(runner);
  Engine.clear(engine);
  render.canvas.remove();
  render.textures = {};
}
```

---

## MOBILE CONSIDERATIONS

```javascript
// Detect touch and simplify physics
const isMobile = window.matchMedia('(hover: none)').matches;

if (isMobile) {
  // Reduce body count
  engine.gravity.y = 2; // faster settling
  // Use tap instead of drag
  render.canvas.addEventListener('touchstart', (e) => {
    const touch = e.touches[0];
    const rect = render.canvas.getBoundingClientRect();
    applyForceAtPoint(touch.clientX - rect.left, touch.clientY - rect.top);
  }, { passive: true });
}
```

---

## WHEN TO USE PHYSICS

| Scenario | Engine | Alternative |
|---|---|---|
| Bouncing ball loader | Matter.js | CSS animation (simpler) |
| Falling scattered elements | Matter.js | GSAP stagger (no collision) |
| 3D objects with gravity | Cannon-es | GSAP + Three.js (no collision) |
| Interactive toy/game | Matter.js | — (physics required) |
| Simple bounce effect | — | CSS `@keyframes` bounce |
| Parallax depth | — | GSAP + ScrollTrigger |

**Rule**: Only use physics when collision detection or realistic motion is essential. GSAP handles most motion needs with less overhead.
