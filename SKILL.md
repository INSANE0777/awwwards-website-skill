---
name: awwwards-website
description: >
  Design and build Awwwards-caliber websites with cinematic animations, immersive interactions, and world-class visual execution. Use this skill whenever the user asks to build a premium, award-winning, or visually extraordinary website, portfolio, landing page, or digital experience. Trigger on: awwwards, award-winning website, cinematic website, immersive site, museum-quality, premium portfolio, GSAP animations, WebGL site, luxury website, gallery-style, interactive experience, redesign to awwwards level, high-end website, creative developer portfolio, Lusion-style, Bruno Simon style, antigravity, Stitch MCP, Three.js website, ScrollTrigger, cinematic scroll, immersive portfolio, digital exhibition, or any request emphasizing visual drama, storytelling, and interaction design. Also trigger for reference sites like alanmenken.com, bruno-simon.com, lusion.co, or any SOTY/SOTD-level work. MANDATORY — never skip for cinematic, immersive, or award-level web design.
---

# The Definitive Awwwards-Level Website Design Skill
## Based on SOTY/SOTD winner analysis, Codrops research, and real winner breakdowns (2022–2025)

**Read this file completely before writing code. The planning phase IS the work.**

---

## THE AWWWARDS SCORING SYSTEM (design for it)

Jury criteria with weights:
- **Design 40%** — typography, color, composition, visual identity
- **Usability 30%** — navigation clarity, mobile, accessibility, intuitiveness
- **Creativity 20%** — originality, concept, the unexpected
- **Content 10%** — storytelling, writing quality, relevance

**Developer Award** (separate): performance, SEO, accessibility, responsive quality, code standards.

SOTY winners' shared traits (Igloo Inc 2024, Lusion v3 2024, Noomo 2023, Mana Yerba Mate 2023):
1. Singular creative concept driving every decision
2. 3D or cinematic experience as the primary surface
3. Micro-interactions that reward curiosity
4. Mobile that feels designed, not compressed
5. Performance that matches the ambition

---

## PHASE 0 — RESEARCH & VISUAL ANALYSIS
### Handles: URLs · Uploaded screenshots · Images · Figma exports · Any design reference

**This phase runs FIRST — before any design decision, before any code — whenever the user provides a URL, uploads an image, shares a screenshot, or references a real site.**

---

### 0A — TRIGGER CONDITIONS

| What the user gives | What to do |
|---|---|
| A URL (`https://...`) | → Run URL Research (Section 0B) |
| A brand/site name (`"like Stripe"`) | → Search + fetch that site (Section 0B) |
| An uploaded screenshot/image | → Run Visual Analysis (Section 0C) |
| Both a URL AND an image | → Run 0B first, then 0C, merge findings |
| A Figma screenshot or wireframe | → Run Visual Analysis (Section 0C) |
| A design mood board or reference collage | → Run Visual Analysis (Section 0C) |
| None of the above | → Skip Phase 0, go to Phase 1 |

---

### 0B — URL RESEARCH

**Step 1 — Fetch the homepage**
Read `references/asset-sourcing.md` if the site is missing images or icons.
```
web_fetch(url)
```
Extract:
- Brand name and exact tagline
- Hero headline — exact wording
- Navigation items — every page/link in the nav
- All sections present on homepage (hero, features, about, CTA, footer, etc.)
- Color palette — any hex values, CSS variables, or color descriptions found
- Font names — look in `<link>` tags, CSS `font-family`, or visible font names
- Tone of copy — formal / editorial / playful / technical / warm / bold
- Key content — services, works, products, team members featured

**Step 2 — Fetch every sub-page linked in nav**
```
web_fetch(url/about)
web_fetch(url/work)    OR  url/projects  OR  url/portfolio
web_fetch(url/services)
web_fetch(url/contact)
web_fetch(url/news)    OR  url/blog
```
From each page extract:
- Real headlines and body copy
- Section order and structure
- Unique layout patterns or components
- Any team names, project names, client names, dates

**Step 3 — Deep search for missing context**
```
web_search("[brand name] website design colors typography")
web_search("[brand name] founder team about history")
web_search("[brand name] projects clients awards")
```
Use to fill any gaps the site itself didn't provide.

**Step 4 — Try to extract CSS/design tokens**
```
web_fetch(url + '/styles.css')   OR  any stylesheet URL found in the HTML
```
Look for:
- CSS custom properties (`--color-primary`, `--font-heading`)
- Tailwind config hints
- Font-face declarations
- Keyframe animation names (reveals animation style)

---

### 0C — VISUAL ANALYSIS (uploaded image / screenshot)

When the user uploads ANY image — screenshot, photo, Figma export, mood board, wireframe — analyze it completely before writing code. This is pixel-level forensic analysis.

**What to extract from every uploaded image:**

**Layout & Structure**
- Overall layout type: single column / multi-column / full-bleed / grid / asymmetric
- Header position: fixed top / sticky / hidden / side
- Navigation style: horizontal bar / hamburger / side drawer / overlay / none visible
- Hero structure: full viewport / split layout / text-only / image-dominant
- Section count and order visible in the screenshot
- Footer: visible or not, what's in it
- Spacing pattern: generous/minimal/dense, consistent gutters visible

**Color Palette — extract every color**
- Background color(s) — exact description or approximate hex
- Primary text color
- Secondary/muted text color
- Accent/highlight color(s)
- Button/CTA color
- Border/divider colors
- Any gradient directions and stops visible
- Overall palette mood: warm / cool / neutral / saturated / muted / dark / light

**Typography — analyze every text element visible**
- Hero headline: font style (serif/sans/slab/script), weight (thin/regular/bold), size (approximate), tracking (tight/normal/wide)
- Body text: font style, weight, size, line-height
- Labels/tags: uppercase? tracked? small?
- Any mix of fonts visible (serif + sans pairing?)
- Letter spacing on any element

**Visual Elements**
- Images: photography style (editorial/commercial/documentary/abstract), treatment (full-color/grayscale/duotone), cropping
- Icons: style (outline/filled/custom illustrated/emoji)
- Decorative elements: lines, shapes, textures, grain, noise, patterns
- Cards: present? border style? shadow style? rounded corners?
- Buttons: shape (pill/rectangular/text-only), size, border style

**Interactions visible or implied**
- Hover states if multiple states shown
- Active/selected states
- Animation hints (motion blur, transition arrows, overlay states)
- Mobile vs desktop — is this a mobile screenshot?

