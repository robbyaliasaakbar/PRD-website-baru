# Plan - AI Specialist Technical Test POC | CV Screening Automation

**Project:** Automasi Seleksi CV - Perusahaan Manufaktur  
**Mode:** Planning  
**Tanggal:** 1 September 2026  
**Workspace:** `/Project-Technical-Test`  
**Workflow n8n:** `Technical Test` (ID: v1lQYQUZndoeoR6w) - Webhook `POST /upload-cv`  
**Stack Wajib:** HTML5, Tailwind CSS CDN, Vanilla JS, n8n, PostgreSQL (tahap 2)

---

## 1. Tujuan & Ruang Lingkup POC (3 Hari)

### Latar Belakang
Perusahaan manufaktur growth cepat, hiring massal, screening CV masih manual oleh HR -> lambat, boros, rawan bias. Butuh POC otomatisasi.

### Fokus POC Saat Ini (Disepakati Bang Rob)
**Fase 1 - Landing Page (Fokus Utama):** Mampu upload 1-5 file PDF CV, ekstrak text di browser, bungkus jadi JSON terstruktur, langsung POST ke webhook n8n `Technical Test`. Tanpa preview JSON yang membingungkan user non-teknis.

**Fase 2 - Workflow n8n Screening AI (Bukan Fokus Utama, Garis Besar Saja):** Webhook terima JSON array CV -> loop per CV -> AI extract (pengalaman, pendidikan, skill) -> matching vs kriteria vacancy -> scoring & ranking -> output kandidat terurut + alasan.

> Keputusan: Landing page dibikin **simple tapi maksimal**, tidak rumit, mobile-first karena user kerja tidak selalu di PC.

---

## 2. Arsitektur Sistem (End-to-End)

```
[ User (Mobile/Desktop) ]
        |
        v
[ Landing Page - Single Page App ]
 - Drag & Drop / Tap to Browse (PDF only)
 - Antrian max 5 file (bisa 1-1 atau sekaligus)
 - Validasi: PDF, max 5MB/file, max 5 file
 - Ekstraksi: pdf.js (CDN) di browser -> text per halaman digabung
 - Bungkus JSON Array [{id, filename, size_kb, text, page_count, uploaded_at}]
 - Loading: "Mengekstrak 2/5..." + progress
 - POST JSON ke Webhook n8n (Technical Test)
 - Fallback: Download JSON jika webhook gagal/offline
        |
        | POST application/json
        v
[ n8n Workflow: Technical Test ]
 - Webhook Trigger: POST /upload-cv
 - Function: Loop array CV
 - (Fase 2) AI Node: Ekstrak info + Matching + Scoring
 - (Fase 2) Postgres: Simpan hasil screening (optional)
 - Respond: { success, received_count, message }
        |
        v
[ Output Terstruktur (Fase 2) ]
 - Daftar kandidat terurut + score + alasan lolos/tidak
```

**Kenapa Ekstraksi di Landing Page?**
- Landing page ringan, n8n tinggal terima JSON bersih (tidak perlu handle binary PDF)
- Demo POC lebih cepat & bisa jalan walau n8n belum ada AI
- Mengurangi beban n8n, skalabel untuk 5 file sekaligus

---

## 3. Spesifikasi Landing Page

