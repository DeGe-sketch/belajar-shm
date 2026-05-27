# Scalev Deploy Notes — PlanTrade LP

**Product:** PlanTrade
**Platform:** Scalev
**Files to deploy:** index.html, style.css (pre-inline), lp-assets/ folder

---

## Pre-Deploy

1. **Run post_process_lp()** — this step:
   - Reads `lp-image-manifest.json`
   - Fetches 10 images from Pexels using keywords
   - Downloads to `lp-assets/` folder
   - Swaps relative paths (`lp-assets/hero.jpg`) to absolute Pexels CDN
     URLs in `index.html`
   - Inlines `style.css` content into `<style>` block in `index.html`
   - Output: single portable `index.html` (no external CSS dependency)

2. **Update canonical URL** — in `index.html`, replace all instances of
   `https://plantrade.id/` with your actual Scalev-assigned URL or custom
   domain before upload.

3. **Update priceValidUntil** — in JSON-LD schema, update
   `"priceValidUntil": "2025-05-31"` to actual promotion end date.

4. **Verify legal disclaimer** — confirm the 4 disclaimer instances
   (Section 10, Section 12, Footer ×2) are present in final HTML.

---

## Scalev Upload Steps

1. Log in to Scalev dashboard → pilih produk PlanTrade atau buat baru.

2. Di halaman produk, pergi ke **"Landing Page"** atau **"Custom Page"**
   settings.

3. **Upload file:**
   - Pilih opsi "Custom HTML"
   - Upload `index.html` (post-processed, CSS already inlined)
   - Upload `lp-assets/` folder contents (10 image files) ke Scalev
     media library atau static assets

4. **Checkout button mapping:**
   - Scalev akan inject checkout form setelah tombol CTA diklik.
   - Pastikan semua tombol CTA di LP mengarah ke anchor `#pricing` atau
     ke Scalev's checkout embed trigger (sesuai Scalev documentation
     versi terbaru).
   - **PENTING:** Jangan tambah `<form>` atau `<input>` apapun ke LP —
     Scalev handles this. LP sudah di-design tanpa form (Section 12 =
     CTA strip only).

5. **Set product price:** Rp 99.000 di Scalev product settings.

6. **Custom domain (optional):** Connect `plantrade.id` di Scalev domain
   settings → SSL auto-provisioned.

---

## Post-Deploy Checks

- [ ] Open LP URL di browser — pastikan CSS loaded (bukan unstyled HTML)
- [ ] Test di Chrome DevTools 375px — hero CTA above fold
- [ ] Test checkout flow: klik CTA → Scalev form appears → isi data →
  payment options muncul (Scalev injects BCA/QRIS/dll automatically)
- [ ] Test email delivery: lakukan test purchase → konfirmasi link
  download terkirim ke email dalam 5 menit
- [ ] Update Facebook Debugger dengan URL live: scrape fresh OG meta
- [ ] Test WhatsApp link preview dengan URL live

---

## Ads Integration

**Meta / Facebook Ads:**
- LP URL masuk sebagai destination URL di Ad Creative
- Pastikan Facebook Pixel dipasang (tambah ke `<head>` di `index.html`
  sebelum deploy):
  ```html
  <!-- Meta Pixel Code -->
  <script>
  !function(f,b,e,v,n,t,s){...}(window, document,'script',
  'https://connect.facebook.net/en_US/fbevents.js');
  fbq('init', 'YOUR_PIXEL_ID');
  fbq('track', 'PageView');
  </script>
  ```
- Event tracking: tambah `fbq('track', 'InitiateCheckout')` di CTA
  button click handler.

