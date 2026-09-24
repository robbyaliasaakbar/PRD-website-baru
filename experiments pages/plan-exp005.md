# PLAN — Experiment 005

**Building a Full Feature Services Business Website Using Only Local AI**

> **Location:** `/public_html/exp005.html`  
> **Plan file:** `PRD/plan-exp005.md`  
> **Date publish:** `2026-09-01` (modified: `2026-09-01`)  
> **Status:** `Stable` (Live via Hostinger — https://indeepcleaningid.site)  
> **Lang:** `en` (target: multinational)  
> **Author:** Robby Aliasa Akbar

---

## 1. Objective

Answer the research question:

> **Can a full-feature services business website — with before-after slider, scroll animations on every page, a 44-item cost-estimation calculator, and full SEO optimization — be built end-to-end using only local open-source LLMs (no cloud AI)?**

This page documents the *real* website (`indeepcleaning id` — 14 pages, 28 before-after images, 44 pricelist items, 3 vanilla JS files, 1306 CSS lines, 15 animation types) as a technical case study, consistent with `exp001–004`.

Reference pages used: `exp001.html` (MoE 12GB VRAM), `exp002.html` (256K prefill), `exp003.html` (agent workflow), `exp004.html` (digital employees) + business site `Website indeepcleaning id/public_html`.

---

## 2. Audience & Goals

**Primary:** SMB owners, indie hackers, frontend devs who want to know if local AI can replace agency/cloud for production websites.  
**Secondary:** AI experimenters evaluating `ORNITH` / `llama.cpp` for code generation.

**Goals:**
1. Prove full-feature business site is feasible with local AI alone (deterministic prompting, not “AI-First” blind trust).
2. Document *real* failures (33 revisions, 4M context window split across sessions) and the iteration that fixed them.
3. Provide reusable evidence: gallery, calculator logic, Lighthouse scores, stat tiles.
4. Keep SEO / a11y / performance honest — no overclaim. Add disclaimer before footer.

---

## 3. Styling & Stack (MUST MATCH PORTFOLIO)

* **Stack:** `HTML5 + Tailwind CDN + Vanilla JS` (no build step) — same as portfolio.
* **Colors:** `ink #0C0C0C / paper #FFFFFF / accent #D70000` (portfolio) — *business site itself uses `#062C3D / #F4A100 / Poppins+Inter`, but exp005 page stays in portfolio design system for consistency.*
* **Fonts (portfolio):** `Manrope` (display), `Inter` (body), `JetBrains Mono` (code) — loaded via Google Fonts.
* **Tokens & components to reuse:** `eyebrow`, `status-badge status-stable`, `border border-black/10 rounded-2xl`, `summary-callout`, `quote-block`, `detail-label`, `stat-tile`, `code-block`, `data-reveal` (IntersectionObserver), `bg-grid`, `bg-dark-section` footer.
* **Business site design is *evidence*, not the page theme.** Screenshots + code snippets show its own design system.

**Why:** Keeps EXP005 visually coherent with EXP001–004 in `experiments-list.html`. Audience sees portfolio as unified case-study collection.

---

## 4. SEO & Metadata (match exp004 pattern)

**Head:**
```html
<title>Experiment 005 — Building a Full Feature Services Business Website Using Only Local AI — Robby Aliasa Akbar</title>
<meta name="description" content="Can a full-feature business website with before-after slider, page animations, 44-item cost calculator, and full SEO be built end-to-end using only local open-source LLMs? Real build evidence from 14 pages, 28 images, 3 JS files.">
<link rel="canonical" href="https://robbyaliasaakbar.github.io/exp005.html">
<link rel="alternate" hreflang="en" href="https://robbyaliasaakbar.github.io/exp005.html">
<link rel="alternate" hreflang="x-default" href="https://robbyaliasaakbar.github.io/exp005.html">
<link rel="alternate" hreflang="id" href="https://robbyaliasaakbar.github.io/exp005.html">
<meta name="robots" content="index, follow, max-image-preview:large">
<meta property="og:type" content="article">
<meta property="og:title" content="Experiment 005 — Building a Full Feature Services Business Website Using Only Local AI">
<meta property="og:description" content="Full build evidence: 14 pages, slider 14 pairs, 44-item calculator, 3 vanilla JS files, 1306 CSS lines, Lighthouse scores.">
<meta property="og:image" content="https://robbyaliasaakbar.github.io/images/experiments005/homepage.webp">
<meta property="og:image:width" content="1440">
<meta property="og:image:height" content="878">
<meta name="twitter:card" content="summary_large_image">
<script type="application/ld+json"> Article (headline, description, image, datePublished 2026-09-01, keywords) + BreadcrumbList (Home > Experiments > Exp005) + FAQPage (3 Qs)
```

**Keywords:** `["local AI", "open-source LLM", "business website", "before-after slider", "cost calculator", "Tailwind CSS", "vanilla JavaScript", "SEO optimization", "llama.cpp", "ORNITH"]`

**Sitemap & List updates:**
* `sitemap.xml` add `<url><loc>.../exp005.html</loc><lastmod>2026-09-01</lastmod> + 5 images`
* `experiments-list.html` add card EXP005 (position 5, Stable) + update `numberOfItems:5` in JSON-LD `ItemList`

---

## 5. System Requirements (9 tiles — 1 tile for DE tools)

Grid: `grid-cols-2 md:grid-cols-3` (same as exp004)

| Tile | Label | Value |
|------|-------|-------|
| 1 | CPU | `Intel Core i5 11400F` |
| 2 | RAM | `16GB DDR4 3200 MT/s` |
| 3 | GPU | `RX 6700 XT` |
| 4 | OS | `Ubuntu 26.04 LTS` |
| 5 | Inference Engine | `Llama.cpp` |
| 6 | Coding Tools | `OpenCode + Cline (VS Code)` |
| 7 | The Model That Helped Me | `ORNITH-1.0-35B-A3B-IQ4_NL-GGUF` |
| 8 | Frontend Stack | `Tailwind CDN + Vanilla JS` |
| 9 | Status | `STABLE` |

*If needed, 10th tile: `Host` = `Hostinger` (live). Keep 9 primary to match exp004.*

---

## 6. Screenshots — Gallery (5 images, 2 rows)

**Files already in `public_html/images/experiments005/` (verified):**
1. `homepage.webp` — `alt: Homepage layout of InDeepCleaningID — hero, badge, CTA, services grid` (layout: homepage)
2. `gallery.webp` — `alt: Gallery layout with 14 before-after sliders and filter buttons` (gallery)
3. `estimation-calculator.webp` — `alt: Cost estimation calculator UI with 22 service categories and 44 price items` (calculator ui/ux)
4. `seo-score-pagespeed-mobile.webp` — `alt: PageSpeed Insights score on mobile — performance 65, accessibility 86, best practices 100, SEO 100` (pagespeed mobile)
5. `seo-score-pagespeed-desktop.webp` — `alt: PageSpeed Insights score on desktop — performance 88, accessibility 86, best practices 100, SEO 100` (pagespeed desktop)

**Layout (as approved):**
* **Row 1 (3 cols on lg):** homepage | gallery | estimation-calculator (each `border rounded-2xl overflow-hidden bg-paper shadow-sm`, `loading="lazy"`, `data-reveal` staggered 0/120/240)
* **Row 2 (2 cols):** seo-mobile | seo-desktop side-by-side with label `Pagespeed — Mobile` / `Pagespeed — Desktop` below image, `data-reveal`
* Section wrapper: `bg-neutral-50 py-20`, eyebrow `Screenshots`

**Alt text must include experiment context (SEO). Provide `width`/`height` if known, else omit but keep `loading="lazy" decoding="async"`.**

---

## 7. Visualization — Bar Chart (Option B1 with real data)

**User chose B1 but with concrete Pagespeed data. Do NOT fabricate. Use 2 grouped bar charts (Mobile vs Desktop) for 5 metrics.**

### Chart 1: Pagespeed Scores — Mobile vs Desktop (Grouped Bar)

* **Title:** `Lighthouse Pagespeed Scores — Mobile vs Desktop (pagespeed.web.dev)`
* **Subtitle:** `Real scores from the live site (14 pages). Performance is the only metric with clear headroom.`
* **Data format for `js/charts.js` (`data-chart="bar"` with grouped series):**
  ```json
  {
    "yMax": 100,
    "labels": ["Performance","Accessibility","Best Practices","SEO","Agentic Explorer"],
    "series": [
      {"label":"Mobile","values":[65,86,100,100,50],"color":"#0C0C0C"},
      {"label":"Desktop","values":[88,86,100,100,100],"color":"#D70000"}
    ]
  }
  ```
* If `charts.js` only supports single series bar, fallback to **two separate bar charts** stacked vertically:
  * `Mobile — 65 / 86 / 100 / 100 / 50`
  * `Desktop — 88 / 86 / 100 / 100 / 100`
  Each with `data-y-max="100"` and caption `Lower is not failure — mobile performance reflects image weight and needs further optimization.`
* **Legend:** Must show `Mobile (ink)` vs `Desktop (accent)`. Add note: `Agentic Explorer is an experimental diagnostic, not a Lighthouse core metric.`
* **Container:** `border border-black/10 rounded-2xl p-8 sm:p-10 bg-paper` with `data-reveal`

### Optional secondary tiny stat row below chart (3 tiles):
* `Mobile Performance: 65` / `Desktop Performance: 88` / `SEO Both: 100` — emphasizes gap honestly.

**Do not claim 100 across the board. The honesty is the evidence.**

---

## 8. Experiment Details (9 subsections + Specifications)

Section wrapper: `py-20 bg-neutral-50`, eyebrow `Experiment Details`, `space-y-12`

**All body copy in ENGLISH (user request). Style: first-person, honest, deterministic tone like exp004.**

1.  **Problem**
    * Business needed a home-service website that explains services, proves results (before-after), gives transparent pricing via calculator, and ranks (SEO) — without cloud AI / agency cost.

2.  **Specifications (8 points — approved list)**
    1. Multi-page architecture: 14 pages (7 main: index, pricing-estimator, gallery, coverage, about, contact, services/index; 5 detail: cuci-kasur, cuci-sofa, cuci-karpet-gordyn, interior-mobil, baby-stuff; 1 landingpage/v2 for Meta/Google Ads conversion; 1 invoice/admin for data control).
    2. Before-after slider: 14 pairs (28 .webp), draggable `clip-path: inset()`, divider + handle, `data-slider`, filter `data-filter` (all/kasur/sofa/mobil/baby/karpet).
    3. Scroll animations per page: `reveal / reveal-left/right/scale` via `IntersectionObserver`, `stagger-1..8`, `counter data-counter="5000"`, `magnetic-btn`, `tilt-card` 3D, `navbar-glass` hide-on-scroll, `float-organic`, `pulse-ring`.
    4. Cost calculator: 22 service categories → 44 price items (`indeepPricelist`), dependent dropdowns (`service-type` → `item-size`), dynamic extras (Kasur Latex +40k, Sofa Lepas Pasang +20k/seat, Lebar >75cm +20k/seat), result + WA deep-link (`encodeURIComponent`).
    5. Separate data source: `assets/data/pricelist.js` + `assets/pricelist.json` for easy price updates without touching UI.
    6. SEO complete: canonical, OG `1200x630`, Twitter, hreflang, `preload Hero.webp`, JSON-LD `LocalBusiness + BreadcrumbList + Service + OfferCatalog + GeoCircle 50km`, `sitemap.xml`, `robots.txt`, `optimize-meta.sh` / `add-schema.sh`.
    7. Responsive & performant: `assets/css/style.css` 1306 lines, 15 animation types, 3 vanilla JS files (main 487, estimator 214, slider 76) — no heavy framework.
    8. Consistent brand: `primary #062C3D / accent #F4A100` + `Poppins (heading) + Inter (body)` (portfolio page itself stays in `ink/paper/accent + Manrope/Inter`).

3.  **Hypothesis**
    * A local open-source LLM (ORNITH 35B IQ4_NL) is sufficient to generate production vanilla JS + Tailwind syntax **if** prompts are deterministic and the builder understands HTML/Tailwind/JS/SEO well enough to judge correctness. AI writes syntax; human judges syntax.

4.  **Experiment**
    * Built the 14-page site by prompting local AI per component (slider, estimator, navbar, reveal), per batch to keep context quality. Used OpenCode + Cline in VS Code, inference via Llama.cpp.

5.  **What Failed (8 concrete points from user, expand in English, no fabrication)**
    1. Too many revisions — 33 revisions, ~4M total context split across sessions; single-shot generation collapsed.
    2. `slider.js` before-after logic is complex for local LLM; requires fully deterministic, zero-ambiguity instructions (clip-path direction, divider %, handle sync, mouse vs touch).
    3. Initial calculator did not match real `pricelist.js` (44 items) — hallucinated prices/sizes.
    4. First designs for all pages were too templated / “safe” — did not match brand preference.
    5. Internal linking needed separate optimization pass (not auto-SEO).
    6. Initial SEO was too basic; required manual deeper understanding to optimize beyond baseline meta.
    7. Initial `sitemap.xml` was not readable in Google Search Console; required manual fix.
    8. Animations must be revised piece-by-piece, batched — cannot be done all at once without losing context window / output quality.

6.  **Iteration (pivot — English, in user’s voice)**
    * Do not rely 100% on AI. Need real understanding of HTML, Tailwind CSS, Vanilla JS, SEO, chart types, styling and business needs. AI only helps write syntax — it does not decide whether syntax is correct. Shifted from “AI-First generate all” to “Deterministic-First: AI drafts → human validates → batch per feature” to preserve context quality.

7.  **Evidence (8 stat tiles — grid 2/3/4 cols)**
    * `Total Pages: 14` (7 main + 5 detail + 1 landing + 1 admin)
    * `Before-After Pairs: 14` (28 images)
    * `Calculator Coverage: 44 items` (22 categories, matches pricelist)
    * `Custom JS Files: 3` (estimator.js, main.js, slider.js)
    * `Custom CSS: 1306 lines` / `15 animation types`
    * `SEO Mobile: 65 Perf / 86 A11y / 100 BP / 100 SEO` (or concise `SEO 100 / Perf 65 mobile`)
    * `SEO Desktop: 88 Perf / 86 A11y / 100 BP / 100 SEO`
    * `Status: Stable (Hostinger Live)` / `Revisions: 33` (pick 8, keep `Manageable` style like exp001)
    * Use `stat-tile border border-black/10 rounded-xl p-5 bg-paper` pattern. Labels in `text-[0.62rem] tracking-[0.14em]`.

8.  **Result**
    * The site is live, stable, with 14 pages, 14 sliders, 44-item calculator, 5-filter gallery, full SEO schemas, and honest Lighthouse scores (100 SEO, 65/88 perf). Not perfect, but sufficient given local model + consumer hardware.

9.  **What I Learned**
    * Local AI capability = prompting determinism + builder’s domain knowledge. Model is engine; builder’s understanding is chassis. Batch work, keep context small.

10. **Practical Implication**
    * If you want to replicate: start with one component (slider or calculator), master deterministic prompts, then batch. Don’t ask AI to build 14 pages in one go. Learn Tailwind/JS/SEO first — AI accelerates, not replaces.

---

## 9. FAQ (3 — approved)

```json
{
  "FAQPage": [
    {"Q":"Can local AI really generate a production estimator with 44 price rules?","A":"Yes, but only with deterministic prompts and manual validation. Initial calc hallucinated prices; splitting pricelist.js as single source of truth fixed it. AI drafted the dependent dropdowns and WA link — human ensured 44 items matched."},
    {"Q":"What broke when you let AI build the slider fully?","A":"Clip-path direction, divider sync, and touch vs mouse handlers. Local LLM needed zero-ambiguity instructions for inset() percentages and event handling. One-shot full-page generation failed — batched per-component succeeded."},
    {"Q":"How did you get SEO 100 without cloud tools?","A":"Manual SEO layer: canonical, OG, 3 JSON-LD schemas, preload, sitemap fix for Search Console, internal linking pass. AI gave baseline meta; human optimized beyond template. Pagespeed still shows perf headroom (65 mobile / 88 desktop)."}
  ]
}
```

Section: `py-20`, `eyebrow FAQ`, `h2 Frequently Asked Questions`, 3 `<details>` with `group-open:rotate-180`.

---

## 10. Configuration Template (code-block like exp001)

Show a *short, readable* snippet — not full file — to prove determinism. Choose `estimator.js` dependent dropdown logic (approved):

```js
// assets/js/estimator.js — dependent dropdown (deterministic)
function updateSizeDropdown(service) {
  sizeSelect.innerHTML = '<option>-- Pilih Ukuran / Tipe --</option>';
  if (!service || !priceData[service]) { sizeSelect.disabled = true; return; }
  priceData[service].forEach(item => {
    const opt = document.createElement('option');
    opt.value = item.id;
    opt.textContent = `${item.nama_barang} (${item.ukuran}) — Rp ${item.harga.toLocaleString('id-ID')}`;
    sizeSelect.appendChild(opt);
  });
  sizeSelect.disabled = false;
}
```

*Caption below:* “Do not copy blindly — adapt to your pricelist structure. This pattern keeps data (`pricelist.js`) separate from UI.”

Follow with 4–5 bullet explanations (like exp001’s 8 points, but shorter for this exp).

---

## 11. Non-Technical Summary (summary-callout)

Section: `py-20` (or `bg-neutral-50 py-20` alternating). Inside `summary-callout p-8 sm:p-12` with `data-reveal`.

**Heading:** `For those of you unable to read this data from technical standpoint, here is the conclusion`

**Body (5 paragraphs, plain English, honest):**
1. This was not a demo — it’s a live business site with 14 pages serving real customers.
2. Local AI helped write the code for sliders, animations and calculator, but it took 33 revisions to get it right.
3. The site now shows real before-after proofs (14 pairs), gives instant price estimates (44 items), and is fully indexed for search.
4. It works stably on consumer hardware with a 35B local model; you don’t need cloud AI to build it.
5. It’s not flawless — performance can still be improved — but it’s sufficient and ready for business, and it proves local AI is practical when used deterministically.

---

## 12. Disclaimer — NEW SECTION (directly before footer)

**Position:** Immediately before `<footer class="bg-dark-section">`, after `Footer CTA` section. Full-width, `py-16` with `border-t border-black/10` or `bg-neutral-50`.

**User original:**
> "I wouldn't say this is the optimal result from a purely objective standpoint. However, considering the model and hardware used, I feel personally and subjectively that the result is sufficient. That said, there are certainly areas that could be further optimized, particularly regarding website performance."

**Polished copy (recommended — preserves humility, no overclaim):**

> **Disclaimer — A Note on Results**
> 
> From a purely objective standpoint, I wouldn’t claim this is the optimal outcome. Given the local model (ORNITH 35B IQ4_NL) and consumer-grade hardware used, I personally consider the result to be sufficient and practical for real business use. That said, there is clear room for further optimization — particularly in overall website performance (65 mobile / 88 desktop) and image weight.

**Alternative shorter (if space tight):**
> Objectively, this isn’t the ceiling of what’s possible. Subjectively, for a 35B local model on consumer hardware, the result is sufficient and production-ready — with known headroom to improve performance.

**Implementation:** `<section class="py-16 border-t border-black/10 bg-white">` → `<div class="mx-auto max-w-6xl px-6 lg:px-8">` → `<div class="max-w-3xl" data-reveal>` → `<p class="eyebrow">Disclaimer</p>` → `<blockquote class="quote-block mt-6 text-sm sm:text-base text-neutral-600 leading-relaxed">`… polished copy …`</blockquote>`

*Tone: humble, specific, references 65/88 perf to prove honesty.*

---

## 13. Footer CTA (same as exp004)

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

* **New page:** `public_html/exp005.html` (from `template-exp000.html`, not from scratch)
* **Images (5):** `public_html/images/experiments005/homepage.webp`, `gallery.webp`, `estimation-calculator.webp`, `seo-score-pagespeed-mobile.webp`, `seo-score-pagespeed-desktop.webp` — verify `alt`, `loading="lazy"`, `width/height` if available
* **Update:** `public_html/experiments-list.html` — add 5th card + JSON-LD `numberOfItems:5`
* **Update:** `public_html/sitemap.xml` — add `<url>` for `exp005.html` with 5 `<image:image>` entries, `lastmod 2026-09-01`
* **Content source:** Business site at `/Website indeepcleaning id/public_html` (read-only reference — do NOT modify)
* **No new deps:** Keep `cdn.tailwindcss.com`, `fonts.googleapis.com`, `js/main.js`, `js/charts.js`, `css/style.css` only

---

## 15. Build Steps (after plan approval)

1. **User ACCs this plan** (this file).
2. Generate `exp005.html` from `template-exp000.html` → fill sections 1–12 above, keep semantic HTML, responsive, `title`/`meta`/`OG`/`JSON-LD`.
3. Verify locally: `python -m http.server` + Lighthouse check matches 65/86/100/100 mobile, 88/86/100/100 desktop values (do not edit scores).
4. Update `experiments-list.html` + `sitemap.xml`.
5. User reviews built HTML, then we go to `exp006` (invoice) batch.

---

**Ready for your approval, bro.**  
If you want the disclaimer polished differently, tell me which version (long honest vs short) — I’ll swap it verbatim before BUILDING. No code until you say “gas”.
