# The Final Quality Audit (SOTY Checklist)
## 50 Points to Site of the Year (2026)

Before you submit to Awwwards, your site must pass this "Elite" internal audit.

---

## 1. PERFORMANCE & CORE WEB VITALS
- [ ] Lighthouse Performance Score: **95+**
- [ ] LCP (Largest Contentful Paint): **< 1.2s** (Targeting 1.0s)
- [ ] INP (Interaction to Next Paint): **< 200ms** (Critical for JS-heavy sites)
- [ ] CLS (Cumulative Layout Shift): **0.0**
- [ ] **Dispose Protocol**: All WebGL geometries/textures are disposed of on unmount (No iOS Safari crashes).
- [ ] **3-Tier AQS**: Dynamic quality scaling (High, Med, Potato) based on real-time FPS monitoring.
- [ ] **Edge Delivery**: Assets served with `Cache-Control: immutable` headers via Vercel/Netlify.

## 2. SEO & DISCOVERABILITY
- [ ] **Semantic Mirroring**: Hidden `section` contains all critical text content from the WebGL/Canvas layer for indexing.
- [ ] **JSON-LD Schema**: `CreativeWork` and `Award` schema implemented for Rich Results.
- [ ] **Meta-Vibe**: OpenGraph/Social previews utilize high-fidelity AI-generated assets.

## 3. ASSET PIPELINE & AI POLISH
- [ ] **PBR Verification**: AI-generated textures converted to Normal/Roughness/Metalness maps.
- [ ] **De-noising**: No AI artifacts or "fuzziness" visible in high-resolution textures.
- [ ] **Asset Cascades**: Serving `.webp` or `.avif` with fallback logic for older browsers.

## 4. THE "AWE" FACTOR (CREATIVITY)
- [ ] At least one "Impossible" feature implemented (AI-Vibe, Portal, etc.).
- [ ] Page transitions occur in < 600ms (no "long waits").
- [ ] Hover states are delayed by 50ms to prevent "interaction spray".
- [ ] Text reveals are staggered and consistent.

## 5. ACCESSIBILITY & INCLUSIVITY
- [ ] `prefers-reduced-motion` respected (animations stop/simplify).
- [ ] Keyboard navigation (Tab) handles the 3D menu correctly.
- [ ] Color contrast passes AA (except for intentional artistic choice).
- [ ] Mute toggle for all audio/synthesizers.

---

## 6. THE "SOTD" SUBMISSION TIP
- **Video Submission:** Awwwards judges often look at the video preview first. Record your site at 4K @ 60fps using a lossless codec. Show the "Signature Moment" in the first 3 seconds.
- **Project Description:** Use the "Case Study" archetype to tell a story about the technical challenges solved.
