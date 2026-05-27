# Mobile QA Checklist — PlanTrade LP

**Product:** PlanTrade
**File:** index.html
**Date:** 2025-05
**Run before:** Every deploy + every copy update

---

## DEVICE MATRIX

| Device | Width | Status |
|--------|-------|--------|
| iPhone SE 3rd gen | 375px | ⬜ Test |
| Samsung Galaxy A series | 360px | ⬜ Test |
| iPhone 14 | 390px | ⬜ Test |
| Large Android (A54) | 414px | ⬜ Test |
| iPad / Tab | 768px | ⬜ Test |
| Desktop 1280px | 1280px | ⬜ Test |

Use Chrome DevTools Device Toolbar for emulation. Test on real device
for Samsung Internet compatibility.

---

## CRITICAL — LAUNCH BLOCKERS

- [ ] **Hero CTA visible above fold at 320px width** — open DevTools,
  set to 320px, verify "📦 Akses PlanTrade Sekarang — Rp 99k" button
  is fully visible without scrolling. Hero block height ≤ 568px at
  this width.
- [ ] **No horizontal scroll** — at all tested widths (320 / 375 / 390
  / 414 / 768 / 1024). Common culprits: `.mockup-doc` fixed width,
  `.testi-track` overflow.
- [ ] **Page weight < 1 MB** — Chrome DevTools → Network → Disable cache
  → reload → check total transferred.
- [ ] **Mobile PageSpeed Score ≥ 80** — test at pagespeed.web.dev after
  deploy.
- [ ] **CTA tap targets ≥ 44×44px** — primary CTAs, FAQ summary rows,
  footer links.

---

## 12-POINT CHECKLIST

### 1. CTA Above Fold (320px)
- [ ] Open 320px width in DevTools
- [ ] "📦 Akses PlanTrade Sekarang — Rp 99k" button fully visible
  without scrolling
- [ ] Hero block height within 568px (iPhone SE viewport)

### 2. Tap Targets
- [ ] All `.cta` buttons: min-height 52px ✓ (set in CSS)
- [ ] FAQ `.faq__q` summary rows: min-height 56px ✓ (set in CSS)
- [ ] Footer links: tap area ≥ 44px (check padding)
- [ ] Sticky CTA button: min-height 44px ✓

### 3. Body Font Size
- [ ] Body text renders at 16px minimum on all tested widths
- [ ] No `.pricing__disclaimer`, `.footer__legal` text below 12px
  (set at 0.78rem = 12.5px — acceptable for legal text)

### 4. Line Length
- [ ] Body paragraphs max ~65–70 chars per line on mobile
- [ ] Hero headline ≤ 12 kata per line — check at 320px
- [ ] `.container` max-width set to 720px — enforces line length

### 5. Page Weight
- [ ] HTML + CSS + JS inline: target < 100 KB
- [ ] After post-process (Pexels images fetched): < 1 MB total
- [ ] Hero image (hero.jpg) compressed < 200 KB
- [ ] Google Fonts load: 2 families (Plus Jakarta Sans + Inter) — each
  ~20-30 KB

### 6. Page Load (3G Simulation)
- [ ] Chrome DevTools → Network → Slow 3G → reload
- [ ] First Contentful Paint < 2 sec
- [ ] Hero CTA visible < 1.5 sec
- [ ] Time to Interactive < 3.5 sec

### 7. No Horizontal Scroll
- [ ] 320px: no horizontal scroll
- [ ] 375px: no horizontal scroll
- [ ] 414px: no horizontal scroll
- [ ] 768px: no horizontal scroll
- [ ] Check `.hero__inner` flex layout at tablet width
- [ ] Check `.mockup-doc` does not overflow on narrow screens

### 8. Touch / Interactive Elements
- [ ] Testimonial carousel: touch swipe works on real mobile
- [ ] Carousel auto-scrolls every 3.5 sec (pauses on touch) ✓
- [ ] FAQ accordion: tap opens/closes smoothly
- [ ] Sticky CTA: appears after scrolling 600px ✓
- [ ] All `<a href="#...">` scroll anchors work (smooth scroll)
- [ ] Countdown timer ticking live ✓

### 9. Form (N/A — Scalev injects)
- [ ] Confirm NO `<form>` element in LP HTML ✓ (Section 12 = CTA strip
  only, no form fields)
