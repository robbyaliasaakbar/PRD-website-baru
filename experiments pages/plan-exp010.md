# PLAN — Experiment 010

**Building a Production-Ready Job Tracker UI With Only a 35B Local Coder — No Framework**

> **Location:** `/public_html/exp010.html`
> **Plan file:** `PRD/plan-exp010.md`
> **Date publish:** `2026-09-09` (2 hari sebelum 2026-09-11, locked sesuai request Bang Rob)
> **Status:** `Stable` — frontend locked, live di GitHub Pages
> **Lang:** `en` (full English di page, PRD ini Indonesian biar gampang QC)
> **Author:** Robby Aliasa Akbar
> **Live demo:** `https://robbyaliasaakbar.github.io/jobtracker` (frontend `index.html` + `auth.html`)
> **Sources:** folder `public_html/jobtracker/` (`index.html`, `auth.html`, `css/style.css`, `js/guard.js`, `js/store.js`, `js/app.js`, `js/auth.js`) + backend `backend/PRD/prd-backend.md` (hanya untuk teaser, detail full di EXP 011)
> **Relation to EXP 011:** EXP 010 = frontend only. EXP 011 = backend entrypoint (`:7002`, PHP 8.3 + SQLite, 6 endpoint, OTP Gmail, token opaque). EXP 010 wajib kasih kisi-kisi tapi JANGAN bongkar backend detail.
> **Claim locked:** dibangun dengan local LLM `Tiel-Coder-35B-A3B-MTP-UD-Q4_K_S.gguf` via `llama.cpp-server` dalam `4-5 jam seharian`. Flag config inference TIDAK dishare (sudah dibahas di EXP 001 + pages lain, cukup refer).

---

## 1. Objective

Answer the research question:

> **Can a production-ready, recruiter-usable Job Tracker frontend — real auth flow, CRUD, search/filter/sort, responsive table-to-cards — be shipped in 4-5 hours with only a 35B local coder, no framework, just HTML5 + Tailwind CDN + Vanilla JS?**

This page documents **frontend only**. Backend ada dan real (bukan statis), tapi full breakdown-nya di EXP 011. EXP 010 harus bikin audience — vibe coders, orang awam, recruiters, CTO — paham dalam 3 menit: ini webapp beneran yang live, bukan mockup.

**Key stance:** *No framework needed for internal tools at this scale. Vanilla + clear separation (`guard.js` = satpam, `store.js` = otak database, `app.js` = otak tampilan, `auth.js` = pintu depan) is easier to audit for CTO and easier to vibe-code for beginners. Backend running locally on personal PC for now (not 24/7 VPS) — honest limitation, documented as expected behavior, not a bug.*

Source of truth: JANGAN ngarang di luar file yang sudah dibaca. Semua angka/nama file/endpoint harus ngikutin yang ada di `jobtracker/` + `backend/PRD/prd-backend.md`.

---

## 2. Audience & Goals

**Primary:**
1. Vibe coders + orang awam — butuh bukti local AI bisa jadiin webapp real dalam sehari.
2. Recruiters + CTO (C-level) — butuh bukti portfolio live, auth beneran, code auditable, honest tentang limitasi.

**Secondary:** Pembaca EXP 001 / EXP 005 / EXP 006 — rantai baru: EXP 010 (frontend Job Tracker) → EXP 011 (backend entrypoint).

**Goals:**
1. Buktikan **live + usable**: tombol demo ke `/jobtracker` + 5 screenshot sebagai fallback kalau backend lagi mati.
2. Buktikan **auth real**: 5 mode dalam 1 `auth.html` (login, daftar, OTP 6-digit, lupa, reset) + `guard.js` (`GET /api/me`, tendang ke `auth.html` kalau no token).
3. Buktikan **CRUD real**: form kiri (company, position, date, status 7 tahap, portal, link) + tabel desktop + kartu mobile + search + filter status + sort + toast.
4. Buktikan **no framework**: 0 build step, Tailwind CDN + Vanilla JS only, ringan di GitHub Pages.
5. Jaga credibility: status `Stable` untuk frontend, tapi `Live Demo Status` jujur bilang backend masih local PC, belum VPS 24/7.

---

## 3. Styling & Stack (plek EXP 009, wajib seragam)

