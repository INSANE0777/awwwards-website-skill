# Image Effects & Gallery Patterns
## Pixel loading, paint reveal, mouse gallery, project showcase, infinite scroll

---

## 1. IMAGE PIXEL LOADING (pixelated → sharp reveal)

```css
.pixel-image {
  image-rendering: pixelated;
  filter: blur(0px);
  transition: filter 1s ease;
}
.pixel-image.loaded { filter: none; }
```

```javascript
function initPixelLoading() {
  document.querySelectorAll('.pixel-image').forEach(img => {
    // Start with tiny version
    const src = img.dataset.src;
    const tiny = document.createElement('canvas');
    const ctx = tiny.getContext('2d');
    tiny.width = 16; tiny.height = 16;

    const tempImg = new Image();
    tempImg.onload = () => {
      ctx.drawImage(tempImg, 0, 0, 16, 16);
      img.src = tiny.toDataURL();
      img.style.imageRendering = 'pixelated';

      // Step up resolution
      const steps = [32, 64, 128, 0]; // 0 = full
      let step = 0;

      const reveal = setInterval(() => {
        if (step < steps.length - 1) {
          const s = steps[step];
          tiny.width = s; tiny.height = s;
          ctx.drawImage(tempImg, 0, 0, s, s);
          img.src = tiny.toDataURL();
          step++;
        } else {
          img.src = src;
          img.style.imageRendering = 'auto';
          img.classList.add('loaded');
          clearInterval(reveal);
        }
      }, 150);
    };
    tempImg.src = src;
  });
}
```

---

## 2. PROJECT GALLERY MOUSE HOVER (image follows cursor)

```html
<div class="project-list">
  <a class="project-item" data-img="project1.jpg">
    <span class="project-title">Project Alpha</span>
    <span class="project-category">Branding</span>
  </a>
  <a class="project-item" data-img="project2.jpg">
    <span class="project-title">Project Beta</span>
    <span class="project-category">Web Design</span>
  </a>
</div>

<div class="project-preview" id="projectPreview">
  <img src="" alt="" />
</div>
```

```css
.project-list { position: relative; }

.project-item {
  display: flex; justify-content: space-between; align-items: center;
  padding: 2rem 0; border-bottom: 1px solid var(--border);
  cursor: pointer; text-decoration: none; color: var(--ink);
}
.project-item:hover { opacity: 0.6; }

.project-preview {
  position: fixed; width: 350px; height: 250px;
  pointer-events: none; z-index: 100;
  overflow: hidden; border-radius: 8px;
  opacity: 0; transform: scale(0.8);
  transition: opacity 0.3s, transform 0.3s;
}
.project-preview.visible { opacity: 1; transform: scale(1); }
.project-preview img { width: 100%; height: 100%; object-fit: cover; }
```

```javascript
function initProjectGallery() {
  const preview = document.getElementById('projectPreview');
  const img = preview.querySelector('img');

  document.querySelectorAll('.project-item').forEach(item => {
    item.addEventListener('mouseenter', () => {
      img.src = item.dataset.img;
      preview.classList.add('visible');
    });

    item.addEventListener('mouseleave', () => {
      preview.classList.remove('visible');
    });

    item.addEventListener('mousemove', e => {
      gsap.to(preview, {
        x: e.clientX + 20,
        y: e.clientY - 130,
        duration: 0.3,
        ease: 'power2.out'
      });
    });
  });
}
```

---

## 3. IMAGE SLIDE PROJECT GALLERY

```css
.slide-gallery {
  display: flex; gap: 14px;
  overflow: hidden; height: 70vh;
}
.slide-item {
  flex: 1; min-width: 80px;
  overflow: hidden; border-radius: 12px;
  transition: flex 0.7s cubic-bezier(0.77, 0, 0.175, 1);
  cursor: pointer; position: relative;
}
.slide-item:hover { flex: 5; }
.slide-item img { width: 100%; height: 100%; object-fit: cover; }

.slide-item .slide-info {
  position: absolute; bottom: 2rem; left: 2rem;
  color: white; opacity: 0;
  transition: opacity 0.4s ease 0.3s;
}
.slide-item:hover .slide-info { opacity: 1; }
```

---

## 4. PROJECT GALLERY COLORED CARD

```css
.colored-card {
  position: relative; padding: 3rem; border-radius: 16px;
  overflow: hidden; cursor: pointer;
  transition: transform 0.4s ease;
}
.colored-card:hover { transform: translateY(-8px); }

.colored-card::before {
  content: ''; position: absolute; inset: 0;
  background: var(--card-color, var(--accent));
  opacity: 0.08;
  transition: opacity 0.4s ease;
}
.colored-card:hover::before { opacity: 0.15; }

.colored-card img {
  width: 100%; border-radius: 8px;
  transition: transform 0.6s ease;
}
.colored-card:hover img { transform: scale(1.05); }
```

---

## 5. INFINITE SCROLL (loop content)

```javascript
function initInfiniteScroll(containerSelector) {
  const container = document.querySelector(containerSelector);
  const items = container.innerHTML;
  container.innerHTML += items; // duplicate content

  let scrollPos = 0;
  const halfHeight = container.scrollHeight / 2;

  gsap.ticker.add(() => {
    scrollPos += 1; // speed
    if (scrollPos >= halfHeight) scrollPos = 0;
    gsap.set(container, { y: -scrollPos });
  });
}
```

