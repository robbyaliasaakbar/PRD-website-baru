# ContentOS - backend-stack

> Tanggal: 24-09-2026
> Status: LOCKED - siap build
> Path kanonis auth (LIVE, yang diedit cuma tambah origin): backend auth `:7002` PHP (dipakai bareng JobTracker/CRM). Arsip museum JANGAN diutak-atik.
> Data baru (isolasi): `backend-server-contentOS/` port `:7010` (tiru `backend-server-minileads/server.js + db.js`).

### B1 Stack

Pilih B reuse auth + data baru pola miniLeads — diputus bang rob 24-09-2026 karena auth udah lolos OTP live + dipakai rame-rame. Bikin auth baru = bayar 2-3 jam + 2 sumber kebenaran.

- A Basic (dipilih): Node Express + better-sqlite3 1 file `data/content.db`, `requireUser` fetch `AUTH_URL/api/me` timeout 5s fail closed 503/401 + `userScope` (admin semua, user `user_email=?`), Docker Compose `network_mode:host`, Funnel. 0 dependency berat, tracing gampang.
- B Basic Upgrade (backlog, bahan experiment): Postgres 16-alpine + Redis 7-alpine rate doang, OTP hash, RAM +<1GB. Berubah: db.js→pg, compose +2 container.
- C Enterprise (backlog, bahan experiment): managed Postgres + Redis cluster + JWT + mailer service + Cloud Run. Buat 100K-1M, JANGAN sekarang.
- Jalan lokal Docker / native (Docker rekomendasi biar `network_mode:host` sama). Auth DILARANG dibangun ulang.

### B2 Skema database

File `backend-server-contentOS/data/content.db` (gitignore) + `db.js` auto-migrasi `PRAGMA table_info` → `ALTER ADD` + backup `.bak-tanggal`. Jangan DROP.

```sql
CREATE TABLE IF NOT EXISTS content_ideas (
  id TEXT PRIMARY KEY,
  user_email TEXT,
  tgl_buat TEXT,
  jadwal_posting TEXT,
  tempat TEXT,
  tipe TEXT,
  kategori TEXT,
  description TEXT,
  storyboard TEXT,
  caption TEXT,
  status TEXT DEFAULT 'ide',
  link_postingan TEXT
);
CREATE INDEX IF NOT EXISTS idx_content_user ON content_ideas(user_email);
CREATE INDEX IF NOT EXISTS idx_content_status ON content_ideas(status);
CREATE INDEX IF NOT EXISTS idx_content_jadwal ON content_ideas(jadwal_posting);
```

UNIQUE cuma `id`. Relasi logis (beda DB): `content_ideas.user_email → auth.users.email`. ID `content-001` reuse slot terkecil (kode Node cari gap, bukan AUTOINCREMENT) — diputus bang rob 24-09-2026. Tanggal `dd-mm-yyyy` aja. Cuma link posted wajib.

Contoh 1 baris:
`content-002 | admin | 24-09-2026 | 28-09-2026 | IG | reels | ai | Hook 3 detik | Hook-problem-CTA | Final + hashtag | revision | (kosong)`

### B3 Auth

Numpang `:7002`, jangan ubah — diputus bang rob 24-09-2026.
- Password bcrypt bawaan (`password_hash`), min 8, pesan generik `Email/password salah`.
- OTP 6-digit 5 mnt max 5x → kunci 15 mnt, pesan `Kode salah atau expired`.
- Token opaque 64-hex 1 jam, simpan DB, key per-app (`content.token` beda dari `crm.token`), gampang cabut. Data `:7010` verifikasi `fetch AUTH_URL/api/me` 5s. Rekomendasi opaque bukan JWT.
- Lupa via OTP reset, sukses hanguskan semua session.
- Rate 5/min/IP, CORS allowlist + same-host pass.

### B4 Mail

Langsung Gmail reuse `:7002` biar reverse engineering jalan — diputus bang rob 24-09-2026. `:7010` GAK kirim mail.
Latihan Mailpit dibuang dari backlog (jadi bahan experiment). Beneran: `smtp.gmail.com:587 STARTTLS`, pengirim akun auth existing. App Password BUKAN password asli, di `.env` auth doang, tidak masuk git (demo admin boleh ditulis atas izin bang rob 24-09-2026). Butuh internet pas kirim.

### B5 Port + multi-app

FIX — diputus bang rob 24-09-2026: FE `:7011`, data `:7010`, auth `:7002`. Funnel `:443→:7002`, `:8443→:7010`. Pages `/contentOS/` bake Funnel pas build.
`API_URL` ngikutin hostname (`location.protocol//hostname:PORT`) — localhost + LAN + Funnel tanpa ganti kode.
CORS `:7010` allowlist: `http://localhost:7011`, `https://robbyaliasaakbar.github.io`, URL Funnel + se-mesin pass. Salah 1 baris = CORS misterius.
Token lintas app: 1 auth, key beda per-app, logout lokal (logout ContentOS ga nendang CRM) — limitasi jujur by design.