**Archetype identification**
- Which of the 26 archetypes does this most closely match?
- Any notable deviations from the archetype pattern?

---

### 0D — COMBINED ANALYSIS OUTPUT

After running 0B and/or 0C, output this complete report before any code:

```
## 🔎 Design Analysis Report

### Source
- URL researched: [url or "none"]
- Images analyzed: [count and description or "none"]

### Brand & Content
- Brand name: [found or inferred]
- Tagline: [exact or "not found"]
- Archetype: [number + name from archetypes.md]
- Tone: [formal / editorial / playful / technical / warm / bold]

### Pages & Structure
- Pages found/visible: [list every page]
- Navigation items: [exact labels found]
- Section order on homepage: [ordered list]

### Visual Identity
- Background: [color + description]
- Primary text: [color]
- Accent color: [color + where used]
- Additional palette: [any other colors]
- Overall palette mood: [warm/cool/dark/light/saturated/muted]

### Typography
- Display/Hero font: [name or description — serif/sans/weight/style]
- Body font: [name or description]
- Labels: [style description]
- Scale pattern: [dramatic/gentle/uniform]
- Notable type treatments: [uppercase labels? tracked? italic accents?]

### Layout
- Grid type: [single col / multi-col / asymmetric / full-bleed]
- Spacing: [generous / minimal / dense]
- Cards present: [yes/no — style if yes]
- Negative space: [heavy / moderate / minimal]

### Interactions & Animations (observed or implied)
- Cursor: [custom / default]
- Hover effects: [description]
- Scroll behavior: [smooth / standard / parallax hints]
- Animations implied: [any motion hints visible]
- Loader: [present / not visible]

### Content Extracted
- Hero headline: "[exact text]"
- Subheading: "[exact text]"
- CTA text: "[exact text]"
- About copy: "[key sentences]"
- Featured work/products: [names and descriptions]
- Contact info: [email / form / social]
- Any other real copy found: [list it]

### What I'll build
[Complete list: every page, every section, with the real content assigned to each]

### Questions before I proceed
[Any genuine ambiguities that need clarification — or "None, ready to build"]
```

**⏸ Output this report. Wait for approval. Then build.**

---

### 0E — RULES (non-negotiable)

**URL research rules:**
- ✅ Always `web_fetch` the actual URL — never approximate from memory
- ✅ Fetch every sub-page found in navigation
- ✅ Try to fetch the CSS stylesheet to extract design tokens
- ✅ Use real extracted copy verbatim — not paraphrased
- ❌ Never skip fetch and "remember" what a famous site looks like
- ❌ Never use Lorem ipsum if real content was found
- ❌ Never invent names, projects, or contacts — use `[PLACEHOLDER: description]` if not found

**Image analysis rules:**
- ✅ Analyze EVERY uploaded image before starting — don't skip any
- ✅ Extract colors to the most specific description possible (approximate hex if needed)
- ✅ Identify font style even if name is unknown ("heavy condensed sans-serif, likely Bebas Neue")
- ✅ Note every visible UI component — nothing is irrelevant
- ✅ If multiple images uploaded — analyze all of them, note inconsistencies between them
- ❌ Never assume a component exists that isn't visible in the image
- ❌ Never "improve" the design during clone mode — pixel-fidelity first
- ❌ Never start coding until the analysis report is shown and approved

**When to ask vs. infer:**
- If color is ambiguous between two options → name both, pick the more likely, note it
- If font is unknown → describe it precisely ("thin weight geometric sans, likely Inter or DM Sans")
- If layout is partially cut off → note what's visible and flag what's assumed
- If image quality is low → say so, extract what's possible, flag uncertain readings

