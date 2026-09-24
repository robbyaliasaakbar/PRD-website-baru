# PRD - ContentOS Webapp (utama)

> Tanggal: 24-09-2026
> Pemilik: bang rob
> Status: LOCKED - siap build
> Sumber mentah: DRAFT_WEBAPP_ContentOS.md (24-09-2026). File EXP014 (DRAFT_EXP014.md) di-PAUSE, jangan dipakai.

## 1. Latar Belakang & Tujuan

ContentOS itu Notion-lite pribadi buat kelola ide + jadwal konten IG / LinkedIn / Reddit / WA.
Ngedit, desain, posting tetap manual. App cuma portal kalender + tracking. NO AI di dalam app.

Kenapa bikin sendiri, bukan Sheets / Notion?
- Authentic FE buatan sendiri, BE buatan sendiri — diputus bang rob 24-09-2026.
- Personal, enteng, ga banyak integrasi macem-macem.
- Easy to use, ramah pemula.

## 2. User & Cara Jalanin

- Operasional: bang rob + tim internal kurleb 5 orang — diputus bang rob 24-09-2026.
- Publik (<100 orang, bukan ribuan): cuma buat marketing / ngetest. Recruiter yang mau nyoba portfolio, follower yang kepo pas bang rob posting. Kalau membludak wajib upgrade — diputus bang rob 24-09-2026.
- Jalan: frontend live GitHub Pages `/contentOS/` sama persis kayak exp012/013, backend lokal (PC 08.00-21.00 WIB). HP + laptop 375px-1280px.
- Login: wajib dari hari-1, reuse auth `:7002`. Tanpa landing statik, buka URL langsung Auth.

## 3. Scope (Wajib vs JANGAN)

Wajib V1 (7 biji, FIX, ga kepanjangan — diputus bang rob 24-09-2026 karena exp012/013 lebih rumit aja kelar):
1. Login reuse `:7002` (register / OTP / login / me)
2. CRUD 10 kolom per-user (`content-xxx`)
3. Filter + search + pagination
4. Export CSV ngikutin filter
5. Validasi link wajib saat `posted`
6. Dashboard count (`total, by_status, by_tempat`, klik = filter)
7. Responsive + dark mode WAJIB

JANGAN V1 di ContentOS (dibuang dari backlog, pindah jadi bahan pages experiment — diputus bang rob 24-09-2026):
V2 teaser (FullCalendar, kanban, upload thumbnail, reminder H-1, link_mirror, n8n worker, AI generator, auto-post), Scale (Postgres/Redis, Next.js/managed DB/JWT), Dev (TS/zod/vitest, Tailwind lokal, Mailpit). Alasan: bukan pending development, penting buat experiment tambahan nanti.
Keep di backlog ContentOS: revisi mobile detail + 1 baris tier B/C.

## 4. Data Model

Tabel `content_ideas` (SQLite `content.db`, isolasi dari `leads.db`):

- `id TEXT PRIMARY KEY` isi `content-001` lanjut terus. Aturan reuse: pake slot terkecil yang kosong dulu. Contoh ada 001,003,004 → baru pake 002 dulu baru 005 — diputus bang rob 24-09-2026.
- `user_email TEXT` (milik siapa, admin lihat semua)
- `tgl_buat TEXT` AUTO `dd-mm-yyyy`
- `jadwal_posting TEXT` date `dd-mm-yyyy` aja, tanpa jam. Input `type=date` — diputus bang rob 24-09-2026.
- `tempat` dropdown: IG, LinkedIn, Reddit, WA
- `tipe` dropdown: story, feeds, reels, carousel, text post, video
- `kategori` dropdown: ai, web-app, app, automation, llm-infra, daily
- `description` textarea ide mentah 1-2 kalimat
- `storyboard` textarea markdown bebas (storyboard + dialog + narasi, JANGAN dipecah)
- `caption` textarea final siap copy-paste
- `status` dropdown: ide, post-production, revision, posted. Warna opsi A — diputus bang rob 24-09-2026: ide abu zinc-500, post-production kuning amber-500, revision biru sky-500, posted ijo emerald-600. Light pastel, dark transparan.
- `link_postingan` url nullable. Aturan: boleh kosong saat ide/post-production/revision. WAJIB saat posted. Posted kosong → 400 + warning merah.

