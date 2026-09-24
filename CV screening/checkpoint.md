# Checkpoint - Progress POC CV Screening

**Project:** AI Specialist Technical Test  
**Start:** 1 September 2026  
**Workspace:** `/Project-Technical-Test`  
**Tim:** Bang Rob + Udin

---

## Ringkasan Checkpoint

| # | Area | Task | Status | Tanggal | Catatan |
|---|------|------|--------|---------|---------|
| 1 | **Landing Page** | Planning `docs/plan.md` | ✅ Done | 2026-09-01 | Diskusi multiple 5 file, JSON, Opsi A |
| 2 | **Landing Page** | Build `lamding-page/index.html` | ✅ Done | 2026-09-01 | HTML5+Tailwind+pdf.js, mobile-first, fallback download |
| 3 | **Landing Page** | Test Manual (Bang Rob) | ✅ Done | 2026-09-02 | 1-5 file robust, webhook-test OK |
| 4 | **n8n Workflow** | Planning `docs/n8n-workflow-plan.md` + `database-setup` | ✅ Done | 2026-09-02 | Qwen local, rule 50-20-30, DB schema |
| 5 | **n8n Workflow** | **CP1: Webhook POST /upload-cv** | ✅ Done | 2026-09-02 | webhook-test `upload-cv`, onReceived+* |
| 6 | **n8n Workflow** | **CP2: Code Extractor** (pecah 5 CV) | ✅ Done | 2026-09-02 | Pecah array → id_cv/nama_file/isi_cv |
| 7 | **n8n Workflow** | **CP3: Code Keyword Router** | ✅ Done | 2026-09-02 | Keyword A/B/C → Posisi, 4A+1B |
| 8 | **n8n Workflow** | **CP4: Edit Fields Rapihin** | ✅ Done | 2026-09-02 | 10 field rapi |
| 9 | **n8n Workflow** | **CP5: Switch Posisi** | ✅ Done | 2026-09-02 | 3 output A/B/C + fallback |
| 10 | **n8n Workflow** | **CP5b: Code Pemecah (A/B/C)** | ✅ Done | 2026-09-02 | 1 CV per 1, clean, 3 jalur |
| 11 | **n8n Workflow** | **CP5c: Code Normalisasi (A/B/C)** | ✅ Done | 2026-09-02 | 4 field + mapping Junior Architect/Drafter/Interior |
| 12 | **n8n Workflow** | **CP5d: Edit Fields (A/B/C)** | ✅ Done | 2026-09-02 | id_cv, nama_file, isi_cv, posisi |
| 13 | **n8n Workflow** | **CP6: Code Hitung (A/B/C)** | ✅ Done | 2026-09-02 | 50-20-30, threshold 65%, 6 field |
| 14 | **n8n Workflow** | **CP6b: Ekstrak Data Penting (A/B/C)** | ✅ Done | 2026-09-02 | 4 field ranking prep |
| 15 | **n8n Workflow** | **CP7: Merge Gabungan** | ✅ Done | 2026-09-02 | Append 3 input (4+1+0) → 5 item |
| 16 | **n8n Workflow** | **CP8: Code Ranking** | ✅ Done | 2026-09-02 | 5 item ranking desc, 2 Lolos 3 Tidak |
| 17 | **n8n Workflow** | **CP9: Postgres `database` (hasil_screening)** | ✅ Done | 2026-09-02 | Upsert 5 row, rank 1-5 |
| 18 | **n8n Workflow** | **CP10: Respond Ranking** | ✅ Done | 2026-09-02 | Webhook responseNode + allIncomingItems, 5 ranking balik |
| 19 | **Integrasi** | Test End-to-End (Landing → n8n → DB) | ✅ Done | 2026-09-02 | Demo 5 CV upload OK (screencast 24s) |
| 20 | **Finishing** | Polish Landing Page | 🔄 In Progress | 2026-09-02 | Fungsional done, butuh polish biar enak dilihat |

---

## Detail Status Landing Page

- [x] Plan dibuat & di-confirm Bang Rob
- [x] Folder `lamding-page/` dibuat
- [x] `index.html` generate, responsive, validasi, fallback
- [x] Functional test: 1-5 file, webhook 5 ranking OK
- [ ] Finishing polish → **NEXT** (biar enak dilihat, demo baru building)

## Detail Status n8n Workflow

- [x] Plan `n8n-workflow-plan.md` + `database-setup.md/.sql`
- [x] CP1 Webhook → Done
- [x] CP2 Extractor → Done (5 CV)
- [x] CP3 Router → Done (4A+1B)
- [x] CP4 Edit Fields Rapihin → Done
- [x] CP5 Switch → Done (3 jalur)
- [x] CP5b Pemecah A/B/C → Done (1 per 1)
- [x] CP5c Normalisasi A/B/C → Done (mapping jabatan)
- [x] CP5d Edit Fields A/B/C → Done
- [x] CP6 Hitung A/B/C → Done (70,80,50,35,25)
- [x] CP6b Ekstrak A/B/C → Done
- [x] CP7 Merge → Done (5 item)
- [x] CP8 Ranking → Done (rank 1-5)
- [x] CP9 Postgres → Done (5 row cv_ranking)
- [x] CP10 Respond → Done (allIncomingItems, 5 ranking JSON)
- [x] Integrasi E2E → Done (demo 5 CV 24s, 1.8M)
- [ ] Finishing Polish → **NEXT** (biar enak dilihat)

**Aturan Main:** 1 node per 1 checkpoint, test, confirm Bang Rob, baru next.

---

## Cara Pakai File Ini

- Setiap selesai 1 checkpoint, Udin update `Status` jadi ✅ Done + isi `Tanggal`.
- Bang Rob bisa cek file ini kapan aja.

---

*Last Update: 2026-09-02 13:30 - Udin (Building done, demo 5 CV OK, now Polish Finishing)*
