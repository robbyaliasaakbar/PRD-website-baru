# PLAN — Experiment 008

**Found the Bug, Fixed the Stack: ROCm gfx1031 vs Vulkan on RX 6700 XT**

> **Location:** `/public_html/exp008.html`
> **Plan file:** `PRD/plan-exp008.md`
> **Date publish:** `2026-09-06` (tentative, after user review)
> **Status:** `Stable` — workaround STABLE, upstream pending (continuation to EXP 009)
> **Lang:** `en` (full English, consistent with EXP 004-007)
> **Author:** Robby Aliasa Akbar
> **Upstream report:** `https://github.com/ggml-org/llama.cpp/issues/26702` (MUST be linked in-page for credibility)

---

## 1. Objective

Answer the research question:

> **Can an "unsupported" RDNA2 card (RX 6700 XT gfx1031) beat Vulkan by 23% wall-clock on 8K context — once the GFX-target bug and flash-attention assert are found and fixed?**

This page documents the **ROCm optimization experiment on gfx1031 (Navi22 / RX 6700 XT)** as reported upstream in `ggml-org/llama.cpp#26702`. If EXP 001 proved *35B MoE can run on 12GB VRAM* and EXP 002 proved *prefill can stay fast at 250K+ context*, EXP 008 goes one layer deeper: *why it can run that fast — because the ROCm stack was debugged and fixed locally*.

It is an **optimization experiment, not a build-something experiment**. The value proposition is debugging skill: finding the bug, fixing the stack, proving with numbers that ROCm is the clearly superior backend for long-context on this hardware *if* the friction is resolved.

Source of truth: issue body #26702 (commit `0a50d99` build 10108, ROCm `7.1.52801-9999`, model `gemma-4-12b-it-IQ4_NL.gguf` 6.24 GiB 11.91B params, test `llama-bench --n-prompt 8192 --n-gen 512 --batch-size 1024 --ubatch-size 1024 --cache-type-k q8_0 --cache-type-v q8_0 --n-gpu-layers 99 --flash-attn on`, 3 runs per backend). Do NOT invent numbers beyond the issue.

**Key stance (to be highlighted):** *Vulkan is zero-friction but slower where it matters. ROCm needs two workarounds today (GFX override + FA assert patch), but wins net wall-clock by ~23% because prefill dominates long-context. Workaround is STABLE locally; upstream fix is pending; decode-side TOP_K gap is explicitly out-of-scope and teased as EXP 009.*

---

## 2. Audience & Goals

**Primary:** Local-LLM builders on AMD RDNA2 (RX 6600 / 6700 series) + llama.cpp contributors — they hit the same `gfx1031` + `max_blocks_per_sm` wall and need a reproducible report.
**Secondary:** Business owners / tech evaluators reading EXP 001-007 — they see *why* the previous experiments could hit those speeds (the stack underneath was optimized).

**Goals:**
1. Prove **ROCm > Vulkan for long-context on gfx1031** with audit-ready numbers: `pp8192 +84.5%`, `tg512 -15.4%`, net `27.3s vs 35.6s = ~23% win`, crossover `~1760 tokens`.
2. Show **debugging capability**: native `gfx1031` target fails → masquerade as `gfx1030` + `HSA_OVERRIDE_GFX_VERSION=10.3.0`; FA assert crash in `fattn-common.cuh` → `max_blocks_per_sm = 1` guard with honest *correctness unknown* disclaimer.
3. Explain **why FA cannot just be turned off**: KV quant `q8_0` depends on FA, usable context shrinks, OOM risk rises, MTP speculative decoding becomes unusable.
4. Keep credibility: link upstream issue #26702 in at least 3 places (hero, evidence, footer CTA area), state workaround vs upstream fix honestly, mark `TOP_K` as out-of-scope for EXP 009.

---

## 3. Styling & Stack (Wajib plek ketiplek)

