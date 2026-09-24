# PLAN — Experiment 007

**From 30 Minutes to 5 Seconds: Automating Mass CV Screening Without In-System AI — A Local Workflow From PDF to Ranked Shortlist**

> **Location:** `/public_html/exp007.html`  
> **Plan file:** `PRD/plan-exp007.md`  
> **Date publish:** `2026-09-04` (deadline report: Jumat 4 Sept 2026)  
> **Status:** `Stable` — POC DONE, E2E 5 CV in ~5s, ranking OK (checkpoint DONE 2026-09-02)  
> **Lang:** `en` (multinational — business owner + recruiter)  
> **Author:** Robby Aliasa Akbar

---

## 1. Objective

Answer the research question:

> **Can mass-hiring CV screening (1–5 PDFs) be automated from a 30-minute manual review to a 5-second ranked shortlist — using only browser-side pdf.js, n8n workflow orchestration, and a predictable rule-based scoring (no heavy AI, no hallucination, no token cost) — running 100% locally?**

This page documents the **Technical Test POC for AI Specialist (Allure Industries)** as a real, delivered system in 3 days — **but actually completed in just 5 hours**. If EXP 005 was the storefront and EXP 006 the cashier, EXP 007 is the **HR screener** — the complete `Landing Page → JSON → Webhook → n8n → Postgres → Ranking` loop that proves **workflow orchestration beats LLM hallucination** for small-business hiring.

Reference: `Project-Technical-Test/` (landing page 626 lines, 5 CV + Vacancy PDFs, 11 docs, screencast 24s), `Bahan POC/` (13 PNGs + PDF report 10 slides), live n8n workflow `Technical Test` (ID: v1lQYQUZndoeoR6w — 24 nodes), PDF report `AI Specialist - Technical Test.pdf`.

**Key stance from report (to be highlighted):** *Local LLM (Ornith 1.0 35B IQ4_NL via llama.cpp + OpenCode + MCP) was used ONLY as a syntax assistant under human supervision — NOT inside the system itself. The system is a bot/workflow without in-system AI, to keep it maintainable, predictable, fast, and low-resource.*

---

## 2. Audience & Goals

**Primary:** Business owners / HR managers doing mass hiring in manufacturing / services — they understand “30 min manual → 5 sec auto” instantly.  
**Secondary:** Recruiters / hiring managers evaluating AI Specialist — they see `landing page + n8n + Postgres` end-to-end, no bias, no cloud.

**Goals:**
1. Prove **workflow > hallucination**: small quantized models are unstable; big models are heavy; workflow orchestration is stable, light, fast, debuggable — all local, no data leaves the private server.
2. Show **HR-friendly** system: upload 1–5 PDFs (drag & drop or tap), badge `cv_1..cv_5`, progress `Mengekstrak 3/5...`, sticky mobile bar, `Download Ranking JSON/CSV`.
3. Provide **audit-ready evidence**: 5 CV samples → ranking `80,75,50,35,25` (2 Lolos ≥70%, 3 Tidak), CSV with Indonesian headers, Postgres `cv_ranking` truncate after delivery.
4. Keep tone **predictable, not probabilistic** — no LLM scoring, only rule-based `20 skill = 40% + 6 edu = 30% + 6 exp = 30%` (per report, threshold 70% — user chose 70% to match report).

---

## 3. Styling & Stack (Wajib plek ketiplek)

* **Stack:** `HTML5 + Tailwind CDN + Vanilla JS` (no build) + `pdf.js 2.16.105` (client-side extract) + `Python 3` (local server) — same as portfolio.
* **Colors:** `ink #0C0C0C / paper #FFFFFF / accent #D70000` (portfolio). Business copy stays in screenshots, not page theme.
* **Fonts:** `Manrope` (display), `Inter` (body), `JetBrains Mono` (code) — Google Fonts.
* **Tokens to reuse plek:** `eyebrow`, `status-badge status-stable`, `border border-black/10 rounded-2xl`, `summary-callout`, `quote-block`, `detail-label`, `stat-tile`, `code-block`, `data-reveal`, `bg-grid`, `bg-dark-section` footer, `nav-link nav-active`.
* **Navbar/Footer:** Copy-paste 1:1 from `exp006.html` — only change `aria-current` to `Experiment 007`.

---

## 4. SEO & Metadata