### Horizontal infinite scroll
```javascript
function initHorizontalInfinite(selector) {
  const track = document.querySelector(selector);
  const content = track.innerHTML;
  track.innerHTML += content;

  const width = track.scrollWidth / 2;

  gsap.to(track, {
    x: -width,
    duration: 20,
    ease: 'none',
    repeat: -1,
    modifiers: {
      x: gsap.utils.unitize(x => parseFloat(x) % width)
    }
  });
}
```

---

## 6. IMAGE PIXEL EFFECT (hover pixelation)

```javascript
function initImagePixelEffect() {
  document.querySelectorAll('[data-pixel-hover]').forEach(wrapper => {
    const img = wrapper.querySelector('img');
    const canvas = document.createElement('canvas');
    canvas.style.cssText = 'position:absolute;inset:0;opacity:0;transition:opacity 0.3s;';
    wrapper.style.position = 'relative';
    wrapper.appendChild(canvas);

    const ctx = canvas.getContext('2d');

    wrapper.addEventListener('mouseenter', () => {
      canvas.width = wrapper.offsetWidth;
      canvas.height = wrapper.offsetHeight;
      ctx.drawImage(img, 0, 0, canvas.width, canvas.height);

      // Apply pixelation
      const size = 12;
      ctx.imageSmoothingEnabled = false;
      const tempCanvas = document.createElement('canvas');
      tempCanvas.width = Math.ceil(canvas.width / size);
      tempCanvas.height = Math.ceil(canvas.height / size);
      const tempCtx = tempCanvas.getContext('2d');
      tempCtx.drawImage(img, 0, 0, tempCanvas.width, tempCanvas.height);
      ctx.drawImage(tempCanvas, 0, 0, canvas.width, canvas.height);

      canvas.style.opacity = '1';
    });

    wrapper.addEventListener('mouseleave', () => {
      canvas.style.opacity = '0';
    });
  });
}
```

---

## 7. MOUSE SCALE IMAGE GALLERY (hover zoom + 3D)

```javascript
function initMouseImageGallery() {
  document.querySelectorAll('.gallery-item').forEach(item => {
    const img = item.querySelector('img');

    item.addEventListener('mousemove', e => {
      const rect = item.getBoundingClientRect();
      const x = (e.clientX - rect.left) / rect.width - 0.5;
      const y = (e.clientY - rect.top) / rect.height - 0.5;

      gsap.to(img, {
        scale: 1.2,
        rotateY: x * 10,
        rotateX: -y * 10,
        transformPerspective: 800,
        duration: 0.4,
        ease: 'power2.out'
      });
    });

    item.addEventListener('mouseleave', () => {
      gsap.to(img, {
        scale: 1, rotateY: 0, rotateX: 0,
        duration: 0.6, ease: 'power3.out'
      });
    });
  });
}
```

---

## 8. CAPSULE PHYSICS (Matter.js image capsules)

```javascript
function initCapsulePhysics(containerId, imageUrls) {
  const { Engine, Render, Runner, Bodies, Composite, Mouse, MouseConstraint } = Matter;
  const container = document.getElementById(containerId);
  const w = container.offsetWidth;
  const h = container.offsetHeight;

  const engine = Engine.create({ gravity: { y: 1 } });
  const render = Render.create({
    element: container, engine,
    options: { width: w, height: h, wireframes: false, background: 'transparent' }
  });

  // Walls
  Composite.add(engine.world, [
    Bodies.rectangle(w/2, h+25, w, 50, { isStatic: true, render: { visible: false } }),
    Bodies.rectangle(-25, h/2, 50, h, { isStatic: true, render: { visible: false } }),
    Bodies.rectangle(w+25, h/2, 50, h, { isStatic: true, render: { visible: false } }),
  ]);

  // Capsule bodies with image textures
  imageUrls.forEach((url, i) => {
    const body = Bodies.rectangle(
      w/2 + (Math.random() - 0.5) * 200,
      -100 - i * 80,
      120, 60,
      {
        chamfer: { radius: 30 },
        restitution: 0.5,
        render: {
          sprite: { texture: url, xScale: 0.5, yScale: 0.5 }
        }
      }
    );
    Composite.add(engine.world, body);
  });

  // Mouse drag
  const mouse = Mouse.create(render.canvas);
  Composite.add(engine.world, MouseConstraint.create(engine, {
    mouse, constraint: { stiffness: 0.2, render: { visible: false } }
  }));

  Runner.run(Runner.create(), engine);
  Render.run(render);
}
```

---

## 9. MASK ENTRY (image reveals with scroll mask)

```javascript
function initMaskEntry() {
  gsap.utils.toArray('.mask-entry-img').forEach(img => {
    gsap.fromTo(img,
      { clipPath: 'inset(100% 0 0 0)' },
      {
        clipPath: 'inset(0% 0 0 0)',
        duration: 1.2,
        ease: 'power4.out',
        scrollTrigger: {
          trigger: img,
          start: 'top 80%',
          toggleActions: 'play none none reverse'
        }
      }
    );
  });
}
```

---

## 10. PAINTING ANIMATION (brush stroke reveal)

```javascript
function initPaintingReveal() {
  document.querySelectorAll('.painting-reveal').forEach(el => {
    gsap.fromTo(el,
      { clipPath: 'polygon(0 0, 0 0, 0 100%, 0 100%)' },
      {
        clipPath: 'polygon(0 0, 100% 0, 100% 100%, 0 100%)',
        duration: 1.5,
        ease: 'power2.out',
        scrollTrigger: {
          trigger: el,
          start: 'top 75%'
        }
      }
    );
  });
}
```