* **Stack:** `HTML5 + Tailwind CSS via CDN + Vanilla JS` only. JANGAN load `charts.js` / Chart.js (user locked: gaperlu chart). Cuma `js/main.js` untuk `data-reveal` + navbar.
* **Colors:** `ink #0C0C0C / paper #FFFFFF / accent #D70000` — sama persis.
* **Fonts:** `Manrope (display) / Inter (body) / JetBrains Mono (mono)` — sama persis.
* **Tokens wajib dipakai:** `eyebrow`, `status-badge status-stable`, `border border-black/10 rounded-2xl`, `summary-callout`, `quote-block`, `detail-label`, `stat-tile`, `code-block`, `code-inline`, `data-reveal`, `bg-grid`, `bg-dark-section`, `nav-link nav-active`, `arrow-link`, `btn-primary`, `btn-outline`, `placeholder-shot` (dashed placeholder style sama kayak EXP 008/009).
* **Navbar/Footer:** Copy 1:1 dari `exp009.html`. Ganti hanya: breadcrumb `Experiment 009 → Experiment 010`, `og/url`, `canonical`, nomor experiment. `nav-active` tetap di `Experiments`. Footer link + copyright sama.
* **Internal links wajib jalan:** `Home (/)`, `profile.html`, `experiments-list.html`, `contact.html`, `Back to Experiments`, `Have a problem worth exploring?`. Tambah teaser link `EXP 011 (backend entrypoint — coming next)` dalam bentuk teks disabled (belum ada file), JANGAN bikin link mati 404.
* **Order sections (locked):** Hero + Breadcrumb → System Requirements → Screenshots → Live Demo → Non-Technical Summary → Experiment Details → Frontend Code → FAQ → Live Demo Status (notice PC local) → Disclaimer + Teaser EXP 011 → Footer CTA.
* **Responsive:** mobile 375px, tablet 768px, desktop 1280px. Tabel `hidden lg:block`, kartu `lg:hidden` — ceritakan di page, bukan cuma di code.

---

## 4. SEO & Metadata

```html
<title>Experiment 010 — Building a Production-Ready Job Tracker UI With Only a 35B Local Coder — Robby Aliasa Akbar</title>
<meta name="description" content="Job Tracker frontend built in 5 hours with only Tiel-Coder 35B local LLM: real OTP auth, CRUD, search/filter/sort, responsive table-to-cards, no framework, live on GitHub Pages.">
<link rel="canonical" href="https://robbyaliasaakbar.github.io/exp010.html">
<meta property="og:title" content="Experiment 010 — Real Job Tracker UI With Local AI, No Framework">
<meta property="og:description" content="5-hour local-AI build: OTP login, CRUD lamaran, responsive UI, vanilla JS. Backend local PC for now, full entrypoint in EXP 011.">
<meta property="og:image" content="https://robbyaliasaakbar.github.io/images/experiments010/dashboard-desktop.webp">
<script type="application/ld+json"> Article + BreadcrumbList + FAQPage
```

**Keywords:** `["job tracker frontend", "local coder 35B", "Tiel-Coder", "vanilla JS CRUD", "OTP auth frontend", "Tailwind GitHub Pages", "responsive table cards", "vibe coding portfolio"]`
**Dates:** `datePublished 2026-09-09`, `dateModified 2026-09-09`.
**Robots:** `index, follow, max-image-preview:large` (sama kayak EXP 009, bukan noindex).

**After review (JANGAN sekarang, tunggu ACC Bang Rob):**
* `experiments-list.html` card position 10 + `numberOfItems:9 → 10`
* `sitemap.xml` add `exp010.html` + 5 images
* `index.html` add card to Selected Experiments

---

## 5. System Requirements (8 tiles)

Grid `grid-cols-2 md:grid-cols-3` (tile terakhir full-width di mobile bila ganjil), style sama kayak EXP 009.

| Tile | Label | Value |
|------|-------|-------|
| 1 | CPU | `Intel Core i5-11400F` (reuse EXP 009) |
| 2 | RAM | `16GB DDR4 3200MT/s` (reuse) |
| 3 | GPU | `RX 6700 XT 12GB gfx1031` (reuse, buat inference lokal) |
| 4 | OS | `Ubuntu Desktop 26.04 LTS` (reuse) |
| 5 | Model | `Tiel-Coder-35B-A3B-MTP-UD-Q4_K_S.gguf` |
| 6 | Engine | `llama.cpp-server` (flag TIDAK dishare, refer ke EXP 001) |
| 7 | Stack | `HTML5 + Tailwind CDN + Vanilla JS` |
| 8 | Time / Status | `4-5 hours in one day / STABLE` |