**When web_fetch fails (error recovery):**
| Failure type | What to do |
|---|---|
| 403 / blocked by bot protection | Try `web_search("[brand] site design fonts colors layout"` + `web_search("[brand] screenshot")` to find cached/described versions |
| JS-rendered SPA (empty HTML returned) | The HTML will have `<div id="root"></div>` with no content — try fetching `/static/css/main.css` or `asset-manifest.json` for font/color clues, plus web_search for screenshots |
| Timeout / connection refused | Try `web_fetch("https://web.archive.org/web/*/[url]")` for a cached version |
| Partial content (only above-fold) | Work with what was returned, clearly flag everything below-fold as `[NOT FETCHED — assumed from context]` |
| 404 (sub-page doesn't exist) | Skip it, note it as "page not found", infer from homepage structure |

**Never silently fail** — always tell the user what was and wasn't fetched successfully.

---

## PHASE 1 — THE CREATIVE BRIEF (mandatory before any code)

If Phase 0 ran, most of these are already answered — confirm and proceed.
If no URL was given, extract from context or ask. Answer all 7:

1. **What is this site FOR?** (portfolio, product, artist, institution, brand)
2. **What should a visitor FEEL?** (awe, intimacy, trust, excitement, nostalgia, wonder)
3. **What is the ONE signature moment?** (the thing they'll screenshot)
4. **What archetype fits?** → `references/archetypes.md`
5. **What is the primary interaction model?** → Phase 3 below
6. **What output format?** → See Phase 4 — HTML, React/Next.js, Vue, Svelte, or other
7. **How many pages/sections?** → Always build the COMPLETE site — see Phase 4B

> **The test**: Remove all copy. Does the site still communicate WHO it's for and WHAT it stands for?
> If yes: proceed. If no: go back to the brief.

---

## PHASE 1B — MID-BUILD CORRECTIONS PROTOCOL

**When the user says "wrong X, change it" or "I don't like Y" during or after a build:**

Never restart from scratch. Never re-ask the entire brief. Follow this:

| User says | What to do |
|---|---|
| "Wrong color / change the color" | Ask: "Which specific element — background, text, accent, or all?" then apply surgically to CSS variables only |
| "Different font" | Swap the Google Fonts import + font-family declarations only. Keep all sizes, weights, spacing intact |
| "Make it darker / lighter" | Adjust the color palette in `:root` CSS variables — never touch layout or animations |
| "More padding / breathing room" | Only touch spacing values — nothing else |
| "Wrong font size" | Only touch the specific element's size — not the whole scale |
| "Add a section" | Append the new section — never rewrite existing sections |
| "Remove X" | Delete only X — never touch surrounding code |
| "More animations" | Add animations to existing elements — never restructure HTML |
| "Completely different style" | This IS a full rebuild — confirm with user, then re-run from Phase 1 |

**Rule: Every correction is surgical. Minimum change, maximum precision.**
**Rule: After any correction, re-run the relevant Phase 8 checklist items only.**
**Rule: Never "improve" unrequested parts while making a correction.**

---

## PHASE 2 — DESIGN SYSTEM (commit, then never waver)

### Color Philosophy — pick ONE, execute with discipline

| Name | Background | Accent | Best for |
|---|---|---|---|
| **Parchment** | `#F2EFE9` | deep red `#8B2500` or ochre | archival, composer, museum |
| **Monochrome Drama** | `#F8F8F8` near-white | electric hue at <5% use | agency, studio |
| **Deep Dark** | `#080808` near-black | luminous cyan, gold, white | 3D, tech studio, immersive |
| **Saturated Editorial** | bold BG (cobalt, forest, crimson) | white + one contrast | brand-forward, fashion |
| **Gradient Cinematic** | shifting dark gradient | atmospheric stops | film, music, narrative |

**Hard rules:**
- Never `#FFFFFF` → use `#F8F6F2` or `#F2EFE9`
- Never `#000000` → use `#1A1A1A` or `#080808`
- Max palette: 2 colors + 1 accent. Discipline creates luxury.
- Paper grain texture at 3–6% opacity = instant premium upgrade (see Phase 5.5)

### Typography — pick ONE pair, use dramatically

Full pairs in `references/typography-pairs.md`. Quick reference:

| Archetype | Display font | UI/Micro font |
|---|---|---|
| Archival/Museum | Cormorant Garamond 300 | Inter 500 |
| Editorial/Magazine | Playfair Display 700 | Libre Baskerville |
| Kinetic Agency | Bebas Neue or DM Serif Display | DM Sans 300 |
| Luxury/Fashion | Gilda Display or Cinzel | Raleway 200 |
| Experimental | Syne 800 | Syne Mono |

Scale must be DRAMATIC. If hero is 120px, body is 16px. No gentle progressions:
```css
:root {
  --text-hero:     clamp(5rem, 12vw, 11rem);
  --text-display:  clamp(2.5rem, 5vw, 5rem);
  --text-body:     1rem;
  --text-micro:    0.625rem; /* 10px, uppercase, letter-spacing: 0.3em */
}
```

Never use Inter/Roboto/Arial as a display font. Ever.

### Layout
- Negative space = composition gravity. Don't fill it.
- 100vh hero minimum. Desktop-first. Mobile is adaptation, not redesign.
- No cards, no generic rounded corners, no default shadows unless they ARE the concept.

### Animation Character — commit to ONE

| Character | Durations | Easing | Tools |
|---|---|---|---|
| **Cinematic** | 1.2–2.5s | `power3.out`, `power4.out` | GSAP |
| **Kinetic** | 0.3–0.8s | `cubic-bezier(0.77,0,0.175,1)` | GSAP timeline |
| **Organic** | 0.6–1.2s | `elastic.out(1,0.4)` | GSAP + spring |
| **Minimal** | 0.3–0.5s | `power2.out` | CSS only |
| **Immersive** | scroll-scrubbed | none (scrub) | Three.js + GSAP |

> [!TIP]
> For advanced GSAP control (Morph, Draw, SplitText), read `references/gsap-plugins.md`.
> For native scroll-driven effects, read `references/css-scroll-animations.md`.
> **For selling this skill as a professional product, read `references/sales-and-proposals.md`.**

Rule: Slower = more premium. If it looks fast, double the duration.

---

## PHASE 3 — INTERACTION ARCHITECTURE

### Choose Your Primary Model

**A) Panel-Based Exploration** → `references/panel-system.md`
Museum/archival style (alanmenken.com). Tall vertical panels, grayscale default,
hover-expand via `flex-grow`, dynamic text updates, video-on-hover, two-state transitions.

**B) Scroll-Driven Narrative** → `references/scroll-system.md`
Most SOTY sites. Lenis + GSAP ScrollTrigger pinned sections, SplitType text reveals,
scrubbed animations, camera paths in 3D scenes. See also `references/advanced-scroll-effects.md`.

**C) Cursor-Reactive** → `references/cursor-system.md`
Agency portfolios. Custom cursor dot + ring, mouse-position parallax, magnetic buttons,
optional image displacement shader. See also `references/mouse-effects.md`.

**D) 3D Immersive** → `references/threejs-system.md`
Lusion/Bruno Simon/Igloo level. Three.js canvas as primary surface, GSAP ScrollSmoother
+ ScrollTrigger for camera paths, custom GLSL shaders, FBO particles (GPU-computed).

**E) Two-State / Multi-Section**
Overview ↔ detail states. Opacity + transform only (NO layout changes). Randomized
per-panel stagger for non-mechanical feel. See panel-system.md for full implementation.

**F) Generative & Procedural** → `references/generative-art.md`
Flow fields, circle packing, or mesh gradients as a living background or interaction surface.

**G) Modern Native Navigation** → `references/view-transitions.md`
Use the browser's native View Transitions API for seamless page-to-page state changes.

### Microinteraction Hierarchy
1. Hover — 0.4–0.8s ease, subtle (opacity, slight scale, color shift)
2. Click — 0.1s immediate feedback + 0.8s slow payoff
3. Entrance — staggered reveal (never all at once)
4. Exit — always animate out (never instant hide)
5. Page transition — the brand's signature cinematic moment (`references/page-transitions.md`)

> [!TIP]
> For creative navigation and menus, read `references/menu-navigation.md`.

Never: bounce easing, `linear`, instant snap-to, identical stagger delays on all elements.

### The Sensory Layer
Don't just design for eyes. **Sound design** is the secret differentiator of SOTY winners.
Read `references/sound-design.md` for ambient audio and hover/click feedback patterns.

---

## PHASE 4 — OUTPUT FORMAT & COMPLETE SITE MANDATE

### 4A — CHOOSE THE RIGHT OUTPUT FORMAT

Never default to HTML. Match the format to the request:

| Request type | Output format |
|---|---|
| Quick demo, artifact, codepen-style | **Single HTML file** (CDN imports) |
| "Build me a website", portfolio, multi-page | **Next.js** (App Router, TypeScript) |
| React component or app | **React JSX** (Vite or CRA) |
| Vue project | **Vue 3** (Composition API) |
| Svelte project | **SvelteKit** |
| WordPress site | **WordPress theme** → `references/wordpress-adaptation.md` |
| Static site with CMS | **Next.js + content layer** |
| User says "just a page" or no framework specified | **Single HTML file** |

