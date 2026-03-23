# Design Archetypes — The Complete Reference
## 31 Awwwards-Level Aesthetic Worlds (2022–2025 verified)

Each archetype is a **complete, coherent design world** with its own rules, palette, typography,
motion character, and interaction signature. Pick ONE. Commit fully. Mixing two archetypes
produces mediocrity. A partial commitment produces nothing.

---

## HOW TO CHOOSE

Ask these three questions:
1. **Who is the audience?** (their taste, expectations, sophistication)
2. **What is the emotional job?** (impress, trust, delight, provoke, inform, seduce)
3. **What does the brand/person stand for?** (heritage, rebellion, luxury, clarity, play)

The archetype whose answers align with all three is the one to use.

---

## ARCHETYPE 01 — ARCHIVAL / MUSEUM
> *"This work deserves to be witnessed, not consumed."*

**Emotional job:** Reverence, timelessness, intimacy with history
**Used for:** Artists, composers, photographers, literary estates, cultural institutions, fine art portfolios

**Visual DNA:**
- Background: `#F2EFE9` warm parchment — never pure white
- Ink: `#1A1A1A` — never pure black
- Accent: deep red `#8B2500` or ochre `#C4900D` — used once, sparingly
- Images: desaturated/grayscale by default, reveal color slowly on hover
- Paper grain texture: 4% opacity — the texture of age
- Maximum negative space — content floats in silence
- No cards, no borders, no shadows

**Typography:**
- Display: Cormorant Garamond 300 Light — large, thin, aristocratic
- Signature/logo: Pinyon Script or Mrs Saint Delafield
- Micro-labels: Inter 500, uppercase, 10px, letter-spacing 0.3em

**Animation character:** Slow, reverent — 1.2–2.5s ease. Nothing snappy. Crossfades only.
- Hover: opacity shift (0.35 → 1), very slow desaturation reversal
- Transition: slow opacity crossfade — no sliding, no scaling
- Text reveal: character by character, unhurried

**Primary interaction:** Tall vertical panel system with flex-expand hover
→ Full implementation in `panel-system.md`

**Real-world examples:** alanmenken.com, literary estate sites, museum collection sites

**Signature detail:** One large serif letter centered in the focal panel — the glyph as identity

---

## ARCHETYPE 02 — KINETIC AGENCY
> *"We move fast, think sharp, and nothing we make looks like anything else."*

**Emotional job:** Energy, confidence, technical mastery, creative dominance
**Used for:** Creative agencies, interactive studios, design firms, digital production houses

**Visual DNA:**
- Background: stark black `#080808` OR stark white `#F8F8F8` — no middle ground
- One electric accent: acid green `#C8FF00`, saturated orange `#FF4D00`, or neon cyan
- Images: full-bleed, high contrast, often distorted
- Clip-path reveals, text scramble on hover
- Grid: intentionally broken — asymmetry signals confidence

**Typography:**
- Display: Bebas Neue (condensed power) OR DM Serif Display (elegant tension)
- Body: DM Sans 300–400
- Accent italic: DM Serif Display Italic — one word in a headline for visual tension

**Animation character:** Fast entrance (0.3–0.6s), overlapping stagger, sharp cubic-bezier
- Cursor: large ring that stretches horizontally on fast movement
- Hover: image displacement shader, text scramble, clip-path reveal
- Entrance: clip-path from bottom or left (not opacity fade)

**Primary interaction:** Cursor-reactive — full implementation in `cursor-system.md`

**Real-world examples:** Active Theory, Resn, Unit9, Fantasy, Locomotive

**Signature detail:** Custom cursor that morphs (circle → horizontal pill on fast horizontal movement)

---

## ARCHETYPE 03 — CINEMATIC PORTFOLIO
> *"Every frame is composed. Nothing is accidental."*

**Emotional job:** Beauty, craftsmanship, visual storytelling, cinematic authority
**Used for:** Directors, cinematographers, photographers, motion designers, VFX studios

**Visual DNA:**
- Background: deep near-black gradient — `#0A0A0A` to `#1A1212`
- Imagery: full-screen, high-res, cinematic — often full-bleed video
- Color: near-monochromatic with one warm accent (gold, amber, deep red)
- Typography: refined weight contrast — thin display + wide-tracked credits
- No UI chrome — navigation floats as translucent overlay

**Typography:**
- Display: Playfair Display 400 Italic (the poster title aesthetic)
- Credits/labels: Inter 300 or Suisse Intl, wide tracked (0.25em), uppercase
- Numbers/timecodes: monospace, small

**Animation character:** Slow, film-like — match-cut style transitions
- Video as default panel content — image is the fallback
- Scroll: scrubbed video playback tied to scroll position
- Transition: long crossfade (1.5–2s), camera pull-back feel

**Primary interaction:** Scroll-driven narrative — full implementation in `scroll-system.md`

**Real-world examples:** Film production companies, award-winning photographer folios, Wren Scott

**Signature detail:** Section titles that fade in over full-bleed imagery — text as caption, not headline

---

## ARCHETYPE 04 — LUXURY / FASHION PRODUCT
> *"You already know the name. This is the feeling."*

**Emotional job:** Exclusivity, desire, material richness, aspirational identity
**Used for:** Fashion brands, perfume, jewelry, spirits, automobiles, haute couture

**Visual DNA:**
- Palette: ivory `#F7F4EE`, deep navy `#0D1B2A`, forest `#1A2E1A`, or cognac `#6B3A2A`
- Material textures: leather grain, fabric weave, gold foil shimmer (CSS)
- Typography: ultra-thin display — hairline serifs signal expense
- Layout: extreme close-ups, asymmetric product placement
- Photography: editorial, never commercial. No white-background product shots.
- No CTA buttons in the traditional sense — interaction is implicit

**Typography:**
- Display: Gilda Display 400 (hairline serifs) or Cinzel 400 (Roman inscription gravitas)
- Body/caption: Raleway 200 Thin — even thinner text = more premium
- Labels: Raleway 300, small, wide tracking

**Animation character:** Slow parallax on product imagery — subtle, never exaggerated
- Horizontal scroll through product collections
- Background shifts color as you scroll through variants
- Price and details revealed only on deliberate interaction

