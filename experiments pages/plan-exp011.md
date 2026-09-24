# PLAN — Experiment 011

**One Auth Backend for Many Frontends — PHP, SQLite & Docker Entry Point in a Day**

> **Location:** `/public_html/exp011.html`
> **Plan file:** `PRD/plan-exp011.md`
> **Date publish:** `2026-09-09` (sama kayak EXP 010, locked request Bang Rob)
> **Status:** `Stable` — backend locked, local PC 08.00–21.00 WIB
> **Lang:** `en` (full English di page, PRD ini Indonesian biar gampang QC)
> **Author:** Robby Aliasa Akbar
> **Live API:** `http://localhost:7002` lokal + tunnel `https://aispec.tail06293c.ts.net` saat PC nyala (frontend di `https://robbyaliasaakbar.github.io/jobtracker`)
> **Sources:** folder `backend/` (`public/index.php`, `src/config.php`, `src/db.php`, `src/response.php`, `src/validate.php`, `src/mail.php`, `src/auth.php`, `src/lamaran.php`, `Dockerfile`, `docker-compose.yml`, `.env.example`, `PRD/prd-backend.md`) + `public_html/jobtracker/` (klien pertama) — JANGAN buka `.env` asli, JANGAN tampilkan `SMTP_PASS` / token / OTP asli
> **Relation:** EXP 010 = frontend (UI diklik) → EXP 011 = backend ini (tempat token + data) → EXP 001 = mesin AI-nya. Lamaran dibahas opsi A: sebagai KLIEN backend (CRUD wajib token + cek milik sendiri), detail UI tetap di EXP 010.
> **Claim locked:** dibangun dengan local LLM sama persis `Tiel-Coder-35B-A3B-MTP-UD-Q4_K_S.gguf` via `llama.cpp-server`. Flag inference TIDAK dishare (refer EXP 001).

---

## 1. Objective

Answer the research question:

> **Can one small auth backend — PHP + SQLite + Docker, no framework, no Composer — serve many frontends with real OTP, opaque tokens, rate limits and per-user lamaran sync, built with only a 35B local coder?**

This page documents **full backend, bukan entrypoint doang**. Biar backend tidak dianggap tempelan: router, 6 endpoint auth, skema DB, Docker, CORS, rate limit, SMTP mentah, plus lamaran sebagai klien. Full breakdown ada di sini, tidak disebar ke page lain.

**Key stance:** *One backend for many apps beats copy-paste auth per app. Vanilla PHP (bawaan `password_hash`, `random_bytes`, `PDO`) + 1 file SQLite + 1 Docker image is enough for local-first internal tools, auditable line-by-line. Backend runs locally on personal PC for now (08.00–21.00 WIB), not 24/7 VPS — honest limitation, expected behavior.*

Source of truth: JANGAN ngarang di luar file yang dibaca. Nama file, endpoint, nama tabel, TTL, limit harus plek `backend/` + `prd-backend.md`.

---

## 2. Audience & Goals (internal, JANGAN sebut eksplisit di page)

**Primary (internal note):** builders yang mau reuse 1 auth buat banyak app + reviewers yang butuh bukti backend real.
**Secondary:** Pembaca EXP 010 — mereka lihat rantai: klik UI (010) → dari mana token/data (011) → mesin apa yang ngebangun (001).

**Goals:**
1. Buktikan **real + runnable**: `docker compose up` → `/api/health` → `register → verify → login → me` via curl.
2. Buktikan **reusable**: Job Tracker (`:7001`) pelanggan pertama, webapp berikutnya (`:7003`...) tinggal daftar origin, tidak ngoding ulang auth.
3. Buktikan **aman buat skala lokal**: hash bcrypt, token opaque 1 jam, OTP 5/10 mnt + max 5 coba, respon generik anti-panen email, query parameterized, CORS allowlist + same-host Tailscale, error 500 generik.
4. Buktikan **no dependency**: 0 Composer, SMTP mentah vanilla, SQLite 1 file.
5. Jaga credibility: `Stable` untuk code, tapi `Live Status` jujur backend masih local PC.

---

## 3. Styling & Stack (plek EXP 010, wajib seragam)

