# Ringkasan Sistem - POC Screening CV Otomatis

**Untuk dijelaskan ke orang awam (HR, Manajer, Tim Rekrutmen)**

---

## 1. Sistem Ini Ngapain Sih?

Perusahaan lagi banyak buka lowongan. HR biasanya buka 5 CV satu-satu secara manual — lama, capek, dan bisa bias.

**Sistem ini bikin jadi otomatis:**
- HR **upload 1–5 CV (PDF)** sekaligus di website
- Sistem **otomatis baca, nilai, dan urutin** kandidat terbaik
- HR **tinggal download hasil ranking** (Excel/CSV) — nggak perlu buka PDF satu-satu

**Coba langsung, nggak perlu login, nggak ribet. Bisa di HP juga.**

---

## 2. Alur Sederhana (Cerita, Bukan Teknis)

```
HR buka website (HP/Laptop)
    ↓
Pilih 1–5 file CV PDF (bisa langsung 5 atau nyicil 1-1)
    ↓
Klik "Kirim ke AI Screening"
    ↓
Sistem baca CV di browser (jadi text) → kirim ke n8n
    ↓
n8n bagi CV per posisi:
  - Junior Architect (butuh S1 Arsitektur, 2 tahun, AutoCAD/Revit/Sketchup)
  - Drafter & 3D Visualization (jago gambar teknik, AutoCAD/SketchUp/Enscape)
  - Interior Designer (jago interior, furniture, rendering)
    ↓
Tiap CV dinilai 1 per 1 (biar nggak berat):
  - Keahlian (max 10 skill) → 50%
  - Pendidikan (max 4) → 20%
  - Pengalaman (max 6 tahun) → 30%
  Total 100, yang ≥65% = Lolos
    ↓
Gabung & urutin ranking (Rank 1 skor tertinggi)
    ↓
Balik ke website → HR lihat tabel ranking + download JSON/CSV
    ↓
Database otomatis di-bersihin (fresh) buat upload berikutnya
```

**HR cuma lihat 3 langkah: Upload → Tunggu 5 detik → Download Ranking.**

---

## 3. Poin Penting Buat Dijelasin ke Orang Awam

### A. Gampang Dipakai
- **Upload 1–5 PDF** — bisa drag & drop di laptop, tap di HP
- **Bisa hapus 1 per 1** kalau salah upload
- **Nggak perlu login** — buka `http://localhost:8010`, langsung pakai
- **Mobile friendly** — di HP tombol Kirim nempel di bawah, enak di-tap jempol

### B. Cepat & Efektif
- **Ekstrak di browser** — nggak perlu upload ke server dulu, langsung jadi text
- **Nilai 1 per 1** — biar laptop biasa nggak ngelag (model kecil Qwen 1.5B nggak kepakai, pakai rule aja)
- **5 CV selesai ~5 detik** — HR nggak perlu buka 5 PDF manual 30 menit

### C. Objektif & Transparan (Nggak Bias)
- **Nilai pakai rumus jelas 50-20-30**, bukan "feeling AI"
  - Misal: CV punya 7 skill (35 poin) + S1 (5 poin) + 7 tahun (30 poin) = 70 → Lolos
- **Alasan jelas di CSV:** `Lolos 70% utk Junior Architect` — HR tau kenapa Lolos/Tidak
- **Per posisi beda skill:** Architect dinilai AutoCAD/Revit, Drafter dinilai Enscape/D5, Interior dinilai Photoshop — jadi adil

### D. Hasil Jelas & Mudah Dibaca
- **Tabel ranking di website:** Rank, Nama File, Posisi, Skor, Status (Lolos hijau / Tidak abu)
- **Download 2 format:**
  - **JSON** — buat IT
  - **CSV** — buat HR, langsung buka di Excel, header Indonesia: `Peringkat, ID CV, Nama File, Posisi Dilamar, Skor (0-100), Status, Alasan`
- **Database selalu fresh** — tiap habis download, DB di-TRUNCATE otomatis. HR nggak perlu bersih-bersih manual.

### E. Aman & Simple
- **Nggak simpen file PDF di server** — cuma di memory browser
- **Nggak ada biaya token** — nggak pakai OpenAI, cuma rule di n8n + Postgres lokal
- **Bisa jalan di laptop biasa** — nggak butuh GPU gede

---

## 4. Contoh Hasil (Biar Kebayang)

Upload 5 CV Sample, hasil ranking:

| Rank | CV | Posisi | Skor | Status |
|------|----|--------|------|--------|
| 1 | CV Sample 3.pdf (Mohamad, 10 tahun) | Drafter & 3D Visualization | 80 | Lolos |
| 2 | CV Sample 1.pdf (Bayu, 7 tahun) | Junior Architect | 75 | Lolos |
| 3 | CV Sample 4.pdf (Indri, S2) | Junior Architect | 50 | Tidak Lolos |
| 4 | CV Sample 2.pdf (Permana, ITB) | Junior Architect | 35 | Tidak Lolos |
| 5 | CV Sample 5.pdf (Dani, SMK) | Junior Architect | 25 | Tidak Lolos |

HR tinggal **download CSV**, buka di Excel, langsung tau **2 Lolos siap interview**.

---

## 5. Yang Perlu Dijelain Kalau Ditanya

**Q: Kalau CV 1 Lolos, kenapa CV 2 Tidak?**
> A: Karena CV 1 punya 7 skill + 7 tahun (70), CV 2 cuma 5 skill + 3 tahun (35). Rumus 50-20-30 nya transparan.

**Q: Kenapa ada 3 posisi?**
> A: Biar adil. Junior Architect dinilai skill Architect, Drafter dinilai skill Drafter — nggak disamain.

**Q: Data HR aman nggak?**
> A: Aman. PDF cuma dibaca di browser, nggak di-upload permanen. DB cuma simpen ranking, dan tiap selesai langsung dihapus (fresh).

**Q: Bisa di HP?**
> A: Bisa. Upload tap di HP, tombol Kirim nempel di bawah, tabel ranking bisa scroll horizontal.

**Q: Perlu training?**
> A: Nggak. Buka website → Upload → Kirim → Download. 3 klik.

---

## 6. Tech Stack (Kalau Ditanya IT, Jawab Singkat)

- **Landing Page:** HTML5 + Tailwind CDN + Vanilla JS + pdf.js (ekstrak di browser) — 1 file `index.html`, mobile first
- **Workflow:** n8n (webhook, 3 jalur A/B/C, scoring 50-20-30, merge, ranking, respond)
- **Database:** Postgres `database_recruitment.cv_ranking` (truncate tiap selesai)
- **Tanpa AI berat:** Rule-based, predictable, nggak perlu token

---

*Dibuat oleh Udin & Bang Rob — POC 3 Hari — 2026-09-02*
*File ini buat bantu Bang Rob jelasin ke orang awam biar komunikasi lancar.*
