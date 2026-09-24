# PLAN — Experiment 009

**From Override to Native: Robust ROCm gfx1031 Without HSA_OVERRIDE (580 tok/s Prefill on RX 6700 XT)**

> **Location:** `/public_html/exp009.html`
> **Plan file:** `PRD/plan-exp009.md`
> **Date publish:** `2026-09-06` (tentative, after user review)
> **Status:** `Stable` — native robust fix, reproducible (continuation of EXP 008)
> **Lang:** `en` (full English, consistent with EXP 008)
> **Author:** Robby Aliasa Akbar
> **Sources:** Reddit `r/LocalLLM` post (native gfx1031 MMA fix) + GitHub `ggml-org/llama.cpp#26702` (Phase 1 workaround)
> **Relation to EXP 008:** EXP 008 = Phase 1 workaround (gfx1030 masquerade + `max_blocks_per_sm` guard, gemma-12B, Vulkan comparison). EXP 009 = Phase 2 native robust fix (no override, `fattn.cu` MMA path, Ornith-35B split, multi-engine). EXP 008 Problem section already links forward; EXP 009 must link back to EXP 008 + issue #26702 + Reddit post.

---

## 1. Objective

Answer the research question:

> **Can RX 6700 XT (gfx1031) run ROCm natively — no HSA_OVERRIDE, no spoofing — with flash-attention stable and prefill sustained above 500 tok/s at 20K+ cached context on a 35B A3B CPU/GPU split?**

This page documents the **native robust fix** as shared on Reddit: one-line change in `ggml/src/ggml-cuda/fattn.cu` forcing `BEST_FATTN_KERNEL_MMA_F16` for `head_dim 512` on AMD, which unblocked flash-attn → KV quant q8_0 → 131K context on 12GB VRAM, across llama.cpp + Ollama + Unsloth + LM Studio + SGLang + vLLM.

It is explicitly a **continuation, not a replacement** of EXP 008. EXP 008 proved ROCm is worth fixing (+84.5% prefill, 23% net win). EXP 009 proves it can be fixed robustly and reproducibly at the root level.

Source of truth: Reddit post body pasted by user (TL;DR + problem + env + logs + fix + launch config + caveats). Do NOT invent numbers beyond it. Exact values to use are listed in §5–§8.

**Key stance:** *Prefill/TTFT at 70K+ is what makes or breaks 35B split usability — not decode. Native recognition was a deliberate requirement (override caused FA-off, top_k CPU fallback, random core dumps). Method was empirical (Vulkan baseline + btop + dispatch logic), not kernel theory — honest about being a creative director, self-taught, inviting correction on the why.*

---

## 2. Audience & Goals

**Primary:** gfx1031 / adjacent-tier AMD owners + llama.cpp / ROCm contributors — they need exact versions, exact flags, and honest caveats to reproduce native run.
**Secondary:** Readers of EXP 001 + EXP 008 — they see the full chain: EXP 009 (native stack) → EXP 001 (35B on 12GB) → EXP 008 (why ROCm beats Vulkan).

**Goals:**
1. Prove **native, no-hack** with `rocminfo gfx1031` + TheRock 7.14 + commit `fc3f10b38` (10307) + Sapphire PULSE board ID.
2. Show **sustained prefill, not cold-start**: 581 → 548 → 531 → 508 → 502 tok/s while 18–23K already cached.
3. Give **full reproducible launch config** (Ornith-35B, `--n-cpu-moe 22`, threads 4, batch 1024, KV q8_0, `--no-kv-unified`, cache-ram 8192) with notes on why each non-obvious choice was made.
4. Keep credibility: link Reddit post + issue #26702 + EXP 008 in at least 3 places each; state unknowns honestly (why SGLang/vLLM also fixed is unexplained; 512 came from observation, not docs).

---

## 3. Styling & Stack (plek EXP 008)

* **Stack:** `HTML5 + Tailwind CDN + Vanilla JS` only.
* **Colors:** `ink #0C0C0C / paper #FFFFFF / accent #D70000` + green `#2E8B2E` for win.
* **Fonts:** `Manrope / Inter / JetBrains Mono`.
* **Tokens:** `eyebrow`, `status-badge status-stable`, `border rounded-2xl`, `summary-callout`, `quote-block`, `detail-label`, `stat-tile`, `code-block`, `data-reveal`, `bg-grid`, `bg-dark-section`, `nav-link nav-active`, `arrow-link`, `placeholder-shot` (same dashed placeholder style as exp008 while screenshots are collected).
* **Navbar/Footer:** Copy 1:1 from `exp008.html`, change breadcrumb to Experiment 009.
* **Order (sama kayak exp008 revisi):** Header → System Requirements → Screenshots → Visualization → Non-Technical Summary → Experiment Details → Config Template → FAQ → Disclaimer → Footer CTA.

