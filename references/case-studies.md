# Awwwards Case Studies
## Technique breakdowns of SOTY/SOTD winners

---

## SOTY 2025 — Lando Norris (landonorris.com)

**Agency:** Built by a creative development team
**Stack:** Next.js, R3F (React Three Fiber), GSAP, Three.js

### Key Techniques Used:
1. **High-energy hero** — Full-viewport video loop of racing footage, masked behind 3D text
2. **Dynamic scroll narrative** — Sections transition via scroll-driven 3D camera movements
3. **Bold typography** — Custom display font at massive viewport-width sizes (`font-size: 12vw`)
4. **Color system** — McLaren papaya orange (#FF8000) as accent on deep dark (#0A0A0A)
5. **Stats with counter animation** — Career stats animate with GSAP counter scroll
6. **3D helmet model** — Interactive Three.js model with mouse parallax
7. **Performance** — Despite heavy 3D, maintains 60fps via instanced meshes + lazy loading
8. **Mobile adaptation** — 3D removed on mobile, replaced with video + images

### Takeaways:
- Athletes/sports sites should feel like the ENERGY of the sport
- 3D is accent, not the whole page — most content is still 2D
- Performance is non-negotiable even with heavy visuals

---

## SOTY 2024 — Igloo Inc.

**Stack:** Custom WebGL, vanilla JS, Howler.js

### Key Techniques Used:
1. **Sound design** — Ambient audio + hover sounds create full sensory experience
2. **Scroll-driven 3D** — GSAP ScrollTrigger driving Three.js camera path
3. **Immersive navigation** — No traditional nav; the scroll IS the navigation
4. **Micro-interactions everywhere** — Every element responds to hover/mouse
5. **Loading experience** — Branded counter loader with percentage
6. **Color restraint** — Near-monochrome with single accent color

### Takeaways:
- Sound design elevates beyond visual
- Restraint in color = sophistication
- The scroll journey should tell a story

---

## COMMON PATTERNS IN SOTY/SOTD WINNERS

| Pattern | Frequency |
|---|---|
| Custom cursor with blend mode | 85% |
| Film grain / noise overlay | 75% |
| Smooth scroll (Lenis/ScrollSmoother) | 90% |
| SplitText character animations | 80% |
| Loading screen with counter | 70% |
| Sound design | 40% (rising) |
| 3D elements (Three.js/R3F) | 65% |
| Dark mode or dark-dominant palette | 60% |
| Horizontal scroll section | 45% |
| Magnetic buttons | 55% |

---

## SCORING BREAKDOWN (what judges look for)

| Category | Weight | What wins |
|---|---|---|
| Design | 40% | Visual coherence, typography, color, spacing, originality |
| Usability | 30% | Navigation clarity, accessibility, mobile adaptation, load time |
| Creativity | 20% | Novel interactions, unique concepts, emotional impact |
| Content | 10% | Copy quality, information architecture, storytelling |

### Scoring tips:
- **Design:** Pick ONE archetype and commit fully. Mixing = mediocrity
- **Usability:** Skip-to-content link, `prefers-reduced-motion`, WCAG AA contrast
- **Creativity:** One signature interaction is better than 20 generic ones
- **Content:** Real content always. Placeholder text = automatic point loss