**Primary interaction:** Scroll-driven with horizontal product scroll section
→ `scroll-system.md` Pattern 4

**Real-world examples:** Cartier, Mana Yerba Mate (SOTY 2023), Lacoste Heritage, Brunello Cucinelli

**Signature detail:** One full-viewport product image that fills the screen — then text fades in over it

---

## ARCHETYPE 05 — PLAYFUL / EXPERIMENTAL
> *"Rules are suggestions. Delight is the only metric."*

**Emotional job:** Surprise, wonder, unexpected joy, creative provocation
**Used for:** Creative developers, art collectives, personal portfolios, interactive installations

**Visual DNA:**
- No single rule — intentional rule-breaking IS the design language
- Unusual color combinations: mint + crimson, lavender + near-black, butter + cobalt
- Variable fonts that respond to mouse position or scroll
- Elements that escape their containers deliberately
- Physics-based interactions: falling objects, elastic snapping, gravity fields
- Random element on every reload (position, rotation, color tint)

**Typography:**
- Variable fonts that stretch/compress with mouse: e.g. Fraunces, Anybody, Recursive
- Or: extreme weight contrast mixing in a single headline (thin + ultra-bold same line)
- Mixed writing directions (vertical + horizontal in same layout)

**Animation character:** Physics-based, springy, unpredictable
- `elastic.out(1, 0.3)` easing as default
- Mouse position drives multiple independent layers at different speeds
- One delightful surprise hidden behind an interaction

**Primary interaction:** Cursor-reactive physics — `cursor-system.md`

**Real-world examples:** Bruno Simon (3D car portfolio), Malik Elijah, Robbie Leonardi

**Signature detail:** Something that reacts to cursor in an unexpected, physical way — objects flee, attract, wobble

---

## ARCHETYPE 06 — EDITORIAL MAGAZINE
> *"Great writing deserves a stage, not a container."*

**Emotional job:** Intelligence, curation, narrative authority, cultural credibility
**Used for:** Media brands, cultural publications, content-heavy editorial sites, journalism

**Visual DNA:**
- Strong typographic grid — column-based, mathematically rigorous
- Black + one editorial accent (deep red `#B5121B`, electric blue `#0033FF`)
- Large pull-quotes as primary design elements — type IS the layout
- Mixed media: text and image in deliberate tension (not side-by-side, overlapping)
- Hover: article preview reveals (title, lede, time-to-read)
- No texture, no grain — crisp, decisive, deliberate

**Typography:**
- Display: Playfair Display 700 (high-contrast strokes scream editorial)
- Deck/standfirst: Playfair Display 400 Italic
- Body: Libre Baskerville 400 — readable at any size
- Byline/labels: Inter 400, micro-tracking

**Animation character:** Line-by-line text reveals on scroll, clip-path image reveals
- Category filter that transforms grid layout (not page reload)
- Hover on article: cover image scales slightly, caption slides up

**Primary interaction:** Scroll-driven with staggered article reveals

**Real-world examples:** The Pudding, NYT Interactives, some Awwwards editorial sites, Pitchfork

**Signature detail:** Pull-quote in massive display type used as a horizontal divider between sections

---

## ARCHETYPE 07 — DATA VISUALIZATION / JOURNALISM
> *"The numbers are the story. Make them impossible to ignore."*

**Emotional job:** Discovery, clarity, beauty in complexity, informed wonder
**Used for:** Data journalism, research institutions, annual reports, science communicators

**Visual DNA:**
- Dark background: `#0A0E1A` deep navy or `#0D0D0D` near-black
- Glowing data colors: cyan `#00D4FF`, green `#00FF88`, amber `#FFB700`
- Monospace font for data values (feels precise, trustworthy)
- D3.js or custom Canvas/WebGL for data rendering
- Dense information architecture — interaction reveals more, never less

**Typography:**
- Display: DM Serif Display or Syne 700 (headlines that frame data)
- Data values: DM Mono or IBM Plex Mono — monospace = precision
- Labels: Inter 400, small, high contrast against dark BG

**Animation character:** Data animates in on scroll, chart elements reveal sequentially
- Hover: data point tooltip with full detail
- Filter/sort: animated reordering (FLIP animation technique)
- Zoom: click to focus on a data cluster

**Primary interaction:** Scroll-driven reveals with interactive chart elements

**Real-world examples:** The Pudding, Bloomberg Visual, Persephone Reimagined (SOTY 2022), Reuters Graphics

**Signature detail:** One visualization that reveals a surprising truth — the "aha moment" data designers live for

---

## ARCHETYPE 08 — IMMERSIVE 3D
> *"Navigation is exploration. The site is a world."*

**Emotional job:** Wonder, spatial imagination, the thrill of discovery in space
**Used for:** Games, interactive art, product showcases, 3D studios, tech innovation labs

**Visual DNA:**
- Three.js WebGL canvas as the primary surface — no traditional layout
- Loading screen with GPU warm-up progress indicator
- Camera movement replaces scrolling — you navigate, not scroll
- Custom GLSL shaders: atmospheric fog, displacement, bloom, chromatic aberration
- FBO particle systems for complex environmental effects

**Typography:**
- Thin, luminous — almost disappears into the scene
- Appears only when camera rests — not while moving
- Display: Space Grotesk 300 or Syne 300 (geometric, space-age)

**Animation character:** Scroll drives camera along predetermined path
- Mouse moves camera slightly (0.1–0.15 parallax factor)
- 3D objects respond to hover: scale, gentle rotation, light shift
- WebGL degraded gracefully on mobile (video fallback)

**Primary interaction:** 3D world navigation — full implementation in `threejs-system.md`

**Real-world examples:** Bruno Simon, Lusion v3 (SOTY 2024), Igloo Inc (SOTY 2025), Active Theory

**Signature detail:** One moment where the camera "discovers" something unexpected — a hidden scene, a reveal

---

## ARCHETYPE 09 — NEO-BRUTALISM
> *"Function is beautiful. Polish is a lie. Here is the raw truth."*

**Emotional job:** Provocation, authenticity, rebellious energy, anti-corporate boldness
**Used for:** Startups, SaaS tools, creative portfolios, fashion disruptors, music platforms

