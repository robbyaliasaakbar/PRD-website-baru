# PLAN — Experiment 012

**Multi-User CRM With Server-Side Pagination, React + Vite, Live on GitHub Pages**

> **Location:** `/public_html/exp012.html`
> **Plan file:** `PRD/plan-exp012.md`
> **Date publish:** `2026-09-14` (locked request Bang Rob)
> **Status:** `Live` — frontend live di GitHub Pages, backend local PC saat nyala
> **Lang:** `en` (full English di page, PRD ini Indonesian biar gampang QC)
> **Copy rules (locked, owner request):** page WAJIB full English dengan grammar 95% (advanced, manusiawi, bukan native-perfect). Larangan: JANGAN pakai `;` dalam 1 kalimat (pecah jadi 2 kalimat pendek). JANGAN pakai em dash `—` (ganti koma/titik/kurung). Cari bagian rawan tapi tetap masuk akal: sesekali article (a/the) meleset, preposisi kaku, word order agak literal. Tone friendly. Khusus Non-Technical Summary: orang awam harus paham maksud dan tujuan, JANGAN bikin mereka berasa bego. Heading summary TETAP verbatim pola 11 EXP lain (seragam template).
> **Author:** Robby Aliasa Akbar
> **Live demo:** `https://robbyaliasaakbar.github.io/miniLeads/` (React + Vite build, `index.html` + `assets/` di root folder)
> **Live API:** data `https://aispec.tail06293c.ts.net:8443` (funnel ke docker `:7005`) + auth `https://aispec.tail06293c.ts.net` (funnel ke `:7002`, backend yang sama kayak EXP 011)
> **Sources:** folder `public_html/miniLeads/` (`src/App.jsx`, `src/api.js`, `src/auth.js`, `src/Auth.jsx`, `src/Settings.jsx`, `src/StatusChart.jsx`, `src/StatusDonut.jsx`, `src/StatusLine.jsx`, `src/AddLeadForm.jsx`, `src/ImportCsv.jsx`, `.env.production`, `index.template.html`, `vite.config.js`) + backend `backend-server-minileads/` (`server.js`, `env.js`, `db.js`, `normalize.js`, `docker-compose.yml`, `leads.db` 2049 rows) — JANGAN buka `.env` asli, JANGAN tampilkan password / token / OTP asli
> **Relation:** EXP 010 = frontend vanilla pertama (Job Tracker) → EXP 011 = backend auth dipakai bareng → EXP 012 = app CRM ini (miniLeads React, auth-nya reuse backend yang sama kayak jobtracker/EXP 010). Rantai: klik UI (010) → dari mana token (011) → app kedua yang reuse token sama (012) → mesin AI yang ngebangun semua (001).
> **Auth disclaimer (wajib render di page, EN):** login di app ini pakai backend auth yang SAMA kayak jobtracker (EXP 010/011). Bukan copy, bukan app terpisah. Detail backend tetap di EXP 011, di sini ringkasan + link.
> **Claim locked:** dibangun dengan local LLM sama persis `Tiel-Coder-35B-A3B-MTP-UD-Q4_K_S.gguf` via `llama.cpp-server`. Flag inference TIDAK dishare (refer EXP 001).

---

## 1. Objective

Answer the research question:

> **Can a second, heavier frontend — React + Vite, 2049-row CRM with server-side pagination, tap-to-filter charts, CSV import/export, dark mode — reuse the SAME auth backend as EXP 011 with zero auth re-code, and ship live on GitHub Pages talking to a home-PC backend over Tailscale Funnel?**

This page documents **the full miniLeads story in one place**: multi-user data scope, server-side pagination truth, React build pipeline + Pages deploy pattern, and the two-funnel networking that makes a static page talk to a home PC. Backend auth detail tetap di EXP 011 (jangan diulang), frontend vanilla pattern tetap di EXP 010 (jangan diulang).

**Key stance:** *One auth backend, many frontends — proven with customer #2. React is justified here (not for hype): filterable charts + paginated tables + CSV flows are state-heavy enough that vanilla would sprawl. But Vite env is build-time, not runtime — and `localhost` in a shipped bundle is a bug that passes every local test. Static hosting + home-PC backend over funnel is honest local-first: live when the PC is on, unreachable when off, documented as expected — not a bug.*

