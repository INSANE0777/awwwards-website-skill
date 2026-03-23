# Text Effects
## Scramble, gradient, clip mask, split, parallax, along path, and more

---

## 1. TEXT SCRAMBLE (decode/glitch reveal)

```javascript
function textScramble(el, finalText, duration = 1500) {
  const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*';
  const steps = 30;
  let step = 0;

  const interval = setInterval(() => {
    const progress = step / steps;
    let output = '';

    for (let i = 0; i < finalText.length; i++) {
      if (i < finalText.length * progress) {
        output += finalText[i];
      } else {
        output += chars[Math.floor(Math.random() * chars.length)];
      }
    }

    el.textContent = output;
    step++;
    if (step > steps) {
      clearInterval(interval);
      el.textContent = finalText;
    }
  }, duration / steps);
}

// Usage with ScrollTrigger
function initTextScrambleOnScroll() {
  document.querySelectorAll('[data-scramble]').forEach(el => {
    const text = el.textContent;
    ScrollTrigger.create({
      trigger: el,
      start: 'top 80%',
      onEnter: () => textScramble(el, text)
    });
  });
}
```

---

## 2. TEXT GRADIENT (animated gradient text)

```css
.text-gradient {
  background: linear-gradient(90deg, var(--accent), #8B5CF6, #06B6D4, var(--accent));
  background-size: 300% 100%;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: gradientShift 4s ease infinite;
}

@keyframes gradientShift {
  0%   { background-position: 0% 50%; }
  50%  { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}
```

### Scroll-driven gradient opacity (word by word)
```javascript
function initTextGradientOpacity() {
  document.querySelectorAll('.scroll-gradient-text').forEach(el => {
    const words = el.textContent.split(' ');
    el.innerHTML = words.map(w => `<span class="sgw">${w} </span>`).join('');

    gsap.from(el.querySelectorAll('.sgw'), {
      opacity: 0.1,
      stagger: 0.08,
      ease: 'none',
      scrollTrigger: {
        trigger: el,
        start: 'top 80%',
        end: 'bottom 30%',
        scrub: 1
      }
    });
  });
}
```

---

## 3. TEXT CLIP MASK ON SCROLL

```css
.clip-text {
  -webkit-text-stroke: 1px var(--ink);
  -webkit-text-fill-color: transparent;
  position: relative;
}
.clip-text-fill {
  position: absolute; inset: 0;
  color: var(--ink);
  clip-path: inset(0 100% 0 0);
}
```

```javascript
function initTextClipMask() {
  gsap.to('.clip-text-fill', {
    clipPath: 'inset(0 0% 0 0)',
    ease: 'none',
    scrollTrigger: {
      trigger: '.clip-text',
      start: 'top 70%',
      end: 'bottom 40%',
      scrub: 1
    }
  });
}
```

---

## 4. SPLIT TEXT REVEAL (character/word/line)

```javascript
function initSplitTextReveal() {
  document.querySelectorAll('[data-split]').forEach(el => {
    const type = el.dataset.split || 'chars'; // 'chars', 'words', 'lines'
    const split = new SplitType(el, { types: type });

    gsap.from(split[type], {
      opacity: 0,
      y: type === 'lines' ? 40 : 20,
      rotateX: type === 'chars' ? 90 : 0,
      stagger: type === 'chars' ? 0.02 : type === 'words' ? 0.05 : 0.1,
      duration: 0.8,
      ease: 'power3.out',
      scrollTrigger: {
        trigger: el,
        start: 'top 85%',
        toggleActions: 'play none none reverse'
      }
    });
  });
}
```

---

## 5. MASK SPLIT TEXT (reveal from behind mask)

```css
.mask-text-container { overflow: hidden; }
.mask-text-line {
  display: block;
  transform: translateY(110%);
}
```

```javascript
function initMaskSplitText() {
  document.querySelectorAll('.mask-text').forEach(el => {
    // Wrap each line in an overflow-hidden container
    const lines = el.innerHTML.split('<br>');
    el.innerHTML = lines.map(line =>
      `<span class="mask-text-container"><span class="mask-text-line">${line}</span></span>`
    ).join('');

    gsap.to(el.querySelectorAll('.mask-text-line'), {
      y: 0,
      duration: 1,
      stagger: 0.1,
      ease: 'power4.out',
      scrollTrigger: {
        trigger: el,
        start: 'top 80%'
      }
    });
  });
}
```

---

## 6. TEXT PARALLAX (horizontal drift on scroll)

```javascript
function initTextParallax() {
  document.querySelectorAll('.text-drift').forEach(el => {
    const direction = el.dataset.direction === 'right' ? 1 : -1;
    const speed = parseFloat(el.dataset.speed) || 100;

    gsap.to(el, {
      x: direction * speed,
      ease: 'none',
      scrollTrigger: {
        trigger: el,
        start: 'top bottom',
        end: 'bottom top',
        scrub: 0.5
      }
    });
  });
}
```

```html
<div class="text-drift" data-direction="left" data-speed="200">CREATIVITY</div>
<div class="text-drift" data-direction="right" data-speed="150">INNOVATION</div>
```

---

## 7. TEXT ALONG PATH (SVG)

```html
<svg viewBox="0 0 500 200" class="text-along-path">
  <defs>
    <path id="textCurve" d="M20,150 Q250,20 480,150" fill="none" />
  </defs>
  <text font-size="18" fill="var(--ink)" letter-spacing="0.2em">
    <textPath href="#textCurve" class="moving-text">
      FOLLOW THE CURVE • FOLLOW THE CURVE • FOLLOW THE CURVE •
    </textPath>
  </text>
</svg>
```

```javascript
function initTextAlongPath() {
  gsap.to('.moving-text', {
    attr: { startOffset: '100%' },
    ease: 'none',
    scrollTrigger: {
      trigger: '.text-along-path',
      start: 'top 80%',
      end: 'bottom 20%',
      scrub: 1
    }
  });
}
```

---

## 8. TEXT SCRAMBLE v2 (typewriter with cursor)

```javascript
function typewriter(el, text, speed = 50) {
  let i = 0;
  el.textContent = '';
  el.style.borderRight = '2px solid var(--accent)';

  const type = setInterval(() => {
    el.textContent += text[i];
    i++;
    if (i >= text.length) {
      clearInterval(type);
      // Blink cursor
      gsap.to(el, {
        borderRightColor: 'transparent',
        repeat: -1,
        yoyo: true,
        duration: 0.5,
        ease: 'steps(1)'
      });
    }
  }, speed);
}
```

---

## 9. TEXT DISPERSE / EXPLODE

```javascript
function initTextExplode() {
  document.querySelectorAll('[data-explode]').forEach(el => {
    const chars = el.textContent.split('');
    el.innerHTML = chars.map(c =>
      `<span style="display:inline-block">${c === ' ' ? '&nbsp;' : c}</span>`
    ).join('');

    gsap.from(el.querySelectorAll('span'), {
      x: () => (Math.random() - 0.5) * window.innerWidth,
      y: () => (Math.random() - 0.5) * window.innerHeight,
      rotation: () => (Math.random() - 0.5) * 720,
      scale: 0,
      opacity: 0,
      duration: 1.5,
      stagger: 0.02,
      ease: 'power4.out',
      scrollTrigger: {
        trigger: el,
        start: 'top 80%'
      }
    });
  });
}
```