**Visual DNA:**
- Stark backgrounds: #FFFFFF or solid saturated color (yellow `#FFE500`, red `#FF2D00`)
- Hard black borders: `2px–4px solid #000` on everything (cards, buttons, images)
- Hard offset drop shadows: `4px 4px 0 #000` — flat, not soft
- Vibrant clashing color fills: one loud primary + pure black
- NO gradients, NO rounded corners (or extremely minimal), NO soft shadows
- Asymmetric grid — elements placed with intentional awkwardness
- Hover: border/shadow shifts (button moves 2px, shadow adjusts)

**Typography:**
- Display: Space Grotesk 700, Syne 800, or DM Serif Display — oversized and unapologetic
- Quirky grotesques: PP Neue Montreal, Satoshi, Cabinet Grotesk
- Mix weights dramatically in the same headline
- Monospace for code or data elements: Space Mono

**Animation character:** Slightly jerky, honest — `cubic-bezier(0.34, 1.56, 0.64, 1)` for slight overshoot
- Hover: buttons shift position by 2–3px (feels physical)
- Text: scramble on hover, then settle
- No long cinematic fades — quick and direct

**CSS Signature:**
```css
.neo-card {
  border: 3px solid #000;
  box-shadow: 5px 5px 0 #000;
  background: #FFE500;
  transition: transform 0.15s ease, box-shadow 0.15s ease;
}
.neo-card:hover {
  transform: translate(-2px, -2px);
  box-shadow: 7px 7px 0 #000;
}
```

**Real-world examples:** Gumroad, Linear (early), Balenciaga, Mailchimp, Tony's Chocolonely (Tinloof)

**Signature detail:** Offset hard shadow on the hero section that shifts on hover — makes the whole page feel physical

---

## ARCHETYPE 10 — TYPOGRAPHIC-LED
> *"The font is the image. The layout is the typography."*

**Emotional job:** Intellectual boldness, typographic craft, pure communication
**Used for:** Type foundries, design studios, festivals, cultural events, manifesto-style brands

**Visual DNA:**
- Typography IS the visual — no imagery needed (or imagery is secondary)
- Oversized headlines that overflow the viewport intentionally
- Mixed writing directions: vertical text, rotated labels, diagonal flow
- Variable fonts that morph on scroll or hover
- Kinetic type: letters that move, stretch, split, reassemble
- Background: pure white or pure black — nothing competes with the type

**Typography (the whole point):**
- Variable fonts: Fraunces (optical variable), Anybody (width variable), Recursive
- Or: extreme weight mixing — e.g. 100 weight + 900 weight in same headline
- Tracking extremes: hero at -0.06em, labels at 0.5em+
- Mix serif + sans in a single headline for visual tension

**Animation character:** Type itself IS the animation
```javascript
// Variable font driven by mouse X position
window.addEventListener('mousemove', e => {
  const wght = 100 + (e.clientX / window.innerWidth) * 800;
  document.querySelector('.var-title').style.fontVariationSettings = `'wght' ${wght}`;
});
// Type scramble on scroll
// Letter splitting + stagger on entrance
```

**Real-world examples:** Pangram Pangram Foundry, Kota studio, some Awwwards type-driven SOTD sites

**Signature detail:** A headline that morphs its font weight as you move the cursor across it

---

## ARCHETYPE 11 — NARRATIVE / DOCUMENTARY
> *"This is a story. You are inside it."*

**Emotional job:** Emotional investment, empathy, bearing witness, social change
**Used for:** NGOs, advocacy campaigns, documentary films, memorial sites, brand storytelling

**Visual DNA:**
- Photography-first — images carry the emotional weight
- Long-form scroll with pinned sections that unfold chapter by chapter
- Text appears over imagery (not beside it) — caption aesthetic
- Color: warm documentary tones or stark black and white for impact
- Pull statistics revealed dramatically mid-scroll
- Audio elements: ambient sound or narration triggered on scroll

**Typography:**
- Display: Playfair Display or Canela — weight of a broadsheet
- Body: large, readable — 18–20px, generous line-height (1.7–1.8)
- Pull stats: enormous display numbers — the data as visual impact

**Animation character:** Slow, heavy — each scroll step is deliberate
- Pinned sections build the story before releasing
- Images crossfade to reveal emotional progression
- Text fades in line by line — not all at once

**Primary interaction:** Scroll-driven narrative — `scroll-system.md`

**Real-world examples:** Persepolis Reimagined (SOTY 2022), The Other Side of Truth (SOTY 2022), NYT Snow Fall

**Signature detail:** One full-viewport stat that stops you mid-scroll — "37 seconds" in 200px type

---

## ARCHETYPE 12 — PRODUCT / SAAS
> *"Powerful enough to trust. Simple enough to use today."*

**Emotional job:** Clarity, trust, capability, ease — "this will solve my problem"
**Used for:** SaaS products, developer tools, productivity apps, B2B platforms

**Visual DNA:**
- Clean white or very light gray background: `#F9FAFB`
- Primary brand color: one strong hue (blue, violet, emerald) for CTAs and accents
- Dark navy or near-black for primary text: `#111827`
- UI screenshots/product demos as hero elements
- Feature sections with icon + headline + short copy
- Social proof: logos, testimonials, numbers — visible without scrolling

**Typography:**
- Display: Inter 700–800 or Plus Jakarta Sans 700 — clean, modern, trustworthy
- Body: Inter 400, 16–18px — maximum readability
- Labels/tags: Inter 500, 12–13px, muted color

**Animation character:** Smooth but quick — not cinematic, not jarring
- Feature reveals on scroll: `y: 30` → `y: 0`, `opacity: 0` → `1`, 0.6s ease
- Product UI: subtle float animation on hero screenshot
- CTA button: subtle scale on hover (1 → 1.03)

**CSS Signature:**
```css
/* The standard SaaS hero */
.hero-badge { background: #EEF2FF; color: #4F46E5; border-radius: 999px; padding: 4px 12px; }
.hero-title { font-size: clamp(2.5rem, 5vw, 4rem); font-weight: 800; color: #111827; }
.hero-cta { background: #4F46E5; color: #fff; padding: 14px 28px; border-radius: 8px; }
```

**Real-world examples:** Linear, Vercel, Resend, Loom, Raycast, Clerk