---

## 4. SEO & Metadata

```html
<title>Experiment 009 — From Override to Native: Robust ROCm gfx1031 Without HSA_OVERRIDE — Robby Aliasa Akbar</title>
<meta name="description" content="Native ROCm on RX 6700 XT gfx1031 with one-line MMA fix: 580 tok/s prefill sustained at 20K+ cached on 35B A3B split, no override, flash-attn + KV q8_0 + 131K ctx.">
<link rel="canonical" href="https://robbyaliasaakbar.github.io/exp009.html">
<meta property="og:title" content="Experiment 009 — Native gfx1031: 580 tok/s Prefill Without Override">
<meta property="og:description" content="Override caused FA-off + CPU fallback + core dumps. Native fix forces MMA kernel for head_dim 512 — 581 to 502 tok/s sustained, multi-engine ROCm.">
<meta property="og:image" content="https://robbyaliasaakbar.github.io/images/experiments009/server-prefill-580.webp">
<script type="application/ld+json"> Article + BreadcrumbList + FAQPage
```

**Keywords:** `["gfx1031 native", "ROCm TheRock", "RX 6700 XT", "flash-attention MMA", "head_dim 512", "prefill TTFT", "35B A3B split", "KV q8_0", "llama.cpp", "reproducible"]`
**Dates:** `datePublished 2026-09-06`, `dateModified 2026-09-06`.

**After review:**
* `experiments-list.html` card position 9 + `numberOfItems:8 → 9`
* `sitemap.xml` add `exp009.html` + 6 images
* `index.html` add card to Selected Experiments

---

## 5. System Requirements (9 tiles)

Grid `grid-cols-2 md:grid-cols-3`, same style as exp008.

| Tile | Label | Value |
|------|-------|-------|
| 1 | CPU | `Intel i5-11400F` |
| 2 | RAM | `16GB DDR4 3200MT/s` |
| 3 | GPU | `RX 6700 XT 12GB gfx1031 Sapphire PULSE` |
| 4 | OS | `Ubuntu Desktop 26.04 LTS` |
| 5 | ROCm | `TheRock HIP 7.14.60850 clang 23.0.0git` |
| 6 | llama.cpp | `10307 fc3f10b38 GNU 15.2.0` |
| 7 | Model | `Ornith-1.0-35B-UD-IQ4_NL + n-cpu-moe 22` |
| 8 | Engines | `llama.cpp Ollama Unsloth LM Studio SGLang vLLM` |
| 9 | Status | `STABLE native, no override` |

Note under grid: `rocminfo shows gfx1031 (not spoofed). lspci Navi 22 rev c1 Sapphire PULSE. Exact HIP + commit matter — see Caveats.`

---

## 6. Screenshots — Gallery (6 placeholders, user collects)

Files in `public_html/images/experiments009/`:

1. `rocminfo-gfx1031.webp` — `rocminfo | grep gfx` showing gfx1031 + `lspci` Sapphire PULSE — proof native, not spoofed
2. `server-prefill-580.webp` — server log 581.81 / 548.24 / 531.24 / 508.61 / 502.11 tok/s at 18–23K cached — proof sustained, not cold-start
3. `btop-cpu-moe.webp` — btop showing CPU MoE layers + GPU load — proof split is real
4. `temp-power-lact.webp` — edge/junction/VRAM temps + power prefill vs decode + LACT — proof stability (83C at 4 threads vs 95C spikes at 3)
5. `vulkan-baseline-slow.webp` — Vulkan 70–100 tok/s at 16–32K, painful past that — proof why fix was needed
6. `multi-engine-rocm.webp` — Ollama/Unsloth/LM Studio/SGLang/vLLM running native ROCm — proof root-level fix

Layout:
* Row 1 (2 cols): `rocminfo` | `server-prefill`
* Row 2 (full): `btop-cpu-moe`
* Row 3 (2 cols): `temp-power` | `vulkan-baseline`
* Row 4 (full): `multi-engine`
* Wrapper `bg-neutral-50 py-20`, `data-reveal`, placeholders with filename + description + commented `<img>` for drop-in.

---

## 7. Visualization — 2 Charts

### Chart 1: Line — Sustained prefill at 20K+ cached (the number that mattered)

