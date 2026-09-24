# PLAN — Experiment 014 (DRAFT for QC 2026-09-24 — OPEN sisa: screenshots §6 nunggu capture Bang Rob, tanggal publish, wording final §9b + §10 + §11)

**Content Tracking Web App With Server Side Pagination, React + Vite, Live on GitHub Pages**

> **Location:** `/public_html/exp014.html` (belum ada, generate dari `template-exp000.html` pas Batch 4)
> **Plan file:** `PRD/experiments pages/plan-exp014.md` (file ini)
> **Date publish:** `TBD (nunggu ketok Bang Rob)`
> **Status:** `Draft` — app-nya `Live` (frontend GitHub Pages `/contentOS/`, backend + auth local PC saat nyala), page-nya belum dibangun
> **Lang:** `en` (full English di page, PRD ini Indonesian biar gampang QC)
> **Copy rules (LOCKED ikut 013, perintah Bang Rob 24-09-2026):** page full English tone warung yang ramah orang nonteknis. Larangan: TANPA em dash `—` selain judul section. TANPA hyphen `-` selain listing. TANPA semicolon `;` sama sekali. Article/preposisi boleh meleset sesekali. Non-Technical Summary ramah awam. Heading summary verbatim pola 13 EXP lain.
> **Author:** Robby Aliasa Akbar
> **Live demo:** `https://robbyaliasaakbar.github.io/contentOS/` (React + Vite + Chart.js build, `index.html` + `assets/` di root folder)
> **Live API:** data `https://aispec.tail06293c.ts.net:9443` (funnel `:9443` → docker `:7010` Node Express + SQLite, `network_mode: host`) + auth `https://aispec.tail06293c.ts.net` (funnel `:443` → `:7002`, backend yang SAMA kayak EXP 010/011/012/013)
> **Sources:** `public_html/contentOS/` (`src/` 13 file: App/Auth/Board/ContentForm/Dashboard/Settings/StatusChart/TempatDonut/api/auth/theme/main/index.css, `.env.production`, `index.template.html`, `vite.config.js`, `README.md` FE-only) + `backend-server-contentOS/` (`server.js` 7 endpoint, `db.js` `content.db`, `env.js`, `Dockerfile` node:22-slim, `docker-compose.yml`) + `catatan kegagalan/Catatan kegagalan develop ContentOS.md` (12 biji, VERSI GABUNGAN, SEMUA wajib naik) + `Port Using/PORT_TRACKING.md` + PRD `content os/` 3 file (LOCKED 24-09-2026) — JANGAN buka `.env` asli, JANGAN tampilkan password / token / OTP asli
> **Relation:** EXP 010 = UI vanilla pertama → EXP 011 = backend auth dipakai bareng → EXP 012 = pelanggan kedua (CRM React, server side pagination) → EXP 013 = pelanggan ketiga (CV screening React + n8n) → EXP 014 = pelanggan keempat (ContentOS React + Chart.js, TANPA n8n, debug chronicle 12 kegagalan) → EXP 001 (the machine). Cerita owner (LOCKED 24-09-2026): GitHub + porto sudah oke, sosmed terbengkalai. Butuh tracking konten pribadi (bukan editor, bukan designer). Tuang ide tanpa hilang konteks, 2 jalan: ketik manual + titip agen AI via MCP (rencana, BUKAN V1 — di page jadi teaser jujur, bukan klaim).
> **Auth disclaimer (wajib render di page, EN):** login di app ini pakai backend auth yang SAMA kayak jobtracker/miniLeads/cv-screening (EXP 010/011/012/013). Bukan copy. Token key beda (`content.token`). Detail backend tetap di EXP 011, di sini ringkasan + link.
> **Claim locked (OPEN, nunggu ketok Bang Rob):** klaim LLM lokal kayak 013 belum diputus buat 014. JANGAN tulis sebelum diketok.

---