**Signature detail:** A product screenshot that's animated — typing effect, live data updating, or interactive demo

---

## ARCHETYPE 13 — GAMIFIED / INTERACTIVE
> *"You don't browse this site. You play it."*

**Emotional job:** Delight, engagement, reward, curiosity, "just one more click"
**Used for:** Games, education platforms, onboarding flows, consumer apps, youth brands

**Visual DNA:**
- Bold, saturated palette — primary colors or game-style gradients
- Progress bars, completion percentages, level indicators as design elements
- Micro-rewards: confetti, sparkle effects, sound cues on completion
- Cards, tokens, badges — game UI language applied to web
- Generous use of illustration and character design
- Rounded corners everywhere — soft, safe, approachable

**Typography:**
- Display: Syne 700, Nunito 800, or custom game-style font
- UI: Nunito or Poppins — rounded, friendly, approachable
- Numbers/scores: monospace or tabular numerals

**Animation character:** Springy, rewarding — every interaction has a payoff
```javascript
// Confetti burst on completion
// Progress bar fills with spring easing
// Badge "unlocks" with scale pop: scale(0) → scale(1.2) → scale(1)
gsap.fromTo(badge, { scale: 0 }, {
  scale: 1, duration: 0.5,
  ease: 'back.out(2)',  // spring overshoot
});
```

**Real-world examples:** Duolingo, Headspace, Kahoot, some onboarding experiences

**Signature detail:** A progress indicator that makes the user feel they're advancing — even on a single page

---

## ARCHETYPE 14 — BRUTALIST RAW
> *"The web looked honest once. It can again."*

**Emotional job:** Authenticity, anti-design provocation, rawness, early-web nostalgia
**Used for:** Experimental art, zines, underground music, counter-culture, avant-garde fashion

**Visual DNA:**
- Plain HTML aesthetic: system fonts, default link underlines, table-based layout references
- Monospace font dominates: Courier New or Space Mono
- Black text on white, or white text on black — full stop
- No hover animations, no transitions — instantaneous
- Images placed without sizing, sometimes overlapping
- Text placed wherever it fits — not aligned to a grid

**Typography:**
- Body: Courier New (the original web font, now punk again)
- OR: a deliberately ugly display font — Helvetica at 900 weight, stacked
- No tracking adjustments, no fine-tuning

**Animation character:** None, or near-none. Deliberately.
- Click = immediate state change, no fade
- One glitch effect allowed — CSS `glitch` keyframe animation
- Scrolling: instant, no smooth scroll

**Real-world examples:** Drudge Report (unintentional), some Balenciaga campaigns, Ezequiel Aquino portfolio

**Signature detail:** Something that looks broken on purpose — and makes the visitor look twice because of it

---

## ARCHETYPE 15 — ORGANIC / HANDCRAFTED
> *"Made by a person. Felt by a person."*

**Emotional job:** Warmth, authenticity, handmade intimacy, human connection
**Used for:** Food & beverage brands, wellness, independent artisans, local businesses, studios

**Visual DNA:**
- Warm earth palette: clay `#C4795B`, sage `#8A9E7B`, cream `#F5EDD6`, sand `#D4B896`
- Handwritten/script typography alongside body text
- Hand-drawn illustrations, imperfect borders, sketched icons
- Texture: paper, linen, watercolor wash
- Asymmetric layouts that feel hand-placed, not grid-snapped
- No hard geometric shapes — organic curves, blob shapes

**Typography:**
- Display: Fraunces Italic (optical variable, ink-trap quality) or Canela Light
- Handwritten accent: Caveat, Kalam, or a custom script for one key phrase
- Body: Lora 400 — warm, readable, slightly oldstyle

**Animation character:** Gentle, organic — slightly imperfect timing
- SVG path draw-on (like being sketched in real time)
- Elements that sway subtly (CSS rotation oscillation)
- Scroll reveals: staggered, slightly offset — not perfectly synced

**CSS Signature:**
```css
.organic-blob {
  border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%;
  animation: blobmorph 8s ease-in-out infinite;
}
@keyframes blobmorph {
  0%,100% { border-radius: 60% 40% 30% 70%/60% 30% 70% 40%; }
  50%      { border-radius: 30% 60% 70% 40%/50% 60% 30% 60%; }
}
```

**Real-world examples:** some Awwwards food/beverage SOTD, independent artisan studios

**Signature detail:** One SVG element that draws itself on page load — a signature, a border, a logo outline

---

## ARCHETYPE 16 — DARK LUXURY / ATMOSPHERIC
> *"You don't see the luxury. You feel it."*

**Emotional job:** Mystery, sophistication, premium atmosphere, cinematic darkness
**Used for:** Nightlife, spirits, fragrance, exclusive clubs, high-end gaming, tech noir brands

**Visual DNA:**
- Background: near-black with warm undertone: `#0D0A08` or `#0A0A14` (cool)
- Luminous accents: gold `#C9A84C`, amber `#E8982A`, or electric blue `#3D7EFF`
- Blur effects: backdrop-filter, glowing halos behind key elements
- Glassmorphism: frosted glass panels (`backdrop-filter: blur(20px)`)
- Particle systems: floating dust, slow-moving light points
- Photography: low-key lighting, deep shadows, single-source light

**Typography:**
- Display: Cormorant Garamond 300 Light Italic — luminous against dark
- OR: Cinzel 400 (Roman gravitas — aristocratic)
- Micro-labels: Inter 400, very muted (60% opacity)

**Animation character:** Slow, atmospheric — things emerge from darkness
- Hover: glow intensifies (box-shadow with blur)
- Entrance: elements fade in from 0% opacity, very slowly (1.5s+)
- Particles drift autonomously in the background

**CSS Signature:**
```css
.glass-panel {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 0 40px rgba(201, 168, 76, 0.08);
}
.glow-text {
  text-shadow: 0 0 30px rgba(201, 168, 76, 0.6);
}
```

**Real-world examples:** High-end nightclub sites, Hennessy, Johnnie Walker, some luxury gaming

**Signature detail:** One element with a soft luminous glow that pulses very slowly — like an ember

---

---

## ARCHETYPE 17 — RETRO / NOSTALGIC
> *"We've been here before. That's exactly why it feels right."*

