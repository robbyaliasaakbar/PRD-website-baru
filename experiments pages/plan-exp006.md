# PLAN — Experiment 006

**Building a Self-Contained Invoice System Without a Database — File-Based Persistence with Local AI**

> **Location:** `/public_html/exp006.html`  
> **Plan file:** `PRD/plan-exp006.md`  
> **Date publish:** `2026-09-03` (modified: `2026-09-03`)  
> **Status:** `Stable` — samain kayak page lain (badge `status-stable` hijau) — Live Internal Tool di Hostinger  
> **Lang:** `en` (target: business owner + recruiter, multinational)  
> **Author:** Robby Aliasa Akbar

---

## 1. Objective

Answer the research question (ACC dari user):

> **Can an internal business tool with customer DB, live A4 preview, and selectable text-based PDF be built end-to-end using only local LLMs, without a traditional database — just .js files + 44 lines of PHP on shared hosting?**

This page documents the **internal counterpart of EXP 005**. Kalau EXP 005 adalah *etalase toko* (14 halaman public), EXP 006 adalah *mesin kasirnya* — 1 halaman admin `invoice/admin/data/invoice.html` (919 lines) yang mengelola customer, kalkulasi, dan PDF tanpa MySQL. Hanya `company.js + customers.js + save.php (44 lines)` + `localStorage` fallback di Hostinger file storage.

Reference: `exp005.html` (public site 14 pages, 44 pricelist items), `Website indeepcleaning id/public_html/invoice/admin/data` (5 file constraint), `template-exp000.html`.

---

## 2. Audience & Goals

**Primary (sesuai jawaban user):** Business owner (jasa cleaning/laundry/service) + Recruiter tim yang menilai kemampuan builder bikin tools operasional end-to-end.  
**Secondary:** Indie hacker anti-SaaS + AI experimenter yang mau lihat local LLM dipakai untuk internal tooling, bukan cuma landing page.

**Goals:**
1. Buktikan alur **end-to-end EXP 005 → 006**: `pricelist.js (44 items)` jadi single source of truth untuk public calculator DAN internal invoice — tidak duplikat harga.
2. Dokumentasikan **file-based persistence** sebagai alternatif MySQL yang lebih praktis untuk SMB di shared hosting (1 file `customers.js` bisa di-download via File Manager, 44 lines PHP).
3. Tunjukkan **PDF text-based selectable** (bukan screenshot) — rekening bisa di-copy, logo tetap masuk, size 45KB vs 850KB.
4. Tetap jujur soal failure (4 poin) & iteration (deterministic batching) — tone sama kayak EXP 005, tidak overclaim.

---

## 3. Styling & Stack (WAJIB plek ketiplek kayak pages lain — jawaban user No.15)

**User minta: samain kayak pages lain, terutama navbar & footer WAJIB mirip plek ketiplek.**

* **Stack:** `HTML5 + Tailwind CDN + Vanilla JS` (no build step) — sama persis.
* **Colors:** `ink #0C0C0C / paper #FFFFFF / accent #D70000` (portfolio) — **TIDAK pakai `#062C3D / #F4A100`** di page portfolio. Warna bisnis hanya muncul di *screenshot* & *code caption* sebagai evidence.
* **Fonts (portfolio):** `Manrope` (display), `Inter` (body), `JetBrains Mono` (code) — via Google Fonts.
* **Tokens & components to reuse plek:** `eyebrow`, `status-badge status-stable`, `border border-black/10 rounded-2xl`, `summary-callout`, `quote-block`, `detail-label`, `stat-tile`, `code-block`, `data-reveal`, `bg-grid`, `bg-dark-section` footer, `nav-link nav-active`, `mobile-menu`.
* **Navbar/Footer:** Copy-paste 1:1 dari `exp005.html` — hanya ubah `nav-active` ke `Experiments` dan `aria-current` ke `Experiment 006`. Tidak boleh ada perbedaan spacing, height (`h-20`), atau link `linkedin/reddit/instagram/email/whatsapp`.
* **Why:** Konsistensi ini yang bikin `experiments-list.html` keliatan satu koleksi. EXP 006 harus terasa satu keluarga dengan EXP 001-005.

---

## 4. SEO & Metadata (match exp005 pattern)