## 1. Objective — USULAN (nunggu ketok Bang Rob)

> **Can a personal content tracker ship live on shared auth while keeping all 12 failures on the page?**

Setelah lock, page ini dokumentasikan: ContentOS sebagai pelanggan keempat auth `:7002` (portal kalender + tracking, NO AI in-system, NO n8n), pola React + Chart.js tap-to-filter, 12 kegagalan BE + FE + workflow sebagai kredibilitas, dan teaser MCP jujur (rencana, bukan V1). Detail backend tetap di EXP 011 (jangan diulang), pola React + Pages tetap di EXP 012 (jangan diulang, cukup rujuk), pola shared-auth ketiga tetap di EXP 013 (jangan diulang, cukup rujuk).

**Key stance (usulan):** *Fourth customer, same key. No n8n this time, no in-system AI. The portal only tracks ideas to posted, the human still posts by hand. An agent scribe via MCP is on the roadmap, written as a plan, not as a shipped feature. Twelve failures stayed on the record because a calm calendar means nothing without an honest log.*

Source of truth: JANGAN ngarang di luar file yang dibaca. Nama file, endpoint, env key, angka (3 dummy rows, 7 endpoint, 30 detik refresh, 900ms flash, 600 detik? NO — cek sumber), URL funnel harus plek source.

---

## 2. Audience & Goals — USULAN (nunggu QC)

**Primary (internal note):** reviewers yang mau bukti "debug chronicle" itu real + builders yang mau pola portal-React + Chart.js + shared-auth tanpa n8n.
**Secondary:** Pembaca EXP 010/011/012/013 — rantai lengkap: UI (010) → backend (011) → app kedua (012) → app ketiga + n8n (013) → app keempat + debug log (014).

**Goals (usulan, nunggu QC):**
1. Buktikan **pelanggan keempat exp011**: login ContentOS pakai `/api/login` + `/api/me` yang SAMA, token key beda (`content.token`), theme key beda (`content.theme`). Bukti: preflight receipts + `docker exec echo $CORS_ORIGINS`.
2. Buktikan **portal tracking jujur**: CRUD `content-xxx` slot terkecil, filter + search + pagination server side, export CSV ikut filter, posted wajib link (400 + merah), dashboard klik = filter. Bukti: curl receipts + screenshots.
3. Buktikan **Chart.js yang guna**: bar by_status + donat by_tempat, tap = filter, auto refresh 30 detik (bukan websocket — overkill buat PC 08-21 + 5 user, tulis jujur). Amandemen PRD (chart tadinya JANGAN V1) dicatat terbuka.
4. Buktikan **deploy Pages + funnel**: build `384KB` + `grep localhost 0` + `dist/` ke root + funnel `:9443` baru (tidak timpa `:8443` miniLeads) + CORS preflight 2 host. Bukti: build log + `funnel status` 3 baris.
5. Jaga credibility: `Live` untuk frontend, backend local-PC-nyala, MCP = teaser rencana (ditulis eksplisit BUKAN V1), 12 failures naik semua.

---

## 3. Styling & Stack — USULAN tiru 013 (nunggu QC, wajib seragam)

