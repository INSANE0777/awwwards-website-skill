# React Three Fiber (R3F) Recipes
## 3D components for React using @react-three/fiber and @react-three/drei

R3F is the dominant 3D framework in 2025 Awwwards sites. Used by ToyFight, Lusion, and most creative agencies.

```bash
npm install @react-three/fiber @react-three/drei @react-three/postprocessing three
```

---

## 1. BASIC SCENE

```jsx
import { Canvas } from '@react-three/fiber';
import { OrbitControls, PerspectiveCamera } from '@react-three/drei';

function Scene() {
  return (
    <Canvas style={{ position: 'fixed', inset: 0, zIndex: -1 }}>
      <PerspectiveCamera makeDefault position={[0, 0, 5]} />
      <ambientLight intensity={0.5} />
      <directionalLight position={[5, 5, 5]} />
      <OrbitControls enableZoom={false} />
      <FloatingMesh />
    </Canvas>
  );
}
```

---

## 2. FLOATING MESH WITH MOUSE TRACKING

```jsx
import { useRef } from 'react';
import { useFrame } from '@react-three/fiber';

function FloatingMesh() {
  const mesh = useRef();
  const mouse = useRef({ x: 0, y: 0 });

  useEffect(() => {
    const handler = e => {
      mouse.current.x = (e.clientX / window.innerWidth) * 2 - 1;
      mouse.current.y = -(e.clientY / window.innerHeight) * 2 + 1;
    };
    window.addEventListener('mousemove', handler);
    return () => window.removeEventListener('mousemove', handler);
  }, []);

  useFrame((state, delta) => {
    mesh.current.rotation.x += delta * 0.1;
    mesh.current.rotation.y += delta * 0.15;
    mesh.current.position.x += (mouse.current.x * 2 - mesh.current.position.x) * 0.05;
    mesh.current.position.y += (mouse.current.y * 2 - mesh.current.position.y) * 0.05;
  });

  return (
    <mesh ref={mesh}>
      <torusKnotGeometry args={[1, 0.3, 128, 32]} />
      <meshPhysicalMaterial color="#6366f1" metalness={0.3} roughness={0.2} clearcoat={1} />
    </mesh>
  );
}
```

---

## 3. PARTICLES WITH INSTANCED MESH

```jsx
import { useMemo, useRef } from 'react';
import { useFrame } from '@react-three/fiber';
import * as THREE from 'three';

function Particles({ count = 3000 }) {
  const mesh = useRef();
  const dummy = useMemo(() => new THREE.Object3D(), []);

  const particles = useMemo(() =>
    Array.from({ length: count }, () => ({
      position: [(Math.random() - 0.5) * 10, (Math.random() - 0.5) * 10, (Math.random() - 0.5) * 10],
      speed: Math.random() * 0.01 + 0.005
    })), [count]);

  useFrame(state => {
    particles.forEach((p, i) => {
      dummy.position.set(...p.position);
      dummy.position.y += Math.sin(state.clock.elapsedTime * p.speed * 100 + i) * 0.002;
      dummy.updateMatrix();
      mesh.current.setMatrixAt(i, dummy.matrix);
    });
    mesh.current.instanceMatrix.needsUpdate = true;
  });

  return (
    <instancedMesh ref={mesh} args={[null, null, count]}>
      <sphereGeometry args={[0.02, 8, 8]} />
      <meshBasicMaterial color="#818cf8" transparent opacity={0.6} />
    </instancedMesh>
  );
}
```

---

## 4. GLASS MATERIAL (PHYSICAL)

```jsx
function GlassSphere() {
  return (
    <mesh>
      <sphereGeometry args={[1, 64, 64]} />
      <meshPhysicalMaterial
        transmission={0.95} thickness={0.5}
        roughness={0.05} metalness={0}
        ior={1.5} clearcoat={1}
        color="white"
      />
    </mesh>
  );
}
```

---

## 5. POST-PROCESSING

```jsx
import { EffectComposer, Bloom, ChromaticAberration, Noise, Vignette } from '@react-three/postprocessing';

function Effects() {
  return (
    <EffectComposer>
      <Bloom intensity={0.5} luminanceThreshold={0.8} luminanceSmoothing={0.9} />
      <ChromaticAberration offset={[0.002, 0.002]} />
      <Noise opacity={0.03} />
      <Vignette offset={0.3} darkness={0.5} />
    </EffectComposer>
  );
}
```

---

## 6. SCROLL-LINKED 3D (with GSAP)

```jsx
import { useEffect, useRef } from 'react';
import { useFrame } from '@react-three/fiber';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

function ScrollScene() {
  const group = useRef();
  const scrollProgress = useRef(0);

  useEffect(() => {
    gsap.registerPlugin(ScrollTrigger);
    ScrollTrigger.create({
      start: 0, end: 'max', scrub: true,
      onUpdate: self => { scrollProgress.current = self.progress; }
    });
  }, []);

  useFrame(() => {
    group.current.rotation.y = scrollProgress.current * Math.PI * 2;
    group.current.position.y = scrollProgress.current * -5;
  });

  return <group ref={group}>{/* 3D content */}</group>;
}
```

---

## 7. TEXT3D

```jsx
import { Text3D, Center } from '@react-three/drei';

function Title3D() {
  return (
    <Center>
      <Text3D font="/fonts/inter_bold.json" size={1} height={0.2}
        curveSegments={12} bevelEnabled bevelThickness={0.02} bevelSize={0.02}>
        HELLO
        <meshPhysicalMaterial color="#6366f1" metalness={0.5} roughness={0.2} />
      </Text3D>
    </Center>
  );
}
```

---

## DREI HELPERS CHEAT SHEET

| Component | Use |
|---|---|
| `<Float>` | Floating animation |
| `<MeshDistortMaterial>` | Blobby distortion |
| `<MeshWobbleMaterial>` | Wobbly mesh |
| `<Sparkles>` | Particle sparkles |
| `<Stars>` | Star field background |
| `<Environment>` | HDR environment map |
| `<Html>` | HTML inside 3D scene |
| `<useGLTF>` | Load 3D models |
| `<ContactShadows>` | Ground shadows |