**Head:**
```html
<title>Experiment 006 — Building a Self-Contained Invoice System Without a Database — Robby Aliasa Akbar</title>
<meta name="description" content="Can an internal invoice tool with live A4 preview, customer autocomplete, and selectable text-based PDF be built with only local LLMs — without MySQL, just .js + 44 lines PHP? Evidence: 919 lines, Hostinger file storage.">
<link rel="canonical" href="https://robbyaliasaakbar.github.io/exp006.html">
<link rel="alternate" hreflang="en" href="https://robbyaliasaakbar.github.io/exp006.html">
<link rel="alternate" hreflang="x-default" href="https://robbyaliasaakbar.github.io/exp006.html">
<link rel="alternate" hreflang="id" href="https://robbyaliasaakbar.github.io/exp006.html">
<meta name="robots" content="index, follow, max-image-preview:large">
<meta property="og:type" content="article">
<meta property="og:title" content="Experiment 006 — Building a Self-Contained Invoice System Without a Database">
<meta property="og:description" content="File-based persistence vs MySQL: 919-line invoice.html, 44-line save.php, live A4 preview, selectable PDF. Built with local AI.">
<meta property="og:image" content="https://robbyaliasaakbar.github.io/images/experiments006/invoice-form-preview.webp">
<meta property="og:image:width" content="1440">
<meta property="og:image:height" content="878">
<meta name="twitter:card" content="summary_large_image">
<script type="application/ld+json"> Article (headline, description, image, datePublished 2026-09-03, keywords) + BreadcrumbList (Home > Experiments > Exp006) + FAQPage (3 Qs)
```

**Keywords:** `["local AI", "invoice generator", "jsPDF", "file-based database", "Hostinger", "vanilla JavaScript", "Tailwind CSS", "small business tools", "ORNITH", "llama.cpp"]`

**Sitemap & List updates (nanti setelah ACC review):**
* `sitemap.xml` add `<url><loc>.../exp006.html</loc><lastmod>2026-09-03</lastmod> + 4 images`
* `experiments-list.html` add card EXP 006 (position 6, Stable) + update `numberOfItems:6` in JSON-LD `ItemList` + add ListItem position 6

---

## 5. System Requirements (9 tiles — same grid as exp005)

Grid: `grid-cols-2 md:grid-cols-3` (same as exp005)

| Tile | Label | Value |
|------|-------|-------|
| 1 | CPU | `Intel Core i5 11400F` |
| 2 | RAM | `16GB DDR4 3200 MT/s` |
| 3 | GPU | `RX 6700 XT` |
| 4 | OS | `Ubuntu 26.04 LTS` |
| 5 | Inference Engine | `Llama.cpp` |
| 6 | Coding Tools | `OpenCode + Cline (VS Code)` |
| 7 | The Model That Helped Me | `ORNITH-1.0-35B-A3B-IQ4_NL-GGUF` |
| 8 | Frontend Stack | `Tailwind CDN + Vanilla JS + jsPDF 2.5.1` |
| 9 | Status | `STABLE` |

*Note: Host = Hostinger (100GB file storage — sesuai koreksi user, bukan 1TB). Kalau perlu 10th tile, taro di Evidence stat tiles aja.*

---

## 6. Screenshots — Gallery (4 images — jawaban user: cukup 4, sudah ada)

**Files verified di `public_html/images/experiments006/` (4 file, siap pakai):**
1. `invoice-form-preview.webp` (34KB) — `alt: Invoice generator desktop — left form (customer, items, totals) and right live A4 preview` — caption: `Form + Live A4 Preview`
2. `invoice-mobile-sticky.webp` (39KB) — `alt: Mobile view with sticky bar Preview | PDF | Print for admin on phone` — caption: `Mobile Sticky Bar — Admin on Phone`
3. `invoice-pdf-selectable.webp` (23KB) — `alt: Generated PDF with selectable text — BCA bank account can be highlighted and copied` — caption: `Text-Based PDF — Selectable & Copyable`
4. `invoice-filemanager-hostinger.webp` (7.1KB) — `alt: Hostinger File Manager showing customers.js file storage` — caption: `File-Based Storage — Hostinger File Manager`

