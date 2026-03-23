# AI Asset & Vibe Pipeline

Awwwards-caliber websites require custom, high-fidelity assets. Use this AI-driven pipeline to generate world-class textures, UI elements, and backgrounds.

## Textures & Backgrounds (Midjourney/Flux)

### Prompts for "Awwwards" Aesthetics
- **Kinetic/Modern**: `Macro photography of iridescent liquid mercury, high-speed shutter, 8k, depth of field, studio lighting, avant-garde --v 6.0`
- **Archival/Museum**: `Scanned parchment texture, subtle ink bleeds, 18th-century typography imprint, ultra-high resolution, micro-details --tile`
- **Luxury**: `Brushed titanium with subtle vertical grain, soft charcoal gradients, elegant, minimalist --ar 16:9`

## UI Components & Icons (Stable Diffusion/DALL-E 3)

### Neo-Glassmorphism
- Generate custom blur maps and refraction textures for WebGL panels.
- Prompt: `Abstract glass refraction patterns, caustic light effects, clean white background, 4k.`

## 3D Asset Generation

### Text-to-Mesh (CSM/Meshy)
- Generate base low-poly meshes for background decorations and environment details.
- Optimize immediately using `meshoptimizer` or `Draco`.

## Vibe Adaptation (Transformers.js)

### Behavioral AI
Use small, on-device models to classify user behavior:
- **Fast Scroller** → Trigger "Kinetic" vibe (high speed, sharp motions).
- **Idle/Slow Reader** → Trigger "Zen" vibe (drifting particles, soft blurs).

## The AI Polish Step
1. **Upscaling**: Use Topaz Photo AI or AI-Upscaler to reach 4k/8k texture clarity.
2. **De-noising**: Clean up AI artifacts before using assets in WebGL.
3. **PBR Generation**: Convert AI images into Roughness/Normal/Metalness maps using Materialize or similar tools.