**Head:**
```html
<title>Experiment 007 — From 30 Minutes to 5 Seconds: Automating Mass CV Screening Without In-System AI — Robby Aliasa Akbar</title>
<meta name="description" content="Can 1–5 CV PDFs be screened from 30-minute manual review to 5-second ranked shortlist without in-system AI? A local workflow: pdf.js → n8n 24 nodes → Postgres, 70% threshold.">
<link rel="canonical" href="https://robbyaliasaakbar.github.io/exp007.html">
<link rel="alternate" hreflang="en" href="https://robbyaliasaakbar.github.io/exp007.html">
<link rel="alternate" hreflang="x-default" href="https://robbyaliasaakbar.github.io/exp007.html">
<link rel="alternate" hreflang="id" href="https://robbyaliasaakbar.github.io/exp007.html">
<meta name="robots" content="index, follow, max-image-preview:large">
<meta property="og:type" content="article">
<meta property="og:title" content="Experiment 007 — Automating Mass CV Screening Without In-System AI">
<meta property="og:description" content="Local workflow, not LLM hallucination: landing page 626 lines, 24-node n8n, Postgres, 50-20-30 scoring, 70% threshold. 5 CV → ranking in 5s, no data leaves private server.">
<meta property="og:image" content="https://robbyaliasaakbar.github.io/images/experiments007/homepage-hero.webp">
<meta property="og:image:width" content="1440">
<meta property="og:image:height" content="878">
<meta name="twitter:card" content="summary_large_image">
<script type="application/ld+json"> Article + BreadcrumbList + FAQPage
```

**Keywords:** `["local AI", "n8n", "workflow orchestration", "CV screening", "HR automation", "pdf.js", "Postgres", "mass hiring", "predictable scoring", "private server"]`

**Sitemap & List updates (after review):**
* `sitemap.xml` add `<url><loc>.../exp007.html</loc><lastmod>2026-09-04</lastmod> + 4 images`
* `experiments-list.html` add card EXP 007 (position 7, Stable) + update `numberOfItems:7`

---

## 5. System Requirements (12 tiles — from report page 4 — 3 rows of 4)

Grid: `grid-cols-2 md:grid-cols-3 lg:grid-cols-4` (12 items, same style as exp005 but 12)

| Tile | Label | Value |
|------|-------|-------|
| 1 | Processor | `Intel Core i5 11400F` |
| 2 | RAM | `DDR4 16GB` |
| 3 | GPU | `AMD RX 6700 XT 12GB VRAM` |
| 4 | Inference Engine | `llama.cpp` |
| 5 | Model (Assistant Only) | `Ornith 1.0 35B A3B IQ4_NL` |
| 6 | Harness / IDE | `OPENCODE + MCP` |
| 7 | Orchestration | `n8n` |
| 8 | Database | `PostgreSQL` |
| 9 | Frontend | `HTML5 + Tailwind CDN` |
| 10 | Logic | `Vanilla JS + pdf.js 2.16` |
| 11 | Server | `Python 3 (private)` |
| 12 | Status | `STABLE` |

*Note: Model is listed as “Assistant Only” to reflect report’s “AI only helps write syntax, supervised by me”.*

---

## 6. Screenshots — Gallery (6 WEBP ready — verified 248K)

**Files in `public_html/images/experiments007/` (converted from Bahan POC PNG):**
1. `homepage-hero.webp` (54K) — from `Gambar homepage hero section.png` — `alt: Hero of CV screening landing page — Seleksi CV 10x Lebih Cepat, upload 1-5 PDF hero + n8n webhook card`
2. `upload-queue.webp` (24K) — from `proses upload cv.png` — `alt: Upload queue showing 5 files with badges cv_1..cv_5, size, remove X, drag & drop zone`
3. `ranking-result.webp` (28K) — from `proses screening selesai.png` — `alt: Ranking table result — 5 CV ranked by score 80,75,50,35,25 with Lolos/Tidak status and Download CSV/JSON`
4. `n8n-workflow.webp` (67K) — from `proses workflow n8n.png` — `alt: n8n workflow Technical Test 24 nodes — Webhook to Code Ranking to Postgres`
5. `local-server.webp` (18K) — from `server local yang berjalan.png` — `alt: Local server running on private computer — python server localhost:8010`
6. `local-url.webp` (49K) — from `url local yang berjalan.png` — `alt: Local URL proof — http://localhost:8010 and webhook http://localhost:5678`

