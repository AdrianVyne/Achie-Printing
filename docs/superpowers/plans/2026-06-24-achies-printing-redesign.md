# Achie's Printing — Full Website Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rebuild Achie's Printing website from Bootstrap 5 to Tailwind CSS with a friendly/playful sticker-style design, Messenger-first CTAs, AI illustrations, and full SEO implementation across 15 static HTML pages.

**Architecture:** 15 static HTML files share a common CDN head block, nav, footer, and floating Messenger CTA defined in Task 1 and Task 3 (copy-paste into every page — no include mechanism in static HTML). Tailwind Play CDN handles styling via a custom config block. Alpine.js handles mobile nav and FAQ accordion. Swiper.js handles carousels. GLightbox handles PDP galleries. AOS handles scroll-reveal. All existing image assets are kept; new AI-generated images are added.

**Tech Stack:** Tailwind CSS v3 (Play CDN), Alpine.js v3, Swiper.js v11, AOS v2.3.4, GLightbox latest, Google Fonts (Nunito 400/700/900), Phosphor Icons Web

## Global Constraints

- No npm, no build step — CDN-only, pure static HTML deployable to GitHub Pages
- All Messenger links: `https://m.me/110237597413659`
- Facebook chat plugin `page_id`: `110237597413659`
- Store hours: Monday–Sunday, 8:00 AM – 7:00 PM (Philippine Time UTC+08:00)
- Address: Doña Pilar Village, Davao City, Philippines
- Phone: +63 901 234 5678 | Email: ansheened@gmail.com
- Google Maps embed src: `https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d989.7589499563268!2d125.64776928811081!3d7.121849299677013!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x32f96d94dfc9c805%3A0x34e4d7ac2fac8395!2sAchie&#39;s%20Printing!5e0!3m2!1sen!2sph!4v1673847724656!5m2!1sen!2sph`
- Geo: lat `7.121849299677013`, lng `125.64776928811081`
- Hero headline: **"The Printing Shop Doña Pilar Grew Up With."**
- Colors: coral `#FF6B6B` | sunny `#FFD93D` | mint `#6BCB77` | charcoal `#2D2D2D` | cream `#FFFDF7`
- Font: Nunito (400, 700, 900)
- One `<h1>` per page; semantic `<main>`, `<section>`, `<article>`
- Commit after every task

---

## Shared Reference Blocks

These blocks are copy-pasted into every page. Defined once here; tasks reference them by name.

### SHARED-HEAD (paste inside `<head>` of every page)

```html
<!-- Google Fonts: Nunito -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;700;900&display=swap" rel="stylesheet">
<!-- Tailwind CSS Play CDN -->
<script src="https://cdn.tailwindcss.com"></script>
<script>
  tailwind.config = {
    theme: {
      extend: {
        colors: {
          coral: '#FF6B6B', sunny: '#FFD93D', mint: '#6BCB77',
          charcoal: '#2D2D2D', cream: '#FFFDF7',
        },
        fontFamily: { nunito: ['Nunito', 'sans-serif'] },
      }
    }
  }
</script>
<!-- Alpine.js -->
<script defer src="https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js"></script>
<!-- AOS -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aos@2.3.4/dist/aos.css"/>
<script src="https://cdn.jsdelivr.net/npm/aos@2.3.4/dist/aos.js"></script>
<!-- Phosphor Icons -->
<script src="https://unpkg.com/@phosphor-icons/web"></script>
<!-- Custom CSS -->
<link href="styles.css" rel="stylesheet">
```

### SHARED-HEAD-PDP (PDPs only — adds GLightbox on top of SHARED-HEAD)

```html
<!-- GLightbox -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/glightbox/dist/css/glightbox.min.css"/>
<script src="https://cdn.jsdelivr.net/npm/glightbox/dist/js/glightbox.min.js"></script>
```

### SHARED-HEAD-HOME (homepage only — adds Swiper on top of SHARED-HEAD)

```html
<!-- Swiper.js -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.css"/>
<script src="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.js"></script>
```

### NAV (paste into every page `<body>` as first element)

```html
<nav class="sticky top-0 z-50 bg-cream border-b-2 border-charcoal font-nunito" x-data="{ open: false }">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="flex items-center justify-between h-16">
      <a href="index.html" class="flex items-center gap-2">
        <img src="img/favicon.png" alt="Achie's Printing" class="h-9 w-9 rounded-full border-2 border-charcoal object-cover">
        <span class="font-black text-xl text-charcoal leading-tight">Achie's<br><span class="text-coral">Printing</span></span>
      </a>
      <div class="hidden md:flex items-center gap-6">
        <a href="index.html#products" class="font-bold text-charcoal hover:text-coral transition-colors">Products</a>
        <a href="index.html#contact" class="font-bold text-charcoal hover:text-coral transition-colors">Contact</a>
        <a href="index.html#faq" class="font-bold text-charcoal hover:text-coral transition-colors">FAQ</a>
        <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
           class="sticker-btn bg-coral text-white px-5 py-2 text-sm">Message Us</a>
      </div>
      <button @click="open = !open" class="md:hidden p-1" aria-label="Toggle menu">
        <i class="ph ph-list text-2xl text-charcoal" x-show="!open" x-cloak></i>
        <i class="ph ph-x text-2xl text-charcoal" x-show="open" x-cloak></i>
      </button>
    </div>
    <div x-show="open" x-transition class="md:hidden pb-4 flex flex-col gap-3 border-t border-charcoal/20 pt-3">
      <a href="index.html#products" @click="open=false" class="font-bold text-charcoal py-1">Products</a>
      <a href="index.html#contact" @click="open=false" class="font-bold text-charcoal py-1">Contact</a>
      <a href="index.html#faq" @click="open=false" class="font-bold text-charcoal py-1">FAQ</a>
      <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
         class="sticker-btn bg-coral text-white px-5 py-3 text-center text-sm">Message Us</a>
    </div>
  </div>
</nav>
```

### FOOTER (paste before `</body>` on every page)

```html
<footer class="bg-charcoal text-cream py-12 font-nunito">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="grid grid-cols-1 md:grid-cols-3 gap-8 pb-8 border-b border-white/20">
      <div>
        <h3 class="font-black text-xl text-white mb-2">Achie's Printing</h3>
        <p class="text-white/70 text-sm leading-relaxed">The printing shop Doña Pilar grew up with. Quick, honest, and priced right.</p>
      </div>
      <div>
        <h4 class="font-bold text-white mb-3">Store Hours</h4>
        <p class="text-white/70 text-sm">Monday – Sunday</p>
        <p class="text-white/70 text-sm">8:00 AM – 7:00 PM</p>
        <p class="text-white/50 text-xs mt-1">Philippine Time (UTC+08:00)</p>
      </div>
      <div>
        <h4 class="font-bold text-white mb-3">Connect</h4>
        <div class="flex gap-3 mb-4">
          <a href="https://www.facebook.com/profile.php?id=100052801946765" target="_blank" rel="noopener" aria-label="Facebook"
             class="bg-white/10 hover:bg-coral rounded-full p-2 transition-colors">
            <i class="ph ph-facebook-logo text-xl text-white"></i></a>
          <a href="https://www.instagram.com/" target="_blank" rel="noopener" aria-label="Instagram"
             class="bg-white/10 hover:bg-coral rounded-full p-2 transition-colors">
            <i class="ph ph-instagram-logo text-xl text-white"></i></a>
          <a href="mailto:ansheened@gmail.com" aria-label="Email"
             class="bg-white/10 hover:bg-coral rounded-full p-2 transition-colors">
            <i class="ph ph-envelope text-xl text-white"></i></a>
          <a href="https://m.me/110237597413659" target="_blank" rel="noopener" aria-label="Messenger"
             class="bg-white/10 hover:bg-coral rounded-full p-2 transition-colors">
            <i class="ph ph-messenger-logo text-xl text-white"></i></a>
        </div>
        <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
           class="sticker-btn bg-coral text-white px-5 py-2 text-sm block text-center">Send us a message</a>
      </div>
    </div>
    <p class="text-center text-white/40 text-xs mt-6">© 2025 Achie's Printing – Doña Pilar Village, Davao City, Philippines</p>
  </div>
</footer>
```

### FLOATING-CTA (paste before FOOTER on every page)

```html
<div class="messenger-float">
  <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
     aria-label="Message us on Facebook Messenger"
     class="sticker-btn bg-coral text-white flex items-center gap-2 px-4 py-3 text-sm">
    <i class="ph-fill ph-messenger-logo text-xl"></i>
    <span class="hidden sm:inline font-bold">Message Us</span>
  </a>
</div>
```

### AOS-INIT (paste before `</body>` on every page, after all other scripts)

```html
<script>AOS.init({ duration: 600, once: true, easing: 'ease-out-quad' });</script>
```

### FB-CHAT (paste before `</body>`, keep existing — no changes needed)

```html
<div id="fb-root"></div>
<div id="fb-customer-chat" class="fb-customerchat"></div>
<script>
  var chatbox = document.getElementById("fb-customer-chat");
  chatbox.setAttribute("page_id", "110237597413659");
  chatbox.setAttribute("attribution", "biz_inbox");
</script>
<script>
  window.fbAsyncInit = function () { FB.init({ xfbml: true, version: "v15.0" }); };
  (function (d, s, id) {
    var js, fjs = d.getElementsByTagName(s)[0];
    if (d.getElementById(id)) return;
    js = d.createElement(s); js.id = id;
    js.src = "https://connect.facebook.net/en_US/sdk/xfbml.customerchat.js";
    fjs.parentNode.insertBefore(js, fjs);
  })(document, "script", "facebook-jssdk");
</script>
```

---

## Task 1: Replace styles.css + Verify Tailwind Works

**Files:**
- Modify: `styles.css` (full replace)

- [ ] **Step 1: Replace entire contents of `styles.css` with:**