* **Stack:** `HTML5 + Tailwind CDN + Vanilla JS` (no build) — same as portfolio. No new deps.
* **Colors:** `ink #0C0C0C / paper #FFFFFF / accent #D70000` + green `#2E8B2E` for ROCm win (already used in exp004). Business copy stays in screenshots, not page theme.
* **Fonts:** `Manrope` (display), `Inter` (body), `JetBrains Mono` (code) — Google Fonts.
* **Tokens to reuse plek:** `eyebrow`, `status-badge status-stable`, `border border-black/10 rounded-2xl`, `summary-callout`, `quote-block`, `detail-label`, `stat-tile`, `code-block`, `data-reveal`, `bg-grid`, `bg-dark-section` footer, `nav-link nav-active`, `arrow-link`.
* **Navbar/Footer:** Copy-paste 1:1 from `exp007.html` — only change `aria-current` / breadcrumb to `Experiment 008`.
* **Layout rhythm:** Alternate `py-20` white / `bg-neutral-50` like exp007. Hero with `bg-grid`. All grids `grid-cols-1 sm:2 lg:3`, responsive, semantic `section/h1-h3`, `alt` text on all images.

---

## 4. SEO & Metadata

**Head:**
```html
<title>Experiment 008 — Found the Bug, Fixed the Stack: ROCm gfx1031 vs Vulkan on RX 6700 XT — Robby Aliasa Akbar</title>
<meta name="description" content="Can RX 6700 XT gfx1031 beat Vulkan by 23% wall-clock on 8K context once the ROCm GFX-target bug and flash-attention assert are fixed? ROCm 653 vs Vulkan 354 t/s prefill.">
<link rel="canonical" href="https://robbyaliasaakbar.github.io/exp008.html">
<link rel="alternate" hreflang="en" href="https://robbyaliasaakbar.github.io/exp008.html">
<link rel="alternate" hreflang="x-default" href="https://robbyaliasaakbar.github.io/exp008.html">
<link rel="alternate" hreflang="id" href="https://robbyaliasaakbar.github.io/exp008.html">
<meta name="robots" content="index, follow, max-image-preview:large">
<meta property="og:type" content="article">
<meta property="og:title" content="Experiment 008 — Found the Bug, Fixed the Stack: ROCm gfx1031 vs Vulkan">
<meta property="og:description" content="ROCm needs 2 workarounds on gfx1031 but wins 23% net wall-clock over Vulkan at 8K context. 653 vs 354 t/s prefill. Upstream report #26702.">
<meta property="og:image" content="https://robbyaliasaakbar.github.io/images/experiments008/rocm-bench.webp">
<meta property="og:image:width" content="1440">
<meta property="og:image:height" content="878">
<meta name="twitter:card" content="summary_large_image">
<script type="application/ld+json"> Article + BreadcrumbList + FAQPage
```

**Keywords:** `["ROCm", "gfx1031", "RX 6700 XT", "llama.cpp", "flash-attention", "Vulkan vs ROCm", "long-context inference", "RDNA2", "HSA_OVERRIDE_GFX_VERSION", "KV cache quantization"]`

**Dates:** `datePublished 2026-09-06`, `dateModified 2026-09-06` (adjust on publish).

**Sitemap & List updates (AFTER review, not in first build):**
* `sitemap.xml` add `<url><loc>.../exp008.html</loc><lastmod>2026-09-06</lastmod> + 5 images`
* `experiments-list.html` add card EXP 008 (position 8, Stable) + update `numberOfItems:7 → 8` + ListItem position 8
* `index.html` add to Selected Experiments if curated

---

## 5. System Requirements (9 tiles — from issue #26702)

Grid: `grid-cols-2 md:grid-cols-3` (same style as exp004)

| Tile | Label | Value |
|------|-------|-------|
| 1 | CPU | `Intel Core i5-11400F` |
| 2 | RAM | `16GB DDR4 3200 MT/s XMP` |
| 3 | GPU | `RX 6700 XT 12GB (gfx1031 Navi22)` |
| 4 | OS | `Ubuntu 26.04 LTS` |
| 5 | ROCm / HIP | `7.1.52801-9999 / clang 21.1.8` |
| 6 | llama.cpp | `0a50d99 build 10108 (ROCm + Vulkan)` |
| 7 | Model Under Test | `gemma-4-12b-it-IQ4_NL 6.24GiB` |
| 8 | Tuning | `LACT Manual Compute 182W -90mV` |
| 9 | Status | `STABLE (workaround)` |

