# Sound Design & Web Audio
## Ambient sounds, hover/click audio, music players, and audio-reactive visuals

Igloo Inc (SOTY 2024) proved: **sound transforms a website into an experience**.

---

## CDN
```html
<script src="https://unpkg.com/howler@2.2.4/dist/howler.min.js"></script>
```

---

## 1. AMBIENT BACKGROUND SOUND

```javascript
function initAmbientSound() {
  const ambient = new Howl({
    src: ['ambient.mp3'],
    loop: true, volume: 0.15, autoplay: false
  });

  // User must interact first (browser policy)
  let started = false;
  document.addEventListener('click', () => {
    if (!started) { ambient.play(); started = true; }
  }, { once: true });

  // Mute toggle
  const muteBtn = document.getElementById('soundToggle');
  muteBtn?.addEventListener('click', () => {
    ambient.mute(!ambient.mute());
    muteBtn.classList.toggle('muted');
  });
}
```

```html
<button class="sound-toggle" id="soundToggle" aria-label="Toggle sound">
  <svg class="sound-on" width="20" height="20" viewBox="0 0 24 24" fill="none"
       stroke="currentColor" stroke-width="2">
    <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"/>
    <path d="M19.07 4.93a10 10 0 0 1 0 14.14M15.54 8.46a5 5 0 0 1 0 7.07"/>
  </svg>
</button>
```

```css
.sound-toggle {
  position: fixed; bottom: 2rem; right: 2rem; z-index: 100;
  width: 44px; height: 44px; border: 1px solid var(--border);
  border-radius: 50%; background: var(--bg); cursor: pointer;
  display: flex; align-items: center; justify-content: center;
  transition: opacity 0.3s;
}
.sound-toggle.muted svg { opacity: 0.3; }
```

---

## 2. HOVER SOUND EFFECTS

```javascript
function initHoverSounds() {
  const hover = new Howl({ src: ['hover.mp3'], volume: 0.05 });
  const click = new Howl({ src: ['click.mp3'], volume: 0.1 });

  document.querySelectorAll('a, button, [data-sound]').forEach(el => {
    el.addEventListener('mouseenter', () => hover.play());
    el.addEventListener('click', () => click.play());
  });
}
```

---

## 3. SCROLL-TRIGGERED SOUND

```javascript
function initScrollSound() {
  const whoosh = new Howl({ src: ['whoosh.mp3'], volume: 0.08 });

  document.querySelectorAll('[data-sound-trigger]').forEach(el => {
    ScrollTrigger.create({
      trigger: el,
      start: 'top 60%',
      onEnter: () => whoosh.play(),
      once: true
    });
  });
}
```

---

## 4. WEB AUDIO API (no library, raw)

```javascript
function playTone(freq = 440, duration = 0.1) {
  const ctx = new (window.AudioContext || window.webkitAudioContext)();
  const osc = ctx.createOscillator();
  const gain = ctx.createGain();
  osc.connect(gain);
  gain.connect(ctx.destination);
  osc.frequency.value = freq;
  osc.type = 'sine';
  gain.gain.setValueAtTime(0.05, ctx.currentTime);
  gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + duration);
  osc.start(); osc.stop(ctx.currentTime + duration);
}

// Musical hover: each nav link plays a different note
document.querySelectorAll('.nav-link').forEach((link, i) => {
  const notes = [261, 293, 329, 349, 392, 440]; // C D E F G A
  link.addEventListener('mouseenter', () => playTone(notes[i], 0.15));
});
```

---

## 5. AUDIO-REACTIVE VISUALS

```javascript
function initAudioReactive(audioElement) {
  const ctx = new AudioContext();
  const source = ctx.createMediaElementSource(audioElement);
  const analyser = ctx.createAnalyser();
  analyser.fftSize = 256;
  source.connect(analyser);
  analyser.connect(ctx.destination);

  const data = new Uint8Array(analyser.frequencyBinCount);

  function update() {
    analyser.getByteFrequencyData(data);
    const avg = data.reduce((a, b) => a + b) / data.length / 255;

    // Scale elements based on audio
    document.querySelectorAll('.audio-reactive').forEach(el => {
      gsap.set(el, { scale: 1 + avg * 0.3, opacity: 0.5 + avg * 0.5 });
    });

    requestAnimationFrame(update);
  }
  update();
}
```

---

## RULES
1. **Never autoplay audio** — browsers block it. Require user interaction first
2. **Always provide a mute button** — fixed position, always visible
3. **Keep volumes LOW** — 0.05–0.15 range. Subtle, never jarring
4. **Respect `prefers-reduced-motion`** — also skip sound for reduced-motion users
5. **Use MP3 format** — widest browser support
6. **Preload small files** — hover/click sounds should be <50KB