```css
html { scroll-behavior: smooth; }

body {
  font-family: 'Nunito', sans-serif;
  background-color: #FFFDF7;
  color: #2D2D2D;
}

[x-cloak] { display: none !important; }

/* Sticker card */
.sticker-card {
  border: 2px solid #2D2D2D;
  border-radius: 1rem;
  box-shadow: 4px 4px 0px #2D2D2D;
  transition: box-shadow 0.15s ease, transform 0.15s ease;
  background: #fff;
}
.sticker-card:hover {
  box-shadow: 2px 2px 0px #2D2D2D;
  transform: translate(2px, 2px);
}

/* Sticker button */
.sticker-btn {
  display: inline-block;
  text-decoration: none;
  font-family: 'Nunito', sans-serif;
  font-weight: 900;
  border: 2px solid #2D2D2D;
  border-radius: 9999px;
  box-shadow: 4px 4px 0px #2D2D2D;
  transition: box-shadow 0.15s ease, transform 0.15s ease;
  cursor: pointer;
  line-height: 1;
}
.sticker-btn:hover {
  box-shadow: 2px 2px 0px #2D2D2D;
  transform: translate(2px, 2px);
}

/* Product card image hover swap */
.product-card {
  position: relative;
  overflow: hidden;
  height: 220px;
}
.product-card img.img-default,
.product-card img.img-hover {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: opacity 0.35s ease;
}
.product-card img.img-default { opacity: 1; }
.product-card img.img-hover { opacity: 0; }
.product-card:hover img.img-default { opacity: 0; }
.product-card:hover img.img-hover { opacity: 1; }
.product-card .card-label {
  position: absolute;
  bottom: 0; left: 0; right: 0;
  background: rgba(45,45,45,0.85);
  color: #fff;
  text-align: center;
  padding: 0.5rem;
  font-weight: 700;
  font-family: 'Nunito', sans-serif;
  font-size: 0.875rem;
  z-index: 2;
  transition: opacity 0.2s ease;
}
.product-card .card-order {
  position: absolute;
  bottom: 0; left: 0; right: 0;
  background: #FF6B6B;
  color: #fff;
  text-align: center;
  padding: 0.65rem;
  font-weight: 900;
  font-family: 'Nunito', sans-serif;
  font-size: 0.875rem;
  z-index: 3;
  transform: translateY(100%);
  transition: transform 0.25s ease;
}
.product-card:hover .card-label { opacity: 0; }
.product-card:hover .card-order { transform: translateY(0); }

/* Swiper */
.swiper-pagination-bullet-active { background-color: #FF6B6B !important; }
.swiper-button-next, .swiper-button-prev { color: #FF6B6B !important; }

/* Floating Messenger */
.messenger-float {
  position: fixed;
  bottom: 1.5rem;
  right: 1.5rem;
  z-index: 999;
}

/* Sticky CTA bar (PDPs) */
.sticky-cta {
  position: fixed;
  bottom: 0; left: 0; right: 0;
  background: white;
  border-top: 2px solid #2D2D2D;
  padding: 0.75rem 1rem;
  z-index: 50;
  box-shadow: 0 -4px 6px rgba(0,0,0,0.08);
}

/* AOS pointer-events fix */
[data-aos] { pointer-events: none; }
[data-aos].aos-animate { pointer-events: auto; }
```

- [ ] **Step 2: Verify by opening `index.html` in a browser.** You will still see Bootstrap layout — that's expected at this stage. Check browser console shows no 404 for `styles.css`. The page should not crash.

- [ ] **Step 3: Commit**

```bash
git add styles.css
git commit -m "replace styles.css with Tailwind-ready custom CSS — sticker design system"
```

---

## Task 2: AI Image Generation (Human Step)

**Files:**
- Create: `img/hero-home.png` through `img/hero-mug.png` (15 sticker illustrations)
- Create: `img/mockup-id.jpg` through `img/mockup-mug.jpg` (14 realistic mockups for PDP hover)

Use **DALL-E 3**, **Midjourney**, or **Adobe Firefly** with the prompts below. Save at minimum 800×800px PNG (illustrations) and 800×600px JPG (mockups).

**Base sticker character prompt (append each variation below):**
> Sticker-style cartoon illustration, Filipino character from Davao Philippines, brown skin, casual everyday clothing, bold thick black outline strokes, rounded puffy shapes, minimal face with dot eyes and small curved smile, bright warm colors (coral red, sunny yellow, mint green), pure white background, clean flat vector art, LINE sticker aesthetic, high quality, no text in image

| File | Variation to append |
|---|---|
| `img/hero-home.png` | cheerful Filipino family — mother father and child — holding a custom t-shirt, a printed mug, and a tarpaulin banner |
| `img/hero-id.png` | young Filipino student in school uniform sitting in a studio chair smiling while being photographed for an ID |
| `img/hero-photo.png` | Filipino woman holding a framed family photo looking at it with nostalgic joy |
| `img/hero-docs.png` | Filipino student holding a neat stack of freshly printed documents looking relieved |
| `img/hero-xerox.png` | Filipino office worker using a photocopier machine looking satisfied |
| `img/hero-lam.png` | Filipino professional holding up a freshly laminated ID card looking proud |
| `img/hero-tarp.png` | two Filipinos hanging a large colorful celebration tarpaulin on a wall smiling |
| `img/hero-hats.png` | cool Filipino teen wearing a custom bucket hat sideways striking a fun pose |
| `img/hero-chain.png` | Filipino child holding up a custom keychain with wide eyes of delight |
| `img/hero-card.png` | Filipino employee proudly showing a brand new PVC ID card with a lanyard |
| `img/hero-shirt.png` | group of three Filipino friends wearing matching custom printed t-shirts giving thumbs up |
| `img/hero-umbrella.png` | Filipino woman under a custom printed umbrella in light rain smiling and dry |
| `img/hero-ref.png` | Filipino family gathered around a refrigerator covered in colorful custom ref magnets |
| `img/hero-ecobag.png` | Filipino woman at a Davao market carrying groceries in a colorful custom eco-bag |
| `img/hero-mug.png` | Filipino man in pajamas happily drinking morning coffee from a custom printed mug |

**Realistic product mockup prompt base:**
> Realistic product photography style, Filipino/Davao Philippines setting, warm natural lighting, clean background, high quality, no people in frame

| File | Variation |
|---|---|
| `img/mockup-id.jpg` | flat-lay of multiple passport 1x1 and 2x2 ID photos on cream table |
| `img/mockup-photo.jpg` | printed 4x6 and 5x7 photos arranged on a wooden table showing Filipino family moments |
| `img/mockup-docs.jpg` | stack of freshly printed documents on a clean office desk |
| `img/mockup-xerox.jpg` | photocopied documents fanned out on a table |
| `img/mockup-lam.jpg` | laminated ID cards and certificates on a wooden surface |
| `img/mockup-tarp.jpg` | colorful celebration tarpaulin hung on a wall at a Filipino party venue |
| `img/mockup-hats.jpg` | three bucket hats in different colors with custom prints on a flat surface |
| `img/mockup-chain.jpg` | custom photo keychains spread out on a light background |
| `img/mockup-card.jpg` | PVC ID cards with lanyards arranged on a table |
| `img/mockup-shirt.jpg` | custom printed t-shirt flat-lay on a white surface |
| `img/mockup-umbrella.jpg` | custom printed umbrella open on a sunny outdoor setting in Davao |
| `img/mockup-ref.jpg` | colorful custom ref magnets on a refrigerator door |
| `img/mockup-ecobag.jpg` | custom printed eco-bag standing upright showing the print design |
| `img/mockup-mug.jpg` | custom printed mug on a wooden table with morning coffee steam |

- [ ] **Step: Generate all 29 images using prompts above, save to `img/` folder**

- [ ] **Step: Commit all new images**

```bash
git add img/hero-*.png img/mockup-*.jpg
git commit -m "add AI-generated sticker hero illustrations and product mockup images"
```

---

## Task 3: Homepage — Full `<head>` + Nav + Hero

**Files:**
- Modify: `index.html` (replace entire file)

- [ ] **Step 1: Replace `index.html` with the following complete file. Build it section by section — start with `<head>` through the end of the hero section:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Achie's Printing – Doña Pilar Village, Davao | ID Photos, Shirts, Tarpaulin & More</title>
  <meta name="description" content="Achie's Printing in Doña Pilar Village, Davao — your community's trusted print shop for ID photos, shirt printing, tarpaulin, mugs, keychains, eco-bags, and more. Message us on Facebook!">
  <link rel="canonical" href="https://adrianvyne.github.io/Achie-Printing/">
  <!-- Open Graph -->
  <meta property="og:type" content="website">
  <meta property="og:url" content="https://adrianvyne.github.io/Achie-Printing/">
  <meta property="og:title" content="Achie's Printing – Doña Pilar Village, Davao">
  <meta property="og:description" content="The printing shop Doña Pilar grew up with. ID photos, shirts, tarpaulin, mugs & more. Fast, friendly, priced right.">
  <meta property="og:image" content="https://adrianvyne.github.io/Achie-Printing/img/carosel1.jpg">
  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="Achie's Printing – Doña Pilar Village, Davao">
  <meta name="twitter:description" content="The printing shop Doña Pilar grew up with.">
  <link rel="icon" type="image/png" href="img/favicon.png">
  <!-- SHARED-HEAD -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;700;900&display=swap" rel="stylesheet">
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: { coral: '#FF6B6B', sunny: '#FFD93D', mint: '#6BCB77', charcoal: '#2D2D2D', cream: '#FFFDF7' },
          fontFamily: { nunito: ['Nunito', 'sans-serif'] },
        }
      }
    }
  </script>
  <script defer src="https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js"></script>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aos@2.3.4/dist/aos.css"/>
  <script src="https://cdn.jsdelivr.net/npm/aos@2.3.4/dist/aos.js"></script>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.css"/>
  <script src="https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.js"></script>
  <script src="https://unpkg.com/@phosphor-icons/web"></script>
  <link href="styles.css" rel="stylesheet">
  <!-- Schema.org LocalBusiness -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "LocalBusiness",
    "name": "Achie's Printing",
    "description": "Family-run printing shop in Doña Pilar Village, Davao. ID photos, shirt printing, tarpaulin, mugs, keychains, eco-bags, and more.",
    "url": "https://adrianvyne.github.io/Achie-Printing/",
    "telephone": "+639012345678",
    "email": "ansheened@gmail.com",
    "address": {
      "@type": "PostalAddress",
      "streetAddress": "72 Doña Pilar Village",
      "addressLocality": "Davao City",
      "addressRegion": "Davao del Sur",
      "addressCountry": "PH"
    },
    "geo": {
      "@type": "GeoCoordinates",
      "latitude": 7.121849299677013,
      "longitude": 125.64776928811081
    },
    "openingHoursSpecification": [{
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": ["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"],
      "opens": "08:00",
      "closes": "19:00"
    }],
    "sameAs": ["https://www.facebook.com/profile.php?id=100052801946765"],
    "priceRange": "₱"
  }
  </script>
</head>
<body class="bg-cream font-nunito text-charcoal">

<!-- NAV (paste NAV block from Shared Reference) -->
<nav class="sticky top-0 z-50 bg-cream border-b-2 border-charcoal font-nunito" x-data="{ open: false }">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="flex items-center justify-between h-16">
      <a href="index.html" class="flex items-center gap-2">
        <img src="img/favicon.png" alt="Achie's Printing" class="h-9 w-9 rounded-full border-2 border-charcoal object-cover">
        <span class="font-black text-xl text-charcoal leading-tight">Achie's<br><span class="text-coral">Printing</span></span>
      </a>
      <div class="hidden md:flex items-center gap-6">
        <a href="#products" class="font-bold text-charcoal hover:text-coral transition-colors">Products</a>
        <a href="#contact" class="font-bold text-charcoal hover:text-coral transition-colors">Contact</a>
        <a href="#faq" class="font-bold text-charcoal hover:text-coral transition-colors">FAQ</a>
        <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
           class="sticker-btn bg-coral text-white px-5 py-2 text-sm">Message Us</a>
      </div>
      <button @click="open = !open" class="md:hidden p-1" aria-label="Toggle menu">
        <i class="ph ph-list text-2xl text-charcoal" x-show="!open" x-cloak></i>
        <i class="ph ph-x text-2xl text-charcoal" x-show="open" x-cloak></i>
      </button>
    </div>
    <div x-show="open" x-transition class="md:hidden pb-4 flex flex-col gap-3 border-t border-charcoal/20 pt-3">
      <a href="#products" @click="open=false" class="font-bold text-charcoal py-1">Products</a>
      <a href="#contact" @click="open=false" class="font-bold text-charcoal py-1">Contact</a>
      <a href="#faq" @click="open=false" class="font-bold text-charcoal py-1">FAQ</a>
      <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
         class="sticker-btn bg-coral text-white px-5 py-3 text-center text-sm">Message Us</a>
    </div>
  </div>