* **Stack:** `HTML5 + Tailwind CSS via CDN + Vanilla JS` only. JANGAN load `charts.js` / Chart.js. Cuma `js/main.js`.
* **Colors:** `ink #0C0C0C / paper #FFFFFF / accent #D70000` + emerald/amber untuk status cards.
* **Fonts:** `Manrope (display) / Inter (body) / JetBrains Mono (mono)`.
* **Tokens wajib:** `eyebrow`, `status-badge status-stable`, `border rounded-2xl`, `summary-callout`, `quote-block`, `detail-label`, `stat-tile`, `code-block`, `code-inline`, `data-reveal`, `bg-grid`, `bg-dark-section`, `nav-link nav-active`, `arrow-link`, `btn-primary`, `btn-outline`.
* **Navbar/Footer:** Copy 1:1 dari `exp010.html`. Ganti hanya breadcrumb `010 → 011`, `og/url`, `canonical`, nomor experiment.
* **Internal links wajib jalan:** `/`, `profile.html`, `experiments-list.html`, `contact.html`, `Back to Experiments`, `Have a problem...`, plus `exp010.html` (frontend) + `exp001.html` (inference). Teaser EXP 012 JANGAN (belum ada, jangan bikin 404).
* **Order locked:** Hero + Breadcrumb → System Requirements → Screenshots (terminal) → Live API (curl, NO live buttons) → Non-Technical Summary → Experiment Details → Backend Code → FAQ → Live Status → Disclaimer → Footer CTA.
* **Aturan page:** full English, JANGAN sebut eksplisit audience (`vibe coders / CTO / recruiter`). Kalimat netral kayak EXP 010 revisi.

### 3b. Tone — EXP 004 locked (request Bang Rob: karakter tetap ada)

Referensi wajib: `exp004.html`. Copywriting EXP 011 harus berasa 1 orang yang sama nulis.

* **Voice:** first-person `I` (bukan `we`). Direct, blunt, humble. Ngaku gagal dulu baru pamer hasil. Contoh pola EXP 004: `My reasoning turned out to be completely flawed`, `failed utterly`, `hallucinates a lot`, `I switched to...`.
* **Concrete over hype:** selalu angka + nama exact. Contoh pola: `4 of 266 nodes`, `0 hallucinations`, `6 endpoints`, `5/min`, `5/10 min OTP`, `1-hour token`, `1 file SQLite`, `1 Docker image`, `0 Composer packages`. JANGAN kata sifat marketing (`cutting-edge`, `seamless`, `revolutionary`).
* **Honest disclaimer:** tiap klaim sensitif kasih batas jujur. Contoh pola: `by my own estimation — not a guarantee`, `I cannot disclose ... yet due to credibility and legal`, `OTP plaintext locally for learning, hashed in production later`, `Mailpit removed 2026-09-09`.
* **Practical:** kasih jalan mulai yang kecil. Contoh pola: `If you want to build something like this, start by...`, `Next app just adds its origin, no re-code`.
* **Emoji:** HANYA di Non-Technical Summary (kayak EXP 004 ✨🤝💰🛡️🎯), 1 per paragraf maksimal. JANGAN di Details / Code / FAQ / Tiles.
* **Kalimat pendek deklaratif:** 1 ide per kalimat. Hindari pasif berlapis. Contoh: `I need...`, `I chose ... because...`, `It handles ... not ...`.

---

## 4. SEO & Metadata

```html
<title>Experiment 011 — One Auth Backend for Many Frontends — PHP SQLite Docker — Robby Aliasa Akbar</title>
<meta name="description" content="One PHP + SQLite + Docker auth backend for many frontends, built with only Tiel-Coder 35B local LLM: OTP via Gmail, opaque tokens, rate limits, per-user lamaran API, zero dependencies.">
<link rel="canonical" href="https://robbyaliasaakbar.github.io/exp011.html">
<meta property="og:title" content="Experiment 011 — One Backend, Many Frontends, Zero Dependencies">
<meta property="og:description" content="PHP 8.3 + SQLite + Docker: 6 auth endpoints, Gmail OTP, 1-hour tokens, CORS + rate limits. Full backend, not just an entrypoint.">
<meta property="og:image" content="https://robbyaliasaakbar.github.io/images/experiments011/docker-ps.webp">
<script type="application/ld+json"> Article + BreadcrumbList + FAQPage
```

**Keywords:** `["auth backend PHP", "SQLite auth", "Docker PHP SQLite", "opaque token", "OTP Gmail SMTP", "CORS Tailscale", "rate limit", "lamaran API"]`
**Dates:** `datePublished 2026-09-09`, `dateModified 2026-09-09`.
**Robots:** `index, follow, max-image-preview:large`.