Aturan tegas: `tempat` = platform doang, `tipe` = format doang. Contoh valid: IG + reels.
Wajib vs opsional: cuma link saat posted yang wajib, sisanya opsional semua (resiko DB kosong ditanggung bang rob, bisa cek manual) — diputus bang rob 24-09-2026.

Contoh 1 object:
```json
{
  "id": "content-002",
  "user_email": "admin",
  "tgl_buat": "24-09-2026",
  "jadwal_posting": "28-09-2026",
  "tempat": "IG",
  "tipe": "reels",
  "kategori": "ai",
  "description": "Hook 3 detik",
  "storyboard": "Hook - problem - CTA",
  "caption": "Final + hashtag",
  "status": "revision",
  "link_postingan": ""
}
```

Dummy 3 baris buat QC:
1. `content-001 | IG | reels | ide | link kosong`
2. `content-002 | LinkedIn | text post | revision | link kosong`
3. `content-003 | Reddit | video | posted | https://reddit.com/r/test (klik balik + copy)`

## 5. Detail Fitur Wajib

USULAN UDIN yang sudah di-lock bang rob (logic tetap bang rob yang atur, cocok dipake, salah revisi pas build).

**5.1 Login reuse**
Input: login (identifier+password), register (nama,username,email,password), verify (email+OTP 6-digit), forgot/reset.
Tombol: Masuk, Daftar, Verifikasi, Kirim OTP, Reset, Logout di Settings.
Validasi: kosong ditolak, email ada @, OTP 6 angka.
Sukses: save `content.token`, redirect Board, toast ijo `Login ok`.
Gagal: 401 generik `Email/password salah`, 429 `Kebanyakan coba`, 503 `Auth mati, nyalain :7002 dulu`. Fail closed.

**5.2 CRUD**
Form 10 kolom + `tgl_buat` auto + `id` auto slot terkecil.
Tombol: Simpan, Update, Hapus (confirm Ya/Batal), Batal, Copy link, Copy caption.
Validasi FE: cuma link saat posted yang wajib. URL harus http.
Sukses: `{action: created|updated}` toast `Tersimpan jadi content-00x`, reset form, nongol di Board sort `jadwal_posting ASC`.
Gagal: 400 `Link wajib diisi saat status posted` + merah inline, 401 login required, 404 bukan milikmu.

**5.3 Filter + search + pagination**
Dropdown tempat/status/tipe/kategori + `q` + page/limit (default 20, jepit 1-100, over-page clamp).
Sukses: tabel update + total update, klik dashboard = filter.
Kosong: `Belum ada konten, bikin dulu yuk` + tombol Ide baru.

**5.4 Export CSV**
Tombol Export ikutin filter aktif via fetch+token. Sukses download `content.csv`. Gagal toast merah.

**5.5 Validasi link**
FE cegah dulu, BE final 400. Tampil klik + copy kalau ada.

**5.6 Dashboard**
Kartu `total + by_status 4 + by_tempat 4` (`GET /dashboard`). Auto refresh tiap CRUD. Klik = filter. Auth mati → `-` + toast.

**5.7 Responsive + dark WAJIB**
Desktop table `hidden lg:block`, HP cards `lg:hidden`, tombol 44px, input 16px anti-zoom iOS. Toggle di Header+Settings, simpan `content.theme`, `class dark` di html. Default light.

## 6. Alur E2E (7 langkah, USULAN UDIN sudah lock)

1. Buka URL Pages `/contentOS/` → langsung Auth (cek `content.token`, valid Board, gak valid Login). No landing.
2. Baru: Daftar → OTP → auto Board kosong + CTA. Lama: Login → Board milik sendiri (admin semua).
3. Lihat dashboard total/by_status/by_tempat, klik kartu = filter.
4. Tambah ide enteng → toast `content-00x` → dashboard +1. Refresh tetap ada (SQLite).
5. Cari/filter → Detail → ganti ide→post-production→revision→posted. Posted kosong ditolak, isi link lolos + copy. Copy caption buat posting manual.
6. Settings: update profile, toggle dark/light, logout + Export CSV filter aktif.
7. PC/auth mati → toast `Server off 08.00-21.00` + kartu `-`. Fail closed.