> [!NOTE]
> For multi-language sites, read `references/i18n-guide.md`.

### HTML CDN IMPORTS (single-file output)
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@studio-freight/lenis@1.0.42/dist/lenis.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/split-type@0.3.4/umd/index.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;1,400&family=Playfair+Display:wght@400;700&family=Pinyon+Script&family=Inter:wght@300;400;500&display=swap" rel="stylesheet"/>
```

### NEXT.JS STACK (multi-page sites)
```
Next.js 14+ App Router
TypeScript
Tailwind CSS
GSAP + @gsap/react (useGSAP hook)
Lenis (@studio-freight/lenis)
Framer Motion (for React-native animations)
```

### REACT CDN IMPORTS (JSX artifacts)
```jsx
// Available via import in artifacts:
import { useEffect, useRef } from "react"
// GSAP via CDN script tag in index.html, or:
import gsap from "gsap" // if bundler available
```

### Standard GSAP initialization (HTML & Next.js):
```javascript
gsap.registerPlugin(ScrollTrigger);
const lenis = new Lenis({
  duration: 1.4,
  easing: t => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
  smoothWheel: true
});
lenis.on('scroll', ScrollTrigger.update);
gsap.ticker.add(time => lenis.raf(time * 1000));
gsap.ticker.lagSmoothing(0);
```

---

## PHASE 4B — THE COMPLETE SITE MANDATE (critical — read this)

**A website is NOT a homepage. Build the COMPLETE site.**

When building any site, always deliver ALL of the following unless explicitly told otherwise:

### Every page that is linked or referenced MUST be built
- Navbar has "About" → build the About page
- Footer has "Contact" → build the Contact page  
- Navigation mentions "Work", "Projects", "Services" → build those pages
- Hero CTA says "See our work" → that link must go somewhere real

### Every page must share the design system
- Same font, same color palette, same grain texture, same animation character
- Page transitions between pages (GSAP crossfade or Next.js page transition)
- Active nav state shows which page you're on

### Minimum page set by site type:
| Site type | Pages to build |
|---|---|
| Portfolio | Home, Work/Projects, About, Contact |
| Agency | Home, Work, Services, About, Contact |
| Artist/Composer | Home, Works, Biography, Press, Contact |
| Product/SaaS | Home, Features, Pricing, About, Contact |
| Restaurant | Home, Menu, About, Reservations, Contact |
| Brand | Home, Story/About, Products/Collection, Contact |
| Blog/Editorial | Home, Articles index, Article detail, About |

### Every page needs:
- ✅ Consistent header/navigation (same on all pages)
- ✅ Page-specific hero or title section
- ✅ Real content (not "Lorem ipsum" — write actual copy that fits the brand)
- ✅ Consistent footer
- ✅ Entrance animation when the page loads
- ✅ Page transition out when leaving

### Navigation must work
```javascript
// All nav links must point to real pages or sections
// Use smooth scroll for same-page anchors
// Use page transitions for cross-page navigation
// Active state: highlight current page in nav

// In HTML single-file: use hash routing or show/hide sections
function navigateTo(page) {
  // Exit current page
  gsap.to('.page-current', {
    opacity: 0, y: -20, duration: 0.5, ease: 'power2.in',
    onComplete: () => {
      document.querySelector('.page-current').classList.remove('page-current', 'active')
      const next = document.querySelector(`[data-page="${page}"]`)
      next.classList.add('page-current', 'active')
      // Enter new page
      gsap.fromTo(next,
        { opacity: 0, y: 20 },
        { opacity: 1, y: 0, duration: 0.7, ease: 'power3.out' }
      )
    }
  })
}
```

### Content — never use placeholder text
Write real, fitting copy for:
- Hero headline and subheading
- About section biography or brand story
- Service/work descriptions
- Contact section CTA
- Footer tagline

> [!TIP]
> Use `references/text-effects.md`, `references/image-effects.md`, and `references/svg-animations.md` to bring this content to life.
> **For optimized AI-human collaboration on high-end builds, read `references/ai-pairing-optimization.md`.**

Copy must match the archetype's emotional tone — archival sites get literary prose, kinetic agencies get sharp manifestos, luxury brands get understated elegance.

---

## PHASE 5 — SIGNATURE TECHNIQUES (implement ≥2 per site)

> [!IMPORTANT]
> Choose techniques that align with your archetype.
> For texture and grain rules, read `references/noise-textures.md`.
> For particle and background effects, read `references/particle-systems.md`.
> For post-processing effects (bloom, glitch, grain), read `references/post-processing.md`.
> **For cutting-edge GLSL (FBO, Noise Fields, Raymarching), read `references/advanced-shader-blueprints.md`.**

### 5.1 Branded Number Loader
```javascript
function initLoader(onComplete) {
  const loader = document.querySelector('.loader');
  const count = document.querySelector('.loader-count');
  let n = 0;
  const t = setInterval(() => {
    n += Math.floor(Math.random() * 12) + 3;
    if (n >= 100) {
      n = 100; clearInterval(t);
      gsap.to(loader, {
        opacity: 0, duration: 0.9, delay: 0.2, ease: 'power2.inOut',
        onComplete: () => { loader.style.display = 'none'; onComplete?.(); }
      });
    }
    count.textContent = n;
  }, 55);
}
```

### 5.2 Custom Cursor
```css
body { cursor: none; }
.c-dot  { position:fixed; width:6px; height:6px; background:#1A1A1A; border-radius:50%;
          pointer-events:none; z-index:10000; transform:translate(-50%,-50%); }
.c-ring { position:fixed; width:36px; height:36px; border:1px solid rgba(26,26,26,0.45);
          border-radius:50%; pointer-events:none; z-index:10000; transform:translate(-50%,-50%); }