**After review (JANGAN sekarang):**
* `experiments-list.html` card 11 + `numberOfItems:10 → 11`
* `sitemap.xml` add `exp011.html` + 6 images
* `index.html` hanya kalau Bang Rob minta (di EXP 010 dibiarin)

---

## 5. System Requirements (8 tiles)

Grid `grid-cols-2 md:grid-cols-3`, sama kayak EXP 010.

| Tile | Label | Value |
|------|-------|-------|
| 1 | CPU | `Intel Core i5-11400F` (reuse) |
| 2 | RAM | `16GB DDR4 3200MT/s` (reuse) |
| 3 | GPU | `RX 6700 XT 12GB` (inference lokal, reuse) |
| 4 | OS | `Ubuntu Desktop 26.04 LTS` (reuse) |
| 5 | Model | `Tiel-Coder-35B-A3B-MTP-UD-Q4_K_S.gguf` (sama persis) |
| 6 | Engine | `llama.cpp-server` (flag di EXP 001) |
| 7 | Stack | `PHP 8.3 + PDO SQLite + Docker` |
| 8 | Time / Status | `Same 35B local coder / STABLE` |

Note under grid (EN):
> `Same local coder as EXP 010 via llama.cpp-server. Inference flags in EXP 001. Backend runs via 1 Docker image (php:8.3-cli + pdo_sqlite), 1 SQLite file, 0 Composer packages.`

---

## 6. Screenshots — Gallery (6 real, sensor-friendly)

Files real di `public_html/images/experiments011/` (case-sensitive, typo biarin):

1. `docker-ps.webp` (1049x653) — `docker compose up` + `docker ps` + health. Alt: `Docker backend running on port 7002`. Jadi `og:image` (980→1049 ok, update width/height 1049x653).
2. `health-curl.webp` (1047x171) — `curl /api/health → {ok:true}`. Alt: `Health check returning ok without login`.
3. `auth-flow.webp` (1048x254) — `register → verify → login → me`, OTP/token di-sensor `******`. Alt: `Register verify login me flow with redacted secrets`.
4. `databse-users.webp` (1590x368, nama typo biarin) — `.tables` + `.schema` + `COUNT(*)`. Alt: `SQLite schema showing 6 tables structure only`. JANGAN `SELECT *`.
5. `Cors-preflight.webp` (809x452, C gede) — `OPTIONS` + `Access-Control-Allow-Origin`. Alt: `CORS preflight allowing frontend origin`.
6. `rate-limit.webp` (744x452) — `401 401 401 401 401 429`. Alt: `Rate limit returning 429 after 5 tries`.

Layout:
* Wrapper `bg-neutral-50 py-20`.
* Row 1 (full): `docker-ps` (hero terminal)
* Row 2 (2 cols): `health-curl` | `rate-limit`
* Row 3 (2 cols): `auth-flow` | `Cors-preflight`
* Row 4 (full): `databse-users` (wide 1590, scroll-x di mobile)
* Tiap figure `border rounded-2xl bg-paper shadow-sm` + figcaption uppercase. NO placeholder.

---

## 7. Live API (pengganti Visualization, NO CHART, NO live buttons)

Section putih `py-20`, setelah Screenshots. JANGAN tombol `Open Backend` (tidak ada UI, + offline di luar jam). Kasih curl copy-paste + note jam.

* Title: `Talk to it with curl — no UI needed.`
* Sub: `Backend live daily 08.00–21.00 WIB (UTC+7) when my PC is on. Outside those hours curl fails — expected (see Live Status).`
* 3 mini blocks (`code-block`):
  1. `GET /api/health` → `{"ok":true}`
  2. `POST /api/register → /api/verify → /api/login → GET /api/me` (kata `******` untuk kode/token, jangan nilai asli)
  3. `GET /api/lamaran` dengan `Authorization: Bearer <TOKEN>` (teaser lamaran sebagai klien)
* Mono note: `Base lokal: http://localhost:7002 • Tunnel saat PC nyala: https://aispec.tail06293c.ts.net • .env tidak pernah ditampilkan`
* JANGAN embed terminal live, JANGAN tampilkan `SMTP_PASS` / token asli.

---

## 8. Experiment Details (10 subsections, EN, first-person, neutral)

Wrapper `py-20 bg-neutral-50`, `space-y-12`, pola `detail-label`.