* Title: `580 to 502 tok/s sustained — not a cold-start number`
* Sub: `Real session, 18–23K already cached, n_tokens 2048→6144`
* Data (`data-chart="line"`):
```json
[{"label":"2K 581","value":581.81},{"label":"3K 548","value":548.24},{"label":"4K 531","value":531.24},{"label":"5K 508","value":508.61},{"label":"6K 502","value":502.11}]
```
* Caption: `Peak ~580, floor >500 across phase. Vulkan was 70–100 in same regime.`

### Chart 2: HTML/CSS comparison — Why prefill, not decode

* Title: `Clock +45% vs bandwidth +7% — prefill is compute, decode is bandwidth`
* Visual: two horizontal bars (Tailwind divs): `6700XT 2581MHz vs 3060 1777MHz (+45%)` and `384 vs 360 GB/s (+7%)` + note `3060 split peaks ~300 tok/s prefill; 6700XT should clear it widely — Vulkan didn't, signal of software bottleneck`.
* Note: `Decode 22–23 tok/s flat past 40K — consistent, not the bottleneck chased.`

---

## 8. Experiment Details (10 subsections, EN, first-person)

Wrapper `py-20 bg-neutral-50`, `space-y-12`.

1. **Problem** — gfx1031 awkward spot (not gfx1030, docs target 6800/6900). Override `10.3.0` tried → FA off, top_k CPU fallback, random core dumps. Required native. Link back to EXP 008 Phase 1.
2. **Specifications (7 pts)** — TheRock 7.14 exact `hipcc --version`; commit 10307 exact `llama-server --version`; rocminfo + lspci proof; Ornith-35B split config overview; Vulkan baseline numbers; thread/batch tuning results (4 vs 3, 1024 vs 2048 +10C); multi-engine list.
3. **Hypothesis** — 6700XT clock gap should show in prefill (compute-bound), not decode (bandwidth-bound). Falling far short of ~300 tok/s 3060 reference on Vulkan signals software bottleneck, fixable at kernel dispatch level.
4. **Experiment** — Builds kept failing (documented head_dim RDNA2 config + guesses 1024/2048/4096 all failed). Stopped, watched terminal, ran Vulkan baseline in btop, correlated kernel activity to `fattn.cu` dispatch → 512 from observation, not docs.
5. **What Failed (3)** — (a) documented head_dim config failed, (b) power-of-2 guesses failed, (c) override path failed functionally (FA/top_k/dumps). Each with symptom.
6. **Iteration** — From spoofing runtime to fixing dispatch: force MMA for 512 on `amd_mfma_available`, avoid tile shared-memory limit. FA stable → KV q8_0 viable → 131K usable. One fix unblocked chain.
7. **Evidence (8 tiles)** — `prefill peak 581.81`, `floor 502.11 at 20K+`, `decode 22–23 flat 40K+`, `ctx 131072 stable`, `KV q8_0`, `threads 4 @83C vs 3 @95C spikes`, `batch 1024 (2048 +10C no gain)`, `engines 6 native`.
8. **Result** — Native ROCm/HIPBLAS/ROCBLAS no override, FA on, KV q8_0, 131K stable, Ollama moved from Vulkan to native ROCm too.
9. **What I Learned** — Generalizable is method, not number 512: stable-backend baseline + kernel watch + dispatch correlation works without kernel theory. Honest unknown: why SGLang/vLLM also fixed (no shared code, parallel ROCm changes untracked). Invite explanation. Author background: creative director, self-taught, empirical.
10. **Practical Implication** — For under-documented AMD: don't hardcode 512 blindly, check failure mode + versions first. For repro: pin HIP 7.14 + commit fc3f10b + board/CPU/RAM context. EXP 008 → EXP 009 → EXP 001 chain: stack → native → 35B usable.

---

## 9. FAQ (3)

```json
[
 {"Q":"Why require native instead of HSA_OVERRIDE? Override works for many.","A":"On this card override gave FA-off, top_k CPU fallback tanking decode, and random mid-inference core dumps. For 35B split at 70K+ that is unusable. Native was slower to achieve but stable — rocminfo proves no spoofing."},
 {"Q":"Is head_dim 512 a universal fix I can copy?","A":"No. It is conditional on amd_mfma_available and the tile shared-memory limit hit on this setup/commit. Validate failure mode, HIP build (7.14.60850), and commit (fc3f10b38) first. Method generalizes, number doesn't."},
 {"Q":"Why did SGLang/vLLM start working too if they don't share ggml code?","A":"Unknown — author flags this honestly. Likely parallel ROCm-level changes resolved alongside, untracked. If you understand shared-memory mechanics or TheRock packaging that explains it, author invites explanation."}
]
```