```
```javascript
function initCursor() {
  const dot = document.querySelector('.c-dot'), ring = document.querySelector('.c-ring');
  window.addEventListener('mousemove', e => {
    gsap.to(dot,  { x: e.clientX, y: e.clientY, duration: 0 });
    gsap.to(ring, { x: e.clientX, y: e.clientY, duration: 0.18, ease: 'power2.out' });
  });
  document.querySelectorAll('a,button,[data-cursor]').forEach(el => {
    el.addEventListener('mouseenter', () => gsap.to(ring, { scale: 2.8, opacity: 0.5, duration: 0.3 }));
    el.addEventListener('mouseleave', () => gsap.to(ring, { scale: 1,   opacity: 1,   duration: 0.4 }));
  });
}
```

### 5.3 Cinematic Char-by-Char Text Reveal
```javascript
function revealText(selector, delay = 0) {
  const split = new SplitType(selector, { types: 'chars' });
  return gsap.from(split.chars, {
    opacity: 0, y: 60, rotateX: -45,
    transformOrigin: '0% 50% -20px',
    duration: 1.1, stagger: 0.025, ease: 'power4.out', delay
  });
}
```

### 5.4 Magnetic Buttons
```javascript
document.querySelectorAll('[data-magnetic]').forEach(el => {
  el.addEventListener('mousemove', e => {
    const r = el.getBoundingClientRect();
    gsap.to(el, {
      x: (e.clientX - r.left  - r.width/2)  * 0.38,
      y: (e.clientY - r.top - r.height/2) * 0.38,
      duration: 0.4, ease: 'power2.out'
    });
  });
  el.addEventListener('mouseleave', () => {
    gsap.to(el, { x: 0, y: 0, duration: 0.8, ease: 'elastic.out(1, 0.35)' });
  });
});
```

### 5.5 Paper Grain (archival/editorial/organic archetypes only)

**Use for:** Archetypes 01, 03, 04, 06, 11, 15, 16, 17, 26
**Never use for:** Archetypes 02, 07, 08, 09, 12, 13, 14, 18, 19, 21, 23, 24, 25 — grain fights their aesthetic
```html
<div class="grain" aria-hidden="true"></div>
```
```css
.grain {
  position:fixed; inset:0; pointer-events:none; z-index:999; opacity:0.045;
  background-image:url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}
```

### 5.6 Clip-Path Image Reveal
```javascript
gsap.utils.toArray('[data-reveal-img]').forEach(img => {
  gsap.fromTo(img,
    { clipPath: 'inset(100% 0% 0% 0%)' },
    { clipPath: 'inset(0% 0% 0% 0%)', duration: 1.4, ease: 'power4.out',
      scrollTrigger: { trigger: img, start: 'top 85%' } }
  );
});
```

### 5.7 Randomized Cinematic Panel Exit/Entrance
```javascript
function exitPanels(panels, cb) {
  panels.forEach((p, i) => {
    gsap.to(p, {
      opacity: 0, scale: 0.88 + Math.random() * 0.06,
      duration: 0.55 + Math.random() * 0.25,
      delay: 0.04 + Math.random() * 0.28,
      ease: 'power2.inOut',
      onComplete: i === panels.length - 1 ? cb : null
    });
  });
}
function enterPanels(panels, targetOpacity = 0.35) {
  panels.forEach(p => {
    gsap.set(p, { opacity: 0, scale: 0.9 });
    gsap.to(p, {
      opacity: targetOpacity, scale: 1,
      duration: 0.9 + Math.random() * 0.35,
      delay: 0.06 + Math.random() * 0.32,
      ease: 'power3.out'
    });
  });
}
```

### 5.8 Scroll-Pinned Scrub Section
```javascript
gsap.timeline({
  scrollTrigger: {
    trigger: '.pin-section', start: 'top top', end: '+=200%',
    pin: true, scrub: 1.2, anticipatePin: 1
  }
})
.to('.pin-title', { opacity: 0, y: -80 })
.to('.pin-bg',    { scale: 1.18 }, '<');
```

### 5.9 Text Scramble on Hover
```javascript
function scramble(el, finalText) {
  const chars = '!<>-_\\/[]{}—=+*^?#_';
  let frame = 0;
  const iv = setInterval(() => {
    el.textContent = finalText.split('').map((c, i) =>
      i < Math.floor(frame/2) ? finalText[i] : chars[Math.floor(Math.random()*chars.length)]
    ).join('');
    if (++frame > finalText.length * 2 + 10) { clearInterval(iv); el.textContent = finalText; }
  }, 35);
}
```

### 5.10 Product Demo Auto-Player (SaaS / Startup archetypes)
For archetypes 12, 23 — show the product working, not a marketing video.
```html
<!-- Tabbed product demo with auto-advance -->
<div class="demo-player">
  <div class="demo-tabs">
    <button class="demo-tab active" data-demo="0">Feature One</button>
    <button class="demo-tab" data-demo="1">Feature Two</button>
    <button class="demo-tab" data-demo="2">Feature Three</button>
  </div>
  <div class="demo-screen">
    <img class="demo-frame active" src="demo-1.png" alt="Feature one screenshot"/>
    <img class="demo-frame" src="demo-2.png" alt="Feature two screenshot"/>
    <img class="demo-frame" src="demo-3.png" alt="Feature three screenshot"/>
  </div>
  <div class="demo-progress"><div class="demo-bar"></div></div>
</div>
```
```javascript
function initDemoPlayer(autoAdvanceMs = 3000) {
  const tabs = document.querySelectorAll('.demo-tab');
  const frames = document.querySelectorAll('.demo-frame');
  const bar = document.querySelector('.demo-bar');
  let current = 0, timer;

  function showDemo(i) {
    tabs.forEach(t => t.classList.remove('active'));
    frames.forEach(f => { f.classList.remove('active'); gsap.set(f, {opacity:0}); });
    tabs[i].classList.add('active');
    frames[i].classList.add('active');
    gsap.fromTo(frames[i], {opacity:0, y:8}, {opacity:1, y:0, duration:0.5, ease:'power2.out'});
    gsap.fromTo(bar, {scaleX:0}, {scaleX:1, duration:autoAdvanceMs/1000, ease:'none',
      transformOrigin:'left', onComplete: () => { current = (current+1) % tabs.length; showDemo(current); }
    });
  }

  tabs.forEach((tab, i) => tab.addEventListener('click', () => { current = i; showDemo(i); }));
  showDemo(0);
}
```

### 5.11 Editorial Article Card Grid (Editorial / Magazine archetypes)
For archetypes 06, 11, 24 — articles as designed objects, not a list.
```html
<div class="article-grid">
  <article class="article-card" data-category="culture">
    <div class="article-card__image">
      <img src="cover.jpg" alt="Article title" loading="lazy"/>
      <span class="article-card__category">Culture</span>
    </div>
    <div class="article-card__body">
      <time class="article-card__date">March 2025</time>
      <h2 class="article-card__title">The headline goes here and can run long</h2>
      <p class="article-card__deck">A 1–2 sentence standfirst that earns the click.</p>
    </div>
  </article>