1. **Problem** — Every new webapp re-builds register + OTP + login + forgot. Duplicated 5x, drifts fast. Need: 1 local auth service many frontends can share, with frontend only doing `fetch`.
2. **Why Not Framework / Composer** — Locked PHP (same family as invoice `save.php`): `password_hash`, `random_bytes`, `PDO` already built-in. Docker because laptop has no native PHP + no sudo. Vanilla SMTP because every line traceable (`EHLO → STARTTLS → AUTH → MAIL → RCPT → DATA → QUIT`). Mailer libs + JWT noted as production upgrades, not v1.
3. **The Build (same local coder)** — Built with same `Tiel-Coder-35B` via `llama.cpp-server`. AI handled syntax + boilerplate, decisions stayed with me. Flags in EXP 001. Honest scope: local + auth flow working, full hardening/deploy = separate project.
4. **Router (entry, not all)** — `public/index.php`: load `.env` simple (no lib) → CORS check (explicit list + same-host Tailscale) → `OPTIONS 204` → route `health / register / verify / login / me / forgot / reset / lamaran` → generic `404/405/500`. All decisions in backend, frontend only throws `fetch`.
5. **Auth Flow (6 endpoints)** — `register` (pending + OTP Gmail) → `verify` (active + 1h token) → `login` (token) → `me` (guard) + `forgot` (always generic 200) → `reset` (rehash + wipe sessions → force relogin). Password rule `≥8 + upper + digit` checked on server. OTP 6-digit 5 mnt, reset 10 mnt, max 5 tries + temp lock. Responses generic anti-harvest.
6. **Data Model (1 file, 6 tables)** — `users(email UNIQUE, password_hash, nama, telepon, pending→active)` + `otps` + `reset_tokens` + `sessions(token 64hex, 1h)` + `rate_limits` + `lamaran(email owner, company, position, tanggal, status, portal, link + idx email)`. Migration via `PRAGMA table_info + ALTER`, no data loss. Honest tradeoff: OTP plaintext short-expiry locally for traceability, hashed in production later.
7. **Docker + Run (1 command)** — `php:8.3-cli + pdo_sqlite + sqlite3` image, `CMD php -S 0.0.0.0:7002 -t public public/index.php`, `docker compose up --build` → webapp `python3 -m http.server 7001`. Data persists in `./data/` (gitignored). No PHP native needed.
8. **CORS + Rate Limit (why different ports work)** — Explicit `CORS_ORIGINS` (`:7001`, tambah `:7003` for next app, no re-code) + same-host pass (Tailscale IP changes need no config). `OPTIONS` preflight 204. Rate: max 5/min/IP on sensitive endpoints via SQLite. Lamaran as CLIENT (opsi A locked): all `/api/lamaran` require token, handlers check `email from token == row owner`, frontend `store.js` only wraps `GET/POST/PUT/DELETE`. UI detail stays in EXP 010.
9. **What Failed** — (a) Different port = different origin → fetch blocked until CORS allowlist fixed. (b) Mailpit removed 2026-09-09, OTP must go via real Gmail App Password (16 chars, not login password). (c) 1-hour expiry first looked like bug, turned out expected guard behavior.
10. **Evidence (6 tiles)** — `1 image docker`, `1 file SQLite`, `6 endpoints auth`, `5/min rate`, `5→10 min OTP`, `0 deps Composer`. Result: stable local backend, reusable by next app. Learned: small explicit files (`config/db/response/validate/mail/auth/lamaran`) beat magic; opaque tokens easy to trace (`SELECT`) + revoke (`DELETE`). Chain: `EXP 010 (UI) → EXP 011 (this backend) → EXP 001 (machine)`.

---

## 9. Backend Code (3 blocks essence, REDACTED)

**Block 1: Router CORS essence (`public/index.php`, dipersingkat, no secrets):**
```php
$origin = $_SERVER['HTTP_ORIGIN'] ?? '';
$boleh = array_map('trim', explode(',', $cfg['CORS_ORIGINS']));
$izinkan = in_array($origin, $boleh, true);
// + same-host Tailscale pass, then:
header("Access-Control-Allow-Origin: $origin"); // only if allowed
// OPTIONS → 204, then route health/register/verify/login/me/forgot/reset/lamaran
```

**Block 2: DB essence (`src/db.php`, schema only):**
```sql
users(email UNIQUE, password_hash, nama, telepon, pending→active)
otps(email, kode 6-digit, +5min, max 5 tries)
sessions(token 64hex, +1h) -- opaque, SELECT to check, DELETE to revoke
lamaran(email owner + idx, company, position, tanggal, status, portal, link)
```