*Note under grid (small, honest): `Native gfx1031 requires HSA_OVERRIDE_GFX_VERSION=10.3.0 + FA assert patch. Upstream fix pending — see GitHub #26702.`*

---

## 6. Screenshots — Gallery (5 WEBP — user-approved list, no build screenshot)

**Files in `public_html/images/experiments008/` (user to supply, placeholder if missing):**
1. `lact-vulkan.webp` — `alt: LACT tuning applied during Vulkan benchmark — Manual Compute profile same as ROCm run`
2. `lact-rocm.webp` — `alt: LACT tuning applied during ROCm benchmark — 182W limit minus 90mV VRAM 1900MHz for long-context`
3. `core-dump.webp` — `alt: Core dump / assert crash GGML_ASSERT max_blocks_per_sm greater than 0 on gfx1031 ROCm target`
4. `rocm-bench.webp` — `alt: llama-bench ROCm 3 runs pp8192 around 653 t/s tg512 around 34.6 t/s on gemma 12B`
5. `vulkan-bench.webp` — `alt: llama-bench Vulkan 3 runs pp8192 around 354 t/s tg512 around 40.9 t/s same model and flags`

**Layout:**
* **Row 1 (2 cols):** `lact-rocm.webp` | `lact-vulkan.webp` + caption `Same LACT tuning for both backends — no backend-specific tuning`
* **Row 2 (full-width):** `core-dump.webp` + caption `The bug that gates ROCm out of long-context`
* **Row 3 (2 cols):** `rocm-bench.webp` | `vulkan-bench.webp` + caption `Same model, same flags — only backend changed`
* Section wrapper: `bg-neutral-50 py-20`, eyebrow `Screenshots`, `data-reveal` staggered, `border rounded-2xl bg-paper shadow-sm`, `loading="lazy"`, `figcaptions`.

*If images missing at build time: use placeholder div with filename + alt, do NOT break layout.*

---

## 7. Visualization — 2 Charts (from issue benchmark table)

### Chart 1: Grouped Bar — Prefill vs Decode (the core proof)

* **Title:** `Prefill Wins, Decode Loses — Net Win`
* **Subtitle:** `Same model, same flags. Only backend changed. 3-run average.`
* **Data for `js/charts.js` (`data-chart="bar"`):**
  ```json
  {
    "yMax": 700,
    "labels": ["pp8192 ROCm 653.9", "pp8192 Vulkan 354.4", "tg512 ROCm 34.6", "tg512 Vulkan 40.9"],
    "values": [653.9, 354.4, 34.6, 40.9]
  }
  ```
* **Caption badges:** `+84.5% prefill` (green) + `-15.4% decode` (neutral) — explain prefill dominates at 8K.
* **Container:** `border border-black/10 rounded-2xl p-6 sm:p-10 bg-paper` with `data-reveal`.

### Chart 2: Wall-Clock Stacked Bar — Full Cycle (HTML/CSS only, no lib)

* **Title:** `23% Net Wall-Clock Win — Why Prefill Matters More`
* **Visual:** Two horizontal stacked bars (pure Tailwind divs):
  `ROCm 12.5s prefill + 14.8s decode = 27.3s` vs `Vulkan 23.1s + 12.5s = 35.6s`
* **Note below:** `Crossover ~1760 tokens — below that Vulkan is net-faster, above that ROCm wins. Effectively all long-context workloads.`
* **Container:** `border border-black/10 rounded-2xl p-6 sm:p-8 bg-paper`, `font-mono text-xs`, `data-reveal`.

*Do NOT claim decode gap is solved — explicitly state TOP_K under separate investigation (EXP 009 teaser).*

---

## 8. Experiment Details (10 subsections — same as exp005/006/007)

Section wrapper: `py-20 bg-neutral-50`, eyebrow `Experiment Details`, `space-y-12`

**All body copy in ENGLISH, first-person, honest, evidence-based tone.**

1.  **Problem**
    * RX 6700 XT is gfx1031 Navi22 — no first-class ROCm target in llama.cpp. Choice is painful: Vulkan just works but is ~85% slower on prefill at 8K, ROCm is faster but needs manual workarounds and crashes on flash-attention. For long-context research/agentic workloads (32K–120K+ tokens) this decides whether 12GB VRAM is usable or OOM.

