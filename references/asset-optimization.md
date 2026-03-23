# Asset Optimization Pipeline (2026)
## Achieving SOTY visual quality with <2MB payloads

Elite sites must be fast. This module covers the 2026 standards for compressing high-end assets.

---

## 1. 3D MODELS (Draco + Meshopt)
Never load raw `.obj` or uncompressed `.gltf`.

- **Standard:** Use `.glb` with **Draco Compression**.
- **The Pipeline:** 
  ```bash
  # Using gltf-pipeline
  npx gltf-pipeline -i model.glb -o compressed.glb -d
  ```
- **Rule of Thumb:** A complex character should be <1MB. Environment chunks <500KB.

---

## 2. CINEMATIC VIDEO (AV1 / HEVC)
Awwwards judges hate buffering.

- **Desktop:** Use **AV1** (30% better than HEVC).
- **Safari/iOS:** Fallback to **HEVC (H.265)**.
- **The FFmpeg recipe:**
  ```bash
  ffmpeg -i input.mp4 -c:v libsvtav1 -preset 10 -crf 35 -an output_av1.mp4
  ```
- **Muted Backgrounds:** Strip audio tracks entirely to save ~10% bitrate.

---

## 3. TEXTURES (Basis Universal / KTX2)
GPU memory is limited, especially on mobile.

- **Basis Universal:** Compresses textures for the GPU, not just the disk. 
- **Benefit:** Reduces VRAM usage by 4x-6x, preventing crashes on older iPhones.
- **Implementation:** Use `KTX2Loader` in Three.js.

---

## 4. IMAGE CASCADES
- **Hero Images:** AVIF (Primary) > WebP (Secondary) > JPG (Legacy).
- **Blur-to-Focus:** Load a 20px blurred JPG first, then swap with the high-res AVIF.