**Block 3: Token check essence (guard pattern):**
```php
$email = email_dari_token($pdo, $_SERVER['HTTP_AUTHORIZATION'] ?? '');
if ($email === null) json(401, ['error' => 'Token tidak sah atau kedaluwarsa']);
```

Note: JANGAN tampilkan `.env` isi, `SMTP_PASS`, `auth.db` rows, OTP/token asli, `src/auth.php` full. Itu sensitif.

---

## 10. Non-Technical Summary (summary-callout)

Heading: `For those of you unable to read this data from technical standpoint, here is the conclusion:`

1. Every new app needs login. Copying login 5 times gets messy fast.
2. I built one small login service on my own machine with local AI — no extra packages.
3. The Job Tracker is the first customer. Next apps just register their address, no re-code.
4. It sends 6-digit codes via email, gives 1-hour passes, blocks spam after 5 tries.
5. It lives on my home PC (08.00–21.00 WIB). Next page in chain is the machine that built it.

---

## 11. FAQ (3, EN, JSON-LD juga)

```json
[
  {"Q":"Why PHP + SQLite + Docker instead of Node + Postgres?","A":"Locked for this scale: PHP built-ins already cover hash + random + PDO, SQLite is 1 file with zero server, Docker gives PHP without native install. Postgres + JWT + mailer libs are noted as production upgrades, not v1."},
  {"Q":"How can :7001 talk to :7002 without being blocked?","A":"Explicit CORS allowlist plus same-host pass for Tailscale. Preflight OPTIONS returns 204 with Allow-Origin only for listed hosts. Next app (:7003) just gets added to the list."},
  {"Q":"Why does the API sometimes not answer?","A":"Not on a 24/7 VPS yet — home PC, online daily 08.00–21.00 WIB (UTC+7). Outside those hours fetch fails and frontend shows a toast. Screenshots + curl logs remain as proof."}
]
```

---

## 12. Live Status + Disclaimer

### 12a. Live Status (2 kolom + amber, sebelum footer, sama kayak EXP 010)
Title: `Live Status — Local PC, Not Yet 24/7 VPS`
Kiri `What works now`: Docker 1-command up, health ok, 6 auth endpoints, Gmail OTP, 1h tokens, rate 5/min, lamaran per-user sync, reusable origins, online 08.00–21.00 WIB.
Kanan `What isn't production yet`: not 24/7, still local, early features, collecting feedback, hardening/deploy = separate project.
Amber body: `Runs locally on my personal PC, live daily 08.00–21.00 WIB (UTC+7). Off-hours → unreachable, expected. Screenshots + curl logs show how it works.`

### 12b. Disclaimer
> **Disclaimer — Local stable, not production-hardened.** Validated with same Tiel + llama.cpp-server (flags EXP 001). OTP plaintext short-expiry locally for learning, hashed in production later. Mailpit removed 2026-09-09, Gmail only.
>
> **Frontend:** `exp010.html` + `/jobtracker/` · **This:** `exp011.html` backend · **Machine:** `exp001.html`

---

## 13. File Checklist

* New: `public_html/exp011.html` from `template-exp000.html` (navbar/footer 1:1 dari `exp010.html`)
* Images: pakai 6 real di `images/experiments011/` (perhatikan `Cors-preflight.webp` C gede + `databse-users.webp` typo biarin, jangan rename biar link tidak putus)
* NO new deps. NO `charts.js`. Hanya `css/style.css` + `js/main.js`
* After ACC (JANGAN bareng build): `experiments-list.html` (card 11, 10→11), `sitemap.xml` (exp011 + 6 images), `index.html` hanya kalau diminta
* Validasi: title/desc EN, alt semua img, h1→h2, canonical/OG/Twitter/Article+Breadcrumb+FAQ, `data-reveal` aman, tidak ada secret (`.env`, pass, token, OTP) di HTML maupun JSON-LD

---

## 14. Build Steps

1. Bang Rob QC PRD ini → ACC/revisi (saat ini: DONE, tunggu `gas build`).
2. Generate `exp011.html` full EN, order §3, tanpa chart, 6 gambar real, curl blocks redacted.
3. Verify: http.server + mobile/desktop, internal links, `prefers-reduced-motion`, tidak ada secret ter-`grep`.
4. JANGAN update list/sitemap/index dulu — tunggu review.
5. Review → publish list/sitemap (/index opsional) → done.

---
**Ready for QC.** Balas `gas build` — gue generate `exp011.html` 1 halaman penuh sesuai PRD ini.
