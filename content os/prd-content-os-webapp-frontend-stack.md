# ContentOS - frontend-stack

> Tanggal: 24-09-2026
> Status: LOCKED - siap build
> Pola acuan: public_html/miniLeads (src/api.js, auth.js, Auth.jsx, Settings.jsx, theme.js) + exp012/013 palette. Backend data :7010, auth :7002.

## 7. Arsitektur + Struktur File

Folder `public_html/contentOS/` (copy pola miniLeads, css/js pisah — diputus bang rob 24-09-2026):

```
contentOS/
 index.html (root jarang disentuh, mount #root)
 vite.config.js (base './', port dari .env VITE_FRONTEND_PORT)
 .env (lokal, gitignore)
 .env.production (URL Funnel publik, bukan rahasia, di-bake pas build)
 package.json (react 18, vite 6)
 src/
  main.jsx (mount App + applyTheme(getTheme()))
  App.jsx (pegang user + routing Auth/Board/Settings, jangan fetch langsung)
  api.js (SATU-SATUNYA yang fetch BE data :7010)
  auth.js (SATU-SATUNYA yang fetch auth :7002, TOKEN_KEY='content.token')
  theme.js (THEME_KEY='content.theme', get/save/apply class dark)
  Auth.jsx (login/register/OTP/forgot/reset, reuse pola Auth.jsx)
  Board.jsx (table hidden lg:block + cards lg:hidden + filter + pagination)
  ContentForm.jsx (form 10 kolom + warning link posted)
  Dashboard.jsx (kartu total/by_status/by_tempat, klik = filter)
  Settings.jsx (tampil AUTH_URL+API URL, profile, toggle dark, logout)
  index.css (SATU-SATUNYA tempat style, Tailwind + custom)
```

Aturan upgrade: ganti style → `index.css` doang. Ganti fitur → `Board.jsx/ContentForm.jsx/api.js`. Ganti auth → `auth.js`. Ganti DB → backend, FE ga ikut. Root disentuh cuma pas ganti port/build.

Kontrak fungsi FE (input → output):
- `getContent({q,tempat,status,tipe,kategori,page,limit}) → {total,page,limit,totalPages,count,data}` (tiru getLeads)
- `ingestContent(payload 10 kolom) → {action: created|updated, item}` (tiru ingestLead, id auto slot terkecil di BE)
- `deleteContent(id) → {ok:true}`
- `getContentById(id) → item | {error}`
- `downloadExport(filter) → content.csv` (fetch+token, bukan <a>)
- `getDashboard() → {total, by_status, by_tempat}`
- Auth: `apiLogin(identifier,password), apiRegister({nama,username,email,password}), apiVerify(email,kode), apiForgot(email), apiReset(email,kode,password_baru), apiMe(token)` → `{ok,status,data}`. Auth mati → `{ok:false,status:0}` + toast.
- Theme: `getTheme() → light|dark, applyTheme(t), saveTheme(t)`

Kenapa ada (reverse engineering, pemula-friendly):
api.js ada biar 1 pintu data + host ngikutin browser. auth.js ada biar 1 pintu :7002 + beda key `content.token` biar logout lokal per-app. theme.js ada biar dark kesimpen.

## 8. UI/UX

Palette plek exp012/013 — diputus bang rob 24-09-2026 atas usulan Udin:
ink #0C0C0C, paper #FFFFFF, accent #D70000, border zinc. Judul Manrope, isi Inter, angka JetBrains Mono. Light default, dark via `class dark`.

Layout atas-ke-bawah Board:
1. Header ramping: logo + ContentOS + badge online/offline + toggle dark + logout.
2. Dashboard: Total + by_status 4 (warna A: zinc-500/amber-500/sky-500/emerald-600) + by_tempat 4. Klik = filter.
3. Filter bar: search q + 4 dropdown + Reset + Export + `+ Ide baru` kanan.
4. Board: desktop table (ID, jadwal dd-mm-yyyy, tempat, tipe, status badge, link icon, aksi), mobile cards 1 kartu 1 ide tombol gede.
5. Form: 10 input vertikal, storyboard/caption gede + copy, warning merah link posted kosong.
6. Settings: URL + profile + mode + logout (tiru Settings.jsx).
7. Toast kanan-bawah ijo/merah/kuning + skeleton shimmer loading + fade 150ms + progress tipis export. Tanpa lib animasi.

Mobile 375px-1280px: tabel→kartu, filter grid 2 kolom, input 16px anti-zoom iOS, tombol min 44px. Revisi mobile detail masuk backlog (PRD ga harus sempurna) — diputus bang rob 24-09-2026.

## 9. Tech Stack FE + Cara Jalan

Tier A Basic (mastered, REKOMENDASI, dipilih bang rob 24-09-2026 karena personal bukan client):
React 18 + Vite 6 + Tailwind + `base './'` + Chart? No chart V1 (dashboard kartu doang). Dev `:7011`, BE `:7010`, auth `:7002`. GitHub Pages folder `/contentOS/` (index.html + assets di root folder kayak miniLeads). Funnel auth `:443→:7002`, data `:8443→:7010`. Online 08.00-21.00 WIB, mati toast jujur.
Plus: jalan lokal, tracing gampang. Minus: <100 oke, 1000+ ngos.

Tier B Basic Upgrade (tetap lokal, backlog, JANGAN sekarang):
+TS+zod+vitest, Postgres 16-alpine + Redis 7-alpine rate doang. RAM +<1GB. Berubah: api.js→ts, validasi zod, compose +2 container.

Tier C Enterprise (scalable, backlog, JANGAN sekarang kecuali bang rob bilang sekarang):
Next.js Vercel + CDN, Cloud Run/VPS, Postgres managed + Redis cluster, JWT+refresh, Resend/SES. Buat 100K-1M user. Biaya bulanan.

Port FIX: 7010 BE, 7011 FE, 7002 auth (range 7010-7015 masih kosong, dipilih Udin atas izin bang rob 24-09-2026). `.env` lokal kosongin URL, `.env.production` isi Funnel publik. API_URL ngikutin hostname (`location.protocol//hostname:PORT`) biar localhost + LAN + Funnel jalan tanpa ganti kode.

CDN/offline: butuh internet pas dev/build (Tailwind CDN, npm, Fonts, OTP Gmail) + runtime Pages+Funnel. Full offline backlog (vendor Tailwind lokal).

Verifikasi bundle: `npm run build → dist/ → grep -r localhost dist → 0` (harus 0, URL sudah Funnel). `vite preview --port 7011` cek dark + 375px + 1280px.