</nav>

<main>

<!-- HERO -->
<section class="bg-cream min-h-[88vh] flex items-center relative overflow-hidden">
  <div class="absolute top-0 right-0 w-[500px] h-[500px] bg-sunny/30 rounded-full translate-x-1/3 -translate-y-1/3 pointer-events-none"></div>
  <div class="absolute bottom-0 left-0 w-[400px] h-[400px] bg-coral/15 rounded-full -translate-x-1/3 translate-y-1/3 pointer-events-none"></div>
  <div class="absolute top-1/2 left-1/2 w-[300px] h-[300px] bg-mint/20 rounded-full -translate-x-1/2 -translate-y-1/2 pointer-events-none"></div>
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-24 flex flex-col md:flex-row items-center gap-12 relative z-10">
    <div class="flex-1 text-center md:text-left" data-aos="fade-right">
      <span class="inline-flex items-center gap-1 bg-sunny border-2 border-charcoal text-charcoal font-bold text-sm px-4 py-1 rounded-full mb-5">
        <i class="ph ph-map-pin"></i> Doña Pilar Village, Davao
      </span>
      <h1 class="font-nunito font-black text-5xl sm:text-6xl lg:text-7xl text-charcoal leading-tight mb-6">
        The Printing Shop<br>
        <span class="text-coral">Doña Pilar</span><br>
        Grew Up With.
      </h1>
      <p class="font-nunito text-lg text-charcoal/70 mb-8 max-w-md mx-auto md:mx-0">
        T-shirts, tarpaulins, ID photos, mugs, and more — fast, friendly, and priced for the community.
      </p>
      <div class="flex flex-col sm:flex-row gap-4 justify-center md:justify-start">
        <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
           class="sticker-btn bg-coral text-white px-8 py-4 text-lg">Order via Messenger →</a>
        <a href="#products" class="sticker-btn bg-white text-charcoal px-8 py-4 text-lg">See Products</a>
      </div>
    </div>
    <div class="flex-1 flex justify-center" data-aos="fade-left">
      <img src="img/hero-home.png"
           alt="Sticker-style cartoon Filipino family from Doña Pilar holding custom printed products"
           class="w-full max-w-md drop-shadow-2xl">
    </div>
  </div>
</section>

<!-- Wavy Divider -->
<div class="-mt-1 overflow-hidden">
  <svg viewBox="0 0 1440 80" xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="none" class="w-full h-16">
    <path d="M0,40 C180,80 360,0 540,40 C720,80 900,0 1080,40 C1260,80 1350,20 1440,40 L1440,80 L0,80 Z" fill="#ffffff"/>
  </svg>
</div>
```

- [ ] **Step 2: Verify** — open `index.html` in browser. Hero should show large headline, blob shapes, two buttons. Nav should be sticky. Mobile hamburger should open/close on narrow viewport.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "homepage: add Tailwind head, nav, and hero section"
```

---

## Task 4: Homepage — Products Grid

**Files:**
- Modify: `index.html` (append products section after the wavy divider)

- [ ] **Step 1: Append this after the wavy divider in `index.html`:**

```html
<!-- PRODUCTS GRID -->
<section class="bg-white py-16" id="products">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <h2 class="font-nunito font-black text-4xl md:text-5xl text-charcoal text-center mb-3" data-aos="fade-up">
      Printing Products &amp; Services
    </h2>
    <p class="font-nunito text-charcoal/60 text-center mb-12" data-aos="fade-up" data-aos-delay="100">
      Hover a card to preview · click to learn more
    </p>
    <div class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-4 gap-4 md:gap-6">

      <a href="id.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="0">
        <img src="img/id-front.jpg" alt="ID photo printing Doña Pilar Davao – Achie's Printing" class="img-default">
        <img src="img/mockup-id.jpg" alt="Sample ID photos printed at Achie's Printing Davao" class="img-hover">
        <span class="card-label">ID Photo</span><span class="card-order">Order This →</span>
      </a>

      <a href="photo.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="50">
        <img src="img/photo-front.jpg" alt="Photo printing service Davao – Achie's Printing" class="img-default">
        <img src="img/mockup-photo.jpg" alt="Printed photos sample from Achie's Printing" class="img-hover">
        <span class="card-label">Photo Printing</span><span class="card-order">Order This →</span>
      </a>

      <a href="docs.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="100">
        <img src="img/doc-front.jpg" alt="Document printing Davao – Achie's Printing" class="img-default">
        <img src="img/mockup-docs.jpg" alt="Printed documents sample" class="img-hover">
        <span class="card-label">Document Print</span><span class="card-order">Order This →</span>
      </a>

      <a href="xerox.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="150">
        <img src="img/photocopier.png" alt="Photocopy service Davao – Achie's Printing" class="img-default">
        <img src="img/mockup-xerox.jpg" alt="Photocopied documents sample" class="img-hover">
        <span class="card-label">Photocopy</span><span class="card-order">Order This →</span>
      </a>

      <a href="laminate.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="0">
        <img src="img/lam-front.jpg" alt="Lamination service Davao – Achie's Printing" class="img-default">
        <img src="img/mockup-lam.jpg" alt="Laminated documents sample" class="img-hover">
        <span class="card-label">Lamination</span><span class="card-order">Order This →</span>
      </a>

      <a href="tarp.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="50">
        <img src="img/tarp-front.jpg" alt="Tarpaulin printing Davao – Achie's Printing" class="img-default">
        <img src="img/mockup-tarp.jpg" alt="Tarpaulin print sample" class="img-hover">
        <span class="card-label">Tarpaulin</span><span class="card-order">Order This →</span>
      </a>

      <a href="hats.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="100">
        <img src="img/hats-front.jpg" alt="Custom bucket hat printing Davao – Achie's Printing" class="img-default">
        <img src="img/mockup-hats.jpg" alt="Custom bucket hats sample" class="img-hover">
        <span class="card-label">Bucket Hat</span><span class="card-order">Order This →</span>
      </a>

      <a href="chain.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="150">
        <img src="img/key-front.jpg" alt="Custom keychains Davao – Achie's Printing" class="img-default">
        <img src="img/mockup-chain.jpg" alt="Custom keychains sample" class="img-hover">
        <span class="card-label">Key Chain</span><span class="card-order">Order This →</span>
      </a>

      <a href="PVC-ID.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="0">
        <img src="img/card-front.jpg" alt="PVC ID card printing Davao – Achie's Printing" class="img-default">
        <img src="img/mockup-card.jpg" alt="PVC ID card sample" class="img-hover">
        <span class="card-label">ID Card</span><span class="card-order">Order This →</span>
      </a>

      <a href="shirtprint.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="50">
        <img src="img/shirt-front.jpg" alt="Shirt printing Davao – Achie's Printing" class="img-default">
        <img src="img/mockup-shirt.jpg" alt="Custom shirt print sample" class="img-hover">
        <span class="card-label">Shirt Printing</span><span class="card-order">Order This →</span>
      </a>

      <a href="umbrella.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="100">
        <img src="img/brela-front.jpg" alt="Custom umbrella printing Davao – Achie's Printing" class="img-default">
        <img src="img/mockup-umbrella.jpg" alt="Custom umbrella sample" class="img-hover">
        <span class="card-label">Umbrella</span><span class="card-order">Order This →</span>
      </a>

      <a href="refmagnet.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="150">
        <img src="img/ref-front.jpg" alt="Custom ref magnets Davao – Achie's Printing" class="img-default">
        <img src="img/mockup-ref.jpg" alt="Custom ref magnets sample" class="img-hover">
        <span class="card-label">Ref Magnet</span><span class="card-order">Order This →</span>
      </a>

      <a href="eco-bag.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="0">
        <img src="img/eco-front.jpg" alt="Custom eco-bags Davao – Achie's Printing" class="img-default">
        <img src="img/mockup-ecobag.jpg" alt="Custom eco-bag sample" class="img-hover">
        <span class="card-label">Eco-Bag</span><span class="card-order">Order This →</span>
      </a>

      <a href="mugs.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="50">
        <img src="img/mug-front.jpg" alt="Custom mug printing Davao – Achie's Printing" class="img-default">
        <img src="img/mockup-mug.jpg" alt="Custom mug sample" class="img-hover">
        <span class="card-label">Custom Mug</span><span class="card-order">Order This →</span>
      </a>

    </div>
  </div>
</section>
```

- [ ] **Step 2: Verify** — open `index.html`. Product grid shows 14 cards in a 2-col (mobile) / 3-col (tablet) / 4-col (desktop) layout. Hover each card to verify image swap and "Order This →" CTA slides up. If mockup images don't exist yet (Task 2 incomplete), the hover image will be broken — that is expected.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "homepage: add unified products grid with sticker cards and hover CTAs"
```

---

## Task 5: Homepage — Moments Carousel + Why Achie's

**Files:**
- Modify: `index.html` (append after products section)

- [ ] **Step 1: Append after the products section:**

```html
<!-- Wavy Divider -->
<div class="overflow-hidden bg-white">
  <svg viewBox="0 0 1440 80" xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="none" class="w-full h-16">
    <path d="M0,20 C360,80 1080,0 1440,40 L1440,80 L0,80 Z" fill="#FFFDF7"/>
  </svg>
</div>

<!-- MOMENTS CAROUSEL -->
<section class="bg-cream py-16">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <h2 class="font-nunito font-black text-3xl md:text-4xl text-charcoal text-center mb-2" data-aos="fade-up">
      Unique Items For Every Special Moment
    </h2>
    <p class="font-nunito text-charcoal/60 text-center mb-10" data-aos="fade-up" data-aos-delay="100">
      We print for life's milestones, big and small.
    </p>
    <div class="swiper moments-swiper" data-aos="fade-up" data-aos-delay="200">
      <div class="swiper-wrapper pb-10">
        <div class="swiper-slide">
          <div class="sticker-card overflow-hidden">
            <img src="img/savethedate.jpg" alt="Custom Save the Date wedding invitations – Achie's Printing Davao" class="w-full h-56 object-cover">
            <div class="p-4 text-center"><h3 class="font-nunito font-black text-lg text-charcoal">Save the Date</h3></div>
          </div>
        </div>
        <div class="swiper-slide">
          <div class="sticker-card overflow-hidden">
            <img src="img/babyshower.jpg" alt="Baby shower printed giveaways – Achie's Printing Davao" class="w-full h-56 object-cover">
            <div class="p-4 text-center"><h3 class="font-nunito font-black text-lg text-charcoal">Baby Shower</h3></div>
          </div>
        </div>
        <div class="swiper-slide">
          <div class="sticker-card overflow-hidden">
            <img src="img/christening.jpg" alt="Christening printed giveaways – Achie's Printing Davao" class="w-full h-56 object-cover">
            <div class="p-4 text-center"><h3 class="font-nunito font-black text-lg text-charcoal">Christening</h3></div>
          </div>
        </div>
        <div class="swiper-slide">
          <div class="sticker-card overflow-hidden">
            <img src="img/birthday.jpg" alt="Birthday printed giveaways – Achie's Printing Davao" class="w-full h-56 object-cover">
            <div class="p-4 text-center"><h3 class="font-nunito font-black text-lg text-charcoal">Birthday</h3></div>
          </div>
        </div>
        <div class="swiper-slide">
          <div class="sticker-card overflow-hidden">
            <img src="img/reference.jpg" alt="Wedding printed giveaways – Achie's Printing Davao" class="w-full h-56 object-cover">
            <div class="p-4 text-center"><h3 class="font-nunito font-black text-lg text-charcoal">Wedding</h3></div>
          </div>
        </div>
      </div>
      <div class="swiper-pagination"></div>
      <div class="swiper-button-prev"></div>
      <div class="swiper-button-next"></div>
    </div>
  </div>