**Emotional job:** Nostalgia, comfort, authenticity, affectionate humor
**Used for:** Vintage brands, retro-styled products, music from a specific era, indie studios, craft beer, vinyl shops

**Visual DNA:**
- Warm sepia or faded color palettes: cream `#F5E6C8`, rust `#B5431A`, mustard `#D4A017`
- Halftone dot patterns, film grain, worn paper textures at high opacity (10–20%)
- Retro typography: slab serifs, distressed fonts, hand-lettered feel
- Rounded or irregular borders that suggest hand-cutting
- Old printing press color separation artifacts (slight RGB offset)
- Reference specific decades: 70s earth tones, 80s neon+black, 90s grunge, Y2K chrome

**Typography:**
- Display: Playfair Display (Victorian), Abril Fatface (slab poster), or a deliberately retro font
- Body: Lora 400 or Libre Baskerville — oldstyle, warm
- Labels: Courier Prime for typewriter feel

**Animation character:** Slightly imperfect — things don't always land perfectly
- Film projector flicker effect (CSS opacity keyframes)
- Slight camera shake on page load
- Hover: vintage stamp effect (scale + slight rotation)

**Real-world examples:** BrewDog, Jack Daniel's heritage pages, some vinyl record shop sites

**Signature detail:** A halftone texture overlay that reveals the archival image beneath — like an old newspaper

---

## ARCHETYPE 18 — SCIFI / FUTURISTIC
> *"This is not yet. But it will be."*

**Emotional job:** Awe at the future, technological wonder, edge-of-possibility excitement
**Used for:** Web3, blockchain, AI companies, space tech, biotech, advanced research labs, sci-fi games

**Visual DNA:**
- Background: deep space black `#020408` or dark navy `#050A14`
- Primary accent: cyan `#00D4FF`, neon blue `#3D7EFF`, or electric violet `#7B2FFF`
- Grid lines: very subtle wireframe grid on background (0.5px, 4% opacity)
- Glow effects: elements emit colored light (box-shadow blur, text-shadow)
- Holographic/iridescent gradients: `linear-gradient(135deg, #667eea, #764ba2, #00d4ff)`
- Geometric precision: sharp angles, technical diagrams as decorative elements
- Monospace font for data/stats (terminal aesthetic)

**Typography:**
- Display: Orbitron (too obvious — use sparingly), Space Grotesk 600, or Syne 700
- Body: Space Grotesk 400 or DM Sans — clean, technical
- Data/code: Space Mono or IBM Plex Mono

**Animation character:** Precise, technical — like systems booting up
- Terminal text type-on effect for headlines
- Grid lines draw on page load
- Hover: scan line effect, subtle flicker
- Loading: progress bars with technical readouts

**CSS Signature:**
```css
.holo-card {
  background: linear-gradient(135deg, rgba(61,126,255,0.1), rgba(123,47,255,0.1));
  border: 1px solid rgba(0, 212, 255, 0.2);
  box-shadow: 0 0 20px rgba(0, 212, 255, 0.1), inset 0 0 20px rgba(61,126,255,0.05);
}
.scan-line::after {
  content: '';
  position: absolute;
  inset: 0;
  background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,212,255,0.015) 2px, rgba(0,212,255,0.015) 4px);
  pointer-events: none;
}
```

**Real-world examples:** Some Awwwards Web3 SOTD, SpaceX-inspired sites, AI lab sites

**Signature detail:** A subtle animated scan line that sweeps the page very slowly — like a radar

---

## ARCHETYPE 19 — MINIMALIST / ZEN
> *"The loudest thing here is the silence between elements."*

**Emotional job:** Calm, clarity, confidence, trust through restraint
**Used for:** Architecture firms, high-end interior design, meditation apps, premium stationery, Japanese-inspired brands

**Visual DNA:**
- Background: pure near-white `#FAFAFA` or warm white `#F7F5F2`
- One ink color: `#1A1A1A` — used for everything (no accents at all, or one very muted one)
- White space IS the design — sections breathe more than on any other archetype
- Single rule lines (1px) as dividers instead of sections
- Photography: square format, carefully composed, single subject
- No decorative elements whatsoever

**Typography:**
- Display: thin-weight sans — Helvetica Neue 100 Thin, or DM Sans 200
- Body: same font, 300 weight — absolute consistency
- Scale: large jumps but understated — 80px hero, 16px body

**Animation character:** Barely there — things exist, then they don't
- Fade transitions only: 0.6s ease, nothing else
- No scroll triggers — content is simply there when you arrive
- Hover: opacity shift only (1 → 0.5)

**Real-world examples:** Muji digital, Kinfolk magazine, some architecture studio sites, Japanese brand sites

**Signature detail:** One section that is JUST white space with one sentence in it — and it hits harder than anything else

---

## ARCHETYPE 20 — MEMPHIS / POP ART
> *"Color is the message. Loud is the point."*

**Emotional job:** Playful energy, irreverence, maximum personality, fun
**Used for:** Youth brands, pop culture, music festivals, colorful consumer products, bold startups

**Visual DNA:**
- Multiple saturated colors simultaneously: hot pink `#FF2D78` + yellow `#FFE500` + cobalt `#0033FF`
- Geometric shapes: triangles, squiggles, dots, zigzags as decorative elements
- Pattern backgrounds: repeating geometric motifs
- No hierarchy — everything competes for attention (intentionally)
- Thick borders, graphic elements borrowed from print design

**Typography:**
- Display: Bebas Neue (stacked), or Impact (unironic), or Syne 900
- Mixing ALL CAPS with Title Case in the same layout
- Multiple font weights on the same line

**Animation character:** Energetic, bouncy — `back.out(1.7)` spring everywhere
- Shapes rotate and bounce on hover
- Background color changes between sections (hard cut, no transition)
- Cursor leaves a trail of colored dots

**Real-world examples:** Some festival sites, fashion pop-ups, Spotify campaign pages

**Signature detail:** Overlapping geometric shapes in multiple colors that move at different parallax speeds

---

## ARCHETYPE 21 — SPORTS / ATHLETIC
> *"Performance. Now."*

**Emotional job:** Adrenaline, power, achievement, physical peak
**Used for:** Sports brands, athletes, fitness apps, sports events, activewear