**TikTok Ads:**
- Sama seperti Meta — tambah TikTok Pixel di `<head>`.
- LP sudah mobile-first — kompatibel dengan TikTok in-app browser.

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| CSS tidak muncul | Pastikan post_process_lp() sudah inline CSS ke `<style>` block |
| Gambar tidak muncul | Cek `lp-assets/` folder terupload semua; atau pastikan Pexels CDN URLs sudah di-swap |
| Checkout tidak muncul setelah klik CTA | Cek Scalev embed script terpasang; hubungi Scalev support |
| Font tidak load | Google Fonts URL di `<head>` — cek koneksi; fallback ke system font sudah di-set |
| Countdown menunjukkan NaN | Cek timezone offset di countdown JS — pastikan WIB offset benar (+7) |
| Live ticker tidak berubah | Cek JavaScript tidak error di console — jalankan di browser normal (bukan file://) |

---

*PlanTrade Scalev Deploy Notes v1*
```

---

## Self-Check Report

**Wireframe:**
- ✅ All 14 sections present in exact order
- ✅ Section 2 (testimonial carousel) BEFORE Section 3 (pain) — proof-first
- ✅ Section 7 (bonus stack) — 6 items with rupiah anchors (Rp 150k–250k each)
- ✅ Section 10 — 3-tier reveal: Rp 1.649.000 anchor → Rp 499.000 standard → Rp 99.000 special
- ✅ Section 13 upsell: NOT emitted (`include_upsell` not specified, default false)
- ✅ Section 14 footer: live ticker + countdown + legal

**Copy:**
- ✅ Voice `kamu/aku` consistent throughout all 14 sections
- ✅ Hero headline: "Punya Trading Plan Lengkap Dalam 10 Menit Sebelum Klik Beli Saham Apapun" — 13 kata, concrete number "10 Menit" present
- ✅ Hero subheadline: `KHUSUS kamu yang sudah punya aplikasi sekuritas...`
- ✅ Pain bullets: 4 items, all verbatim from brief §3, ending with `...`
- ✅ Reframe: `Dulu vs Sekarang` contrast, 3 negation bullets + `SEMUA SUDAH KAMI SIAPKAN! ✅`
- ✅ Reframe: 4 audience segments (3 spesifik + 1 universal)
- ✅ Transformation: "Bayangin Ini deh..." framing, 4 time beats (pagi → istirahat kantor → sore → akhir bulan), triple-negative close
- ✅ Pricing daily-cost framing: "Rp 3.300-an sehari... lebih murah dari 1 gelas kopi sachet di kantin"
- ✅ FAQ: 6 questions, conversational tone, emoji endings (😊 / 👍 / 🤝)
- ✅ Zero banned phrases (no `passive income`, `kaya raya`, `dijamin profit`, `rahasia` except branded)
- ✅ Scarcity: "50 pembeli pertama HARI INI" — verifiable

**Visual:**
- ✅ BVI hex applied: `--primary: #1A7A3C`, `--teal: #0F8A6E`, `--blue: #2A6EBB`, `--olive: #6B7C3A`, `--bg-sage: #EFF4EF`, `--bg-base: #FAFAFA`
- ✅ Typography: Plus Jakarta Sans (display) + Inter (body) + JetBrains Mono (mono)
- ✅ Mobile-first CSS, 320px base, @media 640/768/1024/1280
- ✅ Section alternating backgrounds: Hero white→ Testi sage → Pain white → Reframe sage → Package white → Deliverables white → Bonus sage → Transformation white → Why Now sage → Pricing dark → FAQ white → CTA Strip sage → Footer dark
- ✅ BVI visual brand tone: clean worksheet, not tech dashboard, no hype
- ✅ Hero CTA visible at 320px (hero padding 48px, mockup doc moves to row layout at 768px)

**Output:**
- ✅ `index.html` — full 14 sections, semantic HTML5, OG meta, JSON-LD Product schema
- ✅ `style.css` — separate, BVI palette via `:root` vars, mobile-first
- ✅ `lp-image-manifest.json` — 10 entries with keywords, orientations, alt text
- ✅ `mobile-qa-checklist.md` — PlanTrade-specific + standard 12-point
- ✅ `scalev-deploy-notes.md` — step-by-step deploy + ads integration
- ✅ Section 12: NO `<form>`, NO payment badges, NO `<input>` — CTA strip only ✓