Source of truth: JANGAN ngarang di luar file yang dibaca. Nama file, endpoint, env key, angka (2049, 103 pages, 367KB), URL funnel harus plek source.

---

## 2. Audience & Goals (internal, JANGAN sebut eksplisit di page)

**Primary (internal note):** builders yang mau reuse 1 auth buat app kedua + reviewers yang butuh bukti "React di Pages + backend rumahan" itu real, bukan mockup.
**Secondary:** Pembaca EXP 010/011 — mereka lihat rantai lengkap: UI pertama (010) → backend (011) → pelanggan kedua (012) → mesin (001).

**Goals:**
1. Buktikan **reuse real**: login miniLeads pakai token + endpoint yang SAMA kayak jobtracker (`/api/login`, `/api/me` di `:7002`), tanpa ngoding auth baru. Bukti: `AUTH_API` nempel ke funnel 443, `apiMe` gate sebelum data diambil.
2. Buktikan **data jujur di skala**: 2049 rows, server-side pagination `{ total, page, limit, totalPages }`, limit jepit 1–100, over-page clamp. Dashboard (total 2049, New 270, Qualified 321, top channel Organic Search 521) konsisten sama tabel.
3. Buktikan **React beralasan**: 3 chart tap-to-filter (bar, donat, garis) + search/filter 4 kolom + import CSV + manual dedup + export CSV + settings tema — semua state nyambung, 1 halaman.
4. Buktikan **deploy pattern Pages + funnel**: build sekali dengan URL publik ditempel (`VITE_API_URL`, `VITE_AUTH_URL`), `dist/` ke root (`index.html` + `assets/`), 2 funnel (443 auth, 8443 data). Plus efek `localhost` trap sebagai failure jujur.
5. Jaga credibility: status `Live` untuk frontend, tapi `Live Status` jujur backend masih local PC + funnel. Bukan 24/7 VPS.


---

## 3. Styling & Stack (plek EXP 011, wajib seragam)

* **Stack page ini:** `HTML5 + Tailwind CSS via CDN + Vanilla JS` only (page artikelnya, bukan app-nya). JANGAN load `charts.js` / Chart.js. Cuma `js/main.js` untuk `data-reveal` + navbar.
* **Stack app yang didokumentasikan:** `React 18 + Vite 6 + Chart.js 4` (app) · `Node + Express + SQLite` (`:7005`, docker `network_mode: host`) · auth reuse `:7002` (EXP 011).
* **Colors:** `ink #0C0C0C / paper #FFFFFF / accent #D70000` + emerald/amber untuk status cards.
* **Fonts:** `Manrope (display) / Inter (body) / JetBrains Mono (mono)` — sama persis.
* **Tokens wajib dipakai:** `eyebrow`, `status-badge status-live`, `border border-black/10 rounded-2xl`, `summary-callout`, `quote-block`, `detail-label`, `stat-tile`, `code-block`, `code-inline`, `data-reveal`, `bg-grid`, `bg-dark-section`, `nav-link nav-active`, `arrow-link`, `btn-primary`, `btn-outline`.
* **Navbar/Footer:** Copy 1:1 dari `exp011.html`. Ganti hanya: breadcrumb `Experiment 011 → Experiment 012`, `og/url`, `canonical`, nomor experiment. `nav-active` tetap di `Experiments`.
* **Internal links wajib jalan:** `Home (/)`, `profile.html`, `experiments-list.html`, `contact.html`, `Back to Experiments`, `Have a problem worth exploring?`. Tambah link rantai: `EXP 010 (first customer)`, `EXP 011 (the shared backend)`, `EXP 001 (the machine)`.
* **Order sections (locked):** Hero + Breadcrumb → System Requirements → Screenshots → Live Demo → Non-Technical Summary → Experiment Details → Failure Log → Frontend Code → Backend Notes → Networking (2 funnel) → FAQ → Live Status → Disclaimer + Teaser rantai → Footer CTA.
* **Responsive:** mobile 375px, tablet 768px, desktop 1280px. Tabel app `hidden lg:block`, kartu `lg:hidden` — ceritakan di page.
* **Screenshot note:** 8 file real di `images/experiments012/` (homepage/auth-login/auth-regis/setting-page, light+dark). Light = bukti utama, dark = bukti tema. JANGAN tampilkan password/token/OTP asli (cek ulang sebelum publish).

