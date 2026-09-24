# PLAN — Experiment 013 (QC ronde 1 DONE 2026-09-15 — OPEN sisa: tanggal publish, wording final §9b)

**CV Screening Upgrade: technical test mentah jadi platform live multi-user di 1 backend auth**

> **Location:** `/public_html/exp013.html`
> **Plan file:** `PRD/plan-exp013.md` (file ini)
> **Date publish:** `TBD (nunggu ketok Bang Rob)`
> **Status:** `Live` — frontend live di GitHub Pages (`/cv_screening/`), backend + n8n local PC saat nyala
> **Lang:** `en` (full English di page, PRD ini Indonesian biar gampang QC)
> **Copy rules (USULAN, tiru lock 012 — belum diketok):** page full English grammar 95% (advanced, manusiawi). Larangan: JANGAN `;` dalam 1 kalimat. JANGAN em dash `—`. Article/preposisi boleh meleset sesekali. Non-Technical Summary ramah awam. Heading summary verbatim pola 12 EXP lain.
> **Author:** Robby Aliasa Akbar
> **Live demo:** `https://robbyaliasaakbar.github.io/cv_screening/` (React + Vite build, `index.html` + `assets/` di root folder)
> **Live API:** screening `https://aispec.tail06293c.ts.net:10000/webhook/upload-cv` (funnel path-restricted `/webhook` → docker n8n `:5678`) + auth `https://aispec.tail06293c.ts.net` (funnel `:443` → `:7002`, backend yang SAMA kayak EXP 010/011/012)
> **Sources:** folder `Belajar/sandbox-cv-screening/` (`src/` 14 file, `.env.production`, `index.template.html`, `vite.config.js`, `what-failed-cv-screening.md` ##0–27) + backend kanonis `Project website/backend/` (`src/profile.php` BARU + route `PUT /api/me` di `public/index.php`, sisanya 0 sentuh) + workflow n8n `Technical Test` (`v1lQYQUZndoeoR6w`, active, 24 node + guard DITOLAK — canvas pristine) — JANGAN buka `.env` asli, JANGAN tampilkan password / token / OTP asli
> **Relation:** EXP 007 = POC mentah (single-file + n8n, no login) → EXP 010 = frontend vanilla pertama → EXP 011 = backend auth dipakai bareng → EXP 012 = pelanggan kedua (CRM React) → EXP 013 = pelanggan ketiga (CV screening React, auth reuse + endpoint profil baru aditif). Rantai: POC (007) → UI (010) → token (011) → app kedua (012) → upgrade + live (013) → mesin (001).
> **Auth disclaimer (wajib render di page, EN):** login di app ini pakai backend auth yang SAMA kayak jobtracker/miniLeads (EXP 010/011/012). Bukan copy. Plus endpoint profil baru (`PUT /api/me`, additive-only). Detail backend tetap di EXP 011, di sini ringkasan + link.
> **Claim locked (USULAN, tiru 012):** dibangun dengan local LLM sama persis `Tiel-Coder-35B-A3B-MTP-UD-Q4_K_S.gguf` via `llama.cpp-server`. Flag inference TIDAK dishare (refer EXP 001).

---

## 1. Objective — LOCKED (QC Bang Rob: judul pakai "technical test", bukan "take-home")

> **Can a technical-test CV screener evolve into a live multi-user platform on one shared auth backend — with every failure kept?**

Setelah lock, page ini dokumentasikan: upgrade POC→semi-prod (upload 1–5 PDF, ekstrak pdf.js, realtime 7 step, auth wajib, settings profil+password, live Pages + 2 funnel), pola 1-backend-banyak-app pelanggan ketiga, dan failure log full sebagai kredibilitas. Detail POC mentah tetap di EXP 007 (jangan diulang), detail backend tetap di EXP 011 (jangan diulang), pola React+Pages tetap di EXP 012 (jangan diulang, cukup rujuk).

**Key stance (usulan):** *Third customer, same key. The screening brain stays in n8n (rule-based 50-20-30, no in-system AI); the portal just got honest: required login, per-user tokens, self-wiping results. Security sized to the threat (open webhook kept BY DECISION with written assessment, editor locked down) — portfolio judgment over enterprise theater.*

Source of truth: JANGAN ngarang di luar file yang dibaca. Nama file, endpoint, env key, angka (5 CV, 7 step, 38 modules, threshold 65), URL funnel harus plek source.

---

## 2. Audience & Goals — AGREED (QC Bang Rob, internal, JANGAN sebut eksplisit di page)

**Primary (internal note):** reviewers yang mau bukti "upgrade POC jadi live" itu real + builders yang mau pola portal-React + n8n + shared-auth.
**Secondary:** Pembaca EXP 007/010/011/012 — rantai lengkap: POC (007) → UI (010) → backend (011) → app kedua (012) → upgrade live (013).

**Goals (usulan, nunggu QC):**
1. Buktikan **evolusi**: POC `lamding-page/index.html` 1 file → React+Vite multi-page (Home/Settings/Auth) + realtime pipeline + wajib login. Bukti: file list + build receipts.
2. Buktikan **pelanggan ketiga exp011**: login CV pakai `/api/login` + `/api/me` yang SAMA, token key beda (`cv.token`), plus endpoint profil aditif (`PUT /api/me`, backup dulu, 401/409/422 verified).
3. Buktikan **otak tetap n8n**: webhook terima array → 3 jalur A/B/C → scoring 50-20-30 threshold 65 → ranking → TRUNCATE (fresh by design, bukan bug). Tanpa AI in-system.
4. Buktikan **deploy Pages + funnel**: build kejepret 2 URL, `dist/` ke root, funnel path-restricted (`/webhook` doang, editor 404), CORS preflight (github.io echo, evil tolak).
5. Jaga credibility: `Live` untuk frontend, backend local-PC-nyala, webhook open BY DECISION (bukan kelalaian — dengan threat assessment), no-history claim dilarang (DB self-wipe).

---

## 3. Styling & Stack — AGREED (QC Bang Rob, plek EXP 012, wajib seragam)

* **Stack page ini:** `HTML5 + Tailwind CSS via CDN + Vanilla JS` only. JANGAN load `charts.js` / Chart.js. Cuma `js/main.js` (`data-reveal` + navbar).
* **Stack app yang didokumentasikan:** `React 18 + Vite 6 + pdf.js 2.16 (CDN)` (portal) · `n8n` docker host-network (`:5678`) + rule-based scoring · auth reuse `:7002` PHP + SQLite (+ `PUT /api/me` aditif).
* **Colors:** `ink #0C0C0C / paper #FFFFFF / accent #D70000` + emerald untuk Lolos/Shortlisted.
* **Fonts:** `Manrope (display) / Inter (body) / JetBrains Mono (mono)` — sama persis.
* **Tokens wajib:** `eyebrow`, `status-badge status-live`, `border border-black/10 rounded-2xl`, `summary-callout`, `quote-block`, `detail-label`, `stat-tile`, `code-block`, `code-inline`, `data-reveal`, `bg-grid`, `bg-dark-section`, `nav-link nav-active`, `arrow-link`, `btn-primary`, `btn-outline`.
* **Navbar/Footer:** Copy 1:1 dari `exp012.html`. Ganti hanya: breadcrumb `Experiment 012 → Experiment 013`, `og/url`, `canonical`, nomor experiment. `nav-active` tetap di `Experiments`.
* **Internal links wajib jalan:** `Home (/)`, `profile.html`, `experiments-list.html`, `contact.html`, `Back to Experiments`, `Have a problem worth exploring?`. Tambah link rantai: `EXP 007 (the raw POC)`, `EXP 010`, `EXP 011 (the shared backend)`, `EXP 012 (second customer)`, `EXP 001 (the machine)`.
* **Order sections (locked, tiru 012):** Hero + Breadcrumb → System Requirements → Screenshots → Live Demo → Non-Technical Summary → Experiment Details → Failure Log → Frontend Code → Backend Notes → Networking (2 funnel + path-restriction) → FAQ → Live Status → Disclaimer + Teaser rantai → Footer CTA.
* **Responsive:** mobile 375px, tablet 768px, desktop 1280px. Ranking `table hidden md:block`, kartu `md:hidden` — ceritakan di page.
* **Screenshot note — OPEN (belum capture):** usul 8 file di `images/experiments013/`: `home-upload`, `extract-json`, `realtime-steps`, `ranking-cards-mobile`, `auth-login`, `auth-register`, `settings`, `homepage-dark` (atau variasinya — nunggu ketok + capture). Light = bukti utama. JANGAN tampilkan password/token/OTP asli.

---

## 4. SEO & Metadata — DRAFT cukup (QC Bang Rob: QC ulang pas page jadi, sebelum live)

```html
<title>Experiment 013 — [TBD ikut judul lock] — Robby Aliasa Akbar</title>
<meta name="description" content="TBD 1-2 kalimat (upload 1-5 CV, shared auth, n8n scoring, live Pages).">
<link rel="canonical" href="https://robbyaliasaakbar.github.io/exp013.html">
<meta property="og:title" content="Experiment 013 — [pendek]">
<meta property="og:image" content="https://robbyaliasaakbar.github.io/images/experiments013/[hero].webp">
<script type="application/ld+json"> Article + BreadcrumbList + FAQPage
```

**Keywords (usulan):** `shared auth backend third app, React Vite GitHub Pages, n8n webhook scoring, Tailscale Funnel path restriction, pdf.js browser extract, local LLM app, Tiel-Coder, rule-based ranking, additive backend upgrade`
**Dates:** `TBD`.
**Robots:** `index, follow, max-image-preview:large` (sama kayak 010/011/012).

**After review (JANGAN sekarang, tunggu ACC Bang Rob):**
* `experiments-list.html` card position 13 + `numberOfItems:12 → 13`
* `sitemap.xml` add `exp013.html` + images
* `index.html` hanya kalau diminta

---

## 5. System Requirements — AGREED (QC Bang Rob, angka plek sumber)

| Item | Spec |
|---|---|
| Live app | `https://robbyaliasaakbar.github.io/cv_screening/` (static, `index.html` + `assets/`, base `./`) |
| Screening API | `https://aispec.tail06293c.ts.net:10000/webhook/upload-cv` (funnel path `/webhook` → docker n8n `:5678`, host-network) |
| Auth API | `https://aispec.tail06293c.ts.net` (funnel `:443` → `:7002`, same backend as EXP 010/011/012) + `PUT /api/me` (new, additive) |
| Engine | n8n `Technical Test` (`v1lQYQUZndoeoR6w`, active): webhook → 3 tracks A/B/C → 50-20-30 scoring, threshold 65 → ranking → TRUNCATE (fresh by design) |
| Data | 5 sample CVs (`Bahan/`, 2/2/1/2/1 pages, 2780/5280/2367/7523/3377 chars extracted in-browser) |
| Build | `npm run build` → 38 modules → `dist/` → copied to `cv_screening/` root (localhost 0 hits, 2 funnel URLs baked) |
| Local dev | `npm run dev` → `:7008` (localhost, `.env`, funnel URLs NOT used) |
| Browser | Any modern browser, 360px → 1280px. JS required (React). |

---

## 6. Screenshots — LOCKED 12 file real (QC Bang Rob, 637x959 portrait HP, webp valid)

1. `homepage-light.webp` / `homepage-dark.webp` — hero + upload queue. Caption: 1–5 PDFs, validation first, same flow both themes.
2. `extracting-pdf-to-json-light.webp` / `-dark.webp` — per-CV cards (pages + chars) + Download JSON. Caption: text read in-browser, n8n receives clean JSON.
3. `realtime-visual-progress-light.webp` / `-dark.webp` — 7 live steps mid-run. Caption: display mirrors the real order, ends on a real response.
4. `login-light.webp` / `login-dark.webp` — Welcome back + show/hide. Caption: same backend as EXP 011, zero re-code.
5. `register-light.webp` / `register-dark.webp` — miniLeads-mirror fields (username + birthdate). Caption: same contract, own styling.
6. `settings-light.webp` / `settings-dark.webp` — Profile edit + password + appearance. Caption: account is real (`PUT /api/me`), theme in browser.

Alt text deskriptif + "CV Screening — Robby Aliasa Akbar". NO password/token/OTP terbaca (cek ulang pas build page).

---

## 7. Live Demo — AGREED (QC Bang Rob)

* Tombol primer: `Open Live App — /cv_screening/`.
* Teks jujur: `Live frontend, home-PC backend + n8n. PC/funnel off → login/screening fails with a message — expected, not broken. Screenshots above show the full flow. Webhook intentionally open (spam ROI ≈ nil, self-wiping results); editor locked to /webhook path.`
* JANGAN janji 24/7. JANGAN tulis kredensial asli.

---

## 8. Experiment Details — AGREED semua failure masuk (QC Bang Rob: no nutup-nutupin, EN di page, 5 blok + sumber failure)

> Tiap blok mencantumkan `← what-failed #X` (`Belajar/sandbox-cv-screening/what-failed-cv-screening.md`).

### 8a. Third customer, same key (← #8, #12, #14)
Login CV pakai endpoint yang SAMA (`/api/login`, `/api/me`), token key beda (`cv.token`), guard wajib-login (token basi → Auth). Plus endpoint profil aditif `PUT /api/me` (backup dulu, 401/409/422 verified, file lama 0 sentuh). Pelajaran ##8 (aturan ngarang) sebagai receipt kejujuran.

### 8b. Browser reads, n8n thinks (← plan §3.4, #10, #19)
Upload 1–5 PDF + validasi → pdf.js ekstrak (5 sample: 2/2/1/2/1 hal) → JSON array kontrak → POST. Robust: 1 file korup tidak gugurkan sisanya. Hasil akhir: ranking dari response, bukan tebak-tebakan. DB self-TRUNCATE by design (##19) — no-history claim dilarang.

### 8c. Display that tells the truth about the pipeline (← realtime, #7)
7 step mirror urutan n8n asli; step 1+7 fetch beneran, 2–6 cerminan + keyword preview. Mobile: kartu vertikal tanpa slide (##7: hasil dulu, aksi kemudian — download di bawah tabel).

### 8d. English, floating, red (← #2, #5)
Copy dapur digrep sampai 0 (`Scaffold/Museum/Portal`), kartu `card-float` + border ink + tombol merah (hover = bg merah — setelah 1 ronde ketuker ##6). Words are UI.

### 8e. Infra honesty: doors, not servers (← #16, #17, #18, #25)
Funnel `:10000` path-restricted (editor 404, webhook jawab; 2 ronde semantics), `:443` user-service (bukan terminal — ralat ##25), docker host-network semua (ralat ##17). PC-on = server-on tradeoff sadar. + Threat sizing (##24): webhook open BY DECISION (ROI ≈ nil), bukan kelalaian.

---

## 9. Failure Log — AGREED SEMUA tampil (QC Bang Rob: 27 failure + Evidence, 2 tier, anti "too good to be true")

> SEMUA 27 (`##0`–`##27`, catat `##17` urutan loncat) dirender EN. Redact: password/token/OTP/isi `.env`/nama sensitif. Rujukan `← #X` = `what-failed-cv-screening.md`.

### 9a. TIER 1 — sorotan (5 panggung penuh)

1. **Editor exposed, STOPped (← #16/#18).** Funnel full-mount → editor publik + kata `Setup`. Fix: `--set-path=/webhook` (2 ronde semantics) → `/` 404, webhook faithful `200=200`. Lesson: cek 2 sisi tiap pintu.
2. **"Fragile funnel" yang ternyata sehat (← #25).** Vonis tanpa verifikasi boot → dibantah `ps` (TTY `?`) + user service + daemon-held config. Lesson: vonis boot tanpa reboot = karangan.
3. **Guard 4 node yang ditolak owner (← #22/#24/#27).** Solusi kelas bank buat masalah kelas warung + nambah tanpa ketok + echo semu. Lesson: tolak = keputusan teknik sah; asisten diukur dari masalah yang TIDAK dibuat.
4. **Aturan ngarang + wilayah owner (← #8/#23).** "Tanpa login" + pola induk injeksi keputusan. Lesson: supervisi bukan slogan.
5. **HMR half-update (← #3/#4).** Rename → tab belang (served campur `home`+`screen`). Lesson: verifikasi code tersaji + restart+tab baru; edit struktur = info restart di depan.

### 9b. TIER 2 — full log (22 sisanya, ringkas 1-2 kalimat + lesson, EN)

6+. Zinc: scaffold copy-identik (##0) → zombie port (##1) → copy dapur (##2) → curl-grep ngawur (##10) → edit no-op (##11) → aksen résumé (##5) → hover ketuker (##6) → tombol bawah tabel (##7) → "basic" tanpa tanya (##9) → "langsung login" stale-token (##12) → tooling `which` (##13) → E2E+cleanup (##14) → echo di-skip (##15) → DB misteri→self-truncate (##19) → CORS preflight (##20) → jebakan #25 (##20) → pesan misleading (##21) → ralat native×3 (##17) → restart tak diomong (##4) → ancaman dinilai ulang (##24). (Final wording + urutan pas nulis page, nunggu QC.)

---

## 10. Non-Technical Summary — LOCKED versi friendly (QC Bang Rob: awam harus merasa pinter, grammar tidak perfect)

Heading verbatim pola: `For those of you unable to read this data from technical standpoint, here is the conclusion:`

1. I got a hiring test once: a simple uploader with a smart back room. Instead of throwing it away, I rebuilt the front room properly.
2. One login now opens all three of my apps. The third one only added a small profile door. Nothing was rebuilt twice.
3. Your CVs get read inside your own browser, scored in seconds, and the score table gets wiped fresh after every run. Nothing piles up anywhere.
4. The page is public but the engines sleep in my home computer. When it is off, the page tells you honestly. The pictures above are the proof, so you do not have to take my word for it.
5. Everything that broke along the way is written on this same page. You are smart enough to see the full story, and that is exactly the point.

---

## 11. FAQ (6 LOCKED — QC Bang Rob ronde 2: valid + setuju)

```json
[
  {"Q":"Why does screening require login when the original test did not?","A":"The test proved the engine. The platform proves one account serves many apps. Login ties screenings to people, and the same backend serves all three apps with zero re-code."},
  {"Q":"Where do my CVs go? Is anything stored?","A":"Text is extracted in your browser, scored by the workflow, then the result table is wiped fresh every run by design. Downloads (JSON/CSV) are yours to keep. 5 sample CVs, 2780 to 7523 chars each, never leave your browser as files."},
  {"Q":"Why is the screening webhook open? Is that not a hole?","A":"Measured decision, documented on-page: stateless endpoint, self-wiping results, near-zero attacker ROI (a spammer only scores their own junk). The n8n editor itself answers 404 on the public door. Hardening scales with threat, not with fear."},
  {"Q":"Too good to be true? 27 failures says otherwise — which one hurt most?","A":"Exposing the n8n editor to the internet during go-live. Full stop, documented with curl receipts. Also: a workflow edit that never persisted while the tool said saved. Both are on this page with evidence."},
  {"Q":"Why does it fail when your PC is off?","A":"No VPS by decision, home PC plus secure tunnels (:443 auth, :10000 screening). Off means doors closed and the page says so instead of pretending. Same honesty as EXP 010/011/012."},
  {"Q":"Why React for the portal but rule-code instead of AI for scoring?","A":"React earns its place (upload queue, live pipeline view, auth, settings in one state tree). Scoring stays rule-based 50-20-30 so every point is explainable. AI is used where it helps, not where it impresses."}
]
```

---

## 12. Live Status + Disclaimer — AGREED (QC Bang Rob, tiru 012)

### 12a. Live Status
Title: `Live Status — Live Frontend, Home-PC Backend + n8n`
Kiri `What works now`: live React app, upload 1–5 + extract + realtime + ranking + CSV/JSON, required login via shared backend, profile edit + password change, 2 funnels verified.
Kanan `What isn't production yet`: not 24/7, local PC, webhook open by decision, no result history (self-wipe by design), funnel/service hardening = separate project.
Amber body: `Frontend live 24/7 on GitHub Pages. Backend + n8n run on my personal PC (funnels :443 + :10000). Off → login/screening fails with a message, expected. Screenshots + curl logs show how it works.`

### 12b. Disclaimer
> **Disclaimer — Live frontend, local backend + n8n, not production-hardened.** Replica CVs, no real applicant data. OTP/token/passwords never shown.
>
> **POC:** `exp007.html` · **First UI:** `exp010.html` + `/jobtracker/` · **Shared backend:** `exp011.html` (`:7002`) · **Second customer:** `exp012.html` (`/miniLeads/`) · **This:** `exp013.html` (`/cv_screening/`) · **Machine:** `exp001.html`

---

## 13. File Checklist + Evidence — AGREED semua yang bisa diverifikasi tampil (QC Bang Rob ronde 1+2: positioning = evolusi berfikir, anti "too good to be true"; redaksi password/token/OTP/isi .env LOCKED — keamanan, bukan nutupin gagal; token tampil cuma versi dummy kayak 012)

* New: `public_html/exp013.html` from `template-exp000.html` (navbar/footer 1:1 dari `exp012.html`)
* Images: 8 real di `images/experiments013/` (nunggu capture, nama dikunci pas capture)
* NO new deps. Hanya `css/style.css` + `js/main.js`
* After ACC (JANGAN bareng build): `experiments-list.html` (card 13, 12→13), `sitemap.xml` (exp013 + images), `index.html` hanya kalau diminta
* Validasi: title/desc EN, alt semua img, h1→h2, canonical/OG/Twitter/Article+Breadcrumb+FAQ, `data-reveal` aman, tidak ada secret, screenshot dicek ulang
* Evidence receipts (verbatim, redacted): funnel 404/200 kembar, probe ranking `cv_9` 40, CORS echo/tolak, build 38 modules + localhost 0, DB 5 users + integrity ok, backup list

---

## 14. Build Steps

1. Bang Rob QC PRD ini → ACC/revisi (termasuk lock §1 judul + §6 screenshots).
2. Capture 8 screenshots → `images/experiments013/`.
3. Generate `exp013.html` full EN, order §3, tanpa chart.
4. Verify: http.server + mobile/desktop, links, `prefers-reduced-motion`, grep secrets.
5. JANGAN update list/sitemap/index dulu — tunggu review.
6. Review → publish list/sitemap (/index opsional) → done.

---

**DRAFT for QC — OPEN: §1 judul, §6 screenshots, §4 tanggal/keywords final, §9b wording final.**
Balas revisi per nomor bab — gue betulin per bab, bukan borongan.
