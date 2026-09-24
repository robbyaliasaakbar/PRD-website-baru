# PRD Awal - Aplikasi Sorting Lamaran Kerja (Job Tracker Lokal)

> **Versi:** v0.5 Sinkron - sesuai hasil build sekarang
> **Tanggal:** 2026-09-09 (sinkron dari v0.4)
> **Pemilik:** Bang Rob (supervisi AI, keputusan di Bang Rob)
> **Dibuat oleh:** Udin (asisten syntax + kasih saran)
> **Workspace isolated:** `/Belajar/project-pertama/` (gak nyentuh project live)
> **Status:** CLONE project-kedua - arena obok-obok. Isi bawah = riwayat project-pertama (port 7000).
> Run clone ini di **port 7001**: `python3 -m http.server 7001` → `http://localhost:7001`. project-pertama dikunci FIX, jangan diutak-atik.

**Riwayat:**
- v0.1 (2026-09-06): draft awal buat dibaca
- v0.2 (2026-09-08): locked sesuai jawaban Bang Rob - ID urut lanjut, field 6 tanpa Note, status final 7, port 7000, fitur ikut rekomendasi Udin
- v0.3 (2026-09-09): tambah field `link` opsional (hasil test UI Bang Rob: biar bisa klik langsung ke postingan lamaran). Field jadi 7: id, company, position, date, status, portal, link.
- v0.4 (2026-09-09): tambah chart konversi (request Bang Rob: line chart persentase interview awal / interview user / offering + animasi).
- v0.5 (2026-09-09): sinkron penuh - warna chart merah/kuning/hijau, legend dots bulet full solid gede, versi mobile kartu HP, header ramping (judul doang + badge 1 baris).

---

## 1. Latar Belakang & Tujuan

**Masalah:**
Bang Rob udah nyebar lamaran ke banyak perusahaan. Kalau dicatat di Google Sheets / Excel, bisa sih, tapi tampilannya kaku, gak ada animasi, gak enak dibuka tiap hari.

**Tujuan:**
Bikin aplikasi web lokal (jalan di browser via localhost, gak perlu internet / server online) buat nyatet & nge-sorting lamaran yang udah dilempar. Rasanya kayak aplikasi modern: clean light theme, ada animasi halus, ada statistik.

**Prinsip belajar (dikunci):**
Reverse engineering ala Bang Rob. Udin bangun full dulu yang simple tapi polanya bener, Bang Rob bedah struktur kodenya setelah project kelar sampe paham luar-dalem. AI cuma bikin syntax + kasih saran, arsitektur & keputusan di Bang Rob. Anti makan mentah-mentah hasil AI.

---

## 2. Siapa User & Cara Jalanin

- **User:** 1 orang doang, Bang Rob sendiri. Tanpa login (login disiapkan modular buat nanti).
- **Cara jalanin (E2E lokal via server, BUKAN double-klik):**
  ```bash
  python3 -m http.server 7000
  ```
  Terus buka `http://localhost:7000` di Chrome. Tanpa install, tanpa n8n, tanpa Postgres, tanpa PHP.
- **Kenapa wajib localhost, bukan klik `index.html`?** Karena klik file itu pake `file://`, browser kadang nge-blok JS modular. Pake `localhost` itu pola web beneran.
- **Konvensi port Bang Rob (dikunci):**
  - `8xxx` = website static (landing, company profile)
  - `7xxx` = webapp yang lebih rumit (job tracker ini). Kita pake `7000`. Kalau kepake, geser ke 7001, 7002.
- **Kenapa tanpa login dulu?** Karena login butuh backend + urus password + session. Itu 3x lebih berat dan bakal nutupin pola utama (CRUD + sorting). Kita bikin pintunya modular dulu, nanti tinggal colok login.

---

## 3. Scope - Wajib vs Opsional

### WAJIB (MVP v0.1 - locked ikut rekomendasi Udin)

1. Tambah lamaran baru
2. Tampil list dalam tabel modern
3. Search (cari perusahaan/posisi), Filter (saring per status), Sort (urut tanggal terbaru / terlama)
4. Edit (terutama ganti status) + Hapus + confirm
5. Statistik kecil di atas (Total, Proses, Interview, Offering)
6. Data kesimpen permanen walau di-refresh (pakai LocalStorage)
7. UI clean light + animasi halus + responsive (enak di HP & laptop)

### OPSIONAL (dikunci, JANGAN dikerjain sekarang)

- Login email + password (disiapkan modular, nanti ganti `store.js` doang)
- Export ke CSV / Excel
- Kanban drag-drop
- Upload PDF CV + scoring otomatis (reuse ilmu Technical Test)
- Deploy ke Hostinger
- Template format PRD khusus (ide Bang Rob: dari PRD ini jadi template buat project berikutnya - dikerjain setelah app jadi)