**Layout (approved):**
* **Row 1:** 2 cols on lg: `invoice-form-preview` (span 2 cols kalau perlu) | `invoice-mobile-sticky` — atau 2+2 grid seimbang. Rekomendasi: `grid-cols-1 lg:grid-cols-2 gap-8` untuk 4 gambar (2x2).
* **Alternatif biar hero:** Row 1 full-width `invoice-form-preview` (karena ini hero evidence), Row 2 `3 cols` untuk 3 sisanya (mobile, pdf, filemanager).
* **Pilih:** **Opsi Hero** — Row 1: 1 big image (form-preview), Row 2: 3 small images (mobile, pdf, filemanager) — paling storytelling.
* Section wrapper: `bg-neutral-50 py-20`, eyebrow `Screenshots`, `data-reveal` staggered 0/120/240/360, `border rounded-2xl overflow-hidden bg-paper shadow-sm`, `loading="lazy" decoding="async"`.

**Alt text wajib include experiment context (SEO).**

---

## 7. Visualization — 2 Visuals (ACC user No.10)

### Chart 1: Bar Chart — Text-based PDF vs Screenshot PDF (js/charts.js)

* **Title:** `Why Text-Based PDF Won — Size, Selectability, and Logo`
* **Subtitle:** `The same invoice: generated via jsPDF text API (selectable) vs html2canvas screenshot (image). Local AI initially suggested screenshot — human corrected to text-based.`
* **Data untuk `js/charts.js` (`data-chart="bar"`):**
  ```json
  {
    "yMax": 900,
    "labels": ["Size (KB)", "Selectable", "Logo OK", "Copy Rekening"],
    "series": [
      {"label":"Text-based (jsPDF)","values":[45,100,100,100],"color":"#0C0C0C"},
      {"label":"Screenshot (html2canvas)","values":[850,0,100,0],"color":"#D70000"}
    ]
  }
  ```
* Fallback jika charts.js single series: **2 separate bar charts** stacked:
  * `Text-based: 45KB / Selectable Yes / Logo Yes / Copy Yes`
  * `Screenshot: 850KB / Selectable No / Logo Yes / Copy No`
* **Container:** `border border-black/10 rounded-2xl p-6 sm:p-10 bg-paper` with `data-reveal`
* **Note below:** `Text-based size is ~19x smaller and keeps bank account copyable — critical for business owner. Logo fix required canvas PNG conversion.`

### Diagram 2: Flow — File-Based Persistence (HTML/CSS only, no chart lib)

* **Title:** `File-Based Flow — No Database`
* **Visual:** Flex row `Input (form)` → `calcTotals()` → `updatePreview() (600ms)` → `localStorage (fallback)` + `fetch(save.php)` → `customers.js / company.js (Hostinger 100GB)` — pakai `arrow →` dan `badge` untuk `POST JSON {type, content}`.
* **Container:** `border border-black/10 rounded-2xl p-6 sm:p-8 bg-paper` with `data-reveal`, `font-mono text-xs`.

**Do not fabricate Lighthouse scores — this exp has no public Pagespeed, jadi chart-nya arsitektur, bukan performance.**

---

## 8. Experiment Details (10 subsections — follow exp005)

Section wrapper: `py-20 bg-neutral-50`, eyebrow `Experiment Details`, `space-y-12`

**All body copy in ENGLISH (same as exp005). Style: first-person, honest, deterministic.**

1.  **Problem**
    * Public site (EXP 005) solved marketing, but operations still manual: admin ngetik invoice di Word/Excel, customer data tercecer di WA, harga 44 item harus di-ingat, PDF tidak konsisten. Need internal tool yang bisa (a) autocomplete customer, (b) hitung otomatis dari pricelist yang sama, (c) preview A4 live, (d) PDF yang rekeningnya bisa di-copy customer — tanpa langganan SaaS dan tanpa setup MySQL di shared hosting.

