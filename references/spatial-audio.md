# Spatial Audio-Reactive Environments
## The Browser as a Synthesizer (2026 Standard)

A SOTY-level site should sound as good as it looks. In 2026, background music is replaced by **Generative Soundscapes**.

---

## 1. THE WEB AUDIO ENGINE
Instead of a `.mp3` loop, we use a synthesized engine that reacts to the DOM.

```javascript
const audioContext = new (window.AudioContext || window.webkitAudioContext)();
const filter = audioContext.createBiquadFilter();
const oscillator = audioContext.createOscillator();

// Link to mouse/scroll
oscillator.connect(filter).connect(audioContext.destination);
```

---

## 2. SCROLL-DRIVEN MODULATION (The "Ocean" Effect)
Use the scroll position to open/close a Low Pass Filter.

```javascript
window.addEventListener('scroll', () => {
  const scrollRatio = window.scrollY / (document.body.scrollHeight - window.innerHeight);
  
  // As user scrolls down, more high frequencies are allowed in (brighter sound)
  const frequency = 200 + (scrollRatio * 2000);
  
  gsap.to(filter.frequency, {
    value: frequency,
    duration: 0.5,
    ease: 'power2.out'
  });
});
```

---

## 3. MOUSE-POSITION "PANNING"
Create a 3D audio space where sounds come from where your mouse is.

```javascript
const panner = audioContext.createPanner();
panner.panningModel = 'HRTF'; // Realistic 3D audio

window.addEventListener('mousemove', (e) => {
  const x = (e.clientX / window.innerWidth) * 2 - 1;
  const y = (e.clientY / window.innerHeight) * 2 - 1;
  
  panner.setPosition(x, y, 1); // Move the "sound" in virtual 3D space
});
```

---

## 4. THE PSYCHOLOGICAL EFFECT
The user feels "grounded" in the site. Movement has weight. This is the difference between a "page" and a "place".

---

## 5. ACCESSIBILITY NOTE:
- **Mandatory:** Always provide a visual `Mute` toggle in the top corner.
- **Mandatory:** Default to "Muted" (Browser autoplay policy). Use a "Start Experience" button to unlock audio context.
