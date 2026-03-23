# Internationalisation (i18n) Guide
## Multi-language patterns for Awwwards-level sites

---

## WHEN YOU NEED i18n

- Client says "we need the site in English and French"
- Site targets multiple countries / languages
- User uploads a reference site with a language switcher in the nav
- Brand is international (luxury, fashion, global NGO, etc.)

---

## APPROACH 1 — HTML (single file, simple)

For HTML artifacts with 2–3 languages. No framework needed.

```html
<!-- Language switcher in nav -->
<div class="lang-switcher">
  <button class="lang-btn active" data-lang="en">EN</button>
  <button class="lang-btn" data-lang="fr">FR</button>
  <button class="lang-btn" data-lang="ar">AR</button>
</div>
```

```javascript
// Translation data object
const translations = {
  en: {
    nav_home: 'Home',
    nav_work: 'Work',
    nav_about: 'About',
    nav_contact: 'Contact',
    hero_title: 'Creative Director',
    hero_sub: 'Based in London, working globally',
    about_heading: 'About',
    about_body: 'Ten years of directing...',
    contact_cta: 'Start a project',
  },
  fr: {
    nav_home: 'Accueil',
    nav_work: 'Travaux',
    nav_about: 'À propos',
    nav_contact: 'Contact',
    hero_title: 'Directeur Créatif',
    hero_sub: 'Basé à Londres, travail mondial',
    about_heading: 'À propos',
    about_body: 'Dix ans de direction...',
    contact_cta: 'Démarrer un projet',
  },
  ar: {
    nav_home: 'الرئيسية',
    nav_work: 'الأعمال',
    nav_about: 'عن',
    nav_contact: 'تواصل',
    hero_title: 'مدير إبداعي',
    hero_sub: 'مقيم في لندن، أعمل عالمياً',
    about_heading: 'عن',
    about_body: 'عشر سنوات من الإخراج...',
    contact_cta: 'ابدأ مشروعاً',
  }
}

let currentLang = localStorage.getItem('lang') || 'en'

function applyTranslation(lang) {
  currentLang = lang
  localStorage.setItem('lang', lang)
  const t = translations[lang]

  // Update all elements with data-i18n attribute
  document.querySelectorAll('[data-i18n]').forEach(el => {
    const key = el.getAttribute('data-i18n')
    if (t[key]) el.textContent = t[key]
  })

  // Handle RTL languages
  const rtlLangs = ['ar', 'he', 'fa', 'ur']
  document.documentElement.setAttribute('dir', rtlLangs.includes(lang) ? 'rtl' : 'ltr')
  document.documentElement.setAttribute('lang', lang)

  // Update active button
  document.querySelectorAll('.lang-btn').forEach(btn => {
    btn.classList.toggle('active', btn.dataset.lang === lang)
  })

  // Fade transition
  gsap.fromTo('body', { opacity: 0.7 }, { opacity: 1, duration: 0.3 })
}

// Wire up buttons
document.querySelectorAll('.lang-btn').forEach(btn => {
  btn.addEventListener('click', () => applyTranslation(btn.dataset.lang))
})

// Apply on load
applyTranslation(currentLang)
```

```html
<!-- Mark every translatable element with data-i18n -->
<nav>
  <a href="#home"    data-i18n="nav_home">Home</a>
  <a href="#work"    data-i18n="nav_work">Work</a>
  <a href="#about"   data-i18n="nav_about">About</a>
  <a href="#contact" data-i18n="nav_contact">Contact</a>
</nav>
<h1 data-i18n="hero_title">Creative Director</h1>
<p  data-i18n="hero_sub">Based in London, working globally</p>
```

---

## APPROACH 2 — NEXT.JS (next-intl, recommended)

```bash
npm install next-intl
```

### File structure
```
messages/
├── en.json
├── fr.json
└── ar.json
src/
├── i18n.ts
├── middleware.ts
└── app/
    └── [locale]/
        ├── layout.tsx
        └── page.tsx
```

### messages/en.json
```json
{
  "nav": {
    "home": "Home",
    "work": "Work",
    "about": "About",
    "contact": "Contact"
  },
  "hero": {
    "title": "Creative Director",
    "subtitle": "Based in London, working globally",
    "cta": "View my work"
  },
  "about": {
    "heading": "About",
    "body": "Ten years of directing..."
  }
}
```

### middleware.ts (auto-detect and redirect)
```typescript
import createMiddleware from 'next-intl/middleware'

export default createMiddleware({
  locales: ['en', 'fr', 'ar'],
  defaultLocale: 'en',
  localeDetection: true, // auto-detect from browser
})

export const config = {
  matcher: ['/((?!api|_next|.*\\..*).*)']
}
```