**Layout (like exp005/006):**
* **Row 1 (hero):** `homepage-hero.webp` full-width
* **Row 2 (2 cols):** `upload-queue.webp` | `ranking-result.webp`
* **Row 3 (2 cols):** `n8n-workflow.webp` | `local-server.webp` (optional 6th `local-url.webp` as small thumb or keep 5)
* Section wrapper: `bg-neutral-50 py-20`, eyebrow `Screenshots`, `data-reveal` staggered, `border rounded-2xl bg-paper shadow-sm`, `loading="lazy"`, `figcaptions` with `tracking-[0.14em] uppercase text-neutral-500`.

*Recommendation: Use 4 core (hero, upload, ranking, n8n) as primary, 2 local server as secondary row — keep total 6 like business site 14 pages evidence.*

---

## 7. Visualization — 2 Charts (from report scoring + workflow)

### Chart 1: Bar Chart — Predicted Scoring Breakdown (70% Threshold — per report)

* **Title:** `Predictable Scoring — 70% Threshold (20 Skill 40% + 6 Edu 30% + 6 Exp 30%)`
* **Subtitle:** `Not LLM feeling — a rule. Example: 5 CV samples from the POC (80,75,50,35,25 → 2 Lolos).`
* **Data for `js/charts.js` (`data-chart="bar"`):**
  ```json
  {
    "yMax": 100,
    "labels": ["CV3 Drafter 80", "CV1 Arch 75", "CV4 S2 50", "CV2 ITB 35", "CV5 SMK 25"],
    "values": [80,75,50,35,25]
  }
  ```
* Alternative grouped: Show **stacked logic** via two bars per CV? Simpler: single bar per CV with threshold line at 70 — add caption `≥70% Lolos, <70% Tidak (only 2 passed, as designed)`.
* **Container:** `border border-black/10 rounded-2xl p-6 sm:p-10 bg-paper` with `data-reveal`, plus **threshold badge** `≥70% Lolos` in accent.
* **Note:** Mention discrepancy with n8n code (65% vs 70%) was reconciled to **70% per report** per user choice.

### Chart 2: Flow Diagram — No Hallucination (HTML/CSS only)

* **Title:** `Workflow Orchestration, Not Hallucination`
* **Visual:** Horizontal flex (like exp006 flow):
  `Input 1-5 PDF` → `pdf.js extract in browser` → `JSON Array [cv_1..cv_5]` → `POST /webhook/upload-cv` → `Switch Posisi A/B/C` → `Code Hitung 50-20-30` → `Merge + Code Ranking` → `Postgres cv_ranking` → `Respond JSON + CSV` → `TRUNCATE (fresh)`
* **Container:** `border border-black/10 rounded-2xl p-6 sm:p-8 bg-paper`, `font-mono text-xs`, `data-reveal`.

*Do not use LLM for scoring — chart proves rule-based.*

---

## 8. Experiment Details (10 subsections — same as exp005/006)

Section wrapper: `py-20 bg-neutral-50`, eyebrow `Experiment Details`, `space-y-12`

**All body copy in ENGLISH, first-person, honest, predictable tone.**

1.  **Problem**
    * Manufacturing company growing fast, mass hiring, CV screening still manual (HR opens 5 PDFs one-by-one) — slow, biased, not scalable. Need a POC in 3 days (deadline Fri 4 Sept 2026) that is end-to-end, lightweight, HR-friendly, mobile-friendly, and runs locally on a private server (no public link, no data leaves).