2.  **Specifications (8 points — approved)**
    1. Single-file app: `invoice.html` 919 lines — `lg:grid-cols-12` (form 5 + preview 7 sticky), Tailwind CDN + jsPDF 2.5.1 + html2canvas 1.4.1 (loaded but not used for final PDF), Vanilla JS only, `noindex, nofollow` (internal only at `.../invoice/admin/data/invoice.html` — masked per user No.4).
    2. Company data: `company.js` 19 lines — `name, logo, address, whatsapp, email, rekening {bank, no, an}` — editable via `⚙️ Edit Company` modal (6 fields + logo upload base64 max 500KB), auto-save via `save.php?file=company` + `localStorage` fallback.
    3. Customer DB file-based: `customers.js` 21 lines — array `[{id, name, address, whatsapp}]` with 2 dummy samples (Budi & Siti — labeled dummy per user No.6), `datalist` autocomplete, dedup `Map(name|wa)` on `init()`, auto-grow via `save.php` (POST JSON) — storage di Hostinger 100GB file storage, download via File Manager.
    4. Backend: `save.php` 44 lines — `header JSON + CORS`, `file_get_contents('php://input')`, allowlist `['customers','company']`, `file_put_contents($filename, $content)` — no DB, no ORM, 1KB.
    5. Items logic: reads `indeepPricelist` (22 kategori → 44 items, same as EXP 005) — dependent dropdowns `kategori → ukuran`, harga auto, qty, diskon, plus dynamic extras `latex +40k, lepasPasang +20k/seat, lebar75 +20k/seat` — `calcTotals()` + `renderItems()`.
    6. Live preview: `preview-a4` 210mm, `setInterval(updatePreview, 600)` — header with logo, meta (No, Date, Due +1 day), 7-col table, payment box, signature — updates on every input change.
    7. PDF generation: **text-based only** `generateTextPDF()` via `jsPDF` — logo from DOM `canvas.toDataURL('image/png')` (fix CORS), all text via `pdf.text()` + `pdf.rect()` so `BCA 5771 •• 465` is selectable (masked in portfolio per user No.5, full in real PDF), filename `INV-YYYYMMDD-001 - Customer.pdf`.
    8. Mobile & print: `@media (max-width: 1024px)` table `overflow-x: auto`, `preview-a4` full-width, `mobile-sticky-bar` fixed bottom (`Preview | PDF | Print`), `@media print` hide `.no-print`, `window.print()` fallback.

3.  **Hypothesis**
    * Local LLM (ORNITH 35B) can generate this operational tool if prompts are deterministic per component (form, preview, PDF, save.php) and builder understands Hostinger file persistence vs DB trade-off. File-based is sufficient for SMB (<10k customers) and more maintainable than MySQL on shared hosting.

4.  **Experiment**
    * Built by prompting local model per batch: first `invoice.html` skeleton, then `company.js` modal, then `customers.js` autocomplete + `save.php`, then `calcTotals/updatePreview`, then `generateTextPDF` — each validated in small context window via OpenCode + Cline, Llama.cpp — not one-shot.

5.  **What Failed (4 points — ACC user No.11)**
    1. Logo CORS to PDF — `fetch(logo.png)` failed on `file://` and Hostinger path `../../../assets/img/logo.png` — had to grab from DOM `<img>` that already loaded, draw to canvas, `toDataURL('image/png')`, then `pdf.addImage()` — initial `fetch` + `FileReader` blob approach broke.
    2. Pricelist hallucination (continuation of EXP 005) — model invented prices for Kasur 200/180 etc. — fixed by forcing `indeepPricelist[kategori]` verbatim and disabling `ukuran` until `kategori` selected.
    3. Mobile table overflow — 7-col table broke on HP — fixed with `display:block; overflow-x:auto;` and `table thead/tbody display:table; width:100%` at 1024px.
    4. Dual persistence race — `localStorage` vs `save.php` out of sync after save — fixed with `Map` dedup `name|wa` on load + `Export manual` button fallback when `fetch` fails (500 or CORS).

6.  **Iteration**
    * Same pivot as EXP 005: stop relying 100% on AI. AI drafts syntax, human decides if `save.php` allowlist is safe, if `pdf.addImage` needs PNG conversion, if `pricelist.js` is single source. Added business insight: for small business, **file-based > MySQL** — one `customers.js` downloadable via File Manager beats phpMyAdmin setup.

7.  **Evidence (8 stat tiles — ACC user No.9, storage 100GB per user)**
    * `Total Files: 5` (1 html + 2 js + 1 php + 1 txt)
    * `Invoice Lines: 919` (invoice.html)
    * `Backend: 44 lines` (save.php)
    * `Storage: 100GB Hostinger` (file-based, no DB — per user correction)
    * `Preview Sync: 600ms` (setInterval)
    * `Customer DB: 2 → ∞` (auto-grow)
    * `PDF Mode: Text-based 100%` (selectable)
    * `Status: Stable` (internal live)
    * Use `stat-tile border border-black/10 rounded-xl p-5 bg-paper` — 8 tiles grid `2/3/4 cols`.

