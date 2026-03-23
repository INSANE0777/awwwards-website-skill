# Advanced SEO for Creative Experiences

Creative websites often struggle with SEO due to heavy JS usage. This guide ensures your site ranks while remaining visually extraordinary.

## Technical Foundation

### 1. Server-Side Rendering (SSR) / SSG
- **Crucial**: Ensure all content (even if hidden by reveals) is in the initial HTML.
- **No-JS Fallback**: Provide a meaningful `noscript` experience for crawlers.

### 2. Schema.org for Awards
Add `CreativeWork` schema to help Google understand the project context:
```json
{
  "@context": "https://schema.org",
  "@type": "CreativeWork",
  "name": "Project Name",
  "author": "Your Agency",
  "award": "Awwwards Site of the Day",
  "description": "An immersive WebGL experience exploring...",
  "thumbnailUrl": "https://yoursite.com/thumb.jpg"
}
```

## Immersive Content SEO

### 1. Hidden Content
- Content inside WebGL canvases is invisible to crawlers.
- **Mirror Strategy**: Keep a hidden `section` with semantic HTML that mirrors the canvas text for indexing.

### 2. Page Transitions & History
- Use the **View Transitions API** correctly with standard URL structures.
- Ensure `canonical` tags update on every virtual page change.

## Performance & Core Web Vitals

- **INP (Interaction to Next Paint)**: High-end sites often fail this. Use `requestIdleCallback` for heavy calculations to keep the main thread responsive.

## Indexing Tools
- **IndexNow API**: Instantly notify search engines of content updates.
- **GSC API**: Automate the submission of new experimental sub-pages.