### B6 Secrets

Frontend `contentOS/.env` (gitignore): `VITE_BACKEND_PORT=7010` WAJIB, `VITE_FRONTEND_PORT=7011` WAJIB, `VITE_AUTH_PORT=7002` WAJIB, `VITE_API_URL` kosong lokal / Funnel di `.env.production`, `VITE_AUTH_URL` sama.
Backend `backend-server-contentOS/.env` (gitignore): `VITE_BACKEND_PORT=7010` WAJIB, `AUTH_API_URL=http://localhost:7002` WAJIB. No password di sini.
Auth `:7002`: tambah origin `:7011` + Pages, App Password tetap di sana.
Baca `getenv()` dulu baru file, DB + config di luar publik, `git status` bersih dari `.env + *.db` — diputus bang rob 24-09-2026.

### B7 Kontrak endpoint

Semua `:7010` WAJIB Bearer. Auth `:7002` referensi (jangan bikin baru): `POST /api/register 201 {ok:true} / 409 Email sudah terdaftar`, `POST /api/verify 200 / 400 Kode salah atau expired`, `POST /api/login 200 {token} / 401 Email/password salah`, `GET /api/me 200 {email,nama,username,role} / 401 Invalid or expired token`.

| Method + Path | Request contoh | Sukses | Gagal | Efek DB |
|---|---|---|---|---|
| GET /health | - | 200 {ok:true} | fail curl | none |
| GET /content?q&tempat&status&tipe&kategori&page&limit | /content?tempat=IG&status=ide&page=1&limit=20 ORDER jadwal ASC | 200 {total,page,limit,totalPages,count,data[]} | 401 Login required / Invalid token, 503 Auth unreachable | read + userScope |
| POST /content/ingest | {tempat:IG,jadwal:28-09-2026,tipe:reels,kategori:ai,description,status:ide} | 200 {action:created\|updated,item} auto content-xxx | 400 Link wajib posted, 400 URL tidak valid, 401/503 | INSERT/UPDATE milik user_email |
| GET /content/export?... | /content/export?tempat=IG (di atas /:id) | 200 content.csv | 401/503 | none full filtered |
| GET /content/:id | /content/content-001 | 200 item | 404 bukan milikmu,401/503 | none |
| DELETE /content/:id | - | 200 {ok:true} | 404/401/503 | DELETE 1 (admin semua), slot jadi kosong |
| GET /dashboard | - | 200 {total,by_status 4,by_tempat 4} scoped | 401/503 | none |

Validasi link: posted kosong → 400 `{error: Link wajib diisi saat status posted}` — diputus bang rob 24-09-2026.

### B8 Curl checklist

Siap copas, semua hijau baru DONE. 1 merah berhenti. Demo admin atas izin bang rob 24-09-2026.

```bash
curl -s http://localhost:7010/health
# {ok:true}
curl -s -X POST http://localhost:7002/api/login -H 'Content-Type: application/json' -d '{"identifier":"admin","password":"Leads7006"}'
# 200 {token} → $TOKEN
curl -s http://localhost:7002/api/me -H "Authorization: Bearer $TOKEN"
# 200 {email,role:admin}
curl -s "http://localhost:7010/content?page=1&limit=20" -H "Authorization: Bearer $TOKEN"
# 200 {total,page,data[]}
curl -s -X POST http://localhost:7010/content/ingest -H "Content-Type: application/json" -H "Authorization: Bearer $TOKEN" -d '{"tempat":"IG","jadwal_posting":"28-09-2026","tipe":"reels","kategori":"ai","description":"Hook 3 detik","status":"ide"}'
# 200 {action:created}
curl -s -X POST http://localhost:7010/content/ingest -H "Content-Type: application/json" -H "Authorization: Bearer $TOKEN" -d '{"tempat":"Reddit","tipe":"text post","status":"posted"}'
# 400 Link wajib
curl -s -X POST http://localhost:7010/content/ingest -H "Content-Type: application/json" -H "Authorization: Bearer $TOKEN" -d '{"tempat":"Reddit","tipe":"text post","status":"posted","link_postingan":"https://reddit.com/r/test"}'
# 200 created
curl -s http://localhost:7010/dashboard -H "Authorization: Bearer $TOKEN"
# 200 {total,by_status,by_tempat}
curl -s "http://localhost:7010/content/export?tempat=IG" -H "Authorization: Bearer $TOKEN" -o /tmp/content.csv && head /tmp/content.csv
# CSV filtered
curl -s http://localhost:7010/content -H "Authorization: Bearer ngawur"
# 401 Invalid
curl -s http://localhost:7010/content
# 401 Login required
# matikan :7002 → 503 Auth unreachable (fail closed)
```