**Visual DNA:**
- Background: deep black or midnight blue — darkness = intensity
- Accent: brand red, electric yellow, or neon orange — ONLY one
- Photography: extreme motion blur, mid-action shots, muscle definition
- Bold diagonal layouts — nothing is perfectly horizontal
- Numbers as design elements: jersey numbers, stats in enormous type
- Dynamic crops: images cut by diagonal lines

**Typography:**
- Display: Bebas Neue (the athletic font), or a condensed grotesque
- Numbers: tabular numerals, bold, sometimes wider tracked
- Body: DM Sans 700 — strong, not delicate

**Animation character:** Fast, powerful — `cubic-bezier(0.77,0,0.175,1)` hard deceleration
- Hero image zooms in slightly on load (scale: 1.08 → 1.0)
- Stats count up on scroll reveal
- Fast clip-path reveals (from sides, not bottom)

**Real-world examples:** Nike campaigns, Red Bull digital, Adidas Running

**Signature detail:** One stat that counts up dramatically on scroll — "3,847 athletes. One standard."

---

## ARCHETYPE 22 — ECOMMERCE EDITORIAL
> *"Shopping as culture. The store as gallery."*

**Emotional job:** Desire, discovery, curation — "I need this in my life"
**Used for:** Premium fashion ecommerce, design shops, curated marketplaces, art prints, independent boutiques

**Visual DNA:**
- Editorial grid: products treated as art objects, not commodities
- Large product photography with editorial styling (not white background)
- White space between products — room to breathe = perceived value
- Color: neutral base (off-white, warm gray) + product colors dominate
- Cart/UI elements are invisible until needed

**Typography:**
- Display: DM Serif Display (editorial authority)
- Product names: Inter 500 or Suisse Intl — clean, confident
- Price: same size as name — not larger, not smaller

**Animation character:** Smooth, considered — product reveals like gallery unveilings
- Grid items stagger in on scroll (`y: 40` → `y: 0`)
- Hover: additional product image crossfades in
- Cart: slides in from right, never interrupts browsing

**Real-world examples:** SSENSE, Are.na shop, some Awwwards ecommerce SOTD

**Signature detail:** Product hover that shows a second lifestyle image with a smooth crossfade — model vs. flat lay

---

## ARCHETYPE 23 — STARTUP LANDING / GROWTH
> *"Clear value. Fast trust. One decision."*

**Emotional job:** Immediate comprehension, urgency, social proof, confidence
**Used for:** Early-stage startups, growth-focused products, apps seeking signups, B2C SaaS

**Visual DNA:**
- Clean, bright — white or very light background
- Strong hero: product screenshot + clear value prop in 6 words or less
- Social proof: logos, numbers, testimonials — visible without scrolling
- One primary CTA color (the ONLY saturated element on the page)
- Pricing table — clear, fair, no tricks
- FAQ section — because objections are real

**Typography:**
- Display: Plus Jakarta Sans 800 or Inter 800 — maximum clarity
- Body: Inter 400, 17px, 1.6 line-height
- CTA: Inter 600, never Inter 400

**Animation character:** Helpful, not decorative — animations explain the product
- Product demo auto-plays in hero (3–5s loop)
- Feature icons animate on scroll (draw-on, or pop-in)
- CTA button: subtle pulse to draw attention (not annoying)

**Real-world examples:** Linear, Clerk, Resend, Vercel, Fly.io, Supabase

**Signature detail:** A product demo that plays automatically and shows the actual value — not a marketing video

---

## ARCHETYPE 24 — ACADEMIC / RESEARCH
> *"The work speaks. We just organize it."*

**Emotional job:** Trust, rigor, credibility, intellectual respect
**Used for:** Universities, research labs, think tanks, scientific publications, medical institutions

**Visual DNA:**
- Institutional colors: deep navy, forest green, burgundy — one dominant
- White/off-white for content areas — maximum readability
- Structured grid: proper column system, consistent margins
- Publication-style layout: volume numbers, dates, author bylines
- Data tables, citations, footnotes — embraced as design elements

**Typography:**
- Display: EB Garamond or Source Serif Pro — academic authority
- Body: Source Serif Pro 400, generous line-height (1.75)
- Data: Space Mono or IBM Plex Mono for tables and citations
- Captions: smaller, italic, muted color

**Animation character:** Restrained — academic gravity
- Fade-in on scroll only (no sliding, no scaling)
- No decorative animations at all
- Page transitions: simple opacity fade

**Real-world examples:** MIT Media Lab, some university department sites, research lab microsites

**Signature detail:** Research findings presented as pull-stats in a clear typographic hierarchy — the data as hero

---

## ARCHETYPE 25 — MOTION / GENERATIVE ART
> *"The code is the brush. The screen is the canvas."*

**Emotional job:** Wonder at generative process, beauty of algorithms, the art of the system
**Used for:** Generative artists, creative coders, NFT/digital art collections, code art portfolios

**Visual DNA:**
- Dark or white canvas — the generative work IS the visual
- No decorative elements beyond the artwork itself
- Minimal UI: thin labels, small navigation — nothing competes
- Color emerges from the algorithm (not predetermined palette)
- Canvas element as hero: p5.js, Three.js, or raw Canvas API

**Typography:**
- Monospace only: DM Mono or Space Mono — code proximity signals the medium
- Very small, very muted — observer in the artist's studio, not the artist

**Animation character:** Generative — the art is permanently in motion
- Canvas re-generates on each interaction or page load
- Parameters controlled by mouse position (noise fields, particle systems)
- Each visit is unique

```javascript
// p5.js sketch as hero
const sketch = (p) => {
  let particles = []
  p.setup = () => {
    p.createCanvas(p.windowWidth, p.windowHeight)
    for (let i = 0; i < 200; i++) particles.push(new Particle(p))
  }
  p.draw = () => {
    p.background(10, 10, 10, 15) // trail effect
    particles.forEach(part => { part.update(); part.draw() })
  }
}
new p5(sketch)
```

**Real-world examples:** Some Awwwards generative art SOTD, Tyler Hobbs digital presence, Manolo Gamboa Naon

**Signature detail:** Artwork that is provably different every single visit — and users discover this and reload

---