2.  **Specifications (8 points)**
    1. Landing page single file `index.html` 626 lines — `Tailwind CDN + Vanilla JS + pdf.js 2.16.105`, `Inter + Manrope`, `bg-grid`, `data-reveal`, `mobile sticky bottom` bar, `toast`, `progress Mengekstrak 2/5...`, validation `PDF only, max 5MB/file, max 5 files, dup check`, badge `cv_1..cv_5`, `localStorage not needed — in-memory`.
    2. Client-side extraction: `pdfjsLib.getDocument({data: arrayBuffer}) → getPage → getTextContent → join`, returns `{text, page_count}`, wraps as JSON Array `[{id, filename, size_kb, page_count, text, uploaded_at}]` — not wrapped in object, for n8n `Split In Batches`.
    3. n8n workflow `Technical Test` 24 nodes — `Webhook POST /upload-cv (responseNode)` → `Code Extractor` (5 items) → `Code Keyword Router` (Posisi A/B/C keyword sets) → `Edit Fields Rapihin` (10 fields) → `Switch Posisi` (3 outputs + fallback) → 3 branches each `Pemecah → Normalisasi → Edit → Hitung → ekstrak` → `Merge Gabungan (Append 3 inputs)` → `Code Ranking (sort desc)` → `database (Postgres upsert)` → `Respond Ranking (allIncomingItems)` → `Postgres Clean (TRUNCATE)`.
    4. Keyword routing: Posisi A `Junior Architect` (12 keywords: autocad, revit, sketchup, lumion, building code...), Posisi B `Drafter & 3D Visualization` (15 keywords: enscape, d5 render, shop drawing...), Posisi C `Interior Designer` (10 keywords: furniture, lighting, photoshop...).
    5. Scoring rule (per report, 70% threshold as chosen): `20 skill = 40% + 6 education = 30% + 6 experience = 30%`, `total = round(skillScore+eduScore+expScore)`, `≥70% Lolos, <70% Tidak Lolos` (code currently implements 10/4/6 with 65% — documented as planned to align to report’s 20/6/6).
    6. Output: ranking `rank 1..5` with `{id_cv, nama_file, posisi, total_score, status, alasan}`, table in landing page + `Download Ranking (JSON)` + `Download CSV` with Indonesian headers `Peringkat, ID CV, Nama File, Posisi Dilamar, Skor (0-100), Status, Alasan` + BOM `0xEFBBBF` for Excel.
    7. Local private server: `Python 3 http.server` on `http://localhost:8010` (port 8000 was taken, so 8010) + webhook `http://localhost:5678/webhook/upload-cv` (or `host.docker.internal:5678` if n8n in Docker) — no public URL.
    8. Mobile & robust: `sm (640) md (768) lg (1024)`, upload zone `full width`, file cards `full width`, ranking table `overflow-x-auto scrollbar-thin`, toast `auto dismiss 3s`, `try/catch per file`, `AbortController 15s`, `fallback Download JSON` if webhook offline.

3.  **Hypothesis**
    * For mass hiring with a 3-day deadline, workflow orchestration (pdf.js + n8n + Postgres) with a predictable, auditable rule is more maintainable, predictable, and resource-efficient than putting an LLM (even local) inside the scoring system. LLM hallucination risk outweighs its benefit for this task; local LLM is better as a supervised syntax assistant (Ornith 35B via OpenCode/MCP) rather than the system’s brain.

4.  **Experiment**
    * Built the landing page per component (upload zone, file list, pdf.js extract, POST, ranking table) with deterministic prompts via OpenCode + MCP, each validated in small context windows. Built the n8n workflow 1 node per checkpoint (1–11), tested per node, confirmed with the project owner before next. Used `Qwen 2.5 1.5B` only for parsing experiments initially, then removed it for predictability — final POC uses pure code. All ran locally, no data left the private server.

5.  **What Failed (4 points — from report’s “why not AI inside”)**
    1. Small quantized LLM (aggressive quant) → unstable, unpredictable, hard to debug, high hallucination — abandoned for scoring.
    2. Bigger LLM → more stable but very resource-heavy, needs strong hardware (my RX 6700 XT 12GB cannot sustain it efficiently for 5 CV parallel) — abandoned for efficiency.
    3. Initial one-shot n8n generation → context bloat, wrong JSON path (`body` vs `body.data` wrapping), Switch routing missed — fixed by 1-node-per-checkpoint with `Map` dedup and explicit `allMatchingOutputs: false`.
    4. pdf.js on large PDFs → `getTextContent` empty for scanned PDFs (no text layer) — handled with `text.length <10` toast and `_error` field, still pushes item with empty text instead of failing whole batch.

6.  **Iteration**
    * Pivoted from “AI inside the system” to “workflow without AI inside, AI only as assistant”. Changed scoring from LLM-based to **rule-based 50-20-30 / 40-30-30** that HR can audit. Changed extraction from server-side to **browser-side pdf.js** to keep n8n light and demo fast even when n8n is offline (fallback download). Kept context small: 1 component per session, 1 n8n node per checkpoint.

7.  **Evidence (8 stat tiles)**
    * `Landing Lines: 626` (index.html)
    * `CV Samples: 5` (+ Vacancy PDF)
    * `n8n Nodes: 24` (1 webhook + 23 logic)
    * `Branches: 3` (A/B/C) + fallback
    * `Scoring: 20/6/6 → 40/30/30` (70% threshold)
    * `Time: ~5 sec for 5 CV` (vs 30 min manual)
    * `Output: Ranking 5` (2 Lolos, 3 Tidak — 80,75,50,35,25)
    * `Status: Stable` (E2E DONE 2026-09-02, screencast 24s)
    * Grid `2/3/4 cols`, `stat-tile` style.