---

## 4. SEO & Metadata

```html
<title>Experiment 012 — Multi-User CRM With Server-Side Pagination, React + Vite, Live on GitHub Pages — Robby Aliasa Akbar</title>
<meta name="description" content="MiniLeads CRM with shared auth backend: React + Vite, 2049 leads, server-side pagination, tap-to-filter charts, CSV import/export, dark mode, live on GitHub Pages via Tailscale Funnel.">
<link rel="canonical" href="https://robbyaliasaakbar.github.io/exp012.html">
<meta property="og:title" content="Experiment 012 — Multi-User React CRM, Live on GitHub Pages">
<meta property="og:description" content="2049 leads, real pagination, charts that filter tables, funnel networking from static Pages to home PC.">
<meta property="og:image" content="https://robbyaliasaakbar.github.io/images/experiments012/homepage-light.webp">
<script type="application/ld+json"> Article + BreadcrumbList + FAQPage
```

**Keywords:** `shared auth backend, React CRM, server-side pagination, Tailscale Funnel, Vite GitHub Pages, local LLM app, Tiel-Coder, multi-user SQLite, CSV import export, tap filter charts`
**Dates:** `datePublished 2026-09-14`, `dateModified 2026-09-14`.
**Robots:** `index, follow, max-image-preview:large` (sama kayak 010/011, bukan noindex).

**After review (JANGAN sekarang, tunggu ACC Bang Rob):**
* `experiments-list.html` card position 12 + `numberOfItems:11 → 12`
* `sitemap.xml` add `exp012.html` + 8 images
* `index.html` hanya kalau diminta (tambah card EXP 012 di grid terbaru)

---

## 5. System Requirements (tabel, EN di page)

| Item | Spec |
|---|---|
| Live app | `https://robbyaliasaakbar.github.io/miniLeads/` (static, `index.html` + `assets/`) |
| Data API | `https://aispec.tail06293c.ts.net:8443` (funnel ke docker `:7005`, `network_mode: host`) |
| Auth API | `https://aispec.tail06293c.ts.net` (funnel ke `:7002`, same backend as EXP 011) |
| Data | 2049 replica leads (WIZ name redacted per approval), `leads.db` rebuilt via load, never committed |
| Build | `npm run build` → 43 modules → `dist/` → copied to `miniLeads/` root (bundle `index-D3BUY3DC.js` ~367KB / ~120KB gzip) |
| Local dev | `npm run dev` → `:7006` (localhost, `.env`, funnel URLs NOT used) |
| Browser | Any modern browser, 375px → 1280px. JS required (React). |

---

## 6. Screenshots (8, EN caption di page)

1. `homepage-light.webp` — hero "Leads Management", 4 stat tiles (TOTAL 2049, NEW 270, QUALIFIED 321, TOP CHANNEL Organic Search 521), analytics bar + donut + line, search row. Caption: full app, light theme, live data.
2. `homepage-dark.webp` — same view, dark theme, charts keep colors. Caption: theme flips UI, charts stay readable.
3. `auth-login-light.webp` — "Welcome back", username/email + password + eye toggle, LOG IN. Caption: same auth backend as EXP 011, zero re-code.
4. `auth-login-dark.webp` — same login, dark.
5. `auth-regis-light.webp` — "Create account", first/last, email, username, birthdate, password rules, REGISTER + SEND OTP. Caption: 5-mode auth.
6. `auth-regis-dark.webp` — same register, dark.
7. `setting-page-light.webp` — Settings: Account card (admin, email, role ADMIN, password masked) + Appearance Light/Dark picker. Caption: account readonly, theme in browser.
8. `setting-page-dark.webp` — same settings, dark, Dark picker checked red.

Alt text semua deskriptif + "Mini Leads CRM — Robby Aliasa Akbar". NO password/token/OTP terbaca di gambar.

---

## 7. Live Demo (tombol + fallback, EN di page)

* Tombol primer: `Open Live App — /miniLeads/` (link ke `https://robbyaliasaakbar.github.io/miniLeads/`).
* Teks jujur di bawah tombol: `Live frontend, home-PC backend. If the PC or funnel is off, login fails with a toast — expected, not broken. Screenshots above show the full flow.`
* JANGAN janji 24/7. JANGAN tulis kredensial asli.