8.  **Result**
    * Tool is stable and live internally — admin can create invoice in <2 minutes, customer data auto-saves, PDF is 45KB selectable (vs 850KB image), and business owner can backup `customers.js` anytime via File Manager without DB knowledge. Not a SaaS replacement for enterprise, but sufficient for Jabodetabek SMB.

9.  **What I Learned**
    * Determinism matters even more for internal tools: `save.php` must have allowlist, `logo` must be PNG, `pricelist` must be verbatim. File-based persistence is underrated — local AI can generate CRUD without DB if builder understands file I/O trade-offs.

10. **Practical Implication (untuk business owner & recruiter — jawaban user No.13)**
    * For business owners: you don't need Rp200k/month SaaS — with local AI + Hostinger 100GB you own your invoice tool 100%, backup is one download. For recruiters: this proves builder can ship **both public marketing site and internal ops tool** with same local stack — end-to-end ownership, no cloud dependency.

---

## 9. FAQ (3 — tailored untuk business owner & recruiter)

```json
{
  "FAQPage": [
    {"Q":"Do I need a database for a small business invoice system?","A":"No — for <10k customers, a single customers.js file on Hostinger file storage (100GB) is enough. It's downloadable via File Manager, no phpMyAdmin. save.php (44 lines) handles POST {type, content} with allowlist. localStorage is fallback if fetch fails."},
    {"Q":"Why is selectable text in PDF important?","A":"Customer needs to copy bank account BCA 5771 •• 465 without retyping. Screenshot PDFs (850KB, image) don't allow copy. Text-based jsPDF (45KB) does — plus it's 19x smaller. Logo still embeds via canvas PNG conversion."},
    {"Q":"Can local AI really handle jsPDF and file save logic?","A":"Yes, but batched per component. One-shot generation hallucinated pricelist and broke logo CORS. Batched prompting (form → preview → pdf → save.php) with human validation produced 919 lines that work on shared hosting."}
  ]
}
```

Section: `py-20`, `eyebrow FAQ`, `h2 Frequently Asked Questions`, 3 `<details>` with `group-open:rotate-180`.

---

## 10. Configuration Template (2 code blocks — file-based + PDF)

**Block 1: save.php — 44 lines (the whole backend):**
```php
<?php
header('Content-Type: application/json');
header('Access-Control-Allow-Origin: *');
$input = file_get_contents('php://input');
$data = json_decode($input, true);
$allowed = ['customers','company'];
if (!in_array($data['type'], $allowed)) exit();
file_put_contents($data['type'].'.js', $data['content']);
echo json_encode(["status"=>"ok"]);
```
*Caption: 44 lines is the entire backend — no DB.*

**Block 2: customers.js — file-based DB:**
```js
const customersData = [
  { id: "CUST-001", name: "Budi Santoso", address: "Jl. Kemang Raya No. 12...", whatsapp: "081234567890" },
  { id: "CUST-002", name: "Siti Aminah", address: "Cluster Galaxy Blok B2...", whatsapp: "082112345678" }
];
// dummy sample — not real customers
// auto-updated via save.php?file=customers + localStorage Map dedup
```

Follow with 4 bullet explanations (allowlist, file storage 100GB, localStorage fallback, pricelist single source).

---

## 11. Non-Technical Summary (summary-callout — untuk business owner & recruiter)

Section: `py-20` with `summary-callout p-8 sm:p-12` + `data-reveal`.

**Heading:** `For those of you unable to read this data from technical standpoint, here is the conclusion`

**Body (5 paragraphs, plain English, honest — tone untuk business owner):**
1. This is not a public website — it's the internal cashier tool that comes after the website. If EXP 005 is the storefront, this is the cash register.
2. It lets the admin create an invoice in under 2 minutes: pick a returning customer (autocomplete), add items from the same 44-price list used on the website, see a live A4 preview, and download a PDF where the bank account can be copied.
3. It doesn't use a database. Customer data lives in one `customers.js` file on Hostinger (100GB) — you can download it anytime via File Manager, no database setup. A tiny 44-line PHP file saves it.
4. The PDF is text-based (45KB), not a screenshot (850KB) — so it's small and copyable, and the logo still appears. It was built with a local AI on consumer hardware, batched one component at a time.
5. It's not for a big enterprise with millions of rows, but for a small service business in Jabodetabek, it's sufficient, stable, and you own it 100% — no monthly SaaS fee.