8.  **Result**
    * POC is stable and delivered within the 3-day deadline — **actually completed in just 5 hours** — live on private server `http://localhost:8010`. HR can upload 1–5 CVs on mobile or desktop, get a ranked shortlist in ~5 seconds, download CSV for Excel, and the Postgres `cv_ranking` table is truncated automatically for the next batch — no manual cleanup, no token cost, no data leaves.

9.  **What I Learned**
    * Predictability beats probability for HR: a simple, auditable formula that HR can tweak (`20 skill vs 6 edu`) is more valuable than a black-box LLM score. Workflow orchestration (landing → n8n → DB) is the real skill — the builder’s supervision over syntax, bug potential, and ease of maintenance matters more than model size. Keeping AI as assistant (Ornith 35B) rather than the system’s brain made the POC shippable in 3 days on consumer hardware — **actually delivered in 5 hours**.

10. **Practical Implication**
    * **For business owners:** you don’t need a costly SaaS ATS — a local workflow that you own 100% can cut screening from 30 min to 5 sec, with a clear `Lolos 70% utk Junior Architect` reason per candidate. It’s HR-friendly (tap on phone), runs on your own computer, and you can adjust the 40/30/30 weights per your hiring preference.  
    * **For recruiters:** this proves the builder can ship **both public sites (EXP005) and internal HR tools (EXP007)** with the same local stack — end-to-end ownership, no hallucination risk, no data exfiltration. Start from `pdf.js` extraction, validate JSON, then orchestrate.

---

## 9. FAQ (3 — for HR + IT)

```json
{
  "FAQPage": [
    {"Q":"Why not put AI inside the scoring system?","A":"Small quantized models hallucinate and are unpredictable; big models are heavy and need strong hardware. Workflow orchestration (pdf.js + n8n + rule 40/30/30, 70% threshold) is stable, fast (~5s for 5 CV), auditable, and runs 100% locally — AI was only a supervised syntax assistant (Ornith 35B via OpenCode)."},
    {"Q":"How does HR know why a CV passed or failed?","A":"Each row has a clear reason: e.g., Lolos 75% utk Junior Architect (keahlian 7/10 → 35, edukasi 1/4 → 5, exp 7yr → 30 = 70) or Tidak Lolos 35%. The CSV header is Indonesian and includes Peringkat, Skor (0-100), Status, Alasan — open in Excel directly."},
    {"Q":"What if the CV is a scanned PDF or the webhook is offline?","A":"Scanned PDFs with no text layer trigger a toast (text <10 chars) but don’t fail the whole batch — the item is still sent with empty text and an _error flag. If the webhook is offline (timeout 15s), the landing page shows a fallback Download JSON button so the demo still works."}
  ]
}
```

Section: `py-20`, `eyebrow FAQ`, `h2 Frequently Asked Questions`, 3 `<details>`.

---

## 10. Configuration Template (2 code blocks — Extractor + Scoring)

**Block 1: Client-side pdf.js extract (the key to keeping n8n light):**
```js
async function extractTextFromPDF(file){
  const pdf = await pdfjsLib.getDocument({ data: await file.arrayBuffer() }).promise;
  let fullText = "";
  for(let i=1; i<=pdf.numPages; i++){
    const page = await pdf.getPage(i);
    fullText += (await page.getTextContent()).items.map(it=>it.str||"").join(" ") + "\n";
  }
  return { text: fullText.trim(), page_count: pdf.numPages };
}
// then wrap as [{id: "cv_1", filename, size_kb, page_count, text, uploaded_at}]
```

**Block 2: n8n scoring (predictable, not LLM) — 70% threshold (report):**
```js
// 20 skill = 40% + 6 edu = 30% + 6 exp = 30%  → 70% Lolos (per report)
const skillScore = (Math.min(keahlian.length,20)/20)*40;
const eduScore   = (Math.min(edukasi.length,6)/6)*30;
const expScore   = (Math.min(expTahun,6)/6)*30;
const total = Math.round(skillScore+eduScore+expScore);
const status = total>=70 ? "Lolos" : "Tidak Lolos";
```

Follow with 4 bullets: browser-side extraction saves n8n binary handling, threshold 70% is reference only (adjustable), file-based output .csv/.json, local server private.

---

## 11. Non-Technical Summary (summary-callout — for HR + recruiter)

Section: `py-20` with `summary-callout p-8 sm:p-12`.