</section>

<!-- WHY ACHIE'S -->
<section class="py-16 bg-white">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <h2 class="font-nunito font-black text-3xl md:text-4xl text-charcoal text-center mb-12" data-aos="fade-up">
      The Achie's Difference
    </h2>
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
      <div class="sticker-card p-6 text-center" data-aos="fade-up" data-aos-delay="0">
        <div class="w-16 h-16 bg-coral/15 rounded-full flex items-center justify-center mx-auto mb-4">
          <i class="ph ph-seal-check text-3xl text-coral"></i>
        </div>
        <h3 class="font-nunito font-black text-xl text-charcoal mb-3">Satisfaction Guarantee</h3>
        <p class="font-nunito text-charcoal/60 text-sm leading-relaxed">Customer satisfaction is our ultimate goal. We're not happy until you are.</p>
      </div>
      <div class="sticker-card p-6 text-center" data-aos="fade-up" data-aos-delay="100">
        <div class="w-16 h-16 bg-sunny/40 rounded-full flex items-center justify-center mx-auto mb-4">
          <i class="ph ph-paint-brush text-3xl text-charcoal"></i>
        </div>
        <h3 class="font-nunito font-black text-xl text-charcoal mb-3">Unique Designs</h3>
        <p class="font-nunito text-charcoal/60 text-sm leading-relaxed">Creative designs by independent artists — or bring your own file. We work with what you have.</p>
      </div>
      <div class="sticker-card p-6 text-center" data-aos="fade-up" data-aos-delay="200">
        <div class="w-16 h-16 bg-mint/30 rounded-full flex items-center justify-center mx-auto mb-4">
          <i class="ph ph-tag text-3xl text-mint"></i>
        </div>
        <h3 class="font-nunito font-black text-xl text-charcoal mb-3">Honest Pricing</h3>
        <p class="font-nunito text-charcoal/60 text-sm leading-relaxed">Transparent and fair. No hidden fees, no surprises. Just the best value in Doña Pilar.</p>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Verify** — Swiper carousel scrolls through the 5 moments cards. "Why Achie's" shows 3 sticker cards with icons side by side on desktop, stacked on mobile.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "homepage: add moments carousel (Swiper) and Why Achie's section"
```

---

## Task 6: Homepage — Contact + FAQ + Footer + Scripts

**Files:**
- Modify: `index.html` (complete the page — append contact, FAQ, footer, close tags, all scripts)

- [ ] **Step 1: Append the rest of `index.html` after the Why Achie's section:**

```html
<!-- CONTACT -->
<section class="py-16 bg-cream" id="contact">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <h2 class="font-nunito font-black text-3xl md:text-4xl text-charcoal text-center mb-12" data-aos="fade-up">Find Us</h2>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-8 items-start">
      <div class="sticker-card overflow-hidden" data-aos="fade-right">
        <iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d989.7589499563268!2d125.64776928811081!3d7.121849299677013!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x32f96d94dfc9c805%3A0x34e4d7ac2fac8395!2sAchie&#39;s%20Printing!5e0!3m2!1sen!2sph!4v1673847724656!5m2!1sen!2sph"
          width="100%" height="320" loading="lazy" referrerpolicy="no-referrer-when-downgrade"
          title="Achie's Printing on Google Maps" class="block w-full"></iframe>
      </div>
      <div class="space-y-4" data-aos="fade-left">
        <div class="sticker-card p-5 flex items-start gap-3">
          <i class="ph ph-map-pin text-2xl text-coral mt-0.5 shrink-0"></i>
          <div>
            <h3 class="font-nunito font-black text-charcoal">Address</h3>
            <p class="font-nunito text-charcoal/70 text-sm">72 Doña Pilar Village, Davao City, Philippines</p>
          </div>
        </div>
        <div class="sticker-card p-5 flex items-start gap-3">
          <i class="ph ph-phone text-2xl text-coral mt-0.5 shrink-0"></i>
          <div>
            <h3 class="font-nunito font-black text-charcoal">Phone</h3>
            <a href="tel:+639012345678" class="font-nunito text-charcoal/70 text-sm hover:text-coral transition-colors">+63 901 234 5678</a>
          </div>
        </div>
        <div class="sticker-card p-5 flex items-start gap-3">
          <i class="ph ph-clock text-2xl text-coral mt-0.5 shrink-0"></i>
          <div>
            <h3 class="font-nunito font-black text-charcoal">Store Hours</h3>
            <p class="font-nunito text-charcoal/70 text-sm">Monday – Sunday, 8:00 AM – 7:00 PM</p>
            <p class="font-nunito text-charcoal/50 text-xs">Philippine Time (UTC+08:00)</p>
          </div>
        </div>
        <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
           class="sticker-btn bg-coral text-white px-6 py-3 block text-center">Chat with us on Messenger →</a>
      </div>
    </div>
  </div>
</section>

<!-- FAQ -->
<section class="py-16 bg-white" id="faq">
  <div class="max-w-3xl mx-auto px-4 sm:px-6 lg:px-8">
    <h2 class="font-nunito font-black text-3xl md:text-4xl text-charcoal text-center mb-12" data-aos="fade-up">Popular Questions</h2>
    <div class="space-y-3" x-data="{ active: null }">

      <div class="sticker-card overflow-hidden" data-aos="fade-up">
        <button @click="active = active === 1 ? null : 1"
                class="w-full flex items-center justify-between p-5 text-left font-nunito font-bold text-charcoal hover:text-coral transition-colors">
          <span>What types of printing services do you offer?</span>
          <i class="ph ph-caret-down text-xl transition-transform duration-300" :class="{ 'rotate-180': active === 1 }"></i>
        </button>
        <div x-show="active === 1" x-transition class="px-5 pb-5">
          <p class="font-nunito text-charcoal/70 text-sm leading-relaxed">We offer a wide range of printing services, including digital printing, screen printing, and large format printing. We can print on paper, cardboard, fabric, vinyl, and more.</p>
        </div>
      </div>

      <div class="sticker-card overflow-hidden" data-aos="fade-up" data-aos-delay="50">
        <button @click="active = active === 2 ? null : 2"
                class="w-full flex items-center justify-between p-5 text-left font-nunito font-bold text-charcoal hover:text-coral transition-colors">
          <span>What file formats do you accept?</span>
          <i class="ph ph-caret-down text-xl transition-transform duration-300" :class="{ 'rotate-180': active === 2 }"></i>
        </button>
        <div x-show="active === 2" x-transition class="px-5 pb-5">
          <p class="font-nunito text-charcoal/70 text-sm leading-relaxed">We accept PDF, JPEG, PNG, and TIFF. For best results, files should be high resolution (300 DPI) in CMYK color mode.</p>
        </div>
      </div>

      <div class="sticker-card overflow-hidden" data-aos="fade-up" data-aos-delay="100">
        <button @click="active = active === 3 ? null : 3"
                class="w-full flex items-center justify-between p-5 text-left font-nunito font-bold text-charcoal hover:text-coral transition-colors">
          <span>What is your turnaround time?</span>
          <i class="ph ph-caret-down text-xl transition-transform duration-300" :class="{ 'rotate-180': active === 3 }"></i>
        </button>
        <div x-show="active === 3" x-transition class="px-5 pb-5">
          <p class="font-nunito text-charcoal/70 text-sm leading-relaxed">Turnaround depends on the job size and complexity. We'll give you an estimated time when you place your order. Many simple jobs are done same day.</p>
        </div>
      </div>

      <div class="sticker-card overflow-hidden" data-aos="fade-up" data-aos-delay="150">
        <button @click="active = active === 4 ? null : 4"
                class="w-full flex items-center justify-between p-5 text-left font-nunito font-bold text-charcoal hover:text-coral transition-colors">
          <span>Do you offer design services?</span>
          <i class="ph ph-caret-down text-xl transition-transform duration-300" :class="{ 'rotate-180': active === 4 }"></i>
        </button>
        <div x-show="active === 4" x-transition class="px-5 pb-5">
          <p class="font-nunito text-charcoal/70 text-sm leading-relaxed">Yes! We can help design your print or work with a file you provide. Message us on Messenger to discuss your project.</p>
        </div>
      </div>

      <div class="sticker-card overflow-hidden" data-aos="fade-up" data-aos-delay="200">
        <button @click="active = active === 5 ? null : 5"
                class="w-full flex items-center justify-between p-5 text-left font-nunito font-bold text-charcoal hover:text-coral transition-colors">
          <span>What are your payment options?</span>
          <i class="ph ph-caret-down text-xl transition-transform duration-300" :class="{ 'rotate-180': active === 5 }"></i>
        </button>
        <div x-show="active === 5" x-transition class="px-5 pb-5">
          <p class="font-nunito text-charcoal/70 text-sm leading-relaxed">We accept cash, GCash, and bank transfers. Ask us for details when you place your order.</p>
        </div>
      </div>

      <div class="sticker-card overflow-hidden" data-aos="fade-up" data-aos-delay="250">
        <button @click="active = active === 6 ? null : 6"
                class="w-full flex items-center justify-between p-5 text-left font-nunito font-bold text-charcoal hover:text-coral transition-colors">
          <span>Do you offer wholesale pricing?</span>
          <i class="ph ph-caret-down text-xl transition-transform duration-300" :class="{ 'rotate-180': active === 6 }"></i>
        </button>
        <div x-show="active === 6" x-transition class="px-5 pb-5">
          <p class="font-nunito text-charcoal/70 text-sm leading-relaxed">Yes, we offer discounts for large quantity orders. Message us on Messenger for a custom quote.</p>
        </div>
      </div>

    </div>
  </div>
</section>

</main>

<!-- FLOATING CTA -->
<div class="messenger-float">
  <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
     aria-label="Message Achie's Printing on Facebook Messenger"
     class="sticker-btn bg-coral text-white flex items-center gap-2 px-4 py-3 text-sm">
    <i class="ph-fill ph-messenger-logo text-xl"></i>
    <span class="hidden sm:inline font-bold">Message Us</span>
  </a>
</div>