Note under grid (EN, 1 baris):
> `Built with local model above via llama.cpp-server. Inference flags covered in EXP 001 — not repeated here. Frontend runs browser-only on GitHub Pages, no build step.`

---

## 6. Screenshots — Gallery (4 real, 1 postponed — update 2026-09-11)

Files real di `public_html/images/experiments010/` (dari Bang Rob):

1. `auth-html.webp` (335x477) — form login. Alt: `Job Tracker login form with email and password`.
2. `otp.webp` (562x710) — mode daftar + OTP 6 digit. Alt: `Register and 6-digit OTP verification flow`.
3. `desktop-version.webp` (980x823) — dashboard full desktop. Ini jadi `og:image` + `twitter:image` + JSON-LD `image` (width 980 height 823). Alt: `Job Tracker dashboard with stats filter form and table`.
4. `mobile-version.webp` (339x727) — kartu mobile, layout `max-w-xs mx-auto` centered karena portrait. Alt: `Mobile cards view replacing table on small screens`.
5. `filter-toast.webp` — POSTPONED (request Bang Rob: data belum banyak, filter belum bisa di-screenshot, ngabisin waktu. JANGAN pake placeholder, langsung hapus dari page).

Layout final:
* Wrapper `bg-neutral-50 py-20`, `data-reveal`.
* Row 1 (2 cols): `auth-html` | `otp`
* Row 2 (full): `desktop-version` (hero shot, og:image)
* Row 3 (centered `max-w-xs`): `mobile-version`
* Tiap figure: `border rounded-2xl overflow-hidden bg-paper shadow-sm` + `figcaption` uppercase `text-xs font-display`. NO `placeholder-shot` (kecuali definisi CSS sisa, harmless).

Kenapa 4 ini (buat QC): cover auth, CRUD, mobile. Filter nyusul pas data udah banyak.

---

## 7. Live Demo (pengganti Visualization, NO CHART)

Section putih `py-20`, setelah Screenshots.

* Title: `Try it live — not a mockup.`
* Sub: `Frontend live on GitHub Pages. New here? Register first, verify the OTP, then login to get your access token (see Live Demo Status for online hours).` — locked: jelasin alur daftar → OTP → login → token (bukan cuma guard redirect), request Bang Rob.
* 1 button only (open new tab `target="_blank" rel="noopener"`): `btn-primary bg-ink` → `https://robbyaliasaakbar.github.io/jobtracker/` label `Open Job Tracker ↗` — locked: tombol `Open Login / OTP` DIHAPUS di section ini karena `guard.js` otomatis tendang ke `auth.html` kalau belum ada token (request Bang Rob).
* Mono note: `Frontend: /jobtracker/ • Auth: /jobtracker/auth.html • API base: window.JOB_API (tunnel, changes when PC restarts)`
* JANGAN embed iframe (berat + CORS + token issue). Cukup tombol + screenshot.

---

## 8. Experiment Details (9 subsections, EN, first-person)

Wrapper `py-20 bg-neutral-50`, `space-y-12`, pola `detail-label` kiri + konten kanan (sama kayak EXP 009).

1. **Problem** — Tiap apply kerja butuh catat manual, scattered di spreadsheet/chat. Internal tool harus: auth beneran (bukan fake login), CRUD cepat, searchable, enak di HP. Framework overkill buat 1 user + portfolio.
2. **The 5-Hour Build** — Built in one day, 4-5 hours total, with `Tiel-Coder-35B` via `llama.cpp-server`. AI handled syntax + boilerplate, decisions stayed with me. No flag sharing here (see EXP 001 for inference setup). Honest scope: frontend only in this page.
3. **Hypothesis** — Vanilla separation (`guard / store / app / auth`) is enough for production-feel internal tooling and easier to review than framework magic. Tailwind CDN gives portfolio-grade look with zero build. JANGAN sebut eksplisit audience (`vibe coders / CTO / recruiter`) di page.
4. **What I Built (frontend tour)** —
   a. `auth.html` 5 modes in 1 file: login, daftar (nama+telp+email+password), OTP verify, lupa, reset. Show/hide password via `data-lihat`. Green/red `auth-msg` box.
   b. `index.html` dashboard: 4 stat cards, funnel container (no chart in EXP 010 story, focus on CRUD), filter bar (search company/position/portal + status 7 + sort terbaru/terlama).
   c. Form left: company/position/portal required, date default today, status 7 (`baru, interview-hr, technical-test, interview-user, offering, diterima, ditolak`), link optional → `Buka ↗`.
   d. Table desktop (`min-w-[800px]`) vs cards mobile (thumb-friendly Edit/Hapus). Empty state `Belum ada lamaran...`.
   e. Feedback: `toast()` 3s (`bg-ink` success, `bg-accent` error), `confirm()` before delete, `lastAddedId` row highlight.
