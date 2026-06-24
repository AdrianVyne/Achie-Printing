# Achie's Printing — Full Website Redesign Design Spec
**Date:** 2026-06-24  
**Status:** Approved

---

## 1. Project Context

Achie's Printing is a long-standing family-run print shop located at Doña Pilar Village, Davao. It operates out of a residential house (gate #72) — not a commercial building. The business is one of the oldest in the area, having served the community for many years. It peaked early with a studio, computer rentals, and student traffic. Today it keeps things simple: quick, honest, and effective work at low prices. The reach has been shrinking and the goal of this redesign is to bring more customers in — primarily through Facebook Messenger inquiries.

**Primary conversion goal:** Get visitors to message the business on Facebook Messenger.  
**Target audience:** Residents of Doña Pilar Village and surrounding Davao barangays, students, small business owners, event organizers.

---

## 2. Tech Stack

All libraries are CDN-only. No npm, no build step. The repo stays a pure static site deployable to GitHub Pages.

| Library | Version | Purpose |
|---|---|---|
| Tailwind CSS | v3 (CDN play script) | Utility-first CSS framework |
| Alpine.js | v3 (CDN) | Mobile nav, FAQ accordion, lightweight interactivity |
| Swiper.js | Latest (CDN) | Hero carousel, moments carousel |
| AOS | Latest (CDN) | Scroll-reveal animations |
| GLightbox | Latest (CDN) | Product image lightbox on PDPs |
| Google Fonts: Nunito | — | Display and body typography |
| Phosphor Icons | Latest (CDN) | Icon set |

**Removed:** Bootstrap 5, Bootstrap Icons, jQuery, Swiffy Slider.

---

## 3. Design System

### 3.1 Color Palette
Derived from sticker-style illustration palette — warm, high contrast, bold outline feel.

| Role | Name | Hex |
|---|---|---|
| Primary | Warm Coral | `#FF6B6B` |
| Secondary | Sunny Yellow | `#FFD93D` |
| Accent | Mint Green | `#6BCB77` |
| Dark | Warm Charcoal | `#2D2D2D` |
| Background | Soft Cream | `#FFFDF7` |
| Card Surface | Pure White | `#FFFFFF` |

### 3.2 Typography
- **Font:** Nunito (Google Fonts), weights 400 / 700 / 900
- **Display headings:** Nunito ExtraBold (900) — big, round, playful
- **Body:** Nunito Regular (400)
- **Buttons/labels:** Nunito Bold (700), slight letter-spacing

### 3.3 Visual Motifs
- Rounded corners everywhere (`rounded-2xl` on cards, `rounded-full` on buttons)
- Thick `2–3px` dark borders on cards — sticker outline feel
- Subtle drop shadows that pop cards off the page like stickers
- Wavy SVG section dividers between major homepage sections
- Pastel blob/shape backgrounds behind hero illustrations

---

## 4. Illustration & Image Strategy

### 4.1 Hero & Banners (per page)
AI-generated sticker-style cartoon characters — bold outlines, rounded shapes, minimal/empty faces (like LINE stickers or Shopee sticker packs). Characters are Davaoeño-coded: brown skin, casual Filipino clothing, relatable settings. One character per page hero, interacting with that page's product category. Total: 15 unique illustrations needed (1 homepage + 14 product pages). Prompts are defined in the implementation plan.

### 4.2 Product Mockups (product pages)
AI-generated realistic product mockups — close-to-reality depictions of each product (shirt flat-lays, ID card mockups, mug on a table, tarpaulin on a wall, etc.) in a Filipino/Davao visual context. These replace or supplement existing product photos in the PDPs.

### 4.3 Real Photos
Existing carousel photos (carosel1.jpg, carosel2.jpg, carosel3.jpg) and product front images are kept in the homepage hero carousel and product cards where they exist.

---

## 5. Page Structure

### 5.1 Homepage (`index.html`)