<!-- FOOTER -->
<footer class="bg-charcoal text-cream py-12 font-nunito">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="grid grid-cols-1 md:grid-cols-3 gap-8 pb-8 border-b border-white/20">
      <div>
        <h3 class="font-black text-xl text-white mb-2">Achie's Printing</h3>
        <p class="text-white/70 text-sm leading-relaxed">The printing shop Doña Pilar grew up with. Quick, honest, and priced right.</p>
      </div>
      <div>
        <h4 class="font-bold text-white mb-3">Store Hours</h4>
        <p class="text-white/70 text-sm">Monday – Sunday</p>
        <p class="text-white/70 text-sm">8:00 AM – 7:00 PM</p>
        <p class="text-white/50 text-xs mt-1">Philippine Time (UTC+08:00)</p>
      </div>
      <div>
        <h4 class="font-bold text-white mb-3">Connect</h4>
        <div class="flex gap-3 mb-4">
          <a href="https://www.facebook.com/profile.php?id=100052801946765" target="_blank" rel="noopener" aria-label="Facebook"
             class="bg-white/10 hover:bg-coral rounded-full p-2 transition-colors">
            <i class="ph ph-facebook-logo text-xl text-white"></i></a>
          <a href="https://www.instagram.com/" target="_blank" rel="noopener" aria-label="Instagram"
             class="bg-white/10 hover:bg-coral rounded-full p-2 transition-colors">
            <i class="ph ph-instagram-logo text-xl text-white"></i></a>
          <a href="mailto:ansheened@gmail.com" aria-label="Email"
             class="bg-white/10 hover:bg-coral rounded-full p-2 transition-colors">
            <i class="ph ph-envelope text-xl text-white"></i></a>
          <a href="https://m.me/110237597413659" target="_blank" rel="noopener" aria-label="Messenger"
             class="bg-white/10 hover:bg-coral rounded-full p-2 transition-colors">
            <i class="ph ph-messenger-logo text-xl text-white"></i></a>
        </div>
        <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
           class="sticker-btn bg-coral text-white px-5 py-2 text-sm block text-center">Send us a message</a>
      </div>
    </div>
    <p class="text-center text-white/40 text-xs mt-6">© 2025 Achie's Printing – Doña Pilar Village, Davao City, Philippines</p>
  </div>
</footer>

<!-- FB Chat Plugin -->
<div id="fb-root"></div>
<div id="fb-customer-chat" class="fb-customerchat"></div>
<script>
  var chatbox = document.getElementById("fb-customer-chat");
  chatbox.setAttribute("page_id", "110237597413659");
  chatbox.setAttribute("attribution", "biz_inbox");
</script>
<script>
  window.fbAsyncInit = function () { FB.init({ xfbml: true, version: "v15.0" }); };
  (function (d, s, id) {
    var js, fjs = d.getElementsByTagName(s)[0];
    if (d.getElementById(id)) return;
    js = d.createElement(s); js.id = id;
    js.src = "https://connect.facebook.net/en_US/sdk/xfbml.customerchat.js";
    fjs.parentNode.insertBefore(js, fjs);
  })(document, "script", "facebook-jssdk");
</script>

<!-- Swiper init -->
<script>
  new Swiper('.moments-swiper', {
    slidesPerView: 1.2, spaceBetween: 16, loop: true,
    pagination: { el: '.swiper-pagination', clickable: true },
    navigation: { nextEl: '.swiper-button-next', prevEl: '.swiper-button-prev' },
    breakpoints: {
      640: { slidesPerView: 2, spaceBetween: 20 },
      1024: { slidesPerView: 3, spaceBetween: 24 },
    }
  });
</script>

<!-- AOS init -->
<script>AOS.init({ duration: 600, once: true, easing: 'ease-out-quad' });</script>
</body>
</html>
```

- [ ] **Step 2: Verify full homepage** — scroll through all sections: nav sticky, hero, products grid, moments carousel (auto-slides), Why Achie's, contact map, FAQ accordion opens/closes, footer, floating Messenger pill fixed bottom-right. Test mobile viewport: hamburger menu opens, all sections stack correctly.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "homepage: complete — contact, FAQ, footer, Swiper, AOS, Messenger CTA"
```

---

## Task 7: PDP Template — `id.html` (ID Photo)

This task establishes the full PDP template. Tasks 8–20 follow the same structure — only the page-specific values change (noted per task).

**Files:**
- Modify: `id.html` (full rewrite)

- [ ] **Step 1: Replace entire `id.html` with:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ID Photo Printing Davao | Achie's Printing – Doña Pilar</title>
  <meta name="description" content="Professional ID photo printing in Doña Pilar Village, Davao. Passport size, 1x1, 2x2 — same-day service available. Message us on Facebook Messenger to order!">
  <link rel="canonical" href="https://adrianvyne.github.io/Achie-Printing/id.html">
  <meta property="og:type" content="website">
  <meta property="og:url" content="https://adrianvyne.github.io/Achie-Printing/id.html">
  <meta property="og:title" content="ID Photo Printing – Achie's Printing Davao">
  <meta property="og:description" content="Professional ID photo printing in Doña Pilar Village, Davao. Passport, 1x1, 2x2 — fast turnaround.">
  <meta property="og:image" content="https://adrianvyne.github.io/Achie-Printing/img/id-front.jpg">
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="ID Photo Printing – Achie's Printing Davao">
  <meta name="twitter:description" content="Professional ID photos in Doña Pilar, Davao. Fast, affordable.">
  <link rel="icon" type="image/png" href="img/favicon.png">
  <!-- SHARED-HEAD -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;700;900&display=swap" rel="stylesheet">
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: { coral: '#FF6B6B', sunny: '#FFD93D', mint: '#6BCB77', charcoal: '#2D2D2D', cream: '#FFFDF7' },
          fontFamily: { nunito: ['Nunito', 'sans-serif'] },
        }
      }
    }
  </script>
  <script defer src="https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js"></script>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aos@2.3.4/dist/aos.css"/>
  <script src="https://cdn.jsdelivr.net/npm/aos@2.3.4/dist/aos.js"></script>
  <!-- GLightbox -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/glightbox/dist/css/glightbox.min.css"/>
  <script src="https://cdn.jsdelivr.net/npm/glightbox/dist/js/glightbox.min.js"></script>
  <script src="https://unpkg.com/@phosphor-icons/web"></script>
  <link href="styles.css" rel="stylesheet">
</head>
<body class="bg-cream font-nunito text-charcoal">

<!-- NAV -->
<nav class="sticky top-0 z-50 bg-cream border-b-2 border-charcoal font-nunito" x-data="{ open: false }">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="flex items-center justify-between h-16">
      <a href="index.html" class="flex items-center gap-2">
        <img src="img/favicon.png" alt="Achie's Printing" class="h-9 w-9 rounded-full border-2 border-charcoal object-cover">
        <span class="font-black text-xl text-charcoal leading-tight">Achie's<br><span class="text-coral">Printing</span></span>
      </a>
      <div class="hidden md:flex items-center gap-6">
        <a href="index.html#products" class="font-bold text-charcoal hover:text-coral transition-colors">Products</a>
        <a href="index.html#contact" class="font-bold text-charcoal hover:text-coral transition-colors">Contact</a>
        <a href="index.html#faq" class="font-bold text-charcoal hover:text-coral transition-colors">FAQ</a>
        <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
           class="sticker-btn bg-coral text-white px-5 py-2 text-sm">Message Us</a>
      </div>
      <button @click="open = !open" class="md:hidden p-1" aria-label="Toggle menu">
        <i class="ph ph-list text-2xl text-charcoal" x-show="!open" x-cloak></i>
        <i class="ph ph-x text-2xl text-charcoal" x-show="open" x-cloak></i>
      </button>
    </div>
    <div x-show="open" x-transition class="md:hidden pb-4 flex flex-col gap-3 border-t border-charcoal/20 pt-3">
      <a href="index.html#products" @click="open=false" class="font-bold text-charcoal py-1">Products</a>
      <a href="index.html#contact" @click="open=false" class="font-bold text-charcoal py-1">Contact</a>
      <a href="index.html#faq" @click="open=false" class="font-bold text-charcoal py-1">FAQ</a>
      <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
         class="sticker-btn bg-coral text-white px-5 py-3 text-center text-sm">Message Us</a>
    </div>
  </div>
</nav>

<main>

<!-- HERO -->
<section class="bg-cream py-16 relative overflow-hidden">
  <div class="absolute top-0 right-0 w-96 h-96 bg-sunny/25 rounded-full translate-x-1/3 -translate-y-1/3 pointer-events-none"></div>
  <div class="absolute bottom-0 left-0 w-72 h-72 bg-mint/20 rounded-full -translate-x-1/3 translate-y-1/3 pointer-events-none"></div>
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex flex-col md:flex-row items-center gap-12 relative z-10">
    <div class="flex-1 text-center md:text-left" data-aos="fade-right">
      <a href="index.html#products" class="inline-flex items-center gap-1 text-charcoal/60 text-sm font-bold mb-4 hover:text-coral transition-colors">
        <i class="ph ph-arrow-left"></i> All Products
      </a>
      <h1 class="font-nunito font-black text-4xl md:text-5xl lg:text-6xl text-charcoal leading-tight mb-4">
        ID Photo <span class="text-coral">Printing</span>
      </h1>
      <p class="font-nunito text-lg text-charcoal/70 mb-8 max-w-md mx-auto md:mx-0">
        Passport size, 1x1, and 2x2 ID photos — printed on high-quality glossy paper. Quick turnaround, available same day in Doña Pilar, Davao.
      </p>
      <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
         class="sticker-btn bg-coral text-white px-8 py-4 text-lg">Order via Messenger →</a>
    </div>
    <div class="flex-1 flex justify-center" data-aos="fade-left">
      <img src="img/hero-id.png"
           alt="Sticker-style cartoon Filipino student getting ID photo taken at Achie's Printing Davao"
           class="w-full max-w-sm drop-shadow-2xl">
    </div>
  </div>
</section>

<!-- GALLERY -->
<section class="py-16 bg-white">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <h2 class="font-nunito font-black text-3xl text-charcoal text-center mb-10" data-aos="fade-up">Sample Work</h2>
    <div class="grid grid-cols-2 md:grid-cols-3 gap-4">
      <a class="glightbox sticker-card overflow-hidden block" href="img/IDset1.jpg" data-gallery="id-gallery"
         data-description="ID photo sample – Achie's Printing Davao" data-aos="fade-up" data-aos-delay="0">
        <img src="img/IDset1.jpg" alt="ID photo sample 1 – Achie's Printing Doña Pilar Davao" class="w-full h-48 object-cover hover:scale-105 transition-transform duration-300">
      </a>
      <a class="glightbox sticker-card overflow-hidden block" href="img/IDset2.jpg" data-gallery="id-gallery"
         data-description="ID photo sample – Achie's Printing Davao" data-aos="fade-up" data-aos-delay="50">
        <img src="img/IDset2.jpg" alt="ID photo sample 2 – Achie's Printing Doña Pilar Davao" class="w-full h-48 object-cover hover:scale-105 transition-transform duration-300">
      </a>
      <a class="glightbox sticker-card overflow-hidden block" href="img/IDset3.jpg" data-gallery="id-gallery"
         data-description="ID photo sample – Achie's Printing Davao" data-aos="fade-up" data-aos-delay="100">
        <img src="img/IDset3.jpg" alt="ID photo sample 3 – Achie's Printing Doña Pilar Davao" class="w-full h-48 object-cover hover:scale-105 transition-transform duration-300">
      </a>
      <a class="glightbox sticker-card overflow-hidden block" href="img/IDset4.jpg" data-gallery="id-gallery"
         data-description="ID photo sample – Achie's Printing Davao" data-aos="fade-up" data-aos-delay="150">
        <img src="img/IDset4.jpg" alt="ID photo sample 4 – Achie's Printing Doña Pilar Davao" class="w-full h-48 object-cover hover:scale-105 transition-transform duration-300">
      </a>
      <a class="glightbox sticker-card overflow-hidden block" href="img/IDset5.jpg" data-gallery="id-gallery"
         data-description="ID photo sample – Achie's Printing Davao" data-aos="fade-up" data-aos-delay="200">
        <img src="img/IDset5.jpg" alt="ID photo sample 5 – Achie's Printing Doña Pilar Davao" class="w-full h-48 object-cover hover:scale-105 transition-transform duration-300">
      </a>
      <a class="glightbox sticker-card overflow-hidden block" href="img/mockup-id.jpg" data-gallery="id-gallery"
         data-description="Realistic ID photo mockup – Achie's Printing Davao" data-aos="fade-up" data-aos-delay="250">
        <img src="img/mockup-id.jpg" alt="Realistic ID photos flat-lay – Achie's Printing Davao" class="w-full h-48 object-cover hover:scale-105 transition-transform duration-300">
      </a>
    </div>
  </div>