2.  **Specifications (7 points, from issue)**
    1. LACT sweet-spot for long-context: Manual, Compute profile, 182/223W, 2350MHz max (vs 2600 stock), -90mV, VRAM 1900MHz — same tuning for both backends.
    2. ROCm build: `-DGGML_HIP=ON -DAMDGPU_TARGETS=gfx1030 -DCMAKE_HIP_ARCHITECTURES=gfx1030` with `clang/hipcc`, runtime `HSA_OVERRIDE_GFX_VERSION=10.3.0`.
    3. Vulkan build: `cmake -B build -DGGML_VULKAN=1`, no tweak, no drama.
    4. Bench template: `llama-bench --n-prompt 8192 --n-gen 512 --batch-size 1024 --ubatch-size 1024 --cache-type-k q8_0 --cache-type-v q8_0 --n-gpu-layers 99 --flash-attn on`, model `gemma-4-12b-it-IQ4_NL.gguf`.
    5. Server template (long-context usable): `llama-server --ctx-size 131072 --image-max-tokens 2048 --spec-draft-model mtp-gemma --flash-attn on --cache-type-k q8_0` etc. (full command in Config Template).
    6. FA dependency: KV quant + long context + MTP speculative decoding all require FA on — turning FA off is not a fallback.
    7. Measurement: 3 runs per backend, identical batch/cache, only backend changed, `llama-bench` excludes sampling time.

3.  **Hypothesis**
    * For gfx1031 long-context, ROCm with correct GFX handling and working flash-attention will beat Vulkan net wall-clock despite decode deficit, because prefill dominates the cycle. The friction is build/runtime, not hardware capability.

4.  **Experiment**
    * Built both backends from same commit `0a50d99`, ran bench 3x each with identical flags, captured `rocm-smi` + terminal tables. Attempted native `gfx1031` target — confirmed fails without override. Hit FA assert — patched locally with `sed` guard, kept FA on, re-ran. Compared wall-clock math + crossover calculation.

5.  **What Failed (2 points — the 2 issues)**
    1. Native `gfx1031` target has no working path — requires building for neighboring `gfx1030` + runtime masquerade. Ask: native Navi22 support without `HSA_OVERRIDE`.
    2. `GGML_ASSERT(max_blocks_per_sm > 0)` in `fattn-common.cuh` crashes on this backend/target combo. Workaround `if <=0 then =1` avoids crash but correctness/occupancy impact unknown. Ask: root-cause fix or reviewed guidance.

6.  **Iteration**
    * From "Vulkan is enough" to "ROCm is worth the friction": kept Vulkan as baseline, isolated ROCm variables one-by-one (GFX override → FA patch → bench), refused to disable FA because that would hide the long-context advantage. Documented workarounds honestly instead of silently forking.

7.  **Evidence (8 stat tiles)**
    * `pp8192 ROCm: 653.9 t/s` (avg 3)
    * `pp8192 Vulkan: 354.4 t/s`
    * `Delta prefill: +84.5%`
    * `tg512 ROCm: 34.6 t/s`
    * `tg512 Vulkan: 40.9 t/s`
    * `Delta decode: -15.4%`
    * `Total 8192+512: 27.3s vs 35.6s`
    * `Net win: ~23% / Stable (workaround)`
    * Grid `2/3/4 cols`, `stat-tile` style.

8.  **Result**
    * ROCm wins net wall-clock by ~23% at 8K despite losing decode. Crossover ~1760 tokens — ROCm is better backend for effectively all long-context on this hardware once buildable without manual patch. Workaround stack is STABLE locally for 131K server + MTP use.

9.  **What I Learned**
    * Optimization lives under the model: GFX target + FA occupancy matter more than prompt tricks for long-context. A one-line assert can gate an entire hardware family out of its best workload. Honest workaround (with unknown-correctness disclaimer) + upstream report beats silent fork — it lets maintainers fix root cause while users stay productive.

10. **Practical Implication**
    * **For RDNA2 owners:** don't settle for Vulkan if you do long-context — use the override + patch documented here, keep LACT tuning conservative, keep KV q8_0 + FA on. Repro steps in Config Template.
    * **For llama.cpp:** two small fixes (native gfx1031 + FA occupancy) would unlock RX 6600/6700 series without user patching. Until then this page + issue #26702 is the reproducible reference. Decode TOP_K gap is next (EXP 009).

