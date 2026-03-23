# Asset Sourcing & Image Generation Guide
## Where to find images, icons, videos, and how to generate custom assets

---

## IMAGE GENERATION (generate_image tool)

Use the `generate_image` tool when you need custom illustrations, backgrounds, or brand-specific imagery that doesn't exist in stock libraries.

### When to Generate
- Custom hero illustrations matching brand aesthetic
- Brand mascots or characters (e.g., DontBoardMe's shaggy dog)
- Abstract backgrounds or patterns
- Product mockups without real photography
- Icon sets with a specific style

### Prompt Tips for Best Results
```
✅ Good: "A hand-drawn minimalist illustration of a shaggy white Old English Sheepdog 
         holding a red leash, flat design, playful aesthetic, isolated on transparent 
         background"

❌ Bad:  "A dog"
```

**Key elements to specify:**
- Art style (hand-drawn, flat, 3D render, watercolor, photorealistic)
- Color palette (mention specific colors)
- Background (transparent, solid color, gradient)
- Mood (playful, professional, dramatic, minimal)
- Composition (isolated, full scene, close-up)

---

## FREE STOCK PHOTOGRAPHY

| Source | Best for | License |
|---|---|---|
| [Unsplash](https://unsplash.com) | High-quality general photography | Free, no attribution |
| [Pexels](https://pexels.com) | Lifestyle, people, nature | Free, no attribution |
| [Pixabay](https://pixabay.com) | Vectors, illustrations, photos | Free, no attribution |
| [Burst](https://burst.shopify.com) | E-commerce, business | Free |

### How to Use in HTML Artifacts
```html
<!-- Always use high-quality parameters -->
<img src="https://images.unsplash.com/photo-XXXX?q=80&w=1200&auto=format&fit=crop"
     alt="Descriptive alt text"
     loading="lazy" />

<!-- Key Unsplash URL parameters -->
<!-- q=80    → quality (80 is sweet spot) -->
<!-- w=1200  → width in pixels -->
<!-- auto=format → WebP when supported -->
<!-- fit=crop → crop to dimensions -->
```

---

## SVG ICON SETS

| Library | Style | CDN |
|---|---|---|
| [Lucide](https://lucide.dev) | Clean, minimal line icons | `unpkg.com/lucide@latest` |
| [Phosphor](https://phosphoricons.com) | Flexible, 6 weights | `unpkg.com/@phosphor-icons/web` |
| [Heroicons](https://heroicons.com) | Tailwind companion | Copy SVG directly |
| [Feather](https://feathericons.com) | Simple, consisent | `unpkg.com/feather-icons` |

### Inline SVG (preferred for animation)
```html
<!-- Copy SVGs directly for animation control -->
<svg width="24" height="24" viewBox="0 0 24 24" fill="none"
     stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <path d="M7 17L17 7M17 7H7M17 7V17"/>
</svg>
```

### Social Media SVG Icons
```html
<!-- Instagram -->
<svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
  <rect x="2" y="2" width="20" height="20" rx="5" ry="5"/>
  <path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z"/>
  <line x1="17.5" y1="6.5" x2="17.51" y2="6.5"/>
</svg>

<!-- Twitter/X -->
<svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor">
  <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
</svg>

<!-- LinkedIn -->
<svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
  <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"/>
  <rect x="2" y="9" width="4" height="12"/><circle cx="4" cy="4" r="2"/>
</svg>

<!-- GitHub -->
<svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor">
  <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23A11.509 11.509 0 0112 5.803c1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576C20.566 21.797 24 17.3 24 12c0-6.627-5.373-12-12-12z"/>
</svg>
```

---

## FREE VIDEO

| Source | Best for |
|---|---|
| [Coverr](https://coverr.co) | Hero background videos |
| [Pexels Videos](https://pexels.com/videos) | General stock video |
| [Mixkit](https://mixkit.co) | Art, nature, abstract |

```html
<video autoplay muted loop playsinline preload="auto">
  <source src="hero.mp4" type="video/mp4" />
</video>
```

---

## TEXTURES & PATTERNS

| Source | Type |
|---|---|
| [Subtle Patterns](https://subtlepatterns.com) | Tileable backgrounds |
| [Hero Patterns](https://heropatterns.com) | SVG pattern generator |
| [Cool Backgrounds](https://coolbackgrounds.io) | Gradient, particle, triangle |

---

## RULES

1. **Never use placeholder images in production** — use `generate_image` or real stock
2. **Always set `alt` text** — descriptive for content images, empty for decorative
3. **Inline SVG icons** when animating — external SVGs can't be styled via CSS
4. **Use `currentColor`** in SVGs — adapts to text color and dark mode
5. **Optimize before embedding** — use `?w=1200&q=80` parameters on Unsplash URLs
6. **Credit is not required** for Unsplash/Pexels but is appreciated