Pola inti: Auth → Board → Form → store → tampil → filter → posted+link.

## 10. Verifikasi

| Cek | Cara | Harapan |
|-----|------|---------|
| Health + login reuse | GET :7010/health + login admin demo exp012/013 tanpa register | {ok:true} + masuk Board |
| 3 dummy | IG ide kosong, LinkedIn revision kosong, Reddit posted+link | Muncul + dashboard +3, kepisah per-user |
| Tolak posted kosong | posted tanpa link → Simpan | 400 + merah `Link wajib diisi`. Isi → lolos + klik + copy |
| Filter/search/page | tempat=IG, q=reels, page2 limit2 | Update bener, clamp, kosong tampil ajakan |
| Export | Export pas filter IG | content.csv cuma isi filter |
| Fail closed | Matikan :7002 → refresh | 503 + toast, bukan bocor |
| Klik dashboard | Klik posted | Auto filter posted |
| Dark+responsive | Toggle Settings, buka 375px + 1280px | Kesimpen, kartu HP gede, tabel aman |
| Build+Funnel | build → dist → Pages /contentOS/, 443→7002 8443→7010, grep localhost→0 | Live 08-21, mati toast jujur |
| Refresh | Refresh abis tambah | Tetap ada |

Akun demo ditulis atas izin bang rob 24-09-2026 (demo exp012/013 `admin`, mitigasi: dummy only, bisa diganti kapan aja). Ceklis 10 baris — diputus cukup bang rob 24-09-2026.

## 11. Rencana Building (3 step, USULAN UDIN sudah lock)

- Step 1 Kerangka + napas: skeleton + env 7010/7011 + health + db + login reuse + tambah + tampil. DONE: login admin + 1 ide + refresh ada.
- Step 2 Otak: search/filter/page + dashboard klik + validasi link. DONE: 3 dummy pisah + tolak kosong + klik filter.
- Step 3 Kulit + live: edit/hapus + export + dark + responsive + build Pages + Funnel + 10 ceklis hijau. DONE: grep 0 + toast jujur.
Diputus bang rob 24-09-2026.

## 12. Keputusan Locked (siapa + kenapa + tanggal)

- Latar authentic/enteng/pemula — bang rob 24-09-2026 karena mau FE/BE sendiri.
- User 5 internal + <100 publik marketing, upgrade kalau membludak — bang rob 24-09-2026.
- Scope 7 wajib + dark WAJIB, ga kepanjangan karena 012/013 lebih rumit — bang rob 24-09-2026.
- ID content-001 reuse slot terkecil — bang rob 24-09-2026.
- Cuma link posted wajib, sisanya opsional — bang rob 24-09-2026.
- Tanggal dd-mm-yyyy aja, input date — bang rob 24-09-2026.
- Warna A abu/kuning/biru/ijo — bang rob 24-09-2026 atas usulan Udin.
- Detail 7 fitur, alur 7 langkah, arsitektur pisah css/js, UI palette #0C0C0C/#FFFFFF/#D70000 — USULAN UDIN di-lock bang rob 24-09-2026 (validasi pas build, logic tetap bang rob).
- Stack A Basic 7010/7011 + CDN karena personal bukan client — bang rob 24-09-2026. B/C jadi bahan experiment.
- Verifikasi 10 baris + admin demo boleh ditulis (mitigasi demo) — bang rob 24-09-2026.
- Backend B reuse :7002, data baru :7010 pola miniLeads, B1 auto-lock, B2 content.db + contoh, B3 bcrypt/OTP6-5mnt-5x/opaque1jam, B4 langsung Gmail reuse, B5 7010/7011 + Funnel + CORS, B6 2 env + git bersih, B7 kontrak 7 endpoint, B8 9 curl — bang rob 24-09-2026.
- Step 1-2-3, backlog keep revisi mobile, buang V2/scale/dev jadi bahan experiment — bang rob 24-09-2026.
- File ini + 2 file stack status LOCKED 24-09-2026, tanpa versi.

Next: bilang **gas build** buat mulai Step 1.