### 3.1 Tech Stack
- **HTML5** semantic
- **Tailwind CSS via CDN** (https://cdn.tailwindcss.com) - tidak install
- **Vanilla JavaScript** (ES6+, no framework)
- **pdf.js** via CDN (https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js + worker)
- **Icons:** Heroicons / SVG inline (tanpa library berat)

### 3.2 Struktur File
```
/ (root)
├── index.html          # Landing page utama (single page)
├── docs/
│   └── plan.md         # File ini
└── Bahan/              # Dataset CV & Vacancy (existing)
```

### 3.3 Fitur Wajib (Maksimal tapi Simple)

| Fitur | Detail | Prioritas |
|-------|--------|-----------|
| **Upload Zone** | Drag & Drop (desktop) + Tap to Browse (mobile). Area besar, dashed border, icon upload. | P1 |
| **Multiple File Max 5** | Bisa pilih 5 sekaligus atau nambah 1-1. State array `selectedFiles`. Jika >=5, disable input + toast "Maksimal 5 CV". Bisa hapus per file (icon X). | P1 |
| **Validasi** | - Hanya `.pdf` (cek `file.type` & ekstensi) <br> - Max 5MB/file (configurable) <br> - Max 5 file total <br> - Toast error jika melanggar | P1 |
| **Pembeda CV1-CV5** | Tiap file diberi `id: cv_1 ... cv_5` (berdasar urutan upload) + tampil badge di list. Di JSON juga ada `filename` asli untuk pembeda human-readable. Urutan bisa di-reorder? V1: tidak, append only. | P1 |
| **Ekstraksi Client-Side** | Pakai pdf.js. Loop tiap file -> `getDocument -> getPage -> getTextContent`. Gabung text per halaman dengan `\n`. Tampilkan progress "Mengekstrak 3/5...". | P1 |
| **Output JSON** | Struktur Array of Objects (lihat 3.4). Tidak ada preview JSON di UI (sesuai request). Hanya status. | P1 |
| **Kirim ke n8n** | Tombol sticky `Kirim ke AI Screening (X CV)` -> POST fetch ke `WEBHOOK_URL`. Loading spinner saat kirim. | P1 |
| **Fallback Download** | Jika fetch gagal / n8n offline, muncul tombol `Download JSON` + toast error. JSON tetap bisa didemokan. | P1 |
| **Responsive Mobile** | Mobile-first. Breakpoint: sm (640), md (768), lg (1024). Upload zone full width di mobile, tombol sticky bottom. | P1 |
| **Feedback UI** | Toast sukses/gagal (auto dismiss 3s), progress bar ekstraksi, disabled state, empty state. | P2 |
| **Aksesibilitas** | Semantic HTML, alt text, label, aria-live untuk toast, keyboard navigable. | P2 |

### 3.4 Struktur JSON Output (Disepakati)

**POST Body** ke webhook adalah **array langsung** (bukan dibungkus object lagi, biar n8n gampang split):

```json
[
  {
    "id": "cv_1",
    "filename": "CV Sample 1.pdf",
    "size_kb": 320,
    "page_count": 2,
    "text": "BUDI SETIAWAN\nPengalaman Kerja: ...\nPendidikan: ...\nSkills: ...",
    "uploaded_at": "2026-09-01T21:00:00.000Z"
  },
  {
    "id": "cv_2",
    "filename": "CV Sample 2.pdf",
    "size_kb": 210,
    "page_count": 1,
    "text": "...",
    "uploaded_at": "2026-09-01T21:00:01.000Z"
  }
]
```

- `id`: `cv_1` sampai `cv_5` sesuai urutan upload (unique, untuk n8n loop)
- `filename`: nama file asli (pembeda human)
- `size_kb`: Math.round(file.size/1024)
- `page_count`: dari pdf.js `pdf.numPages`
- `text`: string full hasil ekstrak (trim, preserve newline)
- `uploaded_at`: `new Date().toISOString()`

**Contoh curl untuk n8n test:**
```bash
curl -X POST https://n8n.obi.test/webhook/upload-cv \
 -H "Content-Type: application/json" \
 -d @payload.json
```

### 3.5 Konfigurasi Webhook
- **Workflow:** `Technical Test` (ID: v1lQYQUZndoeoR6w)
- **Path:** `upload-cv` (akan dibuat saat building n8n)
- **Method:** `POST`
- **Response:** `{ "success": true, "received_count": 5, "message": "CV diterima, screening dimulai" }`
- **URL Final:** Akan diisi di `index.html` sebagai const `WEBHOOK_URL` (placeholder dulu, nanti update setelah workflow dibuat). Untuk POC lokal: `http://localhost:5678/webhook/upload-cv`

### 3.6 SEO & Meta
- `<title>CV Screening POC - AI Automation | Upload & Screening</title>`
- `<meta name="description" content="POC otomatisasi seleksi CV: upload 1-5 PDF, ekstrak otomatis jadi JSON dan kirim ke AI screening. Cepat, objektif, tanpa bias.">`
- Heading hierarchy: H1 hero, H2 upload, H3 list

---

## 4. Desain UI/UX - OPSI A Clean Corporate (Disepakati)

### 4.1 Prinsip
- **Trustable & Professional:** Audience HR & manufaktur -> butuh kesan objektif, bersih, tidak playful.
- **Minimal:** Banyak white space, card shadow soft, radius 12-16px.

### 4.2 Palet Warna
- **Primary:** Navy `#1e3a5f` / `#0f172a` (header, tombol utama)
- **Background:** White `#ffffff` + Slate-50 `#f8fafc` (section)
- **Accent:** Blue-600 `#2563eb` (icon, progress)
- **Border:** Slate-200 `#e2e8f0`
- **Text:** Slate-900 `#0f172a` (heading), Slate-600 `#475569` (body)
- **Success:** Emerald-600, **Error:** Rose-600

### 4.3 Tipografi
- Font: `Inter` via Google Fonts CDN (fallback system sans)
- H1: 30-36px bold, H2: 20-24px semibold, Body: 14-16px regular
- Line-height 1.6 untuk readability

### 4.4 Layout Wireframe (Mobile-First)

**Desktop (lg):**
```
[ Header: Logo | Nav: Tentang | Badge POC ]
[ Hero: Kiri teks + Kanan ilustrasi placeholder (2 kolom) ]
[ Card Upload: Dashed zone besar + tombol Browse + info "Max 5 PDF, 5MB/file" ]
[ List File: Grid card 2 kolom, tiap card: icon PDF | filename | badge cv_1 | size | X ]
[ Sticky Action Bar: "X CV siap" | Tombol Kirim ke AI Screening (primary) ]
[ Footer: copyright + tech stack ]
```

**Mobile (default):**
```
[ Header compact ]
[ Hero stacked, center aligned, CTA scroll to upload ]
[ Card Upload full width, tinggi 180px, tap area besar ]
[ List File stacked vertical, card full width ]
[ Tombol Kirim sticky di bottom viewport (fixed) biar gampang tap jempol ]
```

### 4.5 Komponen UI
- **Upload Zone:** `border-2 border-dashed border-slate-300 rounded-2xl bg-slate-50 hover:bg-white hover:border-blue-500 transition`
- **File Card:** `bg-white border border-slate-200 rounded-xl p-3 flex items-center gap-3 shadow-sm`
- **Badge:** `bg-blue-50 text-blue-700 text-xs px-2 py-1 rounded-full font-medium`
- **Primary Button:** `bg-slate-900 text-white hover:bg-slate-800 rounded-xl px-6 py-3 font-medium shadow`
- **Toast:** Fixed top-right (desktop) / top-center (mobile), auto dismiss.

---

## 5. Workflow n8n - Garis Besar (Fase 2, Bukan Fokus Utama)

> Landing page sudah siap POST JSON, workflow ini hanya outline agar POC end-to-end.

**Nodes:**
1. **Webhook** `POST /upload-cv` (Technical Test)
2. **Split In Batches / Loop** (loop array CV)
3. **Code Node** (clean text)
4. **AI Agent / OpenAI** (extract: pengalaman, pendidikan, skill, tahun kerja)
5. **Postgres** (optional simpan ke `screening_results`)
6. **Code Node** (scoring vs kriteria vacancy)
7. **Sort Node** (ranking desc by score)
8. **Respond to Webhook** (kembalikan ranking + alasan)

**Kriteria Vacancy:** Akan dibaca dari `Bahan/Vacancy.pdf` (perlu di-parse manual untuk POC).

**Output Akhir (Contoh):**
```json
{
  "ranking": [
    { "id": "cv_3", "filename": "CV Sample 3.pdf", "score": 92, "status": "Lolos", "alasan": "Pengalaman 5th relevan, skill Python & SQL sesuai" },
    { "id": "cv_1", "score": 78, "status": "Cadangan", "alasan": "Skill sesuai tapi pengalaman kurang 1th" }
  ]
}
```

---

## 6. Rencana Eksekusi (Step-by-Step Building)

### Tahap 1: Landing Page (Sekarang)
1. Generate `index.html` semantic + Tailwind CDN + Inter font
2. Implement Upload Zone: drag, drop, browse, multiple, max 5, validasi PDF & size
3. Implement state `selectedFiles` array + render list + badge cv_1..5 + hapus
4. Integrasi pdf.js CDN + fungsi `extractTextFromPDF(file)` -> return {text, page_count}
5. Fungsi `buildPayload()` -> array JSON sesuai spec
6. Fungsi `sendToN8n(payload)` -> fetch POST ke WEBHOOK_URL + loading + toast + fallback download
7. Styling OPSI A + responsive mobile + SEO meta + a11y
8. Test di mobile viewport + test upload 5 CV sample

### Tahap 2: Workflow n8n (Setelah Landing Page OK)
1. Update workflow `Technical Test`: tambah Webhook node `upload-cv`
2. Test POST dari landing page (cek execution)
3. (Jika diminta) Tambah AI screening nodes (outline saja)

---

## 7. Pertimbangan Robust & Etika (Sesuai Kriteria Penilaian)

- **Robust:** Validasi file di client, try/catch ekstrak pdf.js per file (jika 1 file corrupt, yang lain tetap diproses), timeout fetch 15s, fallback download JSON.
- **Skalabilitas:** Max 5 file di POC, tapi arsitektur array JSON mudah dinaikkan ke 20-50 file dengan batch.
- **Bias Mitigasi (Untuk Fase 2):** Scoring hanya berdasar skill/pengalaman terukur, bukan nama/gender/foto. Akan dijelaskan di presentasi.
- **Keamanan:** Tidak simpan file di server landing page, hanya di memori browser. Tidak kirim data sensitif ke log.

---

## 8. Konfirmasi Sebelum Building

- [✅] Setuju struktur JSON array langsung?
- [✅] Setuju max 5MB/file & max 5 file?
- [✅] Setuju WEBHOOK_URL placeholder `http://localhost:5678/webhook/upload-cv` dulu, nanti update setelah workflow dibuat?
- [✅] Setuju fallback download JSON?
- [✅] Lokasi file: /home/obi/Documents/dir_openwebui/Project-Technical-Test/lamding-page/index.html (udah gue buat foldernya)

**Jika Bang Rob setuju, Udin langsung gas MODE BUILDING: generate `index.html` sesuai spec di atas. Satu halaman per waktu, setelah jadi Udin minta konfirmasi lagi sebelum lanjut ke workflow n8n.**

---

*Disusun oleh Udin - Sahabat Coding Bang Rob*