1. **Nav** — Logo left, links center (Products, Contact, FAQ), `Message Us` coral button right. Collapses to hamburger on mobile (Alpine.js).
2. **Hero** — Full-width section, cream background with blob shapes. Left: headline + subline + two CTAs. Right: sticker-style AI illustration. Headline: *"The Printing Shop Doña Pilar Grew Up With."*
3. **Wavy SVG Divider**
4. **Products Grid** — Unified Tailwind responsive grid (no duplicate mobile/desktop HTML). Cards have thick border, hover reveals realistic AI product mockup, each card has an `Order This →` button.
5. **Moments Carousel** — Swiper.js. Save the Date, Baby Shower, Christening, Birthday, Wedding.
6. **Why Achie's** — 3-column section. Satisfaction Guarantee, Independent Artists, Honest Pricing. AOS fade-in.
7. **Contact + Map** — Google Maps embed left, contact details right. Includes `Chat with us on Messenger` CTA button.
8. **FAQ** — Alpine.js accordion. All existing FAQ questions kept.
9. **Footer** — Achie's Printing tagline, social links, Messenger link.
10. **Floating Messenger pill** — Fixed bottom-right on all pages.

### 5.2 Product Detail Pages (14 pages)

Same nav and footer as homepage. Structure:
1. **Hero banner** — Product name, sticker character using that product, cream/blob background.
2. **Product Gallery** — GLightbox grid of AI-generated realistic product mockups (3–6 images).
3. **Product Details** — Description, available sizes/materials, use cases. Semantic `<article>`.
4. **Sticky CTA Bar** (fixed bottom) — *"Ready to order? Message Us Now →"* coral button. Dismissible on desktop.
5. **Related Products** — 3 product cards linking to sibling pages.
6. **Footer** (same).

### 5.3 Shared Components
- Nav (identical across all pages)
- Footer (identical across all pages)
- Floating Messenger pill (all pages)
- AOS initialized in a shared `<script>` block

---

## 6. SEO

Every page receives:
- `<meta name="description">` — unique, 150–160 chars, includes "Doña Pilar", "Davao", product keyword
- Open Graph tags: `og:title`, `og:description`, `og:image`, `og:url`, `og:type`
- Twitter Card tags: `twitter:card`, `twitter:title`, `twitter:description`
- `<title>` pattern: `[Product] Davao | Achie's Printing – Doña Pilar`
- Semantic HTML: `<main>`, `<section>`, `<article>`, one `<h1>` per page
- Descriptive `alt` text on every image
- Canonical `<link rel="canonical">` on every page
- `schema.org/LocalBusiness` JSON-LD on homepage only:
  - Name, address (Doña Pilar Village, Davao), phone, email, hours (Mon–Sun 8:00 AM – 7:00 PM), geo coordinates, social profiles
- `sitemap.xml` — static file listing all 15 pages

---

## 7. Call to Action — Messenger

All Messenger links use `https://m.me/110237597413659` deep link format (Facebook page ID from existing chat plugin) for direct conversation open.

| Placement | Label |
|---|---|
| Nav (persistent) | `Message Us` |
| Hero (primary) | `Order via Messenger →` |
| Hero (secondary) | `See Products` |
| Product card (hover) | `Order This →` |
| Product page sticky bar | `Ready to order? Message Us Now →` |
| Contact section | `Chat with us on Messenger` |
| Footer | Messenger icon + `Send us a message` |
| Floating pill (all pages) | Messenger icon, fixed bottom-right |

---

## 8. Files Changed / Created

- `index.html` — full rewrite
- `styles.css` — replaced by Tailwind; minimal custom CSS only for things Tailwind can't do (sticker card borders, wavy dividers)
- `[product].html` × 14 — full rewrite (id, photo, docs, xerox, laminate, tarp, hats, chain, PVC-ID, shirtprint, umbrella, refmagnet, eco-bag, mugs)
- `sitemap.xml` — new file
- `img/` — new AI-generated images added (sticker characters, product mockups)
- `docs/superpowers/specs/` — this design doc

---

## 9. Out of Scope

- No backend, no form submissions, no CMS
- No e-commerce / cart / pricing calculator
- No user accounts
- No analytics integration (can be added later as a single script tag)
- No changes to the Facebook Messenger chat plugin page ID