---

## 9. FAQ (3 — for builders + evaluators)

```json
{
  "FAQPage": [
    {"Q":"Why not just use Vulkan? It works out of the box.","A":"Vulkan needs no patch, but at 8192 context it does 354 t/s prefill vs ROCm 653 t/s. That is 23.1s vs 12.5s prefill — net 35.6s vs 27.3s full cycle. Below ~1760 tokens Vulkan is net-faster, above that ROCm wins. For long-context research/agentic work, ROCm is clearly better once running."},
    {"Q":"Is forcing max_blocks_per_sm = 1 safe?","A":"Unknown — that is why it is reported as a bug, not a fix. It avoids the crash and benches stably, but only a maintainer can confirm correctness/occupancy for gfx1031. Do not treat the sed one-liner as upstream fix; track issue #26702 for reviewed guidance."},
    {"Q":"Does this explain the decode being slower on ROCm?","A":"No. llama-bench excludes sampling time, so the TOP_K sampler gap seen in llama-server logs is not confirmed as the cause of the tg512 gap. Decode deficit is under separate controlled investigation — planned as EXP 009."}
  ]
}
```

Section: `py-20`, `eyebrow FAQ`, `h2 Frequently Asked Questions`, 3 `<details>`.

---

## 10. Configuration Template (2 code blocks)

**Block 1: ROCm workaround build + bench (copy-paste from issue):**
```bash
# Build ROCm as gfx1030 (gfx1031 native fails without override)
cmake -S ~/Projects/llama2 -B ~/Projects/llama2/build-rocm \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=hipcc \
  -DGGML_HIP=ON -DAMDGPU_TARGETS=gfx1030 -DCMAKE_HIP_ARCHITECTURES=gfx1030
cmake --build ~/Projects/llama2/build-rocm -j"$(nproc)"

# FA assert workaround (correctness unknown — see #26702)
sed -i 's/GGML_ASSERT(max_blocks_per_sm > 0);/if (max_blocks_per_sm <= 0) { max_blocks_per_sm = 1; }/g' \
  ~/Projects/llama2/ggml/src/ggml-cuda/fattn-common.cuh

# Bench (only backend changed vs Vulkan)
HSA_OVERRIDE_GFX_VERSION=10.3.0 ~/Projects/llama2/build-rocm/bin/llama-bench \
  --model "$MODEL" --n-prompt 8192 --n-gen 512 \
  --batch-size 1024 --ubatch-size 1024 \
  --cache-type-k q8_0 --cache-type-v q8_0 \
  --n-gpu-layers 99 --flash-attn on
```

**Block 2: Vulkan baseline (no bug, no drama):**
```bash
cmake -B build -DGGML_VULKAN=1
cmake --build build --config Release
~/Projects/llama.cpp/build/bin/llama-bench \
  --model "$MODEL" --n-prompt 8192 --n-gen 512 \
  --batch-size 1024 --ubatch-size 1024 \
  --cache-type-k q8_0 --cache-type-v q8_0 \
  --n-gpu-layers 99 --flash-attn on
```

Follow with 4 bullets: same LACT tuning both runs, FA must stay on for KV q8_0 + MTP, override is runtime not build, link to full server command in issue.

---

## 11. Non-Technical Summary (summary-callout)

Section: `py-20` with `summary-callout p-8 sm:p-12`.

**Heading:** `For those of you unable to read this data from technical standpoint, here is the conclusion`

**Body (5 paragraphs, plain English):**
1. My graphics card (RX 6700 XT) is officially "unsupported" by the fast AI software (ROCm). The easy software (Vulkan) works instantly but is slow where it matters.
2. I found two roadblocks: the software didn't recognize my card so I had to disguise it as its neighbor, and a safety check crashed the fast memory mode I need for long documents.
3. I fixed both locally with two small workarounds — and measured carefully 3 times each: fast mode does 653 pages per second vs 354 for easy mode on long documents.
4. Over a full job (read 8192 + write 512), fast mode finishes in 27 seconds vs 36 seconds — 23% faster — even though it writes slightly slower. For long documents, reading dominates.
5. My fix is stable for my daily use, but I reported it upstream so everyone benefits. I linked the public bug report so you can verify — no magic numbers. The remaining write-speed mystery is next (EXP 009).