---

## 10. Configuration Template (2 blocks)

**Block 1: The fix (`fattn.cu`):**
```cpp
// Force MMA kernel for head_dim 512 on AMD to avoid tile kernel shared memory limit
if (amd_mfma_available(cc) && Q->ne[0] == 512) {
  return BEST_FATTN_KERNEL_MMA_F16;
}
return BEST_FATTN_KERNEL_TILE;
```

**Block 2: Full `llama-server` (Ornith-35B split) + notes:**
```bash
BASE="$HOME/Documents/Model LLM/Ornith-1.0-35B"
MODEL="$BASE/Ornith-1.0-35B-UD-IQ4_NL.gguf"
PORT=8082
~/Projects/llama2/build-rocm-test/bin/llama-server \
 --model "$MODEL" --host 0.0.0.0 --port "$PORT" \
 --n-gpu-layers 99 --threads 4 --threads-batch 4 --n-cpu-moe 22 \
 --ctx-size 131072 --batch-size 1024 --ubatch-size 1024 --keep 20480 \
 --cache-type-k q8_0 --cache-type-v q8_0 --swa-checkpoints 24 \
 --checkpoint-min-step 2048 --embd-normalize 0 --no-kv-unified --kv-offload \
 --jinja --reasoning-preserve --flash-attn on --parallel 1 \
 --cache-ram 8192 --cache-idle-slots \
 --temp 0.6 --top-k 20 --top-p 0.95 --min-p 0.1 \
 --repeat-penalty 1.1 --repeat-last-n 512 --alias udinllama --log-verbosity 4
```
Bullets: `--n-cpu-moe 22` from VRAM headroom (don't blind-copy); threads 4 bandwidth-bound + thermal (3 faster on paper, 95C spikes vs 83C); batch 1536/2048 no prefill gain +10C; q8_0 + kv-offload for 12GB long ctx; `--no-kv-unified` only for parallel 1; cache-ram/idle-slots OOM safety net.

---

## 11. Non-Technical Summary (summary-callout, after Visualization)

Heading: `For those of you unable to read this data from technical standpoint, here is the conclusion:`

1. Running a big 35B model split across 12GB GPU + CPU means waiting for the prompt to process (70K+ context) is what hurts — not writing answers.
2. Easy mode (Vulkan) was okay at 16–32K but painful beyond. Pretending my card is a different model caused crashes and silent slowdowns.
3. I insisted on native recognition, watched a stable mode to find the real kernel, and forced the matrix-core path for one specific size — one line.
4. Result: ~580 pages/sec peak, staying above 500 with 20K already loaded, 131K context stable, works across 6 apps — not just one.
5. I'm self-taught, found this by observation — if you know why it works deeper, I'd love to learn. Repro details + versions inside; don't copy the number blindly.

---

## 12. Disclaimer + Links

> **Disclaimer — Version-pinned, not universal.** Validated on Ubuntu 26.04 + TheRock HIP 7.14.60850 + commit fc3f10b38 + Sapphire PULSE 6700 XT + i5-11400F/16GB DDR4. Commit drift is real; other ROCm sources/distros may differ; 512 is conditional. Numbers are sustained-session logs, not cold-start peaks.
>
> **Upstream:** `ggml-org/llama.cpp#26702` · **Repro write-up:** Reddit `r/LocalLLM` native gfx1031 post · **Previous phase:** `exp008.html`

Inline links in hero (Reddit + GitHub + EXP 008), Evidence, Config, Footer CTA teaser EXP 001 chain.

---

## 13. File Checklist

* New: `public_html/exp009.html` from `template-exp000.html`
* Images 6 placeholders in `images/experiments009/`
* After review: `experiments-list.html` (card 9, 8→9), `sitemap.xml` (exp009 + 6 images, list lastmod bump), `index.html` (Selected card)
* No new deps.

---

## 14. Build Steps

1. User ACCs this plan.
2. Generate `exp009.html` with placeholders, same order as exp008 (summary after visualization).
3. Verify http.server + charts + mobile, links open new tab.
4. JANGAN update list/sitemap/index dulu — tunggu review exp009.
5. Review → publish list/sitemap/index.

---
**Ready for review.** Judul rekomendasi di atas; gampang ganti sebelum generate. Bilang `gas` + judul final (atau pakai rekomendasi) — gue langsung generate `exp009.html`.