> Kalau ada ide baru muncul, masukin ke Opsional dulu. Jaga scope biar MVP kelar.

---

## 4. Data Model - Final Locked

Satu lamaran = satu object kayak gini:

```js
{
  id: "lamaran-001",         // urut 001,002,003... lanjut terus, gak dipakai ulang
  company: "PT Sentosa",     // wajib, ketik bebas custom
  position: "AI System Builder", // wajib, ketik bebas custom
  date: "2026-09-08",        // wajib, input kalender (klik-klik doang), tampil jadi 08-09-2026
  status: "baru",            // wajib, pilih dari 7 di bawah, default "baru"
  portal: "Linkedin",        // wajib, ketik bebas (Linkedin / JobStreet / Glints / Referral / dll), BUKAN link, cuma penanda abis ngelamar via apa
  link: "https://linkedin.com/jobs/123", // opsional, link postingan lamaran, kosong boleh. Otomatis ditambah https:// kalau belum ada. Klik Buka ↗ di tabel/kartu.
  createdAt: "2026-09-08T10:00:00.000Z" // otomatis kapan dicatat
}
```

**Aturan ID (dikunci):** counter disimpan terpisah, tiap tambah +1. Contoh ada 001,002,003 → hapus 002 → tambah baru jadi 004 (lanjut, jangan ngisi 002 lagi biar unik, anti tabrakan).

**List status final 7 (kode → label → warna) - dikunci:**

| Kode | Label di UI | Warna | Artinya |
|------|-------------|-------|---------|
| `baru` | Baru Lamar | slate abu | baru taro, belum ada kabar |
| `interview-hr` | Interview HR | sky | interview awal HR |
| `technical-test` | Technical Test | violet | tes teknis |
| `interview-user` | Interview User | orange | interview user / manager |
| `offering` | Offering Letter | emerald | ditawarin gaji / kontrak |
| `diterima` | Diterima | green solid | lolos, beres! |
| `ditolak` | Ditolak | rose | gak lolos / ghosting, beres! |

> Kenapa 7 ini? Tanpa Diterima/Ditolak, data gantung terus di "proses". Dengan 2 status terminal ini, statistik jujur & loop ketutup. `baru` sampe `offering` = proses, `diterima/ditolak` = selesai.
> Kenapa pakai kode + label? Biar gampang di-filter pakai kode, tapi tampil label enak dibaca.

**Note dibuang (dikunci):** sesuai keputusan Bang Rob, gak perlu field Note. Cukup Portal aja biar ramping (prinsip YAGNI).

**Field link (keputusan Bang Rob pas test UI 2026-09-09):** tambahan 1 field opsional `link` biar bisa klik langsung ke postingan lamaran. Data lama tanpa link tetap aman (muncul `-`).

Contoh 3 data dummy buat testing:
1. lamaran-001 | PT Sentosa | AI System Builder | 2026-09-01 | baru | Linkedin | https://linkedin.com/jobs/1
2. lamaran-002 | PT Kopi Nusantara | Web Dev | 2026-09-03 | interview-hr | JobStreet | (kosong)
3. lamaran-003 | PT Data Solusi | Frontend Dev | 2026-09-05 | technical-test | Glints | https://glints.com/job/3

---

## 5. Fitur Wajib - Detail Final (ikut rekomendasi Udin, disetujui Bang Rob)

