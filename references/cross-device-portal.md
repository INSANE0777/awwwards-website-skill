# Cross-Device "Portal" Control
## Turning the mobile phone into a spatial interface (2026 Standard)

This technique creates a "Magic Mirror" effect where the phone controls the desktop experience in real-time.

---

## 1. THE ARCHITECTURE
1. **Desktop:** A Three.js 3D world with a unique `Room ID`.
2. **QR Code:** Generated on the desktop hero, encoding the `Room ID` and the controller URL.
3. **Mobile:** A simple web page that listens to `DeviceOrientationEvent` and sends data via **WebSockets (Socket.io / PeerJS)**.

---

## 2. THE EMITTER (Mobile Controller)
```javascript
// On mobile controller page
window.addEventListener('deviceorientation', (event) => {
  const data = {
    alpha: event.alpha, // Rotation around Z axis (compass)
    beta: event.beta,   // Rotation around X axis (front/back)
    gamma: event.gamma  // Rotation around Y axis (left/right)
  };
  socket.emit('sensor-data', { roomId, data });
});
```

---

## 3. THE RECEIVER (Desktop Canvas)
```javascript
// In the desktop Three.js scene
socket.on('sensor-data', (data) => {
  // Map beta/gamma to camera rotation or object movement
  const targetRotationX = THREE.MathUtils.degToRad(data.beta);
  const targetRotationY = THREE.MathUtils.degToRad(data.gamma);
  
  gsap.to(camera.rotation, {
    x: targetRotationX,
    y: targetRotationY,
    duration: 0.1,
    ease: 'power2.out'
  });
});
```

---

## 4. SIGNATURE MOMENT: "THE VIRTUAL FLASHLIGHT"
The phone becomes a light source. As the user points the phone, a `PointLight` in the desktop 3D scene moves in real-time, casting shadows on the portfolio.

---

## 5. WHY THIS WINS AWWWARDS:
- **Novelty:** 99% of sites are 2D mouse-based.
- **Engagement:** The user is physically moving to interact with the brand.
- **Shareability:** "Look at this, I can control my PC with my phone".