---

## 12. Disclaimer — same position as exp005

**Position:** Immediately before `<footer class="bg-dark-section">`, after `Footer CTA`, `py-16` with `border-t border-black/10`.

**Copy (adapted for internal tool):**

> **Disclaimer — A Note on Results**
>
> From a purely objective standpoint, this is not an enterprise-grade ERP. Given it was built with a 35B local model on consumer hardware and runs on shared hosting file storage, I consider it sufficient and practical for real small-business invoicing. The trade-off (file-based vs MySQL) is intentional for maintainability at this scale — with known limits if customer count grows beyond file-based practicality.

**Implementation:** same as exp005 — `<section class="py-16 border-t border-black/10 bg-white">` → `max-w-3xl` → `eyebrow Disclaimer` → `blockquote quote-block`.

---

## 13. Footer CTA (copy plek dari exp005 — wajib sama)

```html
<section class="pb-24 lg:pb-32">
  <div class="mx-auto max-w-6xl px-6 lg:px-8">
    <div class="flex flex-col sm:flex-row items-center justify-between gap-6 border-t border-black/10 pt-10" data-reveal>
      <a href="experiments-list.html" class="arrow-link font-display text-sm uppercase"><span aria-hidden="true">&larr;</span> Back to Experiments</a>
      <a href="contact.html" class="arrow-link font-display text-sm uppercase">Have a problem worth exploring? <span aria-hidden="true">-&gt;</span></a>
    </div>
  </div>
</section>
```

---

## 14. File & Asset Checklist

* **New page:** `public_html/exp006.html` (from `template-exp000.html`, not from scratch) — must keep navbar/footer plek ketiplek
* **Images (4 — verified ready):** `public_html/images/experiments006/invoice-form-preview.webp`, `invoice-mobile-sticky.webp`, `invoice-pdf-selectable.webp`, `invoice-filemanager-hostinger.webp` — verify `alt`, `loading="lazy"`
* **Update (AFTER user review):** `public_html/experiments-list.html` — add 6th card EXP 006 (Stable) + JSON-LD `numberOfItems:6` + ListItem position 6 with image `invoice-form-preview.webp`
* **Update (AFTER user review):** `public_html/sitemap.xml` — add `<url>` for `exp006.html` with 4 `<image:image>` entries, `lastmod 2026-09-03`
* **Content source:** `/Website indeepcleaning id/public_html/invoice/admin/data` (read-only — do NOT modify)
* **No new deps:** Keep `cdn.tailwindcss.com`, `fonts.googleapis.com`, `js/main.js`, `js/charts.js`, `css/style.css` only (+ `jspdf` is only inside business site, not portfolio)
* **Corrections applied per user:** Storage 100GB (not 1TB), rekening masked `5771 •• 465`, URL masked `.../invoice/admin/data/invoice.html (internal, noindex)`, audience business owner + recruiter, styling Opsi A

---

## 15. Build Steps (after this plan ACC)

1. **User ACCs this plan** (`plan-exp006.md` ini) — user sudah bilang "bikin plan dulu aja. nanti gue kasih arahan lanjutannya" → jadi STOP after plan, tunggu arahan.
2. Generate `exp006.html` from `template-exp000.html` → fill sections 1–12 above, keep semantic HTML, responsive, `title`/`meta`/`OG`/`JSON-LD`, same navbar/footer.
3. Local verify: `python -m http.server` + check 4 images load, chart renders, no horizontal overflow on mobile.
4. **JANGAN update `experiments-list.html` + `sitemap.xml` dulu** — tunggu user review `exp006.html` (sesuai No.18).
5. User reviews built HTML, then we update list + sitemap to publish.

---

**Ready for your approval, bro.**  
Plan ini sudah sesuaikan 100% dengan 18 jawaban lu (termasuk 4 screenshot ready, 100GB, masked rekening, styling plek). Bilang `gas` kalau udah oke — gue langsung generate `exp006.html` nya. No code sampai lu bilang gas.
