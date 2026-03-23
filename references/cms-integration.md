# Headless CMS Integration (Creative Hand-off)
## Managing High-End Sites via Sanity/Contentful

A common failure is a site that looks great but is "hard-coded". In 2026, clients want to edit their 3D portfolio.

---

## 1. THE "SCHEMA-AS-CODE" APPROACH (Sanity)
Define 3D parameters inside the CMS.

```javascript
// Sanity Schema
export default {
  name: 'project',
  fields: [
    { name: 'title', type: 'string' },
    { 
      name: 'glslParams', 
      type: 'object',
      fields: [
        { name: 'noiseScale', type: 'number' },
        { name: 'primaryColor', type: 'color' }
      ]
    }
  ]
}
```

---

## 2. LIVE PREVIEW (The "Holy Grail")
The client edits a color in Sanity, and the WebGL scene update in real-time.

- **Tech:** Sanity Live Content API + WebSockets.
- **Implementation:**
  ```javascript
  useQuery(PROJECT_QUERY).subscribe(updateThreeJSScene);
  ```

---

## 3. ASSET PIPELINE SYNC
When a client uploads a raw `.obj` to the CMS, use a **Serverless Function (Vercel/Cloudflare)** to automatically run it through `gltf-pipeline` for Draco compression before it hits the frontend.

---

## 4. CLINICALLY POLISHED HAND-OFF
- **The "Safety Rail":** Limit number of characters in titles to prevent breaking GSAP layouts.
- **The "Sandbox":** Provide a hidden `/admin/debug` page where the client can test how their content looks in the 3D world before publishing.