---

## 8. Experiment Details (EN di page, 5 blok + sumber failure)

> Aturan mapping: tiap blok di bawah mencantumkan `← what-failed #X` sebagai rujukan QC. Nomor = nomor section di `Belajar/what-failed-crm-react.md`. SEMUA 26 failure dirender di page (§9: Tier 1 sorotan + Tier 2 full log).

### 8a. Why a second app on the same auth (← #9, #21, #4)
EXP 011 bikin 1 backend; EXP 012 buktiin pelanggan kedua: miniLeads panggil `/api/login` + `/api/me` yang SAMA, token key beda (`crm.token`) tapi verifikasi sama. `apiMe` gate: no token = no data fetch (← #21: token on EVERY fetch, export jadi button karena link `<a>` tidak bisa bawa header). Scope data: admin lihat semua, user biasa cuma baris `user_email` miliknya (← #9: upgrade additive-only — kolom nullable, dual-path login, response extended not changed, dites 8/8 di copy isolasi lalu dihancurkan). Nomor port dari 1 `.env` pilihan owner (← #4: 7005/7006/7002).

### 8b. Pagination that tells the truth (← #8, #20, #16)
`GET /leads` balikin `{ total, page, limit, totalPages, count, data }`, limit jepit 1–100, over-page clamp ke halaman terakhir (← #8: dulu `LIMIT 200` tapi klaim total — sekarang count beneran, bukan `rows.length`). Legacy tanpa param tetap 200. UI: pilihan 20/50/100, "Page X of 103", empty state jujur. Dataset dijaga 2049 persis tiap test: dummy dihapus, container + DB test dihancurkan (← #16 + #20: cleanup protocol + count verification, 0 orphans).

### 8c. Charts that ARE the filter (← #1)
Satu rumus warna ranking (`chartTheme.js`, hijau→kuning→merah) dipakai 3 chart (bar, donat, garis). Tap = filter tabel; tap lagi = clear. Analytics di semua layar (mobile vertikal, desktop berdampingan). Chart bukan dekorasi — dia kontrol. Build session ini: verifikasi CODE YANG DISERVE, bukan file di disk (← #1: dev server basi 2x — `curl` dev server, kill + tab baru, jangan "reload aja").

### 8d. Data in, data out, no duplicates (← #21, #10)
Import CSV + form manual → 1 pintu `POST /leads/ingest`. Backend dedup: phone_digits ≥7 digit atau email_norm sama + `user_email` sama = update, bukan dobel. Export CSV via fetch (link `<a>` tidak bisa bawa token — ← #21). Rate limiter sempat blokir script test sendiri HTTP 429 (← #10: guard terbukti jalan, termasuk ke pemiliknya — tunggu window 60s, retry).

### 8e. React justified, vanilla page kept (← #22, #11/#14, #2/#3)
App = React (state-heavy: 4 filter + pagination + 3 chart + CSV + tema + 2 view). Page artikel ini = vanilla (HTML + Tailwind CDN + `main.js`). Framework dipakai di mana state-nya butuh, tidak di mana statis cukup. UI BORING yang terbukti: hex eksplisit + `dark:` utilities, bukan sintaks variabel ajaib yang gagal diam-diam (← #22, supersedes #11). Copy produksi, bukan demo: grep adjectives sebelum ship (← #14). Env rule Vite: semua diawali `VITE_`, CLI > config (← #2 + #3).


---

## 9. Failure Log — SEMUA tampil (26 failure, 2 tier)

> Aturan: SEMUA 26 failure dari `what-failed-crm-react.md` dirender di `exp012.html` (EN). Alasan owner: kredibilitas = evolusi berpikir, bukan perfect. Dibagi 2 tier biar page tidak tenggelam. Rujukan `← #X` = section `what-failed-crm-react.md`. TIDAK ada yang INTERNAL ONLY.

### 9a. TIER 1 — sorotan (3 failure sesi live, panggung penuh: symptom + fix + lesson + evidence)

1. **`localhost` shipped in the bundle (← #23).** Built without funnel URLs → visitors' browsers called THEIR localhost → login + leads failed everywhere except dev PC. Fix: rebuild with `VITE_API_URL` + `VITE_AUTH_URL` baked in, verify by grepping bundle (`const Bo="https://aispec.tail06293c.ts.net:8443"` + `const lp="https://aispec.tail06293c.ts.net"`). Lesson: JS runs on visitors' devices. Evidence: bundle baru `index-D3BUY3DC.js` ~367KB.
2. **Funnel exposed only auth (← #24).** `:443 → :7002` worked, `:7005` had nothing — felt like "tunnel broken" while Tailscale was healthy (DERP Singapore 16ms). Fix: additive second funnel `--https=8443 → :7005` (tidak nimpa yang pertama). Lesson: one public port per backend (443/8443/10000). Evidence: `curl ...:8443/health` → `{"ok":true}`, dua-duanya di `serve status --json`.
3. **Build ate its own entry (← #25).** `dist/index.html` copied over source `index.html` → next build compiled the OUTPUT (4 modules, no funnel URLs, green build). Fix: `index.template.html` as eternal source, root `index.html` as shippable artifact. Lesson: module-count drop (43→4) = wrong entry, not wrong config. Evidence: rebuild final `43 modules transformed`.

Redacted: no passwords, tokens, OTPs, no `.env` content. Curl blocks pakai token dummy (`Bearer dummy` → error generik). JANGAN render: SMTP/pass, isi `.env`, OTP asli, nama WIZ asli.

### 9b. TIER 2 — full log (23 failure, ringkas: 1-2 kalimat + lesson per item, EN di page)

> Semua tampil. Tulis ringkas tapi jujur. Bukti angka tetap verbatim dari what-failed. Yang berbau secret (SMTP pass, isi `.env`, OTP asli, nama WIZ) tetap diredact — ceritakan kejadiannya, bukan secret-nya.

4. **Stale dev server served half-updated code, twice (← #1).** New features missing after hard refresh, then `rankColor is not defined` although disk was clean. Old Vite process + stuck tab, HMR never recovered. Fix: kill server, fresh tab. Lesson: verify the SERVED code, not files on disk.
5. **Hardcoded `--port` flag overrode `.env` (← #2).** CLI flags beat config files, so the app stayed on 5173 while `.env` said otherwise. Fix: port from config. Lesson: CLI beats config beats defaults.
6. **Vite ignores non-`VITE_` variables (← #3).** `FRONTEND_PORT` read as `undefined`. Fix: rename to `VITE_FRONTEND_PORT`. Lesson: test what the framework actually sees.
7. **Port soup confusion (← #4).** Five ports, nobody knew which was which. Fix: single `.env`, owner picks the numbers. Lesson: config naming belongs to the owner.
8. **Improvised folder structure without asking (← #5).** Right instinct (isolation), wrong execution (unilateral naming). Got called out, fixed with agreed names. Lesson: structure decisions belong to the owner.
9. **Executed on a question (← #6).** Owner asked "right?" and I killed a server instead of answering. Lesson: questions get answers only. Execution needs an explicit go.
10. **`pkill` matched my own shell (← #7).** Pattern matched my own command line and hung the session. Fix: anchored patterns or explicit PID. Lesson: exclude yourself when pattern-matching processes.
11. **List showed 200 rows but claimed the total (← #8).** `LIMIT 200` with `total: rows.length` while dashboard said 2049. Fix: real `{ total, page, limit, totalPages }`, clamp 1–100. Lesson: production tells the truth about data.
12. **No username login, password failed policy (← #9).** Email-only login, chosen password broke the locked rule. Verified by reading `auth.php` first. Additive-only upgrade, 8/8 on isolated copy. Lesson: read the code you depend on. Extend, never rewrite.
13. **Rate limiter blocked my own test script (← #10).** Five rapid logins tripped the guard. Waited out 60s, retried. Lesson: the guard works, including against me.
14. **Dark-mode footer used the same variable twice (← #11).** Bg and text from one variable means invisible in both themes. Lesson: bg and text must be opposites. (Superseded by #22 below, kept here as evolution.)
15. **Duplicate JSX slipped through an edit (← #12).** Caught by re-reading, not by the build. Lesson: green build never means correct UI. Read the result.
16. **Mobile navbar overflowed at 360px (← #13).** Logo plus pill plus buttons did not fit. Fix: hamburger on mobile, desktop unchanged. Lesson: test 360px first, not last.
17. **Copywriting leaked "demo" everywhere (← #14).** Portfolio words in a production app. Fix: production copy, verified by repo-wide grep. Lesson: words are UI.
18. **Favicon CSP console warning (← #15).** API server blocked `/favicon.ico`. Harmless, tab icon only. Lesson: not every console message is a bug.
19. **Test-data hygiene (← #16).** Every dummy lead deleted after testing, total back to 2049 every time. Lesson: leave the dataset exactly as you found it.
20. **Infra reality check, funnel down at the time (← #17).** PC-on meant server-on, no VPS by decision. Superseded by Tier 1 failure #2 (two verified funnels). Kept as evolution: from down, to one door, to two doors.
21. **Tested the WRONG server version (← #18).** Old instance held the port, new one crashed silently. Full suite ran against old code. Caught by process age + version probe. Lesson: verify WHO listens and WHICH version serves.
22. **Test script bug made fake failures (← #19).** Shell variable caught two words, URLs malformed, `curl 000` misread as app failures. Lesson: print variables, verify each step alone.
23. **Cleanup after the failed round (← #20).** Bad round left one ownerless row. Delete by id, verify counts, zero orphans, back to 2049. Lesson: every test plan needs a cleanup step with count verification.
24. **Blank white screen after auth went mandatory (← #21).** Backend required tokens, frontend never attached them. Saved token triggered auto-login into `undefined.map`, full-tree crash. Fix: token on EVERY fetch, error-shape guards everywhere. Lesson: a UI must never whitescreen on an API error.
25. **Exotic CSS failed silently in the browser (← #22).** Fancy color syntax rendered white unstyled buttons. Reverted to proven hex plus explicit `dark:` utilities. Lesson: boring proven syntax for critical UI. Only the target browser votes.
26. **Confusing explanations cost more than the bug (← #26).** Right facts, wrong delivery. Jargon buried what mattered until the owner asked for screen impact. Fix: impact first, mechanism second and only if asked. Lesson: if the owner is confused twice, the explanation is the bug. (Written as process note: technical communication is part of engineering.)

---

## 10. Non-Technical Summary (summary-callout)

Heading: `For those of you unable to read this data from technical standpoint, here is the conclusion:`

1. I already had one login service for my first app. New apps should reuse it, not copy it.
2. The second app is a customer database (2049 contacts) built with local AI — search, charts, import/export, dark mode.
3. It lives on a public page, but its engine sits on my home computer, reachable through a secure tunnel with two doors: one for login, one for data.
4. If my computer is off, the page opens but login fails — that is expected, and the pictures show the full flow.
5. One login, many apps: the third app just gets a new door, no new login code.

---

## 11. FAQ (3, EN, JSON-LD juga)

```json
[
  {"Q":"Why React for this app but vanilla for the article page?","A":"The app juggles filters, pagination, three charts, CSV flows and theme in one state tree — React earns its place. The article is static content — vanilla HTML + Tailwind is lighter and matches every other EXP page."},
  {"Q":"How can a static GitHub Page talk to your home PC?","A":"Two Tailscale Funnel doors: :443 proxies to auth (:7002), :8443 proxies to data (:7005, docker). The JS bundle bakes in those public URLs at build time (Vite env is build-time). PC off means doors closed — the page says so honestly."},
  {"Q":"Why does the API sometimes not answer?","A":"Not on a 24/7 VPS yet — home PC, online daily. Off-hours fetch fails and the app shows a toast. Screenshots + curl logs remain as proof. Same honesty as EXP 010/011."}
]
```

---

## 12. Live Status + Disclaimer

### 12a. Live Status (2 kolom + amber, sebelum footer, sama kayak EXP 010/011)
Title: `Live Status — Live Frontend, Home-PC Backend`
Kiri `What works now`: live React app on Pages, 2049 leads, real pagination, tap-to-filter charts, CSV import/export with dedup, dark mode, shared-auth login, 2 funnels verified by curl.
Kanan `What isn't production yet`: not 24/7, still local PC, funnel needs PC + docker + tailscaled all up, third-app onboarding untested, hardening/deploy = separate project.
Amber body: `Frontend is live 24/7 on GitHub Pages. Backend runs on my personal PC and answers only while it is on (funnel :443 + :8443). Off-hours → login/data fail with a toast, expected. Screenshots + curl logs show how it works.`

### 12b. Disclaimer
> **Disclaimer — Live frontend, local backend, not production-hardened.** Validated with same Tiel + llama.cpp-server (flags EXP 001). Replica data, WIZ names redacted per approval. OTP/token/passwords never shown.
>
> **First customer:** `exp010.html` + `/jobtracker/` · **Shared backend:** `exp011.html` (`:7002`) · **This:** `exp012.html` second customer (`/miniLeads/`, `:7005` + `:7002`) · **Machine:** `exp001.html`

---

## 13. File Checklist + Evidence yang WAJIB tampil

* New: `public_html/exp012.html` from `template-exp000.html` (navbar/footer 1:1 dari `exp011.html`)
* Images: pakai 8 real di `images/experiments012/` (nama persis, jangan rename biar link tidak putus)
* NO new deps. NO `charts.js` di page artikel. Hanya `css/style.css` + `js/main.js`
* After ACC (JANGAN bareng build): `experiments-list.html` (card 12, 11→12), `sitemap.xml` (exp012 + 8 images), `index.html` hanya kalau diminta
* Validasi: title/desc EN, alt semua img, h1→h2, canonical/OG/Twitter/Article+Breadcrumb+FAQ, `data-reveal` aman, tidak ada secret di HTML maupun JSON-LD, screenshot dicek ulang tidak ada kredensial terbaca
* QC internal tambahan (dari Tier 2, dipakai di validasi tanpa mengurangi cerita): baca file hasil sebelum declare done (← #12), print shell variables + verify tiap step (← #19), behavioral version probe sebelum percaya hasil test (← #18)

### 13a. Evidence Log — blok kode/screenshot yang dirender di page (EN, redacted)

> Semua angka di bawah verbatim dari `what-failed-crm-react.md` Evidence log. Token real JANGAN tampil — pakai `Bearer dummy`.

E1. Health dua pintu (code-block, berdampingan):
```
curl https://aispec.tail06293c.ts.net:8443/health
→ {"ok":true}
curl https://aispec.tail06293c.ts.net/api/me -H "Authorization: Bearer dummy"
→ {"error":"Token tidak sah atau kedaluwarsa"}
```
Caption: data door OK, auth door alive (dummy token correctly rejected).

E2. Pagination truth (code-block atau stat-tiles):
```
total 2049, page 1/103, count 20 · page 2 first-id differs
limit=500 clamped to 100 · page 999 clamped to 103
```
Caption: full count + clamped limits, over-page never empty.

E3. Auth + isolation (code-block, ringkas):
```
Admin login via username → role: admin · bad token → 401
Isolation: A 0→1, B stays 0 · cross read/edit → 404/404
self read → 200 · admin sees all · no token → 401
```
Caption: same backend as EXP 011, per-user scope enforced server-side. Detail full tetap di EXP 011 — di sini ringkasan + link.

E4. Build receipt (code-block):
```
.env.production (VITE_API_URL + VITE_AUTH_URL, funnel URLs)
→ 43 modules transformed
→ dist/index-D3BUY3DC.js (~367KB / ~120KB gzip)
→ index.html + assets/ at miniLeads/ root
```
Caption: public URLs baked at build time (Vite env is build-time, not runtime). `.env` (password) + `dist/` + `node_modules/` never committed.

E5. Test + backup receipts (1 baris, kecil):
```
npm test (leads backend): 6/6 pass · isolated auth tests: 8/8 pass
leads.db.bak-20260914-1357 · 0 orphans · final total 2049
```
Caption: green suite + dataset left exactly as found.

---

## 14. Build Steps

1. Bang Rob QC PRD ini → ACC/revisi.
2. Generate `exp012.html` full EN, order §3, tanpa chart, 8 gambar real, curl blocks redacted.
3. Verify: http.server + mobile/desktop, internal links, `prefers-reduced-motion`, tidak ada secret ter-`grep`.
4. JANGAN update list/sitemap/index dulu — tunggu review.
5. Review → publish list/sitemap (/index opsional) → done.

---
**Ready for QC.** Balas `gas build` — gue generate `exp012.html` 1 halaman penuh sesuai PRD ini.