## ARCHETYPE 26 — RESTAURANT / HOSPITALITY
> *"Before they taste it, they feel it."*

**Emotional job:** Appetite, warmth, occasion, anticipation of pleasure
**Used for:** Restaurants, hotels, bars, cafes, catering, hospitality groups

**Visual DNA:**
- Warm, inviting: deep forest green `#1B3A2D`, burgundy `#5C1A2A`, cream `#F7F0E6`
- Rich food photography — dark backgrounds, dramatic lighting (chiaroscuro)
- Texture: linen, stone, wood grain at low opacity
- Handwritten element for chef name or signature dish
- Reservation/booking CTA prominent but not disruptive

**Typography:**
- Display: Canela Light Italic (THE restaurant font) or Freight Display
- Body: Lora 400 — warm, readable
- Menu items: custom typographic treatment — not a list, a composition

**Animation character:** Warm and unhurried — like good service
- Hero: slow zoom on food photography (Ken Burns effect)
- Menu items fade in as you scroll the menu
- Reservation form: graceful slide-in, never a popup

**Real-world examples:** Eleven Madison Park, some Awwwards restaurant SOTD, Noma

**Signature detail:** The menu page treated as a designed object — not a PDF link. Typography IS the menu.

---

## QUICK SELECTION MATRIX (all 26 archetypes)

| If the client is... | Archetype |
|---|---|
| Composer, artist, heritage institution | 01 Archival/Museum |
| Creative agency, digital studio | 02 Kinetic Agency |
| Filmmaker, photographer, director | 03 Cinematic Portfolio |
| Fashion, perfume, jewelry, spirits | 04 Luxury Product |
| Creative developer personal site | 05 Playful/Experimental |
| Media brand, publication, editorial | 06 Editorial Magazine |
| Data journalism, research, reports | 07 Data Visualization |
| Interactive art, 3D showcase | 08 Immersive 3D |
| Startup, disruptor, anti-corporate brand | 09 Neo-Brutalism |
| Type foundry, design festival, manifesto | 10 Typographic-Led |
| NGO, advocacy, documentary, memorial | 11 Narrative/Documentary |
| SaaS, developer tool, B2B product | 12 Product/SaaS |
| Game, edtech, consumer app, youth brand | 13 Gamified/Interactive |
| Underground art, zine, experimental | 14 Brutalist Raw |
| Food artisan, wellness, local studio | 15 Organic/Handcrafted |
| Nightlife, dark spirits, fragrance, luxury | 16 Dark Luxury/Atmospheric |
| Vintage brand, retro music, craft nostalgia | 17 Retro/Nostalgic |
| Web3, AI lab, space tech, biotech | 18 Scifi/Futuristic |
| Architecture, interior design, meditation | 19 Minimalist/Zen |
| Music festival, youth brand, pop culture | 20 Memphis/Pop Art |
| Sports brand, athlete, activewear | 21 Sports/Athletic |
| Premium fashion ecommerce, design shop | 22 Ecommerce Editorial |
| Early-stage startup, B2C app, growth product | 23 Startup Landing/Growth |
| University, research lab, think tank | 24 Academic/Research |
| Generative artist, creative coder, NFT | 25 Motion/Generative Art |
| Restaurant, hotel, bar, hospitality | 26 Restaurant/Hospitality |

---

## COMBINATIONS THAT WORK (rare, must be intentional)

| Combination | Example use case |
|---|---|
| 01 Archival + 03 Cinematic | Music biopic, film archive |
| 02 Kinetic Agency + 09 Neo-Brutalism | Gen Z creative studio |
| 04 Luxury + 16 Dark Luxury | Prestige spirits |
| 07 Data + 11 Narrative | Documentary with data journalism |
| 12 Product + 09 Neo-Brutalism | Developer-first startup |
| 15 Organic + 06 Editorial | Independent food publication |
| 17 Retro + 20 Memphis | 80s revival brand |
| 18 Scifi + 08 Immersive 3D | Space tech / AI company |
| 22 Ecommerce + 04 Luxury | Couture online boutique |
| 25 Generative + 05 Playful | Creative coder portfolio |
| 26 Restaurant + 15 Organic | Farm-to-table restaurant |

**Never combine:** 01+02 (reverence vs. energy), 04+09 (luxury vs. brutalism), 19+20 (zen vs. chaos), 24+13 (academic vs. gamified) — these archetypes speak contradictory emotional languages.

---

## ARCHETYPE 27 — AI / TECH FUTURIST
> *"Intelligence made visible."*

**Emotional job:** Awe, innovation, trustworthy technological power
**Used for:** AI startups, ML tools, tech research labs, developer platforms, robotics companies

**Visual DNA:**
- Background: `#0B0B1A` deep space navy — almost black but with blue undertone
- Surface: `#12122A` slightly lighter — for cards and elevated elements
- Ink: `#E0E0EC` cool silver-white
- Accent: `#6366F1` electric indigo or `#06B6D4` cyan
- Secondary accent: `#8B5CF6` violet (for gradients)

**Typography:**
- Display: `Space Grotesk` or `Syne` — geometric, modern but readable
- Body: `Inter` or `DM Sans` at 400 weight
- Mono (code): `JetBrains Mono` or `Fira Code`

**Signature patterns:**
- Gradient mesh backgrounds (indigo → cyan → violet)
- Subtle grid overlay at `opacity: 0.03`
- Animated dot matrix or particle field in hero
- Code snippets with syntax highlighting as decorative elements
- Glassmorphism cards: `backdrop-filter: blur(20px)` + semi-transparent borders
- Neural network or node-graph visual in background

**Motion character:** Precise, calculated. Easing: `power2.out`. Elements snap into place with mathematical elegance. Particle systems move slowly, suggesting computation. Hover effects reveal data — tooltips, metrics, technical details.

**Interaction signature:** Terminal-style text animation (typewriter effect for taglines), interactive API playground, code-to-visual transformation demos.

---

## ARCHETYPE 28 — CULINARY / RESTAURANT
> *"Every dish tells a story."*

**Emotional job:** Appetite, warmth, invitation, sensory anticipation
**Used for:** Fine dining, bistros, cafés, food brands, chef portfolios, recipe platforms