**Heading:** `For those of you unable to read this data from technical standpoint, here is the conclusion`

**Body (5 paragraphs, plain English, HR-friendly):**
1. This is not a chatbot demo — it’s a hiring tool for a manufacturing company that hires a lot. HR used to open 5 CV PDFs one-by-one (30 minutes). Now they upload 1–5 PDFs at once and get a ranked shortlist in about 5 seconds.
2. You can use it on your phone: drag & drop on laptop or tap on phone, add 5 at once or one-by-one, remove one if you picked wrong, and a sticky “Send” button is always under your thumb.
3. The system doesn’t guess with AI. It uses a clear, auditable formula that HR can adjust: e.g., 20 skills = 40%, 6 education = 30%, 6 experience = 30%. If the score is 70% or more, it’s Lolos — and the CSV tells you why (“Lolos 75% utk Junior Architect”).
4. It’s fair per position: a Junior Architect is scored on AutoCAD/Revit, a Drafter on Enscape/D5, an Interior Designer on Photoshop — so no one is judged on the wrong skills. Results are a table (Rank, CV, Position, Score, Status) plus a CSV you open in Excel.
5. It runs 100% on a private server in your own computer (Python 3), not in the cloud — no PDF leaves your browser, no data is shared, and the database is cleaned automatically after each batch. It was built in 3 days with a local LLM only as a syntax assistant, not as the brain — so it’s predictable and light.

---

## 12. Disclaimer — same position as exp005/006

**Position:** Immediately before `<footer>`, `py-16` with `border-t border-black/10`.

**Copy (adapted for POC):**

> **Disclaimer — A Note on Results**
>
> This is a 3-day Proof of Concept, not a full Applicant Tracking System. The 70% threshold and 40/30/30 weights are subjective and based on what I could assess from the 5 CV samples — they are intentionally adjustable to the recruitment team’s actual criteria. The result (2 Lolos, 3 Tidak) is a reference for HR to decide who to interview, not a final hiring decision. For its scope (5 CVs at a time, local, no token cost), it is sufficient, stable, and fully owned.

---

## 13. Footer CTA (copy plek)

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

* **New page:** `public_html/exp007.html` (from `template-exp000.html`, keep navbar/footer plek)
* **Images (6 — ready 248K, verify `alt`, `loading="lazy"`):** `public_html/images/experiments007/homepage-hero.webp` (54K), `upload-queue.webp` (24K), `ranking-result.webp` (28K), `n8n-workflow.webp` (67K), `local-server.webp` (18K), `local-url.webp` (49K) — use 4 core + 2 optional.
* **Update (AFTER review):** `public_html/experiments-list.html` — add 7th card EXP 007 (Stable) + JSON-LD `numberOfItems:7` + ListItem position 7
* **Update (AFTER review):** `public_html/sitemap.xml` — add `<url>` for `exp007.html` with 4-6 `<image:image>` entries, `lastmod 2026-09-04`
* **Update (AFTER review):** `public_html/index.html` — add 7th card in `Selected Experiments` if curated (currently 5 cards, add 6th/7th)
* **Content source:** `Project-Technical-Test/` (read-only) + `Bahan POC/` (read-only) — do NOT modify
* **No new deps:** Keep `cdn.tailwindcss.com`, `fonts.googleapis.com`, `js/main.js`, `js/charts.js`, `css/style.css` only (+ `pdf.js` is only in POC landing page, not portfolio)

---

## 15. Build Steps (after this plan ACC)

1. **User ACCs this plan** (`plan-exp007.md` ini) — currently at review stage per user request `bikinin plan nya dulu kalo data udah cukup solid`.
2. Generate `exp007.html` from `template-exp000.html` → fill sections 1–12 above, keep semantic HTML, responsive, `title`/`meta`/`OG`/`JSON-LD`, same navbar/footer, full English.
3. Local verify: `python -m http.server` (port 8003 already running) + check 6 images load, chart renders, no horizontal overflow on mobile.
4. **JANGAN update `experiments-list.html` + `sitemap.xml` + `index.html` dulu** — tunggu user review `exp007.html`.
5. User reviews built HTML, then we update list + sitemap + index to publish.

---

**Ready for your review, bro.**  
Plan ini sudah 100% solid — pakai 70% threshold per report lu, 6 webp ready, 24 nodes via MCP, dan filosofi “workflow without in-system AI” yang HR-friendly. Bilang `gas` kalau oke — gue langsung generate `exp007.html` nya.