</section>

<!-- DETAILS -->
<section class="py-16 bg-cream">
  <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
    <h2 class="font-nunito font-black text-3xl text-charcoal mb-6" data-aos="fade-up">About ID Photo Printing</h2>
    <article class="font-nunito text-charcoal/70 leading-relaxed space-y-4 mb-10" data-aos="fade-up" data-aos-delay="100">
      <p>We print passport-size, 1x1, and 2x2 photos for all types of IDs — school, government, employment, and more. Printed on high-quality glossy photo paper with vibrant, true-to-life colors.</p>
      <p>Bring your file digitally (on a USB or send via Messenger) or have your photo taken on-site. Fast turnaround available — many orders are done the same day.</p>
    </article>
    <div class="grid grid-cols-2 sm:grid-cols-4 gap-4">
      <div class="sticker-card p-4 text-center" data-aos="fade-up" data-aos-delay="0">
        <i class="ph ph-student text-3xl text-coral mb-2 block"></i>
        <span class="font-nunito font-bold text-sm text-charcoal">School IDs</span>
      </div>
      <div class="sticker-card p-4 text-center" data-aos="fade-up" data-aos-delay="50">
        <i class="ph ph-identification-badge text-3xl text-coral mb-2 block"></i>
        <span class="font-nunito font-bold text-sm text-charcoal">Gov't IDs</span>
      </div>
      <div class="sticker-card p-4 text-center" data-aos="fade-up" data-aos-delay="100">
        <i class="ph ph-briefcase text-3xl text-coral mb-2 block"></i>
        <span class="font-nunito font-bold text-sm text-charcoal">Employment</span>
      </div>
      <div class="sticker-card p-4 text-center" data-aos="fade-up" data-aos-delay="150">
        <i class="ph ph-airplane text-3xl text-coral mb-2 block"></i>
        <span class="font-nunito font-bold text-sm text-charcoal">Passport</span>
      </div>
    </div>
  </div>
</section>

<!-- RELATED PRODUCTS -->
<section class="py-16 bg-white">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <h2 class="font-nunito font-black text-2xl text-charcoal text-center mb-8" data-aos="fade-up">You Might Also Like</h2>
    <div class="grid grid-cols-1 sm:grid-cols-3 gap-6">
      <a href="photo.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="0">
        <img src="img/photo-front.jpg" alt="Photo printing service – Achie's Printing Davao" class="img-default">
        <img src="img/mockup-photo.jpg" alt="Printed photos sample" class="img-hover">
        <span class="card-label">Photo Printing</span><span class="card-order">Order This →</span>
      </a>
      <a href="docs.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="50">
        <img src="img/doc-front.jpg" alt="Document printing – Achie's Printing Davao" class="img-default">
        <img src="img/mockup-docs.jpg" alt="Document print sample" class="img-hover">
        <span class="card-label">Document Print</span><span class="card-order">Order This →</span>
      </a>
      <a href="PVC-ID.html" class="sticker-card product-card" data-aos="fade-up" data-aos-delay="100">
        <img src="img/card-front.jpg" alt="PVC ID card printing – Achie's Printing Davao" class="img-default">
        <img src="img/mockup-card.jpg" alt="PVC ID card sample" class="img-hover">
        <span class="card-label">ID Card</span><span class="card-order">Order This →</span>
      </a>
    </div>
  </div>
</section>

</main>

<!-- STICKY CTA -->
<div class="sticky-cta">
  <div class="max-w-2xl mx-auto flex items-center justify-between gap-4">
    <p class="font-nunito font-bold text-charcoal text-sm hidden sm:block">Ready to order your ID photos?</p>
    <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
       class="sticker-btn bg-coral text-white px-6 py-2 text-sm w-full sm:w-auto text-center">Message Us Now →</a>
  </div>
</div>

<!-- FLOATING CTA -->
<div class="messenger-float" style="bottom: 5rem;">
  <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
     aria-label="Message Achie's Printing on Facebook Messenger"
     class="sticker-btn bg-coral text-white flex items-center gap-2 px-4 py-3 text-sm">
    <i class="ph-fill ph-messenger-logo text-xl"></i>
    <span class="hidden sm:inline font-bold">Message Us</span>
  </a>
</div>

<!-- FOOTER -->
<footer class="bg-charcoal text-cream py-12 font-nunito">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="grid grid-cols-1 md:grid-cols-3 gap-8 pb-8 border-b border-white/20">
      <div>
        <h3 class="font-black text-xl text-white mb-2">Achie's Printing</h3>
        <p class="text-white/70 text-sm leading-relaxed">The printing shop Doña Pilar grew up with. Quick, honest, and priced right.</p>
      </div>
      <div>
        <h4 class="font-bold text-white mb-3">Store Hours</h4>
        <p class="text-white/70 text-sm">Monday – Sunday</p>
        <p class="text-white/70 text-sm">8:00 AM – 7:00 PM</p>
        <p class="text-white/50 text-xs mt-1">Philippine Time (UTC+08:00)</p>
      </div>
      <div>
        <h4 class="font-bold text-white mb-3">Connect</h4>
        <div class="flex gap-3 mb-4">
          <a href="https://www.facebook.com/profile.php?id=100052801946765" target="_blank" rel="noopener" aria-label="Facebook"
             class="bg-white/10 hover:bg-coral rounded-full p-2 transition-colors">
            <i class="ph ph-facebook-logo text-xl text-white"></i></a>
          <a href="mailto:ansheened@gmail.com" aria-label="Email"
             class="bg-white/10 hover:bg-coral rounded-full p-2 transition-colors">
            <i class="ph ph-envelope text-xl text-white"></i></a>
          <a href="https://m.me/110237597413659" target="_blank" rel="noopener" aria-label="Messenger"
             class="bg-white/10 hover:bg-coral rounded-full p-2 transition-colors">
            <i class="ph ph-messenger-logo text-xl text-white"></i></a>
        </div>
        <a href="https://m.me/110237597413659" target="_blank" rel="noopener"
           class="sticker-btn bg-coral text-white px-5 py-2 text-sm block text-center">Send us a message</a>
      </div>
    </div>
    <p class="text-center text-white/40 text-xs mt-6">© 2025 Achie's Printing – Doña Pilar Village, Davao City, Philippines</p>
  </div>
</footer>

<!-- FB Chat -->
<div id="fb-root"></div>
<div id="fb-customer-chat" class="fb-customerchat"></div>
<script>
  var chatbox = document.getElementById("fb-customer-chat");
  chatbox.setAttribute("page_id", "110237597413659");
  chatbox.setAttribute("attribution", "biz_inbox");
</script>
<script>
  window.fbAsyncInit = function () { FB.init({ xfbml: true, version: "v15.0" }); };
  (function (d, s, id) {
    var js, fjs = d.getElementsByTagName(s)[0];
    if (d.getElementById(id)) return;
    js = d.createElement(s); js.id = id;
    js.src = "https://connect.facebook.net/en_US/sdk/xfbml.customerchat.js";
    fjs.parentNode.insertBefore(js, fjs);
  })(document, "script", "facebook-jssdk");
</script>

<!-- GLightbox init -->
<script>GLightbox({ selector: '.glightbox' });</script>
<!-- AOS init -->
<script>AOS.init({ duration: 600, once: true, easing: 'ease-out-quad' });</script>
</body>
</html>
```

- [ ] **Step 2: Verify `id.html`** — Hero shows, gallery opens in lightbox on click, sticky CTA bar fixed at bottom, floating Messenger pill sits above the sticky bar, related products show hover effect. All links work.

- [ ] **Step 3: Commit**

```bash
git add id.html
git commit -m "id.html: full Tailwind PDP rewrite — hero, GLightbox gallery, sticky CTA, related products"
```

---

## Tasks 8–20: Remaining 13 Product Pages

Each page uses the exact same HTML structure as `id.html` from Task 7. Only the values in the table below change. Copy `id.html`, apply the changes, commit.

**How to apply each page:**
1. Copy `id.html` as starting point
2. Replace every value from the table row for that page
3. In the gallery section, use the `img/` files listed in "Gallery images" — replace `IDset1.jpg`…`IDset6.jpg` with those filenames (keep same `<a>` + `<img>` structure, update `alt` text, `data-description`, and `data-gallery` attribute to match the page)
4. Replace the 3 related product cards with those listed under "Related"
5. Replace the hero illustration `src` and `alt`
6. Replace the use-case icons (the 4 sticker-card icons in the Details section) with the ones listed
7. Commit with the message pattern shown

---

### Task 8: `photo.html` — Photo Printing

| Field | Value |
|---|---|
| `<title>` | `Photo Printing Davao \| Achie's Printing – Doña Pilar` |
| `<meta description>` | `Print your favorite memories at Achie's Printing, Doña Pilar Village, Davao. 4x6, 5x7, and large format photo prints on glossy paper. Order via Messenger!` |
| `canonical` | `https://adrianvyne.github.io/Achie-Printing/photo.html` |
| `og:image` | `img/photo-front.jpg` |
| Hero `<h1>` | `Photo <span class="text-coral">Printing</span>` |
| Hero description | `Print your memories on high-quality glossy paper. 4x6, 5x7, wallet size, and more — available same day at Doña Pilar, Davao.` |
| Hero illustration | `src="img/hero-photo.png"` alt: `Sticker-style Filipino woman holding a printed family photo` |
| Details `<h2>` | `About Photo Printing` |
| Details body | `We print your digital photos on glossy or matte photo paper in multiple sizes — from wallet prints to large format. Perfect for gifting, framing, or albums. Bring your file on USB or send via Messenger.` |
| Use-case icons | `ph-images` "Portraits", `ph-gift` "Gifts", `ph-frame-corners` "Framing", `ph-heart` "Albums" |
| Gallery `data-gallery` | `photo-gallery` |
| Gallery images | `img/Photoprint.jpg`, `img/photo-front.jpg`, `img/mockup-photo.jpg` (and duplicate first two as needed for 6 slots — use only available images, no broken paths) |
| Sticky CTA text | `Ready to print your photos?` |
| Related | `id.html` (ID Photo), `docs.html` (Document Print), `mugs.html` (Custom Mug) |
| Commit message | `photo.html: Tailwind PDP rewrite — photo printing page` |

---

### Task 9: `docs.html` — Document Print