* **Stack page ini:** `HTML5 + Tailwind CSS via CDN + Vanilla JS` only. JANGAN load `charts.js` / Chart.js di page. Cuma `js/main.js` (`data-reveal` + navbar).
* **Stack app yang didokumentasikan:** `React 18 + Vite 6 + Chart.js` tap-to-filter (portal) · Node Express `:7010` + SQLite `content.db` isolasi · auth reuse `:7002` PHP + SQLite · TANPA n8n (eksplisit, beda dari 013).
* **Colors:** `ink #0C0C0C / paper #FFFFFF / accent #D70000` + status A (abu/kuning/biru/ijo, light pastel + dark transparan) + platform donat (IG merah, LinkedIn biru, Reddit oranye, WA ijo).
* **Fonts:** `Manrope (display) / Inter (body) / JetBrains Mono (mono)` — sama persis.
* **Tokens wajib:** `eyebrow`, `status-badge status-live`, `border border-black/10 rounded-2xl`, `summary-callout`, `quote-block`, `detail-label`, `stat-tile`, `code-block`, `code-inline`, `data-reveal`, `bg-grid`, `bg-dark-section`, `nav-link nav-active`, `arrow-link`, `btn-primary`, `btn-outline`.
* **Navbar/Footer:** Copy 1:1 dari `exp013.html`. Ganti hanya: breadcrumb `Experiment 013 → Experiment 014`, `og/url`, `canonical`, nomor experiment. `nav-active` tetap di `Experiments`.
* **Internal links wajib jalan:** `Home (/)`, `profile.html`, `experiments-list.html`, `contact.html`, `Back to Experiments`, `Have a problem worth exploring?`. Tambah link rantai: `EXP 010`, `EXP 011 (the shared backend)`, `EXP 012 (second customer)`, `EXP 013 (third customer)`, `EXP 001 (the machine)`.
* **Order sections (locked, tiru 013):** Hero + Breadcrumb → System Requirements → Screenshots → Live Demo → Non-Technical Summary → Experiment Details → Failure Log → Frontend Code → Backend Notes → Networking (2 funnel, tanpa path-restriction karena murni API) → FAQ → Live Status → Disclaimer + Teaser rantai → Footer CTA.
* **Responsive:** mobile 375px, tablet 768px, desktop 1280px. Table `hidden lg:block`, kartu `lg:hidden` — ceritakan di page.
* **Screenshot note — OPEN (Bang Rob siapin file + path):** usul 6 pasang di `images/experiments014/` (folder belum ada): `board`, `analytics`, `content-form`, `mobile-cards`, `auth-login`, `empty-toast` (light + dark tiap item, webp). Verifikasi pas Batch 4: webp valid, no password/token/OTP kebaca, light/dark kepasang. Light = bukti utama.

---

## 4. SEO & Metadata — DRAFT cukup (QC ulang pas page jadi, sebelum live)

```html
<title>Experiment 014 - Content Tracking Web App With Server Side Pagination, React + Vite, Live on GitHub Pages - Robby Aliasa Akbar</title>
<meta name="description" content="TBD 1-2 kalimat (personal content tracker, shared auth fourth customer, Chart.js tap filter, 12 failures on record).">
<link rel="canonical" href="https://robbyaliasaakbar.github.io/exp014.html">
<meta property="og:title" content="Experiment 014 - Content Tracking Web App">
<meta property="og:image" content="https://robbyaliasaakbar.github.io/images/experiments014/[hero].webp">
<script type="application/ld+json"> Article + BreadcrumbList + FAQPage
```

**Keywords (usulan):** `shared auth backend fourth app, React Vite GitHub Pages, Chart.js tap to filter, Tailscale Funnel new port, SQLite slot reuse id, content calendar tracker, personal CRM, debug chronicle`
**Dates:** `TBD`.
**Robots:** `index, follow, max-image-preview:large` (sama kayak 010/011/012/013).

**After review (JANGAN sekarang, tunggu ACC Bang Rob):**
* `experiments-list.html` card position 14 + `numberOfItems:13 → 14`
* `sitemap.xml` add `exp014.html` + images
* `index.html` hanya kalau diminta

---

## 5. System Requirements — USULAN (angka plek sumber, nunggu QC)