### app/[locale]/layout.tsx
```tsx
import { NextIntlClientProvider } from 'next-intl'
import { getMessages } from 'next-intl/server'

export default async function LocaleLayout({
  children, params: { locale }
}: {
  children: React.ReactNode, params: { locale: string }
}) {
  const messages = await getMessages()

  return (
    <html lang={locale} dir={['ar', 'he', 'fa'].includes(locale) ? 'rtl' : 'ltr'}>
      <body>
        <NextIntlClientProvider messages={messages}>
          {children}
        </NextIntlClientProvider>
      </body>
    </html>
  )
}
```

### Using translations in components
```tsx
// Server component
import { useTranslations } from 'next-intl'

export default function HeroSection() {
  const t = useTranslations('hero')
  return (
    <section>
      <h1>{t('title')}</h1>
      <p>{t('subtitle')}</p>
      <a href="/work">{t('cta')}</a>
    </section>
  )
}

// Client component
'use client'
import { useTranslations } from 'next-intl'
export function NavMenu() {
  const t = useTranslations('nav')
  return <nav><a href="/">{t('home')}</a></nav>
}
```

### Language switcher component
```tsx
'use client'
import { useLocale } from 'next-intl'
import { useRouter, usePathname } from 'next/navigation'

const LANGUAGES = [
  { code: 'en', label: 'EN', name: 'English' },
  { code: 'fr', label: 'FR', name: 'Français' },
  { code: 'ar', label: 'AR', name: 'العربية' },
]

export function LanguageSwitcher() {
  const locale = useLocale()
  const router = useRouter()
  const pathname = usePathname()

  const switchLocale = (newLocale: string) => {
    // Replace current locale in path with new locale
    const newPath = pathname.replace(`/${locale}`, `/${newLocale}`)
    router.push(newPath)
  }

  return (
    <div className="lang-switcher">
      {LANGUAGES.map(lang => (
        <button
          key={lang.code}
          onClick={() => switchLocale(lang.code)}
          className={locale === lang.code ? 'active' : ''}
          aria-label={`Switch to ${lang.name}`}
        >
          {lang.label}
        </button>
      ))}
    </div>
  )
}
```

---

## RTL SUPPORT (Arabic, Hebrew, Farsi, Urdu)

RTL (right-to-left) requires CSS adjustments. Use logical properties instead of `left`/`right`:

```css
/* Instead of left/right, use logical properties */

/* ❌ Wrong — breaks in RTL */
.nav { padding-left: 2rem; }
.icon { margin-right: 8px; }

/* ✅ Correct — works in both LTR and RTL */
.nav { padding-inline-start: 2rem; }
.icon { margin-inline-end: 8px; }

/* Logical property reference */
/* left  → inline-start  */
/* right → inline-end    */
/* top   → block-start   */
/* bottom → block-end    */

/* Flex direction auto-flips with dir="rtl" on html element */
/* Text alignment auto-flips with logical values */
.text { text-align: start; } /* = left in LTR, right in RTL */

/* RTL-specific font override (Arabic needs larger size to match Latin) */
:root[lang="ar"] {
  --font-body: 'Cairo', 'Noto Sans Arabic', sans-serif;
  --text-body-size: 1.1rem; /* Arabic text reads smaller at same px */
  line-height: 1.9; /* Arabic needs more line height */
}
```

```html
<!-- Arabic Google Font -->
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700&display=swap" rel="stylesheet"/>
```

---

## SEO FOR MULTI-LANGUAGE SITES

```html
<!-- hreflang tags — tell Google about language versions -->
<link rel="alternate" hreflang="en" href="https://example.com/en/page"/>
<link rel="alternate" hreflang="fr" href="https://example.com/fr/page"/>
<link rel="alternate" hreflang="ar" href="https://example.com/ar/page"/>
<link rel="alternate" hreflang="x-default" href="https://example.com/en/page"/>
```

```tsx
// In Next.js generateMetadata
export async function generateMetadata({ params: { locale } }) {
  return {
    alternates: {
      canonical: `https://example.com/${locale}`,
      languages: {
        'en': 'https://example.com/en',
        'fr': 'https://example.com/fr',
        'ar': 'https://example.com/ar',
      }
    }
  }
}
```

---

## QUICK CHECKLIST

- [ ] Every text element marked with `data-i18n` (HTML) or `t('key')` (Next.js)
- [ ] `lang` attribute updated on `<html>` when language changes
- [ ] `dir="rtl"` applied for Arabic/Hebrew/Farsi/Urdu
- [ ] RTL languages use logical CSS properties (`inline-start` not `left`)
- [ ] Arabic/Hebrew font loaded (Cairo, Noto Sans Arabic)
- [ ] Language preference saved to `localStorage`
- [ ] `hreflang` tags in `<head>` for SEO
- [ ] Language switcher visible and accessible (keyboard-navigable)
- [ ] Animations work in both LTR and RTL (check GSAP `x` values don't break)