</div>
```
```css
.article-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 2px; }
.article-card__image { overflow: hidden; aspect-ratio: 3/2; }
.article-card__image img { width:100%; height:100%; object-fit:cover;
  transform: scale(1.05); transition: transform 0.8s cubic-bezier(0.2,0.8,0.2,1); }
.article-card:hover .article-card__image img { transform: scale(1); }
.article-card__title { font-family: var(--font-display); font-size: clamp(1.1rem, 2vw, 1.4rem); }
```

### 5.12 Menu as Typography (Restaurant / Hospitality archetype)
For archetype 26 — the menu is a designed object, not a PDF.
```html
<section class="menu">
  <div class="menu-category">
    <h3 class="menu-category__name">Entrées</h3>
    <ul class="menu-items">
      <li class="menu-item">
        <div class="menu-item__header">
          <span class="menu-item__name">Pan-Seared Sea Bass</span>
          <span class="menu-item__dots"></span>
          <span class="menu-item__price">£32</span>
        </div>
        <p class="menu-item__description">Lemon beurre blanc, samphire, heritage tomatoes</p>
      </li>
    </ul>
  </div>
</section>
```
```css
.menu-item__header { display:flex; align-items:baseline; gap:8px; }
.menu-item__dots { flex:1; border-bottom: 1px dotted rgba(26,26,26,0.25); margin-bottom: 4px; }
.menu-item__name { font-family: var(--font-display); font-weight:400; font-size:1.1rem; }
.menu-item__price { font-family: var(--font-display); font-style:italic; }
.menu-item__description { font-size:0.8rem; color:var(--ink-muted); margin-top:4px; letter-spacing:0.05em; }
```

### 5.13 Stats Counter Grid (Sports / SaaS / Data archetypes)
For archetypes 07, 12, 21, 23 — numbers as drama.
```html
<div class="stats-grid">
  <div class="stat" data-target="2847" data-suffix="+">
    <span class="stat__number">0</span>
    <span class="stat__label">Users worldwide</span>
  </div>
</div>
```
```javascript
function initStatsCounters() {
  document.querySelectorAll('.stat').forEach(stat => {
    const el = stat.querySelector('.stat__number');
    const target = parseInt(stat.dataset.target);
    const suffix = stat.dataset.suffix || '';
    gsap.fromTo({val:0}, {val:target}, {
      duration: 2, ease: 'power2.out',
      scrollTrigger: { trigger: stat, start: 'top 85%', once: true },
      onUpdate: function() { el.textContent = Math.round(this.targets()[0].val).toLocaleString() + suffix; }
    });
  });
}
```

### 5.14 Dark Mode Toggle (Gap 10 — all archetypes that support it)
Read `references/dark-mode.md` for full system detection and animated toggle patterns.
```css
/* Define both themes as CSS variable sets */
:root {
  --bg: #F2EFE9; --ink: #1A1A1A; --ink-muted: #6B6B6B; --accent: #8B2500;
}
[data-theme="dark"] {
  --bg: #0D0A08; --ink: #F2EFE9; --ink-muted: #8A8A8A; --accent: #C4900D;
}
/* System preference fallback */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    --bg: #0D0A08; --ink: #F2EFE9; --ink-muted: #8A8A8A; --accent: #C4900D;
  }
}
body { background: var(--bg); color: var(--ink); transition: background 0.4s ease, color 0.4s ease; }
```
```javascript
function initDarkMode() {
  const btn = document.querySelector('[data-theme-toggle]');
  const root = document.documentElement;
  // Restore saved preference
  const saved = localStorage.getItem('theme');
  if (saved) root.setAttribute('data-theme', saved);

  btn?.addEventListener('click', () => {
    const current = root.getAttribute('data-theme');
    const next = current === 'dark' ? 'light' : 'dark';
    root.setAttribute('data-theme', next);
    localStorage.setItem('theme', next);
  });
}
```

### 5.15 SEO Meta Tags (Gap 9 — every site, every page)

> For full implementation, see `references/spa-routing.md`, `references/physics-engines.md`, and `references/modern-css.md`.

```html
<head>
  <!-- Primary SEO -->
  <title>Page Title — Brand Name</title>
  <meta name="description" content="150–160 char description. Front-load the value proposition."/>
  <meta name="robots" content="index, follow"/>
  <link rel="canonical" href="https://example.com/page"/>

  <!-- Open Graph (social sharing) -->
  <meta property="og:type" content="website"/>
  <meta property="og:title" content="Page Title — Brand Name"/>
  <meta property="og:description" content="Same as meta description"/>
  <meta property="og:image" content="https://example.com/og-image.jpg"/> <!-- 1200×630px -->
  <meta property="og:url" content="https://example.com/page"/>
  <meta property="og:site_name" content="Brand Name"/>

  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image"/>
  <meta name="twitter:title" content="Page Title — Brand Name"/>
  <meta name="twitter:description" content="Same as meta description"/>
  <meta name="twitter:image" content="https://example.com/og-image.jpg"/>

  <!-- Structured data (Person schema for portfolio/artist sites) -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Person",
    "name": "Full Name",
    "url": "https://example.com",
    "sameAs": ["https://instagram.com/handle", "https://linkedin.com/in/handle"],
    "jobTitle": "Role / Title",
    "description": "Short bio"
  }
  </script>
</head>
```

**SEO rules:**
- Every page gets unique `<title>` and `<meta description>`
- OG image must be 1200×630px — never skip it
- `canonical` on every page to prevent duplicate content
- Structured data type matches the site: `Person`, `Organization`, `Restaurant`, `Product`

---

## PHASE 6 — PERFORMANCE & DEVELOPER AWARD

### GPU-Safe Animation (non-negotiable)
- ✅ Animate ONLY: `opacity`, `transform`, `filter`
- ❌ Never animate: `width`, `height`, `top`, `left`, `margin`, `padding`
- ✅ `will-change: transform` on animated elements (sparingly)
- ✅ All complex transitions: `opacity + transform` only, no layout mutations

### Assets
- Images: WebP, `loading="lazy"` below fold
- Videos: `autoplay muted loop playsinline`, don't autoload below fold
- Fonts: `display=swap` + preconnect to fonts.googleapis.com

### Accessibility (required for Developer Award)
Read `references/accessibility.md` for complete ARIA, focus, and reduced-motion patterns.
```html
<!-- Reduced motion — always include -->
<style>
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
</style>
<a href="#main" class="skip-link">Skip to content</a>
<main id="main" role="main">
```
```javascript
// Respect reduced motion in GSAP
if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
  gsap.globalTimeline.timeScale(100);
}
```

### Mobile Excellence
- Touch targets ≥44px
- No horizontal overflow
- 3D/WebGL degrades gracefully (video fallback)
- No hover-only interactions on touch devices
- Font size ≥16px for body (prevents iOS zoom)

---

## PHASE 7 — RESPONSIVE STRATEGY

```css
/* Panel systems: stack on mobile */
@media (max-width: 768px) {
  .panels-container {
    flex-direction: column;
    height: auto;
    overflow-y: auto;
  }
  .hero-panel {
    width: 100%;
    height: 35vh;
    flex: none !important;
  }
  .panels-container .hero-panel:hover {
    flex: none; /* disable hover-expand on touch */
  }
}