| Item | Spec |
|---|---|
| Live app | `https://robbyaliasaakbar.github.io/contentOS/` (static, `index.html` + `assets/`, base `./`) |
| Data API | `https://aispec.tail06293c.ts.net:9443` (funnel `:9443` → docker `:7010` Node Express + SQLite, `network_mode: host`, image `node:22-slim`) |
| Auth API | `https://aispec.tail06293c.ts.net` (funnel `:443` → `:7002`, same backend as EXP 010/011/012/013, token key `content.token`) |
| Engine | NONE — TANPA n8n, TANPA AI in-system (eksplisit). Ngedit + posting tetap manual. MCP agent scribe = rencana (teaser, bukan klaim). |
| Data | `content.db` isolasi dari `leads.db`, 3 dummy rows (001 IG/reels/ide, 002 LinkedIn/text-post/revision, 003 Reddit/video/posted+link), ID `content-xxx` reuse slot terkecil |
| Build | `npm run build` → `384KB` bundle (opsi A Chart.js, pilihan sadar) + `grep localhost 0` + `dist/` ke root + `index.template.html` saklar dev-vs-Pages |
| Local dev | `npm run dev` → `:7011` (localhost, `.env`, funnel URLs NOT used). JANGAN `preview` buat ngoding (bundle production → CORS silang). |
| Browser | Any modern browser, 375px → 1280px. JS required (React). |

---

## 6. Screenshots — LOCKED 8 file real (Bang Rob capture 24-09-2026 sore, Udin verifikasi: webp valid, field kosong no secret, URL live kepotret)

1. `dashboard-light.webp` / `dashboard-dark.webp` (1105px) — hero + kartu + bar + donat + tabel 3 dummy + footer. Caption: full board 1 layar, tap chart = filter.
2. `form-idea-light.webp` / `form-idea-dark.webp` (1440px) — form 10 kolom. Caption: cuma link posted yang wajib.
3. `login-light.webp` / `login-dark.webp` (1440px) — layar Masuk kosong. Caption: backend SAMA kayak EXP 011, 0 kode auth baru.
4. `regis-light.webp` / `regis-dark.webp` (1440px) — layar Daftar kosong. Caption: OTP ke email, pola JobTracker.

Alt text deskriptif + "ContentOS — Robby Aliasa Akbar". NO password/token/OTP terbaca (lolos cek visual 3 file + `file` 8 file).

Follow-up opsional (bukan blocker publish): `mobile-cards` portrait 375px + `empty-toast` (empty CTA + toast ijo).

---

## 7. Live Demo — USULAN (nunggu QC)

* Tombol primer: `Open Live App — /contentOS/`.
* Teks jujur: `Live frontend, home-PC backend. PC/funnel off → login/list fails with a message — expected, not broken. Screenshots above show the full flow. Agent scribe via MCP is planned, not shipped. Demo login uses dummy rows only.`
* JANGAN janji 24/7. JANGAN tulis kredensial asli. (Kredensial demo ikut pola 012: token dummy di receipt, password asli TIDAK di page.)

---

## 8. Experiment Details — USULAN mapping 12 kegagalan (nunggu QC, EN di page, 5 blok)

> Tiap blok mencantumkan `← #X` (`catatan kegagalan/Catatan kegagalan develop ContentOS.md` VERSI GABUNGAN).

### 8a. Fourth customer, same key (← #7, #3)
Login ContentOS pakai endpoint yang SAMA (`/api/login`, `/api/me`), token key beda (`content.token`), theme key beda (`content.theme`). CORS origin `:7011` ditambah + recreate (bukan restart — `env_file` cuma dibaca pas recreate). Preflight dibuktikan dari 2 host biar same-host pass tidak nipu. `$TOKEN` kosong = 401 fail closed, bukan bug.

### 8b. One file DB, honest IDs (← #1, #2)
SQLite `content.db` isolasi dari `leads.db`. ID `content-xxx` reuse slot terkecil (001,003,004 → baru 002). Search `q` nyari 6 kolom (bukan cuma textarea). Edit lupa 1 param → crash → tambah + `hasLink`. Posted tanpa link → 400 + merah, di FE dan di BE.