**Visual DNA:**
- Background: `#FAF8F5` warm cream — feels like linen tablecloth
- Ink: `#2C2218` warm dark brown — never cool black
- Accent: `#B94E2B` terracotta / burnt sienna
- Secondary: `#5C7A3B` olive green (fresh, natural)
- Warm neutral: `#D4C5B2` muted gold/wheat

**Typography:**
- Display: `Playfair Display` or `Cormorant Garamond` — elegant serif for dish names
- Body: `Lora` or `Source Serif 4` at 400
- Accent: `Pinyon Script` or `Great Vibes` — cursive for the signature/chef name (use once)
- Mono: None — restaurants don't use monospace

**Signature patterns:**
- Full-bleed food photography (hero must feature a dish, not text)
- Warm color temperature on all images (`filter: sepia(0.05) saturate(1.1)`)
- Menu card with elegant pricing alignment (dots or dashes between item and price)
- Reservation CTA as a prominent, warm button
- Parallax ingredient close-ups between sections
- Textured paper/linen background subtle overlay

**Motion character:** Slow, savored. Nothing should feel rushed. Images fade in gently (`duration: 1.2s`). Hover reveals ingredient details. Scroll-driven reveals feel like turning pages of a menu.

**Interaction signature:** Reservation form with date picker, menu category tabs, chef's bio with signature stroke-draw animation.

---

## ARCHETYPE 29 — MUSIC ARTIST / PERFORMER
> *"Hear it. See it. Feel it."*

**Emotional job:** Fandom, identity, emotional connection, energy
**Used for:** Musicians, DJs, bands, record labels, tour pages, album launches

**Visual DNA:**
- Background: `#0A0A0A` pure dark — the stage is dark before the show
- Ink: `#F5F5F5` bright white — high contrast like concert lighting
- Accent: Artist-specific (e.g., `#D40000` for rock, `#00D4FF` for electronic, `#FFD700` for hip-hop)
- Secondary: Complement or split-complement of accent

**Typography:**
- Display: `Bebas Neue` or custom display — bold, unapologetic
- Body: `Inter` or `Barlow` at 400
- Accent: Sometimes a custom/distressed font for the artist name — one use only
- Stats/dates: `Roboto Mono` or `Space Mono`

**Signature patterns:**
- Full-viewport hero with artist photo or video loop
- Floating album artwork with 3D perspective tilt
- Tour dates as a structured list with city names as large text
- Music player integration (embedded Spotify/Apple Music or custom)
- Gritty texture overlays (grain, halftone, VHS noise)
- Social media feed integration or follower count stats

**Motion character:** High-energy or brooding (matches artist genre). Fast GSAP staggers for rock/electronic. Slow cinematic reveals for ballad/classical. Text should feel like it's being performed — enter dramatically.

**Interaction signature:** Audio-reactive, play/pause on hover, album track list with preview snippets, tour date "Get Tickets" modals.

---

## ARCHETYPE 30 — REAL ESTATE / PROPERTY
> *"Welcome home."*

**Emotional job:** Aspiration, trust, lifestyle visualization, investment confidence
**Used for:** Real estate agencies, property developers, luxury homes, co-living, architecture firms

**Visual DNA:**
- Background: `#FAFAF8` clean off-white — airy, spacious feeling
- Ink: `#1A1A2E` deep navy-black — authoritative but not harsh
- Accent: `#2563EB` confident blue or `#C49B63` warm gold (for luxury)
- Surface: `#F0F0EC` light warmth for cards
- Border: `rgba(26, 26, 46, 0.08)` — barely visible, clean

**Typography:**
- Display: `Outfit` or `Plus Jakarta Sans` — modern, trustworthy geometric
- Body: `Inter` at 400 — maximum readability for property details
- Mono: `DM Mono` — for price, area (sqft/sqm), listing IDs

**Signature patterns:**
- Property image carousel with smooth transitions (hero)
- Stats bar: bedrooms, bathrooms, sqft, garage — icon + number
- Map integration (interactive or stylized static)
- Virtual tour CTA (prominent, inviting)
- Floor plan viewer
- Price displayed prominently with large typography
- Amenity grid with subtle icons
- Neighborhood description with lifestyle imagery

**Motion character:** Smooth, confident, trustworthy. Nothing flashy. Easing: `power2.out`. Images reveal with gentle scale transitions. Cards hover with subtle shadow elevation. Everything should feel solid and reliable.

**Interaction signature:** Image gallery with lightbox, property filter/search, mortgage calculator, schedule viewing form, neighborhood map.

---

## ARCHETYPE 31 — CRYPTO / WEB3
> *"Decentralized by design."*

**Emotional job:** Innovation, community belonging, futurism, transparency
**Used for:** Crypto projects, DAOs, DeFi platforms, NFT marketplaces, blockchain companies

**Visual DNA:**
- Background: `#0D0D1A` deep void — deeper than regular dark mode
- Surface: `#1A1A2E` with `border: 1px solid rgba(99, 102, 241, 0.15)` — neon-traced containers
- Ink: `#E2E8F0` cool gray-white
- Accent: `#8B5CF6` purple or `#10B981` emerald green
- Gradient: `linear-gradient(135deg, #6366F1, #8B5CF6, #06B6D4)` — the crypto gradient

**Typography:**
- Display: `Syne` or `Clash Display` — edgy geometric
- Body: `Inter` or `DM Sans` — clean, modern
- Mono: `Fira Code` or `JetBrains Mono` — essential for addresses, hashes, token IDs

**Signature patterns:**
- Animated gradient mesh background (slow-moving)
- Glassmorphism cards everywhere: `backdrop-filter: blur(20px); background: rgba(255,255,255,0.05)`
- Token price tickers (live or simulated)
- Wallet connect button (prominent, top-right)
- DAO governance stats (proposals, votes, treasury)
- NFT grid gallery with hover 3D tilt
- Roadmap as vertical timeline with glowing progress markers
- Community metrics: holders, transactions, TVL

**Motion character:** Smooth, futuristic, slightly cyberpunk. Glow effects on hover. Pulse animations on live data. Subtle parallax on cards. Nothing janky — Web3 sites that lag feel untrustworthy.

**Interaction signature:** Wallet connection flow, token swap interface, live charts, community Discord/Telegram links, animated roadmap.