### F1 - Tambah Lamaran
- Form: Company (text, wajib, custom), Position (text, wajib, custom), Date (date picker kalender, default hari ini, wajib), Status (dropdown 7 di atas, default `baru`), Portal (text, wajib, ketik bebas), Link (url, opsional, placeholder https://..., otomatis tambah https:// kalau belum ada).
- Tombol: `+ Tambah Lamaran`.
- Kalau Company / Position / Portal kosong → tolak + pesan merah "Perusahaan, posisi & portal wajib diisi".
- Kalau sukses → form ke-reset (date balik ke hari ini, status ke baru), muncul di tabel/kartu + toast hijau "Lamaran kesimpen".

### F2 - Tampil Tabel (desktop) + Kartu (mobile)
- Desktop (lg ke atas): tabel. Kolom: ID | Perusahaan | Posisi | Tanggal | Status (badge) | Portal | Link (Buka ↗ / -) | Aksi (Edit / Hapus).
- Mobile (di bawah lg): tabel disembunyiin, ganti kartu 1 lamaran = 1 kartu. Isi kartu: Perusahaan gede + Posisi + badge status + `ID • tanggal • portal` + tombol `Buka Postingan ↗` full lebar (kalau ada link) + `Edit | Hapus` gede sejajar biar jempol friendly.
- Kalau data kosong → empty state: "Belum ada lamaran. Tambah pertama yuk 👆".
- Tanggal tampil format `dd-mm-yyyy` contoh `08-09-2026`.

### F3 - Search + Filter + Sort (otak sorting-nya)
- Search: ketik "sentosa" → cuma muncul PT Sentosa. Cari di company + position + portal. Real-time.
- Filter status: dropdown / chip "Semua + 7 status". Pilih 1 → tabel langsung kesaring.
- Sort tanggal: tombol "Terbaru ↓ / Terlama ↑". Default Terbaru.
- Kombinasi jalan bareng: search + filter + sort nempel semua.

### F4 - Edit
- Klik Edit → form keisi data lama → ganti misal status `baru` → `interview-hr` → Simpan → badge berubah warna + toast.

### F5 - Hapus
- Klik Hapus → confirm "Yakin hapus lamaran-001 PT Sentosa?" → Ya → hilang + toast. ID yang dihapus gak dipakai ulang.

### F6 - Statistik Atas
- 4 kartu: Total Lamaran (semua) | Aktif Diproses (total - diterima - ditolak) | Tahap Interview (interview-hr + technical-test + interview-user) | Offering.
- Update otomatis tiap tambah / edit / hapus.

### F7 - Simpen Permanen (LocalStorage)
- Key: `jobTracker.lamaran.v1` + key counter `jobTracker.counter.v1`.
- Refresh / tutup browser / matiin laptop → data tetap ada.
- Clear cache → hilang (batas LocalStorage, jujur. Nanti upgrade ke DB beneran).

### F8 - Chart Konversi (request Bang Rob, locked v0.4-v0.5)
- 3 kartu persentase + progress bar animasi 0.8s: Sampai Interview Awal (merah) | Sampai Interview User (kuning) | Sampai Offering (hijau). Format `X%` + `Y dari Z lamaran`.
- 1 line chart (Chart.js) 3 garis tren kumulatif: X = Ke-1, Ke-2... urut tanggal lama→baru, Y = 0-100%. Titik Ke-N = % dari N lamaran pertama. Animasi 800ms easeOutQuart tiap data berubah.
- Warna locked: Interview Awal merah `#dc2626`, Interview User kuning `#eab308`, Offering hijau `#059669`. Legend dots bulet full solid gede (usePointStyle circle, box 12, font 13), bukan kotak.
- Cara hitung funnel (jujur, ditulis di UI): Awal = interview-hr + technical-test + interview-user + offering + diterima. User = interview-user + offering + diterima. Offering = offering + diterima. Ditolak tidak dihitung karena riwayat tahapnya tidak ketahuan.
- Kalau data kosong → chart disembunyiin + pesan "Tambah minimal 1 lamaran biar garisnya muncul 📈". Kalau Chart.js gagal load (offline) → persentase tetap jalan + pesan fallback.

---

## 6. Alur E2E - Final (versi localhost)

```
1. Bang Rob nyalain server: python3 -m http.server 7000 → buka http://localhost:7000
   ↓
2. Lihat statistik dulu: "Oh udah 12 lamaran, 3 interview"
   ↓
3. Abis ngirim lamaran baru → isi form (PT Sentosa - AI System Builder - hari ini - baru - Linkedin) → klik Tambah
   ↓
4. store.js simpen ke LocalStorage → app.js render ulang tabel + statistik
   ↓
5. Cari: ketik "sentosa" → muncul 1 baris
   ↓
6. Seminggu kemudian dipanggil → Edit → ganti jadi "interview-hr"
   ↓
7. Filter: pilih Technical Test → kelihatan yang lagi tes aja
   ↓
8. Refresh browser → data tetap ada (bukti persist jalan)
```

> Pola inti buat dipelajari nanti pas bedah code: `Form → store.js (tulis) → LocalStorage → store.js (baca) → app.js (tampil)`. Penjelasan detail A (store modular), B (sorting), C (localhost+port) diserahin ke Udin, dipelajari Bang Rob via baca code setelah app jadi.

---

## 7. Arsitektur Modular - Biar Gampang Upgrade Login Nanti

```
project-pertama/
├── PRD/
│   └── prd-awal.md      ← file ini (v0.5 sinkron)
├── Checkpoint-Belajar/
│   └── 00-tujuan-dan-checkpoint.md ← jejak belajar
├── index.html           ← struktur + layout + Tailwind CDN + Chart.js CDN
├── css/
│   └── style.css        ← animasi fade, scrollbar, badge, jt-bar 0.8s, anti-zoom mobile
└── js/
    ├── store.js         ← OTAK DATABASE (satu-satunya yang pegang data + normalizeLink)
    └── app.js           ← OTAK TAMPILAN (render tabel+kartu, search, filter, sort, funnel, chart)
```

**Aturan modular:**
- `app.js` DILARANG langsung pegang `localStorage`. Semua lewat `store.js`.
- `store.js` kontrak 5 fungsi:

```js
getAll()           // baca semua → array
add(data)          // tambah 1 → return object baru dengan id lamaran-xxx
update(id, dataBaru) // edit 1 berdasar id
remove(id)         // hapus 1 berdasar id
clear()            // hapus semua (testing)
```

Nanti upgrade login (Supabase / n8n + Postgres): ganti isi 5 fungsi itu jadi `fetch(...)`. `app.js` + `index.html` gak diubah. Colokan diganti, lampu tetap sama.

---

## 8. UI/UX - Clean Light + Attractive (sinkron hasil sekarang)

- Light only. BG `slate-50`, kartu putih, teks `slate-900`.
- Header ramping (keputusan Bang Rob): judul `Job Tracker` doang tanpa subteks + badge oval `Lokal • Tanpa Login • :7000` dikunci 1 baris di HP (whitespace-nowrap + shrink-0 + text 11px mobile).
- Layout: Header → Statistik 4 kartu → Chart konversi (3 kartu % + line chart) → Filter bar (search + filter + sort, stack di HP) → Grid 2 kolom desktop (kiri form, kanan tabel/kartu), HP stack vertikal → Footer v0.2 + link.
- Mobile: tabel hidden di HP, ganti kartu (Buka Postingan full lebar + Edit/Hapus gede). Input 16px anti-zoom iOS.
- Animasi: fade+slide up baris/kartu baru 0.3s, badge transisi warna, toast 3 detik, tombol active scale 0.97, progress bar 0.8s, line chart 800ms.
- Font Inter + Poppins via Google Fonts.

---

## 9. Tech Stack

- HTML5 semantic, Tailwind via CDN, Chart.js 4.4.1 via CDN (line chart + legend dots), Vanilla JS only, LocalStorage, tanpa login/backend/n8n/Postgres untuk v0.5
- Run: `python3 -m http.server 7000` → `http://localhost:7000` (butuh internet sekali buat CDN Tailwind + Chart.js + Fonts)

---

## 10. Verifikasi - 12 Ceklis (sinkron sekarang)

| Cek | Cara | Harapan |
|-----|------|---------|
| Tambah | Isi PT Tes - QA - hari ini - baru - Linkedin + link → Tambah | Muncul lamaran-xxx + Total +1, kolom Link Buka ↗ |
| Link | Klik Buka ↗ (desktop) / Buka Postingan (kartu HP) | Kebuka postingan di tab baru. Kosong → tampil `-` |
| Persist | Refresh | Tetap ada |
| Search | Ketik "tes" | Cuma yang cocok |
| Filter | Pilih Technical Test | Cuma status itu |
| Sort | Klik Terlama | Paling lama di atas |
| Edit | Edit jadi Offering + ganti link | Badge emerald + link update |
| Hapus | Hapus | Hilang, ID gak dipakai ulang |
| Server+Animasi | Buka via localhost:7000, tambah data | Jalan + fade halus |
| Mobile | Kecilin ke HP | Tabel hilang ganti kartu, tombol gede, badge 1 baris |
| Chart % | Tambah 1 baru + 1 interview-hr + 1 offering | 3 kartu % + bar gerak 0.8s |
| Chart line | Lihat garis merah/kuning/hijau + legend dots bulet gede | 3 garis animasi 800ms, legend dots bukan kotak |

12 lolos → DONE. 1 gagal → berhenti, benerin dulu.

---

## 11. Rencana Building (Udin yang kerjain, Bang Rob bedah setelah jadi)

- Step 1: Kerangka + Tambah + Tampil
- Step 2: Search + Filter + Sort + Statistik
- Step 3: Edit + Hapus + Poles animasi + verifikasi 8 cek
- Step 4 (nanti): Bang Rob tracing struktur code baris per baris (A,B,C dipelajari di sini) + bikin template PRD khusus dari PRD ini

---

## 12. Keputusan Locked (update 2026-09-09)

1. ✅ ID urutan lanjut terus (004 setelah hapus 002, gak dipakai ulang)
2. ✅ Status final 7 (tambah Diterima/Ditolak biar beres)
3. ✅ Field 7: id, company, position, date, status, portal + link opsional (Note dibuang). Portal ketik bebas, link otomatis https://.
4. ✅ Port 7000 (7xxx webapp, 8xxx static). Fitur ikut rekomendasi Udin. A,B,C diserahin ke Udin, dipelajari via bedah code setelah app jadi.
5. ✅ Mobile kartu HP (tabel hidden, kartu + tombol gede + anti-zoom). Header ramping judul doang + badge oval 1 baris.
6. ✅ Chart: 3 kartu % + line chart 3 garis tren kumulatif, warna merah/kuning/hijau, legend dots bulet full solid gede, animasi bar 0.8s + garis 800ms. Cara hitung funnel ditulis jujur di UI.

*PRD v0.5 sinkron dengan code. Next: bedah code + template PRD khusus nunggu komando Bang Rob.*
