# Framework Guide
## Next.js · React · Vue · Svelte — Awwwards-level patterns per framework

---

## NEXT.JS (App Router) — MULTI-PAGE SITES

Use for: portfolios, agency sites, editorial sites, anything with multiple real pages.

### Page structure
```
app/
├── layout.tsx          ← Root layout (nav + footer + shared providers)
├── page.tsx            ← Homepage
├── about/page.tsx      ← About page
├── work/
│   ├── page.tsx        ← Work index
│   └── [slug]/page.tsx ← Individual work detail
├── contact/page.tsx    ← Contact page
└── globals.css
```

### Root layout with GSAP + Lenis
```tsx
// app/layout.tsx
'use client'
import { useEffect } from 'react'
import { usePathname } from 'next/navigation'
import Lenis from '@studio-freight/lenis'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import Nav from '@/components/Nav'
import Footer from '@/components/Footer'
import Cursor from '@/components/Cursor'
import Grain from '@/components/Grain'

gsap.registerPlugin(ScrollTrigger)

export default function RootLayout({ children }: { children: React.ReactNode }) {
  const pathname = usePathname()

  useEffect(() => {
    const lenis = new Lenis({
      duration: 1.4,
      easing: t => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
    })
    lenis.on('scroll', ScrollTrigger.update)
    gsap.ticker.add(time => lenis.raf(time * 1000))
    gsap.ticker.lagSmoothing(0)
    return () => { lenis.destroy(); gsap.ticker.remove(time => lenis.raf(time * 1000)) }
  }, [])

  return (
    <html lang="en">
      <body>
        <Grain />
        <Cursor />
        <Nav />
        <main>{children}</main>
        <Footer />
      </body>
    </html>
  )
}
```

### Page transition (Next.js App Router)
```tsx
// components/PageTransition.tsx
'use client'
import { useEffect, useRef } from 'react'
import { usePathname } from 'next/navigation'
import { gsap } from 'gsap'

export default function PageTransition({ children }: { children: React.ReactNode }) {
  const pathname = usePathname()
  const ref = useRef<HTMLDivElement>(null)

  useEffect(() => {
    if (!ref.current) return
    gsap.fromTo(ref.current,
      { opacity: 0, y: 20 },
      { opacity: 1, y: 0, duration: 0.8, ease: 'power3.out' }
    )
  }, [pathname])

  return <div ref={ref}>{children}</div>
}
```

### useGSAP hook (prevents memory leaks)
```tsx
// components/HeroSection.tsx
'use client'
import { useRef } from 'react'
import { useGSAP } from '@gsap/react'
import { gsap } from 'gsap'
import SplitType from 'split-type'

export default function HeroSection() {
  const container = useRef<HTMLDivElement>(null)

  useGSAP(() => {
    // All GSAP code here is automatically cleaned up
    const title = container.current?.querySelector('.hero-title')
    if (!title) return

    const split = new SplitType(title as HTMLElement, { types: 'chars' })
    gsap.from(split.chars, {
      opacity: 0, y: 60, rotateX: -45,
      transformOrigin: '0% 50% -20px',
      duration: 1.1, stagger: 0.025, ease: 'power4.out', delay: 0.3
    })
  }, { scope: container })

  return (
    <div ref={container} className="hero">
      <h1 className="hero-title">Your Name</h1>
    </div>
  )
}
```

---

## REACT (JSX Artifact / Vite)

Use for: single-page experiences, interactive components, demos.

### GSAP in React (with cleanup)
```jsx
import { useEffect, useRef } from "react"

function AnimatedSection() {
  const ref = useRef(null)

  useEffect(() => {
    const ctx = gsap.context(() => {
      gsap.from('.animate-in', {
        opacity: 0, y: 40, duration: 1, stagger: 0.1, ease: 'power3.out'
      })
    }, ref) // scope to this component

    return () => ctx.revert() // cleanup on unmount
  }, [])

  return (
    <div ref={ref}>
      <p className="animate-in">First element</p>
      <p className="animate-in">Second element</p>
    </div>
  )
}
```

### Multi-page React (hash routing — no router needed for artifacts)
```jsx
import { useState } from "react"

const PAGES = {
  home: <HomePage />,
  about: <AboutPage />,
  work: <WorkPage />,
  contact: <ContactPage />,
}

function App() {
  const [page, setPage] = useState('home')

  const navigate = (to) => {
    gsap.to('.page-content', {
      opacity: 0, y: -15, duration: 0.4, ease: 'power2.in',
      onComplete: () => {
        setPage(to)
        gsap.fromTo('.page-content',
          { opacity: 0, y: 15 },
          { opacity: 1, y: 0, duration: 0.6, ease: 'power3.out' }
        )
      }
    })
  }

  return (
    <div>
      <nav>
        {Object.keys(PAGES).map(p => (
          <button
            key={p}
            onClick={() => navigate(p)}
            className={page === p ? 'nav-active' : ''}
          >
            {p}
          </button>
        ))}
      </nav>
      <div className="page-content">
        {PAGES[page]}
      </div>
    </div>
  )
}
```

---