5. **How Frontend Talks to Backend (teaser only, NO deep dive)** — `window.JOB_API` override (GitHub Pages tunnel) else `location.hostname:7002`. `store.js` exposes `getAll/add/update/remove` sync-read cache + async write. `guard.js` checks `GET /api/me` with `Bearer` token in `localStorage key jobTracker.token`, redirects to `auth.html` if missing/invalid, read-only toast if server unreachable. Full contract in EXP 011.
6. **What Failed** — (a) Beda port = beda origin → fetch diblokir browser sampai CORS dibenerin di backend (disinggung, detail di EXP 011). (b) Chart offline edge: kalau Chart.js gagal load, persentase tetap jalan (defensive `typeof Chart === undefined`). (c) Token expired 1 jam → ditendang ke login, awalnya dikira bug, ternyata expected.
7. **Evidence (6 tiles `stat-tile`)** — `5 modes auth in 1 file`, `7 status stages`, `2 views table+cards`, `3-way filter search+status+sort`, `3s toast feedback`, `0 framework / 0 build step`. JANGAN ngarang angka tok/s di sini (itu EXP 009).
8. **Result** — Stable frontend live on GitHub Pages. Auditable Vanilla files, responsive, auth-guarded. Backend local limitation documented separately, not hidden.
9. **What I Learned + Practical Implication** — Small, clear files > 1 giant file when iterating quickly. Keep auth decisions on server (opaque token + guard + generic error messages) biar frontend tetap simple dan gampang di-review, walau cuma internal tool. JANGAN sebut eksplisit `vibe coders / CTO / recruiter` di page (kesan ngincer). Chain: `EXP 010 (UI you can click) → EXP 011 (where data goes) → EXP 001 (what machine runs the AI)`.

---

## 9. Frontend Code (2 blocks only, JANGAN bocorin backend secret)

**Block 1: Guard (`js/guard.js` essence, dipersingkat):**
```js
// No token → kick to auth.html before app renders
const token = localStorage.getItem("jobTracker.token");
if (!token) location.replace("auth.html");
fetch(GUARD_API + "/api/me", {
  headers: { Authorization: "Bearer " + token }
}).then(r => { if (!r.ok) location.replace("auth.html"); });
```

**Block 2: Store pattern (`js/store.js` essence):**
```js
// app.js never touches localStorage directly
// getAll() reads cache sync, add/update/remove write via API
window.JOB_API = "https://aispec.tail06293c.ts.net"; // tunnel, changes
```

Note: JANGAN tampilkan `.env`, `SMTP_PASS`, `auth.db` isi, atau full `auth.php`. Itu jatah EXP 011.

---

## 10. Non-Technical Summary (summary-callout, after Live Demo)

Heading (wajib sama pola EXP 009):
`For those of you unable to read this data from technical standpoint, here is the conclusion:`

1. I needed a place to track job applications that works on laptop and phone, with real login.
2. I built it in an afternoon with a local AI coder running on my own GPU — no cloud AI, no framework.
3. You can click and try it live: login, add, edit, search. Looks simple because internal tools should be simple.
4. If login sometimes fails, my home PC (the database) is likely off — photos below still show how it works.
5. Next page explains where the data goes and how login codes work.

---

## 11. FAQ (3, EN, masuk JSON-LD FAQPage juga)

```json
[
  {"Q":"Is this just a static HTML demo?","A":"No. Frontend is static on GitHub Pages, but auth + data go to a real backend (PHP 8.3 + SQLite) running on my PC for now. Guard checks token via /api/me on every open. Full entrypoint in EXP 011."},
  {"Q":"Why no framework? Is vanilla production-ready?","A":"For this scale yes. 4 small files (guard/store/app/auth) are easier to audit and faster to vibe-code than framework boilerplate. No build step means GitHub Pages deploy is trivial."},
  {"Q":"Why does login sometimes fail?","A":"Backend is not on a 24/7 VPS yet — it lives on my personal PC, online daily 08.00–21.00 WIB (UTC+7) when my PC is on. Outside those hours API is unreachable and login can't succeed. Frontend shows a toast and stays readable. Screenshots remain as proof."}
]
```