/* Scale text proportionally */
@media (max-width: 480px) {
  :root {
    --text-hero: clamp(3rem, 14vw, 5rem);
    --text-display: clamp(1.8rem, 8vw, 3rem);
  }
}
```

---

## PHASE 8 — THE FINAL QUALITY AUDIT

**This audit adapts to the archetype. Not every item applies to every site.**

### Visual (40%)
- [ ] Background: intentional color choice — never pure `#FFF` or `#000` unless archetype demands it (14 Brutalist Raw: pure white/black is correct)
- [ ] Paper grain: included for archetypes 01, 03, 04, 06, 11, 15, 16, 17, 26 — NOT for 02, 07, 08, 09, 12, 13, 14, 18, 19, 21, 23, 24, 25
- [ ] Display font matches archetype — NOT Inter/Roboto/Arial as headline (exception: archetype 12/23 SaaS where Inter 800 is correct)
- [ ] Typography hierarchy is appropriate — dramatic for editorial/archival, clear for SaaS/academic
- [ ] Spacing is consistent and intentional
- [ ] Color palette discipline: max 2 colors + 1 accent (exception: archetype 20 Memphis — multiple colors are intentional)
- [ ] Visual language consistent — no generic rounded corners in archival, no sharp brutalist edges in luxury

### Usability (30%)
- [ ] Primary interaction immediately clear to a first-time visitor
- [ ] Keyboard navigable (focus states visible)
- [ ] Mobile fits viewport — zero horizontal overflow
- [ ] Touch targets ≥44px
- [ ] `prefers-reduced-motion` respected
- [ ] Skip link and semantic HTML landmarks
- [ ] SEO meta tags on every page (title, description, OG image) — technique 5.15

### Creativity (20%)
- [ ] ONE signature technique that makes this memorable
- [ ] A clear, specific point of view — not a generic version of the archetype
- [ ] ≥2 signature techniques from Phase 5 (matched to archetype)
- [ ] A "screenshot moment" — one frame that earns being shared

### Animation (archetype-adjusted)
- [ ] All easing: custom cubic-bezier or GSAP named — NO `linear` (exception: archetype 14 Brutalist Raw — no easing is correct)
- [ ] Hover transitions ≥0.4s for premium archetypes (01–06, 15–17, 26); ≥0.15s acceptable for kinetic (02, 09, 21)
- [ ] Exit animations exist — nothing instant-hides (exception: archetype 14 and 19 — instant is intentional)
- [ ] Loader exists for immersive archetypes (01, 03, 04, 08, 16, 18) — optional for utility archetypes (12, 23, 24)

### Technical
- [ ] GPU-safe: only opacity + transform animated
- [ ] No layout shifts during animations
- [ ] Videos: `autoplay muted loop playsinline`
- [ ] Dark mode: CSS variables defined for `[data-theme="dark"]` if site requires it — technique 5.14
- [ ] Custom cursor on desktop: required for archetypes 02, 03, 05, 08, 10, 14, 16, 18, 25 — optional otherwise

---

## PHASE 9 — EXCELLENCE STANDARD BY ARCHETYPE

**Read `references/case-studies.md` for SOTY winner breakdowns and scoring criteria.**

The question is not "is this SOTY-worthy?" — it's **"is this the best possible version of what this site is trying to be?"**

### For immersive/creative archetypes (01–05, 08, 10, 14, 16, 18, 25):
| Good | Excellent |
|---|---|
| Beautiful design | Design that feels INEVITABLE for this subject |
| Smooth hover effects | Hover that reveals something unexpected |
| Page transitions | Transitions that feel like turning a page in a book |
| Generic loader | Loader that establishes the brand's emotional register |
| 3D as decoration | 3D as the primary expressive medium |
| Consistent palette | ONE color moment that surprises — then consistency |

### For editorial/narrative archetypes (06, 07, 11, 17, 19, 24):
| Good | Excellent |
|---|---|
| Clean grid | Grid with deliberate tension and editorial rhythm |
| Images placed correctly | Images and text in active dialogue with each other |
| Text readable | Typography that makes reading feel like a pleasure |
| Standard scroll | Scroll that reveals the story beat by beat |
| Generic article cards | Cards where every detail — date, byline, category — is designed |

### For product/utility archetypes (12, 13, 22, 23):
| Good | Excellent |
|---|---|
| Clear value prop | Value prop understood in 3 seconds without reading |
| Feature list | Product demo that shows the feature working |
| Pricing table | Pricing where the recommended plan feels obvious |
| Testimonials | Testimonials that feel specific, not generic |
| CTA button | CTA that creates urgency without feeling pushy |

### For brand/lifestyle archetypes (04, 09, 15, 20, 21, 26):
| Good | Excellent |
|---|---|
| Nice photography | Photography that makes you feel something physical |
| Brand colors consistent | Color that IS the brand, not just applied to it |
| Good mobile | Mobile that preserves the brand atmosphere completely |
| About page | About page that makes you root for the brand |
| Contact section | Contact that feels like an invitation, not a form |

**The universal test**: Would someone who loves this category of site consider this the best example they've seen?

---

## REFERENCE FILES