| Field | Value |
|---|---|
| `<title>` | `Document Printing Davao \| Achie's Printing – Doña Pilar` |
| `<meta description>` | `Document printing services at Achie's Printing, Doña Pilar Village, Davao. Thesis, reports, forms, and more — fast and affordable. Message us on Facebook!` |
| `canonical` | `https://adrianvyne.github.io/Achie-Printing/docs.html` |
| `og:image` | `img/doc-front.jpg` |
| Hero `<h1>` | `Document <span class="text-coral">Printing</span>` |
| Hero description | `Thesis, reports, forms, certificates — printed fast and crisp at Doña Pilar, Davao. Bring your file digitally or send via Messenger.` |
| Hero illustration | `src="img/hero-docs.png"` alt: `Sticker-style Filipino student holding printed documents` |
| Details body | `We print all types of documents — school reports, thesis, business forms, certificates, and more. Choose from black-and-white or full color. Fast turnaround, no minimum order.` |
| Use-case icons | `ph-graduation-cap` "Thesis", `ph-file-text` "Reports", `ph-certificate` "Certificates", `ph-buildings` "Business" |
| Gallery `data-gallery` | `docs-gallery` |
| Gallery images | `img/doc-front.jpg`, `img/doc-printing.jpg`, `img/mockup-docs.jpg` |
| Sticky CTA text | `Need documents printed?` |
| Related | `xerox.html` (Photocopy), `laminate.html` (Lamination), `id.html` (ID Photo) |
| Commit message | `docs.html: Tailwind PDP rewrite — document printing page` |

---

### Task 10: `xerox.html` — Photocopy

| Field | Value |
|---|---|
| `<title>` | `Photocopy Service Davao \| Achie's Printing – Doña Pilar` |
| `<meta description>` | `Fast photocopy services at Achie's Printing, Doña Pilar Village, Davao. Black-and-white and color photocopying, no minimum. Message us on Facebook!` |
| `canonical` | `https://adrianvyne.github.io/Achie-Printing/xerox.html` |
| `og:image` | `img/photocopier.png` |
| Hero `<h1>` | `Photocopy <span class="text-coral">Service</span>` |
| Hero description | `Quick black-and-white and color photocopying at Doña Pilar, Davao. No minimum order. Bring your documents and we'll get them done fast.` |
| Hero illustration | `src="img/hero-xerox.png"` alt: `Sticker-style Filipino using a photocopier` |
| Details body | `We offer fast black-and-white and color photocopying for any document — IDs, forms, books, receipts, and more. No minimum order. Perfect for students and offices.` |
| Use-case icons | `ph-student` "Students", `ph-buildings` "Offices", `ph-book-open` "Books", `ph-file` "Forms" |
| Gallery `data-gallery` | `xerox-gallery` |
| Gallery images | `img/photocopier.png`, `img/doc-front.jpg`, `img/mockup-xerox.jpg` |
| Sticky CTA text | `Need something photocopied?` |
| Related | `docs.html` (Document Print), `laminate.html` (Lamination), `id.html` (ID Photo) |
| Commit message | `xerox.html: Tailwind PDP rewrite — photocopy page` |

---

### Task 11: `laminate.html` — Lamination

| Field | Value |
|---|---|
| `<title>` | `Lamination Service Davao \| Achie's Printing – Doña Pilar` |
| `<meta description>` | `Lamination services at Achie's Printing, Doña Pilar Village, Davao. IDs, certificates, photos, and more — protect your documents affordably. Order via Messenger!` |
| `canonical` | `https://adrianvyne.github.io/Achie-Printing/laminate.html` |
| `og:image` | `img/lam-front.jpg` |
| Hero `<h1>` | `Lamination <span class="text-coral">Service</span>` |
| Hero description | `Protect your IDs, certificates, and documents with quality lamination. Glossy and matte finish available at Doña Pilar, Davao.` |
| Hero illustration | `src="img/hero-lam.png"` alt: `Sticker-style Filipino holding a laminated ID card` |
| Details body | `Laminate your IDs, certificates, photos, and important documents. Choose from glossy or matte finish. Affordable and fast — done while you wait for most items.` |
| Use-case icons | `ph-identification-card` "IDs", `ph-certificate` "Certificates", `ph-images` "Photos", `ph-file-text` "Documents" |
| Gallery `data-gallery` | `lam-gallery` |
| Gallery images | `img/lam-front.jpg`, `img/laminate.png`, `img/mockup-lam.jpg` |
| Sticky CTA text | `Need something laminated?` |
| Related | `PVC-ID.html` (ID Card), `docs.html` (Document Print), `xerox.html` (Photocopy) |
| Commit message | `laminate.html: Tailwind PDP rewrite — lamination page` |

---

### Task 12: `tarp.html` — Tarpaulin

| Field | Value |
|---|---|
| `<title>` | `Tarpaulin Printing Davao \| Achie's Printing – Doña Pilar` |
| `<meta description>` | `Tarpaulin printing in Doña Pilar Village, Davao. Birthdays, weddings, events, and business tarps — vibrant full-color prints. Message Achie's Printing on Facebook!` |
| `canonical` | `https://adrianvyne.github.io/Achie-Printing/tarp.html` |
| `og:image` | `img/tarp-front.jpg` |
| Hero `<h1>` | `Tarpaulin <span class="text-coral">Printing</span>` |
| Hero description | `Vibrant full-color tarpaulin prints for birthdays, weddings, events, and business. Any size, fast turnaround, delivered right in Davao.` |
| Hero illustration | `src="img/hero-tarp.png"` alt: `Sticker-style Filipinos hanging a colorful celebration tarpaulin` |
| Details body | `We print full-color tarpaulins for celebrations, events, and businesses. From small banners to large outdoor tarps — any size available. Bring your design or let us create one for you.` |
| Use-case icons | `ph-confetti` "Birthdays", `ph-heart` "Weddings", `ph-megaphone` "Events", `ph-storefront` "Business" |
| Gallery `data-gallery` | `tarp-gallery` |
| Gallery images | `img/tarp-front.jpg`, `img/tarp.png`, `img/mockup-tarp.jpg` |
| Sticky CTA text | `Want a tarp printed?` |
| Related | `shirtprint.html` (Shirt Printing), `eco-bag.html` (Eco-Bag), `mugs.html` (Custom Mug) |
| Commit message | `tarp.html: Tailwind PDP rewrite — tarpaulin page` |

---

### Task 13: `hats.html` — Bucket Hat

| Field | Value |
|---|---|
| `<title>` | `Custom Bucket Hat Printing Davao \| Achie's Printing – Doña Pilar` |
| `<meta description>` | `Custom printed bucket hats in Doña Pilar Village, Davao. Great for events, giveaways, and teams. Order via Facebook Messenger at Achie's Printing!` |
| `canonical` | `https://adrianvyne.github.io/Achie-Printing/hats.html` |
| `og:image` | `img/hats-front.jpg` |
| Hero `<h1>` | `Custom <span class="text-coral">Bucket Hats</span>` |
| Hero description | `Personalized bucket hats for events, teams, and giveaways. Multiple colors available, printed right here in Doña Pilar, Davao.` |
| Hero illustration | `src="img/hero-hats.png"` alt: `Sticker-style cool Filipino teen wearing a custom bucket hat` |
| Details body | `We print custom designs on quality bucket hats — perfect for school events, team uniforms, company giveaways, and special occasions. Available in multiple colors.` |
| Use-case icons | `ph-users` "Teams", `ph-gift` "Giveaways", `ph-confetti` "Events", `ph-sun` "Everyday" |
| Gallery `data-gallery` | `hats-gallery` |
| Gallery images | `img/hats-front.jpg`, `img/buckethats.jpg`, `img/mockup-hats.jpg` |
| Sticky CTA text | `Want custom bucket hats?` |
| Related | `shirtprint.html` (Shirt Printing), `umbrella.html` (Umbrella), `chain.html` (Key Chain) |
| Commit message | `hats.html: Tailwind PDP rewrite — bucket hat page` |

---

### Task 14: `chain.html` — Key Chain

| Field | Value |
|---|---|
| `<title>` | `Custom Keychains Davao \| Achie's Printing – Doña Pilar` |
| `<meta description>` | `Custom photo keychains in Doña Pilar Village, Davao. Perfect giveaways for birthdays, christenings, and weddings. Order via Messenger at Achie's Printing!` |
| `canonical` | `https://adrianvyne.github.io/Achie-Printing/chain.html` |
| `og:image` | `img/key-front.jpg` |
| Hero `<h1>` | `Custom <span class="text-coral">Keychains</span>` |
| Hero description | `Custom photo keychains — the perfect personalized giveaway for birthdays, christenings, and special events in Davao.` |
| Hero illustration | `src="img/hero-chain.png"` alt: `Sticker-style Filipino child holding a custom keychain with delight` |
| Details body | `We print custom photo keychains — great for party giveaways, souvenirs, and personalized gifts. Durable, affordable, and available in various shapes.` |
| Use-case icons | `ph-confetti` "Birthdays", `ph-baby` "Christenings", `ph-heart` "Weddings", `ph-gift` "Giveaways" |
| Gallery `data-gallery` | `chain-gallery` |
| Gallery images | `img/key-front.jpg`, `img/keychains.png`, `img/mockup-chain.jpg` |
| Sticky CTA text | `Want custom keychains?` |
| Related | `refmagnet.html` (Ref Magnet), `mugs.html` (Custom Mug), `eco-bag.html` (Eco-Bag) |
| Commit message | `chain.html: Tailwind PDP rewrite — keychains page` |

---

### Task 15: `PVC-ID.html` — ID Card

| Field | Value |
|---|---|
| `<title>` | `PVC ID Card Printing Davao \| Achie's Printing – Doña Pilar` |
| `<meta description>` | `PVC ID card printing in Doña Pilar Village, Davao. School, office, and organization IDs with lanyards. Fast turnaround. Message us on Facebook Messenger!` |
| `canonical` | `https://adrianvyne.github.io/Achie-Printing/PVC-ID.html` |
| `og:image` | `img/card-front.jpg` |
| Hero `<h1>` | `PVC <span class="text-coral">ID Cards</span>` |
| Hero description | `Durable PVC ID cards for schools, offices, and organizations — with lanyards available. Printed fast at Doña Pilar, Davao.` |
| Hero illustration | `src="img/hero-card.png"` alt: `Sticker-style Filipino employee showing a PVC ID card` |
| Details body | `We print full-color PVC ID cards — durable, professional, and perfect for schools, companies, and organizations. Includes lanyards on request. Fast turnaround.` |
| Use-case icons | `ph-student` "Schools", `ph-buildings` "Companies", `ph-users` "Organizations", `ph-identification-card` "Events" |
| Gallery `data-gallery` | `card-gallery` |
| Gallery images | `img/card-front.jpg`, `img/ID-Card.jpg`, `img/IDset1.jpg`, `img/IDset2.jpg`, `img/mockup-card.jpg` |
| Sticky CTA text | `Need ID cards printed?` |
| Related | `id.html` (ID Photo), `laminate.html` (Lamination), `chain.html` (Key Chain) |
| Commit message | `PVC-ID.html: Tailwind PDP rewrite — PVC ID card page` |

---

### Task 16: `shirtprint.html` — Shirt Printing