---

## 12. Disclaimer + Upstream Link (required for credibility)

**Position:** Immediately before `<footer>`, `py-16` with `border-t border-black/10`.

**Copy:**

> **Disclaimer — Workaround, Not Upstream Fix**
>
> This experiment is STABLE with my local workarounds (GFX override + FA assert guard) on the hardware and commit listed above. It is not an upstream fix. Native gfx1031 support and a reviewed flash-attention fix are pending with maintainers. Numbers are from 3-run `llama-bench` on `gemma-4-12b-it-IQ4_NL` — reproducible with the commands above, not a guarantee for every model or ROCm version. I disclosed the `max_blocks_per_sm = 1` guard as correctness-unknown on purpose.
>
> **Upstream report:** `ggml-org/llama.cpp#26702 — Misc. bug: ROCm gfx1031 build report (why ROCM is better than vulkan)` — https://github.com/ggml-org/llama.cpp/issues/26702

Also add inline link in hero (`View upstream issue #26702`) + Evidence section.

---

## 13. Footer CTA (copy plek + upstream link)

```html
<section class="pb-24 lg:pb-32">
  <div class="mx-auto max-w-6xl px-6 lg:px-8">
    <div class="flex flex-col sm:flex-row items-center justify-between gap-6 border-t border-black/10 pt-10" data-reveal>
      <a href="experiments-list.html" class="arrow-link font-display text-sm uppercase"><span aria-hidden="true">&larr;</span> Back to Experiments</a>
      <a href="contact.html" class="arrow-link font-display text-sm uppercase">Have a problem worth exploring? <span aria-hidden="true">-&gt;</span></a>
    </div>
    <p class="mt-6 text-xs text-neutral-500">Upstream: <a class="underline hover:text-accent" href="https://github.com/ggml-org/llama.cpp/issues/26702" target="_blank" rel="noopener">llama.cpp #26702</a> — EXP 009 will cover TOP_K decode investigation.</p>
  </div>
</section>
```

---

## 14. File & Asset Checklist

* **New page:** `public_html/exp008.html` (from `template-exp000.html`, keep navbar/footer plek)
* **Images (5 — user-approved, placeholder if missing):** `public_html/images/experiments008/lact-vulkan.webp`, `lact-rocm.webp`, `core-dump.webp`, `rocm-bench.webp`, `vulkan-bench.webp` — `alt` + `loading="lazy"` required
* **Update (AFTER review):** `public_html/experiments-list.html` — add 8th card EXP 008 (Stable) + JSON-LD `numberOfItems:8` + ListItem position 8
* **Update (AFTER review):** `public_html/sitemap.xml` — add `<url>` for `exp008.html` with 5 `<image:image>` entries, `lastmod 2026-09-06`
* **Update (AFTER review):** `public_html/index.html` — add to Selected Experiments if curated
* **Content source:** issue #26702 body (read-only) — do NOT invent beyond it
* **No new deps:** Keep `cdn.tailwindcss.com`, `fonts.googleapis.com`, `js/main.js`, `js/charts.js`, `css/style.css` only

---

## 15. Build Steps (after this plan ACC)

1. **User ACCs this plan** (`plan-exp008.md` ini).
2. Generate `exp008.html` from `template-exp000.html` → fill sections 1–12 above, keep semantic HTML, responsive, `title`/`meta`/`OG`/`JSON-LD`, same navbar/footer, full English, 5-image gallery with placeholders if files missing.
3. Local verify: `python -m http.server` + check images load, charts render, no horizontal overflow on mobile, upstream link opens in new tab.
4. **JANGAN update `experiments-list.html` + `sitemap.xml` + `index.html` dulu** — tunggu user review `exp008.html`.
5. User reviews built HTML, then we update list + sitemap + index to publish + teaser EXP 009.

---

**Ready for your review, bro.**
Plan ini 100% dari issue #26702 lo — judul #2 yang lo pilih, 5 screenshots yang lo mau, Stable dengan disclaimer workaround, full English, link ggml wajib 3 titik. Bilang `gas` kalau oke — gue langsung generate `exp008.html` nya.