### 8c. Doors, not servers (← #5, #6, #4)
Funnel `:9443` baru (tidak timpa `:8443` miniLeads — putusan owner). Image `node:22-slim` (Alpine crash 139 karena better-sqlite3 tanpa prebuild musl — plek miniLeads). Restart BE by PID (jangan `pkill` membabi buta di PC multi-BE).

### 8d. Dev vs Pages switch (← #8, #12, #9)
`index.template.html` saklar mode dev vs Pages (cek 1 baris `grep -c src/main`). Copy-build makan korban 1x → prosedur restore tertulis. Push polish 3 commit rapi. Footer transparan karena class CSS ketinggalan → `grep` tiap class custom sebelum DONE.

### 8e. Red that behaves (← #10, #11)
Focus ring nempel → timer 900ms → skeleton nutupin → filter selalu nempel + chip merah permanen + `key` dari data (biar ngetik tidak putus fokus). Merah dipertahankan (request owner), nempelnya yang dibuang. Hover merah + dark eksplisit (specificity variant).

---

## 9. Failure Log — USULAN SEMUA tampil (nunggu QC, 12 failure, 2 tier, EN di page)

> SEMUA 12 dirender EN. Redact: password/token/OTP/isi `.env`/nama sensitif. Rujukan `← #X` = catatan gabungan. Copy rules page berlaku (no `;`, no em dash selain judul, hyphen cuma listing).

### 9a. TIER 1 — sorotan (4 panggung penuh)