| Field | Value |
|---|---|
| `<title>` | `Shirt Printing Davao \| Achie's Printing – Doña Pilar` |
| `<meta description>` | `Custom shirt printing in Doña Pilar Village, Davao. Team uniforms, event shirts, and personalized tees. Message Achie's Printing on Facebook to order!` |
| `canonical` | `https://adrianvyne.github.io/Achie-Printing/shirtprint.html` |
| `og:image` | `img/shirt-front.jpg` |
| Hero `<h1>` | `Shirt <span class="text-coral">Printing</span>` |
| Hero description | `Custom printed shirts for teams, events, and family reunions — vibrant prints that last. Available at Doña Pilar, Davao.` |
| Hero illustration | `src="img/hero-shirt.png"` alt: `Sticker-style Filipino friends in matching custom printed shirts` |
| Details body | `We print custom designs on quality shirts using heat press and screen printing. Perfect for uniforms, team events, family reunions, and giveaways. Bring your design or let us create one.` |
| Use-case icons | `ph-users` "Teams", `ph-confetti` "Events", `ph-house` "Reunions", `ph-storefront` "Uniforms" |
| Gallery `data-gallery` | `shirt-gallery` |
| Gallery images | `img/shirt-front.jpg`, `img/shirts.jpg`, `img/shirt.jpg`, `img/shirt1.jpg`, `img/shirt2.jpg`, `img/mockup-shirt.jpg` |
| Sticky CTA text | `Want shirts printed?` |
| Related | `hats.html` (Bucket Hat), `eco-bag.html` (Eco-Bag), `tarp.html` (Tarpaulin) |
| Commit message | `shirtprint.html: Tailwind PDP rewrite — shirt printing page` |

---

### Task 17: `umbrella.html` — Umbrella

| Field | Value |
|---|---|
| `<title>` | `Custom Umbrella Printing Davao \| Achie's Printing – Doña Pilar` |
| `<meta description>` | `Custom printed umbrellas in Doña Pilar Village, Davao. Great for corporate giveaways and events. Order via Facebook Messenger at Achie's Printing!` |
| `canonical` | `https://adrianvyne.github.io/Achie-Printing/umbrella.html` |
| `og:image` | `img/brela-front.jpg` |
| Hero `<h1>` | `Custom <span class="text-coral">Umbrellas</span>` |
| Hero description | `Printed umbrellas for corporate giveaways, events, and promotions. Stand out even on a rainy Davao day.` |
| Hero illustration | `src="img/hero-umbrella.png"` alt: `Sticker-style Filipino woman under a custom printed umbrella in Davao rain` |
| Details body | `We print custom designs on quality umbrellas — ideal for corporate branding, event giveaways, and promotions. A practical and memorable way to showcase your brand in Davao.` |
| Use-case icons | `ph-buildings` "Corporate", `ph-gift` "Giveaways", `ph-megaphone` "Promotions", `ph-confetti` "Events" |
| Gallery `data-gallery` | `umbrella-gallery` |
| Gallery images | `img/brela-front.jpg`, `img/umbrela.jpg`, `img/mockup-umbrella.jpg` |
| Sticky CTA text | `Want custom umbrellas?` |
| Related | `hats.html` (Bucket Hat), `eco-bag.html` (Eco-Bag), `shirtprint.html` (Shirt Printing) |
| Commit message | `umbrella.html: Tailwind PDP rewrite — umbrella page` |

---

### Task 18: `refmagnet.html` — Ref Magnet

| Field | Value |
|---|---|
| `<title>` | `Custom Ref Magnets Davao \| Achie's Printing – Doña Pilar` |
| `<meta description>` | `Custom refrigerator magnets in Doña Pilar Village, Davao. Perfect personalized giveaways for birthdays and special events. Order via Messenger at Achie's Printing!` |
| `canonical` | `https://adrianvyne.github.io/Achie-Printing/refmagnet.html` |
| `og:image` | `img/ref-front.jpg` |
| Hero `<h1>` | `Custom <span class="text-coral">Ref Magnets</span>` |
| Hero description | `Personalized refrigerator magnets — the sweetest giveaway for birthdays, christenings, and milestones in Davao.` |
| Hero illustration | `src="img/hero-ref.png"` alt: `Sticker-style Filipino family looking at custom ref magnets on their fridge` |
| Details body | `We print custom photo and design ref magnets — a fun and practical giveaway that stays on the fridge and keeps your memory alive. Perfect for birthdays, christenings, weddings, and anniversaries.` |
| Use-case icons | `ph-confetti` "Birthdays", `ph-baby` "Christenings", `ph-heart` "Weddings", `ph-star` "Milestones" |
| Gallery `data-gallery` | `ref-gallery` |
| Gallery images | `img/ref-front.jpg`, `img/ref-magnet.png`, `img/mockup-ref.jpg` |
| Sticky CTA text | `Want custom ref magnets?` |
| Related | `chain.html` (Key Chain), `mugs.html` (Custom Mug), `eco-bag.html` (Eco-Bag) |
| Commit message | `refmagnet.html: Tailwind PDP rewrite — ref magnet page` |

---

### Task 19: `eco-bag.html` — Eco-Bag

| Field | Value |
|---|---|
| `<title>` | `Custom Eco-Bag Printing Davao \| Achie's Printing – Doña Pilar` |
| `<meta description>` | `Custom printed eco-bags in Doña Pilar Village, Davao. Sustainable, stylish, and perfect for events and giveaways. Message Achie's Printing on Facebook!` |
| `canonical` | `https://adrianvyne.github.io/Achie-Printing/eco-bag.html` |
| `og:image` | `img/eco-front.jpg` |
| Hero `<h1>` | `Custom <span class="text-coral">Eco-Bags</span>` |
| Hero description | `Sustainable custom eco-bags for events, giveaways, and everyday use. Printed with your design right here in Doña Pilar, Davao.` |
| Hero illustration | `src="img/hero-ecobag.png"` alt: `Sticker-style Filipino woman carrying a custom eco-bag at a Davao market` |
| Details body | `We print custom designs on durable eco-friendly bags — a practical and environmentally conscious giveaway. Great for events, promotions, and school use.` |
| Use-case icons | `ph-leaf` "Eco-Friendly", `ph-gift` "Giveaways", `ph-storefront` "Promotions", `ph-student` "School" |
| Gallery `data-gallery` | `ecobag-gallery` |
| Gallery images | `img/eco-front.jpg`, `img/eco-bag.png`, `img/mockup-ecobag.jpg` |
| Sticky CTA text | `Want custom eco-bags?` |
| Related | `shirtprint.html` (Shirt Printing), `tarp.html` (Tarpaulin), `umbrella.html` (Umbrella) |
| Commit message | `eco-bag.html: Tailwind PDP rewrite — eco-bag page` |

---

### Task 20: `mugs.html` — Custom Mug

| Field | Value |
|---|---|
| `<title>` | `Custom Mug Printing Davao \| Achie's Printing – Doña Pilar` |
| `<meta description>` | `Custom photo mugs in Doña Pilar Village, Davao. Personalized gift mugs for birthdays, anniversaries, and special moments. Order via Facebook Messenger!` |
| `canonical` | `https://adrianvyne.github.io/Achie-Printing/mugs.html` |
| `og:image` | `img/mug-front.jpg` |
| Hero `<h1>` | `Custom <span class="text-coral">Photo Mugs</span>` |
| Hero description | `Personalized photo mugs for birthdays, gifts, and special moments. A daily reminder of your best memories — printed in Doña Pilar, Davao.` |
| Hero illustration | `src="img/hero-mug.png"` alt: `Sticker-style Filipino enjoying morning coffee from a custom printed mug` |
| Details body | `We print custom photos and designs on quality ceramic mugs — the perfect personalized gift. Available in standard 11oz size. Great for birthdays, anniversaries, and thank-you gifts.` |
| Use-case icons | `ph-coffee` "Daily Use", `ph-gift` "Gifts", `ph-confetti` "Birthdays", `ph-heart` "Couples" |
| Gallery `data-gallery` | `mug-gallery` |
| Gallery images | `img/mug-front.jpg`, `img/MUG1.jpg`, `img/mockup-mug.jpg` |
| Sticky CTA text | `Want a custom mug?` |
| Related | `chain.html` (Key Chain), `refmagnet.html` (Ref Magnet), `photo.html` (Photo Printing) |
| Commit message | `mugs.html: Tailwind PDP rewrite — custom mug page` |

---

## Task 21: `sitemap.xml` + Final Verification

**Files:**
- Create: `sitemap.xml`

- [ ] **Step 1: Create `sitemap.xml` in project root:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="https://www.sitemaps.org/schemas/sitemap/0.9">
  <url><loc>https://adrianvyne.github.io/Achie-Printing/</loc><changefreq>monthly</changefreq><priority>1.0</priority></url>
  <url><loc>https://adrianvyne.github.io/Achie-Printing/id.html</loc><changefreq>monthly</changefreq><priority>0.8</priority></url>
  <url><loc>https://adrianvyne.github.io/Achie-Printing/photo.html</loc><changefreq>monthly</changefreq><priority>0.8</priority></url>
  <url><loc>https://adrianvyne.github.io/Achie-Printing/docs.html</loc><changefreq>monthly</changefreq><priority>0.8</priority></url>
  <url><loc>https://adrianvyne.github.io/Achie-Printing/xerox.html</loc><changefreq>monthly</changefreq><priority>0.8</priority></url>
  <url><loc>https://adrianvyne.github.io/Achie-Printing/laminate.html</loc><changefreq>monthly</changefreq><priority>0.8</priority></url>
  <url><loc>https://adrianvyne.github.io/Achie-Printing/tarp.html</loc><changefreq>monthly</changefreq><priority>0.8</priority></url>
  <url><loc>https://adrianvyne.github.io/Achie-Printing/hats.html</loc><changefreq>monthly</changefreq><priority>0.8</priority></url>
  <url><loc>https://adrianvyne.github.io/Achie-Printing/chain.html</loc><changefreq>monthly</changefreq><priority>0.8</priority></url>
  <url><loc>https://adrianvyne.github.io/Achie-Printing/PVC-ID.html</loc><changefreq>monthly</changefreq><priority>0.8</priority></url>
  <url><loc>https://adrianvyne.github.io/Achie-Printing/shirtprint.html</loc><changefreq>monthly</changefreq><priority>0.8</priority></url>
  <url><loc>https://adrianvyne.github.io/Achie-Printing/umbrella.html</loc><changefreq>monthly</changefreq><priority>0.8</priority></url>
  <url><loc>https://adrianvyne.github.io/Achie-Printing/refmagnet.html</loc><changefreq>monthly</changefreq><priority>0.8</priority></url>
  <url><loc>https://adrianvyne.github.io/Achie-Printing/eco-bag.html</loc><changefreq>monthly</changefreq><priority>0.8</priority></url>
  <url><loc>https://adrianvyne.github.io/Achie-Printing/mugs.html</loc><changefreq>monthly</changefreq><priority>0.8</priority></url>
</urlset>
```

- [ ] **Step 2: Full site verification checklist**
  - Open every page in browser — nav links work, footer present, floating pill visible
  - On mobile viewport (375px): hamburger opens, all sections stack correctly, sticky CTA visible
  - On homepage: Swiper carousel scrolls, FAQ accordion opens/closes with Alpine, AOS animations fire on scroll
  - On any PDP: GLightbox opens on gallery image click, sticky CTA bar at bottom, floating pill sits above it
  - Check browser console on each page — zero 404 errors for CSS/JS (image 404s from AI-generated images not yet created are acceptable)
  - Check `<title>` and `<meta name="description">` in page source for every page — each should be unique

- [ ] **Step 3: Commit**

```bash
git add sitemap.xml
git commit -m "add sitemap.xml with all 15 pages listed for SEO"
```

- [ ] **Step 4: Final commit to signal completion**

```bash
git add -A
git commit -m "complete Tailwind redesign — all 15 pages, SEO, Messenger CTAs, sticker design system"
```