---

## 12. Live Demo Status + Disclaimer + Teaser EXP 011

### 12a. Live Demo Status (2 kolom status + amber callout, sebelum footer, request khusus Bang Rob)
Title: `Live Demo Status — Local PC, Not Yet 24/7 VPS`
Layout: `grid sm:grid-cols-2 gap-4` 2 cards + amber notice di bawahnya. `data-reveal`.

Card kiri `What works now` (border emerald, ✅):
- Frontend live on GitHub Pages
- Auth flow (login, register, OTP, reset)
- CRUD lamaran
- Search / filter / sort
- Responsive UI (table → cards)
- Real backend integration
- Usable by invited testers
- Backend online daily 08.00–21.00 WIB (UTC+7) when my PC is on

Card kanan `What isn't production yet` (border amber, 🚧):
- Backend not yet 24/7
- Deployment still local (personal PC)
- Early-stage features
- Collecting user feedback
- Next iteration not yet done

Body (EN) amber callout di bawah 2 cards:
> `As of now the backend runs locally on my personal PC, not yet published online to a VPS for 24/7 running. It is live daily 08.00–21.00 WIB (UTC+7) when my PC is on. If my PC is off, the backend is also off and the login page may fail to reach the server. This is expected. The frontend + screenshots on this page still show exactly how it works.`

Style: cards `border rounded-2xl p-6 bg-paper` (kiri `border-emerald-200`, kanan `border-amber-300 bg-amber-50/50`) + notice `quote-block` + `border-amber-300 bg-amber-50`, bukan `bg-accent/10` biar beda dari error.

### 12b. Disclaimer (Version-pinned, same pattern)
> **Disclaimer — Frontend stable, backend local.** Validated on above rig + `Tiel-Coder-35B` + `llama.cpp-server` (flags in EXP 001). Times are build times (4-5h), not benchmark peaks. Demo backend online daily 08.00–21.00 WIB (UTC+7) when home PC on, unreachable outside those hours. I'm self-taught — corrections welcome.
>
> **Live:** `/jobtracker/` · **Auth:** `/jobtracker/auth.html` · **Next:** `EXP 011 backend entrypoint (PHP 8.3 + SQLite auth.db, Docker :7002, 6 endpoints, Gmail OTP, opaque token 1h)` · **Inference background:** `exp001.html`

Inline links hero must include: Live demo + `exp001.html` + `EXP 011 teaser (text, no 404)`.

---

## 13. File Checklist

* New: `public_html/exp010.html` from `template-exp000.html` (copy navbar/footer 1:1 dari `exp009.html`)
* Images: bikin folder `public_html/images/experiments010/` + 5 `placeholder-shot` + commented `<img>` drop-in
* NO new deps. NO `charts.js`. Hanya `css/style.css` + `js/main.js`
* After ACC Bang Rob (JANGAN bareng build): `experiments-list.html` (card 10, 9→10, ItemList JSON-LD), `sitemap.xml` (exp010 + 5 images, bump lastmod list), `index.html` (Selected card)
* Validasi: `title + meta description EN`, `alt text` semua img, `heading hierarchy h1→h2`, `canonical/OG/Twitter/Article+Breadcrumb+FAQ JSON-LD`, `data-reveal` tidak bikin konten invisible tanpa JS

---

## 14. Build Steps

1. Bang Rob QC PRD ini → ACC atau revisi (saat ini: DONE, tunggu kata `gas build`).
2. Generate `exp010.html` dengan placeholders, order §3, EN full, tanpa chart.
3. Verify: `python3 -m http.server` + cek mobile 375px, tablet, desktop, semua internal link, tombol demo `target=_blank`, `prefers-reduced-motion`.
4. JANGAN update list/sitemap/index dulu — tunggu review exp010.
5. Review → publish list/sitemap/index → done, lanjut PRD EXP 011.

---
**Ready for QC.** Kalau Bang Rob ACC, balas `gas build` — gue langsung generate `exp010.html` sesuai PRD ini, satu halaman penuh, valid HTML5 + Tailwind CDN + Vanilla JS.