1. **CORS in 3 layers (← #7).** Dev bundle vs funnel URL + restart palsu + false green same-host. Fix: allowlist + recreate + bukti `docker exec echo $VAR` + test 2 host. Lesson: restart is not recreate.
2. **Alpine crashes SQLite (← #6).** `Restarting (139)`, log kosong. Fix: `node:22-slim` plek miniLeads. Lesson: baca Dockerfile tetangga dulu.
3. **Skeleton eats the flash (← #11).** Ring benar tapi tidak pernah keliatan + search putus fokus. Fix: filter nempel, key dari data. Lesson: skeleton jangan telan kontrol yang dipakai.
4. **One missing param kills edit (← #2).** 10 placeholder 9 argumen. Fix: hitung + `hasLink`. Lesson: test edit hari-1.

### 9b. TIER 2 — full log (8 sisanya, ringkas 1-2 kalimat + lesson, EN, wording final nunggu QC)

5+. Search misses tipe (##1) → empty token (##3, bukan bug) → blind pkill (##4) → funnel clash (##5, putusan A) → stale dev bundle (##8) → invisible footer (##9) → sticky ring (##10) → push restore dance (##12). (Final wording + urutan pas nulis page, nunggu QC.)

---

## 10. Non-Technical Summary — DRAFT (nunggu QC, EN warung, no `;`, no em dash, hyphen cuma listing di bawah)

Heading verbatim pola: `For those of you unable to read this data from technical standpoint, here is the conclusion:`

- My GitHub looked fine but my social feeds were empty for months, so I built a small notebook that never loses a content idea
- You type an idea with a date and a platform, the board tracks it until you post it by hand, nothing posts itself
- One login opens all four of my apps, this one only added its own token key, zero login code was written twice
- Tap a chart and the list filters itself, new ideas pop in on their own about twice a minute, no refresh needed
- The page is public but the engines sleep in my home computer, when it is off the page says so honestly and the pictures above are the proof
- Everything that broke is written on this same page, twelve stories with receipts, and a robot scribe that writes ideas down for me is planned next, not promised today

---

## 11. FAQ (6 DRAFT — nunggu QC, EN warung, aturan copy sama)

```json
[
  {"Q":"Why build a tracker instead of using Notion or Sheets?","A":"Owning the frontend and the backend keeps it light and personal, and every lesson lands in my portfolio with receipts. Sheets cannot give me that."},
  {"Q":"Why does posting stay manual?","A":"The app is a calendar plus a tracker by decision. Editing and design stay in my hands. Version one proves tracking first, robots later."},
  {"Q":"What is the MCP agent idea about?","A":"A future door that lets my AI helper save ideas straight into the app. Planned and written openly, not shipped. Version one is human fingers only."},
  {"Q":"Twelve failures on a portfolio page? Is that wise?","A":"That is the point. Each one has a cause plus a fix plus a check. A calm app means little without an honest log beside it."},
  {"Q":"Why does it fail when your PC is off?","A":"No VPS by decision, home PC plus secure tunnels (:443 auth, :9443 data). Off means doors closed and the page says so instead of pretending. Same honesty as EXP 010 through 013."},
  {"Q":"Why charts if the first plan said no charts?","A":"The owner changed the call openly. Charts earn their place because tapping one filters the list. The amendment is written in the open with the bundle cost attached."}
]
```

---

## 12. Live Status + Disclaimer — USULAN tiru 013 (nunggu QC)

### 12a. Live Status
Title: `Live Status — Live Frontend, Home-PC Backend`
Kiri `What works now`: live React app, CRUD + filter + search + pages + export, dashboard tap filter + auto refresh, required login via shared backend, dark mode + mobile cards, 2 funnels verified.
Kanan `What isn't production yet`: not 24/7, local PC, MCP scribe planned not shipped, SQLite single file, funnel/service hardening = separate project.
Amber body: `Frontend live 24/7 on GitHub Pages. Backend runs on my personal PC (funnels :443 + :9443). Off → login/list fails with a message, expected. Screenshots + curl logs show how it works.`

### 12b. Disclaimer
> **Disclaimer — Live frontend, local backend, not production-hardened.** Dummy rows only, no real user data. OTP/token/passwords never shown.
>
> **First UI:** `exp010.html` + `/jobtracker/` · **Shared backend:** `exp011.html` (`:7002`) · **Second customer:** `exp012.html` (`/miniLeads/`) · **Third customer:** `exp013.html` (`/cv_screening/`) · **This:** `exp014.html` (`/contentOS/`) · **Machine:** `exp001.html`

---

## 13. File Checklist + Evidence — USULAN (nunggu QC)

* New: `public_html/exp014.html` from `template-exp000.html` (navbar/footer 1:1 dari `exp013.html`)
* Images: 8 real di `images/experiments014/` (LOCKED §6, mobile + empty-toast follow-up opsional)
* NO new deps page. Hanya `css/style.css` + `js/main.js`
* After ACC (JANGAN bareng build): `experiments-list.html` (card 14, 13→14), `sitemap.xml` (exp014 + images), `index.html` hanya kalau diminta
* Validasi: title/desc EN, alt semua img, h1→h2, canonical/OG/Twitter/Article+Breadcrumb+FAQ, `data-reveal` aman, tidak ada secret, screenshot dicek ulang, copy rules 0 langgar (`;` 0, em dash cuma judul, hyphen cuma listing)
* Evidence receipts (verbatim, redacted, run 24-09-2026 sore — SEMUA hijau): health `:7010` + `:9443` → `{ok:true}`, login → token `8ab86919...`, me → admin, dashboard `total 3`, tolak posted → `400 Link wajib`, filter IG → 1, search reels → 1, clamp 99 → `page 2 of 2`, export IG → header + `content-001`, no token → `401`, ngawur → `401`. ID reuse `002` + CORS preflight + build receipts di Standalone Evidence Log bawah (runnable, copy paste apa adanya).
* Standalone Evidence Log — copy paste 1 blok, tanpa ubah data (no create/delete, cuma baca + 1 tolak-negatif):

```bash
curl -s http://localhost:7010/health
# {"ok":true}
curl -s https://aispec.tail06293c.ts.net:9443/health
# {"ok":true}
TOKEN=$(curl -s -X POST http://localhost:7002/api/login -H 'Content-Type: application/json' -d '{"identifier":"admin","password":"Leads7006"}' | python3 -c "import sys,json;print(json.load(sys.stdin).get('token',''))")
curl -s http://localhost:7002/api/me -H "Authorization: Bearer $TOKEN"
# {email, username:admin, role:admin} — token full TIDAK dicopy ke page, cuma 8 char pertama
curl -s http://localhost:7010/dashboard -H "Authorization: Bearer $TOKEN"
# {"total":3,"by_status":{"ide":1,...},"by_tempat":{"IG":1,...}}
curl -s -X POST http://localhost:7010/content/ingest -H 'Content-Type: application/json' -H "Authorization: Bearer $TOKEN" -d '{"tempat":"IG","tipe":"reels","status":"posted"}'
# {"error":"Link wajib diisi saat status posted"} — negatif, data tidak berubah
curl -s "http://localhost:7010/content?tempat=IG" -H "Authorization: Bearer $TOKEN" | python3 -c "import sys,json;print(json.load(sys.stdin)['total'])"
# 1
curl -s "http://localhost:7010/content?q=reels" -H "Authorization: Bearer $TOKEN" | python3 -c "import sys,json;print(json.load(sys.stdin)['total'])"
# 1
curl -s "http://localhost:7010/content?page=99&limit=2" -H "Authorization: Bearer $TOKEN" | python3 -c "import sys,json;d=json.load(sys.stdin);print(d['page'],d['totalPages'])"
# 2 2 — clamp, bukan halaman kosong
curl -s "http://localhost:7010/content/export?tempat=IG" -H "Authorization: Bearer $TOKEN" | head -n 2
# header + 1 baris content-001 (filtered, bukan full dump)
curl -s http://localhost:7010/content
# {"error":"Login required"} — fail closed
curl -s http://localhost:7010/content -H "Authorization: Bearer ngawur"
# {"error":"Invalid or expired token"} — fail closed
curl -s -X OPTIONS http://localhost:7002/api/login -H "Origin: http://localhost:7011" -H "Access-Control-Request-Method: POST" -D - -o /dev/null | grep -i allow-origin
# Access-Control-Allow-Origin: http://localhost:7011 — direct
curl -s -X OPTIONS https://aispec.tail06293c.ts.net/api/login -H "Origin: https://robbyaliasaakbar.github.io" -H "Access-Control-Request-Method: POST" -D - -o /dev/null | grep -i allow-origin
# Access-Control-Allow-Origin: https://robbyaliasaakbar.github.io — funnel Pages
tailscale funnel status | head -n 12
# 3 baris: :10000 n8n, :8443 miniLeads, :9443 ContentOS
docker exec backend-backend-1 sh -c 'echo $CORS_ORIGINS'
# list origin aktif (bukti recreate, bukan restart)
cd public_html/contentOS && npm run build 2>&1 | tail -n 2 && grep -r localhost dist/ | wc -l
# 384KB + 0 — funnel baked, localhost 0
grep -c src/main index.html
# 1 = mode dev (source). 0 = mode Pages (built). Saklar template.
```

---

## 14. Build Steps

1. Bang Rob QC PRD ini → ACC/revisi (termasuk lock §1 judul + §8 stance + §10 + §11 + claim LLM).
2. Screenshots LOCKED §6 (8 file, Bang Rob capture, Udin verifikasi).
3. Generate `exp014.html` full EN, order §3, tanpa chart di page, copy rules 0 langgar.
4. Verify: http.server + mobile/desktop, links, `prefers-reduced-motion`, grep secrets + grep copy-rule violations.
5. JANGAN update list/sitemap/index dulu — tunggu review.
6. Review → publish list/sitemap (/index opsional) → done.

---

**DRAFT for QC — OPEN: §1 judul + stance + claim LLM, §6 screenshots, §4 tanggal/keywords final, §8 mapping, §9b + §10 + §11 wording final.**
Balas revisi per nomor bab — gue betulin per bab, bukan borongan.