- [ ] Confirm NO payment method badges in HTML ✓
- [ ] Confirm Scalev checkout injection point exists (CTA buttons
  anchor to `#pricing` or Scalev embed target)

### 10. Safe Area Insets (iPhone Notch / Home Indicator)
- [ ] Sticky CTA uses `padding-bottom: calc(10px + env(safe-area-inset-bottom))`
  ✓ (set in CSS)
- [ ] Viewport meta includes `viewport-fit=cover` ✓
- [ ] Footer content not cut off by home indicator on iPhone 14

### 11. OG Image Preview
- [ ] OG image `lp-assets/hero.jpg` is 1200×630 after Pexels fetch
- [ ] Test Facebook Debugger: developers.facebook.com/tools/debug
- [ ] Test WhatsApp: paste LP URL in WA test chat — preview shows image
- [ ] Twitter Card Validator: og:title + image renders as
  `summary_large_image`
- [ ] Confirm `og:url` updated to real deploy URL (currently
  `https://plantrade.id/` — update before launch)

### 12. Accessibility Basics
- [ ] Single `<h1>` on page (in `<header class="hero">`) ✓
- [ ] Heading hierarchy: H1 → H2 → H3, no skips ✓
- [ ] All `<img>` have descriptive `alt` text ✓
- [ ] Color contrast: green #1A7A3C on white #FAFAFA = 5.8:1 ✓ (passes
  WCAG AA)
- [ ] Color contrast: text #1A1F1A on #FAFAFA = 17.2:1 ✓
- [ ] Color contrast: muted #7A8C7A on white — check (target ≥ 4.5:1
  for small text)
- [ ] Focus visible states: `:focus-visible` outline on all interactive
  elements ✓
- [ ] `aria-labelledby` on all `<section>` elements ✓
- [ ] `aria-live="polite"` on live ticker ✓
- [ ] `role="timer"` on countdown ✓
- [ ] FAQ uses native `<details>/<summary>` — keyboard accessible ✓

---

## BROWSER MATRIX

| Browser | Mobile | Desktop | Priority |
|---------|--------|---------|----------|
| Chrome for Android | ⬜ | ⬜ | Critical |
| Safari iOS | ⬜ | — | Critical |
| Samsung Internet | ⬜ | — | Critical (30%+ ID market) |
| Firefox Android | ⬜ | ⬜ | Optional |
| Edge | — | ⬜ | Optional |

---

## PERFORMANCE TARGETS

| Metric | Target | Tool |
|--------|--------|------|
| LCP | < 2.5 sec | PageSpeed Insights |
| CLS | < 0.1 | PageSpeed Insights |
| FID / INP | < 100 ms | PageSpeed Insights |
| Mobile Score | ≥ 80 | PageSpeed Insights |
| Total Size | < 1 MB | DevTools Network |

---

## PLANTRADE-SPECIFIC CHECKS

- [ ] Mockup trading plan document renders correctly on mobile — check
  `.mockup-doc` on 320px, all rows visible, no overflow
- [ ] JetBrains Mono font loads for mockup doc numbers — fallback to
  Courier New if not loaded (acceptable)
- [ ] Countdown reaches zero gracefully (displays 00:00:00:00, no NaN)
- [ ] Live ticker updates every 6 seconds — visible on mobile
- [ ] "Rp" formatting in pricing uses non-breaking space (Rp 99.000)
- [ ] Legal disclaimer visible on Section 10, Section 12, and Footer
- [ ] No `passive income` / `dijamin profit` / `pasti cuan` phrases
  anywhere — forbidden phrases check ✓
- [ ] `.pricing__tier-price--main` (Rp 99.000) has no emoji ✓

---

## PRE-PUBLISH FINAL GATE

Don't publish if ANY of these fail:
- ❌ Hero CTA below fold at 320px
- ❌ Page weight > 1.5 MB
- ❌ Mobile PageSpeed < 70
- ❌ Horizontal scroll on any width
- ❌ CTA tap target < 44px
- ❌ OG image not rendering in FB Debugger
- ❌ `<form>` element found in HTML (Scalev conflict)
- ❌ Countdown shows NaN

---

*PlanTrade LP QA v1 — Run before every deploy*