## VUE 3 (Composition API)

Use for: Vue projects, Nuxt.js sites.

### GSAP with Vue lifecycle
```vue
<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

const container = ref(null)
let ctx

onMounted(() => {
  ctx = gsap.context(() => {
    gsap.from('.hero-title span', {
      opacity: 0, y: 60,
      duration: 1.1, stagger: 0.025, ease: 'power4.out'
    })
  }, container.value)
})

onUnmounted(() => ctx?.revert())
</script>

<template>
  <div ref="container" class="hero">
    <h1 class="hero-title">
      <span v-for="char in 'Your Title'.split('')" :key="char">{{ char }}</span>
    </h1>
  </div>
</template>
```

### Vue Router page transitions
```vue
<!-- App.vue -->
<template>
  <RouterView v-slot="{ Component }">
    <Transition name="page" mode="out-in">
      <component :is="Component" />
    </Transition>
  </RouterView>
</template>

<style>
.page-enter-active, .page-leave-active { transition: all 0.6s ease; }
.page-enter-from { opacity: 0; transform: translateY(20px); }
.page-leave-to   { opacity: 0; transform: translateY(-20px); }
</style>
```

---

## SVELTE / SVELTEKIT

Use for: SvelteKit projects, performance-focused sites.

### GSAP in Svelte
```svelte
<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'

  let container

  onMount(() => {
    const ctx = gsap.context(() => {
      gsap.from('.hero-char', {
        opacity: 0, y: 60,
        duration: 1.1, stagger: 0.025, ease: 'power4.out'
      })
    }, container)

    return () => ctx.revert() // onDestroy equivalent
  })
</script>

<div bind:this={container} class="hero">
  <h1>
    {#each 'Your Title'.split('') as char}
      <span class="hero-char">{char}</span>
    {/each}
  </h1>
</div>
```

### SvelteKit page transitions
```svelte
<!-- src/routes/+layout.svelte -->
<script>
  import { fade } from 'svelte/transition'
  import { navigating } from '$app/stores'
</script>

{#key $navigating?.to?.url.pathname}
  <div in:fade={{ duration: 600, delay: 100 }} out:fade={{ duration: 400 }}>
    <slot />
  </div>
{/key}
```

---

## COMPLETE SITE STRUCTURE — TEMPLATE (any framework)

Every complete site needs these sections/pages. Never ship without them.

### Homepage sections (in order)
1. **Loader** — branded, sets emotional tone
2. **Nav** — fixed, subtle, with all page links working
3. **Hero** — full viewport, signature moment
4. **Featured work / key content** — 3–6 items
5. **About teaser** — 2–3 sentences + link to full About
6. **Process or values** (optional but differentiating)
7. **Contact CTA** — one strong call to action
8. **Footer** — links, social, copyright

### About page sections
1. **Hero** — name/brand + one powerful statement
2. **Biography/story** — real copy, 150–300 words
3. **Skills or expertise** — specific, not generic
4. **Selected clients or collaborators** (if applicable)
5. **Contact link or CTA**

### Work/Projects index
1. **Grid or list of projects** — title, category, year
2. **Filter by category** (if >6 projects)
3. **Hover state** — preview image or color change

### Work/Project detail
1. **Project hero** — title, client, year, role
2. **Project description** — what was the challenge, what was built
3. **Visual showcase** — images, video, screenshots
4. **Next/Prev project navigation**

### Contact page
1. **Headline** — not "Contact Us", something human
2. **Short intro** — what to reach out about
3. **Form or direct email** — always functional
4. **Location/timezone** (optional but builds trust)

---

## PAGE TRANSITION — UNIVERSAL PATTERN

Works across all frameworks. Adapt the selectors.

```javascript
// Cinematic page transition — works in HTML, React, Vue, Svelte
class PageTransitionManager {
  constructor() {
    this.overlay = this.createOverlay()
    this.bindLinks()
  }

  createOverlay() {
    const el = document.createElement('div')
    el.style.cssText = `
      position:fixed; inset:0; z-index:9999; pointer-events:none;
      background:#F2EFE9; opacity:0; transform-origin:bottom;
    `
    document.body.appendChild(el)
    return el
  }

  bindLinks() {
    document.querySelectorAll('a[href^="/"], a[href^="#"]').forEach(link => {
      if (link.getAttribute('target') === '_blank') return
      link.addEventListener('click', (e) => {
        const href = link.getAttribute('href')
        if (href.startsWith('#')) return // allow anchor links
        e.preventDefault()
        this.transitionTo(href)
      })
    })
  }

  transitionTo(href) {
    gsap.to(this.overlay, {
      opacity: 1, duration: 0.5, ease: 'power2.inOut',
      onComplete: () => window.location.href = href
    })
  }

  // Call on every page load
  static revealPage() {
    const overlay = document.querySelector('.transition-overlay')
    if (!overlay) return
    gsap.to(overlay, { opacity: 0, duration: 0.7, ease: 'power2.out', delay: 0.1 })
  }
}

// Initialize
const transitions = new PageTransitionManager()
window.addEventListener('load', PageTransitionManager.revealPage)
```