| File | Read when... |
|---|---|
| `references/archetypes.md` | Choosing design direction — **31 complete aesthetic worlds** |
| `references/color-palettes.md` | Selecting colors, tokens, dark mode, WCAG contrast |
| `references/typography-pairs.md` | Curated font pairings for all 31 archetypes |
| `references/animation-library.md` | 30+ GSAP recipes — vanilla JS and React/useGSAP versions |
| `references/gsap-plugins.md` | SplitText, DrawSVG, MorphSVG, MotionPath, ScrollSmoother, Observer |
| `references/advanced-scroll-effects.md` | 17 scroll patterns: parallax, horizontal, zoom, video scrub, masks |
| `references/css-scroll-animations.md` | CSS-native `animation-timeline: view()` — zero-JS scroll effects |
| `references/scroll-system.md` | Scroll-driven narrative experiences (13 patterns + Lenis v2) |
| `references/mouse-effects.md` | 15 cursor effects: blend mode, magnetic, gooey, trailing, paint |
| `references/cursor-system.md` | Advanced cursor + mouse-reactive base setup |
| `references/3d-webgl-effects.md` | 13 WebGL/shader effects: ripple, bulge, fluid, glass, pixel |
| `references/threejs-system.md` | 3D immersive WebGL with Three.js + GSAP |
| `references/r3f-recipes.md` | React Three Fiber: particles, glass, post-processing, scroll-3D |
| `references/post-processing.md` | Bloom, chromatic aberration, film grain, glitch, vignette |
| `references/particle-systems.md` | Canvas, Three.js instanced, CSS-only, confetti, mouse-reactive |
| `references/text-effects.md` | 9 text animations: scramble, gradient, clip mask, split, explode |
| `references/image-effects.md` | 10 image effects: pixel loading, galleries, capsule physics |
| `references/page-transitions.md` | 8 transitions: pixel, stairs, mask, parallel, gooey, SVG morph |
| `references/view-transitions.md` | Native View Transitions API for SPAs and MPAs |
| `references/menu-navigation.md` | 4 creative menus: side overlay, curved, stairs, panel preview |
| `references/svg-animations.md` | Stroke drawing, morphing, marquees, SVG filters |
| `references/generative-art.md` | Flow fields, circle packing, dot matrix, wave patterns, mesh gradients |
| `references/noise-textures.md` | Film grain, paper textures, halftone, noise overlays by archetype |
| `references/sound-design.md` | Web Audio, Howler.js, hover/click sounds, audio-reactive visuals |
| `references/modern-css.md` | Container queries, :has(), @layer, nesting, oklch(), subgrid |
| `references/dark-mode.md` | System preference detection, animated toggle, CSS variable swap |
| `references/accessibility.md` | ARIA, reduced-motion, focus management, keyboard nav, contrast |
| `references/spa-routing.md` | Hash-based SPA routing, page transitions, hamburger menu |
| `references/physics-engines.md` | Matter.js (2D) & Cannon-es (3D) physics patterns |
| `references/responsive-mobile.md` | Breakpoints, fluid typography, iOS gotchas, touch |
| `references/footer-templates.md` | 4 premium footer designs mapped to archetypes |
| `references/form-design.md` | Contact forms, validation, newsletter signups |
| `references/loading-states.md` | Loaders, skeletons, shimmer effects |
| `references/asset-sourcing.md` | Image generation, stock photos, SVG icon libraries |
| `references/panel-system.md` | Building museum/archival/alanmenken-style panel systems |
| `references/case-studies.md` | SOTY 2024/2025 breakdowns, winner patterns, scoring criteria |
| `references/performance-guide.md` | Lighthouse optimization for Developer Award |
| `references/framework-guide.md` | Next.js, React, Vue, Svelte, Astro implementation |
| `references/wordpress-adaptation.md` | Converting to WordPress theme |
| `references/i18n-guide.md` | Multi-language and internationalisation patterns |
| `references/webgpu-blueprints.md` | **WebGPU standard (2025-2026)**, compute shaders, WGSL |
| `references/creative-accessibility.md` | **WCAG 2.1 AA (Awwwards 2025)**, inclusive motion, aria-labels |
| `references/advanced-shader-blueprints.md` | SOTY-level GLSL: FBO particles, Raymarching, Noise |
| `references/ai-pairing-optimization.md` | AI-Director workflow: Prompting assets & structural gen |
| `references/sales-and-proposals.md` | **Selling the Skill**: Pricing ($8k-$50k), Proposals, Retainers |
| `references/manifesto-of-the-impossible.md` | **The Awwwards Alpha**: Features that no other dev can build |
| `references/ai-vibe-adaptation.md` | Real-time design shifts using on-device AI models |
| `references/cross-device-portal.md` | Phone-as-a-controller sensor sync via WebSockets |
| `references/generative-identity.md` | Each visitor gets a unique "Fingerprint" design |
| `references/spatial-audio.md` | Scroll-reactive & 3D soundscapes using Web Audio API |
| `references/svg-liquid-effects.md` | **Liquid/Gooey reveals**, `feTurbulence`, displacement maps |
| `references/advanced-gsap-recipes.md` | **Pro-Tier**: Text physics, scramble, clip-path, rubber-box |
| `references/interaction-physics.md` | **Mass & Weight**: Magnetic buttons, friction, inertia, rebound |
| `references/webgl-post-processing.md` | **Cinema Polish**: Chromatic aberration, motion blur, pixelation |
| `references/asset-optimization.md` | **The 2026 Pipeline**: Draco, AV1, KTX2, Image Cascades |
| `references/immersive-i18n.md` | Global typography, RTL in 3D, CJK dynamic atlases |
| `references/cms-integration.md` | Contentful/Sanity hand-off, schema-as-code, live preview |
| `references/quality-audit.md` | **The SOTY Checklist**: 50 points to winning Site of the Year |
| `references/modern-component-ecosystems.md` | **ReactBits, 21st.dev, Magic UI**: Sourcing & "Awwward-ifying" |
| `references/bridging-framer-gsap.md` | Technical guide: Using Framer Motion + GSAP together |
| `references/shading-ui-components.md` | Injecting custom GLSL into pre-built UI components |
| `references/edge-cases-and-edge-tech.md` | **Deep Thinking**: "Potato" devices, synesthesia, ultra-wide |
| `references/adaptive-quality-scaling.md` | Tiered experience design: Tier 1, 2, 3 fallback logic |
| `references/immersive-privacy-consent.md` | Turning GDPR/Legal requirements into immersive art |
| `references/prompting-for-perfection.md` | **The MotionSites Layer**: Architecting elite AI prompts |
| `references/deployment-optimization.md` | **Edge Performance**: Vercel/Netlify, caching, & global delivery |
| `references/advanced-seo-strategies.md` | **Creative SEO**: Ranking JS-heavy & WebGL-dominant sites |
| `references/ai-asset-pipelines.md` | **Asset Gen**: Midjourney/Flux prompts for award-level UI |

