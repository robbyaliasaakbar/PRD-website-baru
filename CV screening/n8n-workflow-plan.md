# Plan - Workflow n8n CV Screening | Technical Test

**Workflow:** `Technical Test` (ID: v1lQYQUZndoeoR6w)  
**Webhook:** `POST /upload-cv`  
**Mode:** Planning - Implementasi 1 Node per 1 Node (Checkpoint)  
**Tanggal:** 1 September 2026  
**Stack:** n8n, PostgreSQL, Ollama Qwen 2.5 1.5B Q8_0 (local), Vanilla JS Code Node  
**Filosofi:** AI cuma parsing, scoring predictable rule-based (50-20-30), tanpa biaya token.

---

## 1. Filosofi & Keputusan Desain (Disepakati Bang Rob)

- **AI Minimalis:** Qwen 2.5 1.5B Q8_0 via Ollama local hanya untuk **parsing** text CV jadi JSON terstruktur. Tidak untuk scoring/penilaian. Hemat, private, jalan di laptop biasa.
- **Scoring Predictable:** Rule-based yang bisa diaudit HR, bukan black-box AI.
  - Keahlian max 10 skill → **50%**
  - Edukasi max 4 item → **20%**
  - Experience max 6 tahun → **30%**
- **Routing Efisien:** Keyword catcher di Code JS, rapihin di Edit Fields, lalu Switch berdasar posisi. Satu flow untuk Posisi A/B/C, tinggal duplikat cabang.
- **Implementasi Bertahap:** 1 node per checkpoint, test per node, baru lanjut. Anti error numpuk.

---

## 2. Arsitektur End-to-End (Gambaran Kasar Bang Rob + Refinement Udin)

```
[Landing Page] --POST JSON Array 1-5 CV--> [n8n Webhook /upload-cv]
                                                    ↓
                                    [1. Webhook] Terima array
                                                    ↓
                                    [2. Code: Extractor] Pecah 5 CV
                                                    ↓ (5 item, masing-masing {id_cv, nama_file_cv, nama_pengirim, isi_cv})
                                    [3. Code: Keyword Router] Tangkep kata kunci bidang, tetap bawa full JSON
                                                    ↓
                                    [4. Edit Fields] Rapihin data buat Switch
                                                    ↓
                                     [5. Switch] Routing berdasar posisi
                                     ┌──────────────┼──────────────┐
                                     ↓              ↓              ↓
                                 Posisi A       Posisi B       Posisi C
                              (Junior Arch)  (Drafter)      (Interior)
                                     ↓              ↓              ↓  (flow sama, duplikat)
                                     [5b. Code: Pemecah Jalur] Truncate + batch_index (robust)
                                     [5c. Split In Batches:1 + Wait 800ms] Sequential
                                     ↓
                                     [6. HTTP Request: Ollama Qwen 2.5 1.5B]
                                          POST http://host.docker.internal:11434/api/generate
                                          prompt: parse skill, edukasi, experience → JSON
                                                    ↓
                                    [7. Code: Scoring Engine] Hitung 50-20-30
                                                    ↓
                                    [8. Code: Standarisasi Kelolosan] Threshold 65%
                                         ≥65% Lolos, <65% Tidak Lolos
                                                    ↓
                                    [9. Postgres] Insert ke db
                                                    ↓
                                    [10. Code: Output JSON] Bikin ranking + alasan
                                                    ↓
                                    [11. Respond to Webhook / HTTP Request] Kirim balik ke landing page
```

**Catatan:** Untuk POC awal, fokus **Posisi A (Junior Architect)** dulu biar cepat. Posisi B/C tinggal duplikat node 6-11 setelah Posisi A stabil.

---

## 3. Detail Per Node (Checkpoint 1 Node = 1 Step)

### Checkpoint 1: Webhook POST /upload-cv
- **Node:** `n8n-nodes-base.webhook`
- **Config:**
  - `httpMethod`: POST
  - `path`: `upload-cv` (hasilkan `http://localhost:5678/webhook/upload-cv`)
  - `responseMode`: `responseNode` (biar bisa custom response di akhir)
  - `options`: `rawBody: false`
- **Input Expected (dari landing page):**
  ```json
  [
    { "id": "cv_1", "filename": "CV Sample 1.pdf", "size_kb": 320, "page_count": 2, "text": "...", "uploaded_at": "..." },
    { "id": "cv_2", ... }
  ]
  ```
- **Test:** POST via curl / landing page, cek execution muncul.

### Checkpoint 2: Code JS - Extractor (Pecah CV)
- **Node:** `n8n-nodes-base.code`
- **Tugas:** Terima 1 item berisi array (dari webhook), pecah jadi max 5 item individual. Output per item tetep bawa full JSON + field baru yang rapi.
- **Input:** `$input.first().json` bisa jadi array atau object. Webhook n8n kadang bungkus body di `body`.
- **Output per item:**
  ```json
  {
    "id_cv": "cv_1",
    "nama_file_cv": "CV Sample 1.pdf",
    "nama_pengirim": "Budi Setiawan", // ekstrak dari text: ambil baris pertama atau regex Nama
    "isi_cv": "full text...",
    "size_kb": 320,
    "page_count": 2,
    "uploaded_at": "...",
    "raw": { /* original object */ }
  }
  ```
- **Logic JS:**
  ```javascript
  const body = $input.first().json.body || $input.first().json; // handle webhook wrapping
  const arr = Array.isArray(body) ? body : (Array.isArray(body.data) ? body.data : [body]);
  return arr.slice(0,5).map(item => {
    const text = item.text || item.isi_cv || "";
    const firstLine = text.split('\n')[0]?.trim().slice(0,60) || item.filename;
    return { json: {
      id_cv: item.id || item.id_cv,
      nama_file_cv: item.filename || item.nama_file_cv,
      nama_pengirim: firstLine,
      isi_cv: text,
      size_kb: item.size_kb,
      page_count: item.page_count,
      uploaded_at: item.uploaded_at
    }};
  });
  ```
- **Test:** Input 2 CV dummy, cek output jadi 2 item terpisah.

### Checkpoint 3: Code JS - Keyword Router (Penentu Jalur)
- **Node:** `n8n-nodes-base.code`
- **Tugas:** Tangkep kata kunci spesifik bidang, tentuin `posisi`, tapi tetap bawa semua field sebelumnya.
- **Posisi & Keyword (berdasar Vacancy Junior Architect):**
  - **Posisi A (Junior Architect):** `autocad, revit, sketchup, architect, technical drawing, building code`
  - **Posisi B (Interior Designer) - dummy:** `interior, 3d max, vray, rendering, furniture, lighting`
  - **Posisi C (Drafter) - dummy:** `drafter, autocad, drawing, detail, structure, civil`
- **Output tambah field:**
  ```json
  { "posisi": "Posisi A", "keyword_match": ["autocad","revit"], "keyword_score": 2, ...separated fields }
  ```
- **Logic:** Lowercase isi_cv, cek includes tiap keyword, hitung match. Tentukan posisi dengan match terbanyak. Default ke Posisi A kalau tie.
- **Test:** CV dengan kata "AutoCAD & Revit" harus jadi Posisi A.

### Checkpoint 4: Edit Fields (Perapihan Data)
- **Node:** `n8n-nodes-base.set` (Edit Fields)
- **Tugas:** Rapihin & normalisasi field biar Switch gampang baca, tanpa logic berat.
- **Fields to set:**
  - `id_cv` = `{{$json.id_cv}}`
  - `nama_file_cv` = `{{$json.nama_file_cv}}`
  - `nama_pengirim` = `{{$json.nama_pengirim}}`
  - `isi_cv` = `{{$json.isi_cv}}`
  - `posisi` = `{{$json.posisi}}`
  - `keyword_match` = `{{$json.keyword_match}}`
- **Mode:** `Manual Mapping`, `Keep Only Set` = true (buang field sampah biar clean).
- **Test:** Cek output cuma 6 field rapi.

### Checkpoint 5: Switch (Routing Berdasar Posisi)
- **Node:** `n8n-nodes-base.switch`
- **Config:**
  - `mode`: `Rules`
  - `rules`:
    - `Route 0`: `posisi` `is Equal` `Posisi A`
    - `Route 1`: `posisi` `is Equal` `Posisi B`
    - `Route 2`: `posisi` `is Equal` `Posisi C`
  - `fallbackOutput`: `extra` (untuk yang tidak match, masuk antrian manual)
  - `allMatchingOutputs`: false (satu CV satu jalur)
- **Test:** Kirim 3 CV dengan posisi beda, cek keluar di output yang benar.

### Checkpoint 5b: Code - Pemecah Jalur (Robust untuk Qwen 1.5B)
- **Node:** `n8n-nodes-base.code`
- **Tugas:** Mecah jalur biar Qwen 1.5B tidak kewalahan kalau 4 CV masuk bareng. Truncate `isi_cv` kalau >6000 char, tambah `batch_index`, tetap bawa semua field.
- **Logic:** `isi_cv_truncated = isi_cv.slice(0,6000)`, `batch_total`, `batch_index`.
- **Test:** 4 item masuk → keluar 4 item dengan `isi_cv_truncated` ≤6000 char.

### Checkpoint 5c: Split In Batches + Wait (Sequential)
- **Nodes:** `n8n-nodes-base.splitInBatches` (batchSize=1) + `n8n-nodes-base.wait` (800ms)
- **Tugas:** Proses 1 CV per loop, kasih jeda biar Ollama tidak OOM. Loop sampai semua batch selesai.
- **Config:** `batchSize: 1`, `options: {}` ; Wait `amount: 800ms`
- **Test:** 4 CV → 4 loop sequential, execution terlihat 1-1.

### Checkpoint 6: HTTP Request - Ollama Qwen 2.5 1.5B (Parsing Only)
- **Node:** `n8n-nodes-base.httpRequest`
- **Tugas:** Cuma parsing, bukan scoring. Minta Qwen balikin JSON terstruktur.
- **Config:**
  - `method`: POST
  - `url`: `http://host.docker.internal:11434/api/generate` (jika n8n di Docker) atau `http://localhost:11434/api/generate` (jika native). Sediakan dua opsi, fallback.
  - **Catatan Port Landing Page:** Landing page di-serve via `http://localhost:8010` (port 8000 kepake), webhook tetap `http://localhost:5678/webhook/upload-cv`.
  - `body`: JSON
    ```json
    {
      "model": "qwen2.5:1.5b",
      "prompt": "Ekstrak dari CV berikut. Balikin HANYA JSON valid tanpa teks lain. Format: {\"keahlian\":[\"skill1\",...],\"edukasi\":[\"S1 Arsitektur\",...],\"experience_tahun\":2,\"experience_detail\":\"...\"}\n\nCV:\n{{ $json.isi_cv }}",
      "stream": false,
      "format": "json",
      "options": { "temperature": 0, "num_predict": 500 }
    }
    ```
  - `options`: `timeout: 30000`
- **Response handling:** Qwen balikin `{ response: "{\"keahlian\":...}" }`, perlu Code Node kecil untuk `JSON.parse` (bisa digabung di node 7).
- **Alternative jika Ollama belum ready:** Fallback ke regex sederhana di Code Node (tanpa AI) biar workflow tetap jalan.
- **Test:** Kirim CV Sample 1 text, cek response JSON keahlian terisi.

### Checkpoint 7: Code JS - Scoring Engine (50-20-30 Predictable)
- **Node:** `n8n-nodes-base.code`
- **Input:** `{{$json}}` dari Ollama (sudah diparse) + field sebelumnya.
- **Logic:**
  ```javascript
  const keahlian = $json.keahlian || []; // array skill
  const edukasi = $json.edukasi || [];
  const exp = Number($json.experience_tahun || 0);

  const skillScore = Math.min(keahlian.length, 10) / 10 * 50;
  const eduScore = Math.min(edukasi.length, 4) / 4 * 20;
  const expScore = Math.min(exp, 6) / 6 * 30;

  const total = Math.round(skillScore + eduScore + expScore);
  const detail = {
    keahlian_count: keahlian.length,
    edukasi_count: edukasi.length,
    experience_tahun: exp,
    skillScore, eduScore, expScore
  };
  return [{ json: { ...$json, ...detail, total_score: total }}];
  ```
- **Contoh:** 8 skill (40) + 2 edukasi (10) + 3 tahun (15) = 65%
- **Test:** Keahlian 10, edukasi 4, exp 6 harus = 100.

### Checkpoint 8: Code JS - Standarisasi Kelolosan
- **Node:** `n8n-nodes-base.code`
- **Config:** Threshold **≥65% Lolos, <65% Tidak Lolos** (clean, tidak overlap).
- **Logic:**
  ```javascript
  const score = $json.total_score;
  const status = score >= 65 ? "Lolos" : "Tidak Lolos";
  const alasan = status==="Lolos"
    ? `Memenuhi ${score}%, keahlian ${$json.keahlian_count}/10, edukasi ${$json.edukasi_count}/4, exp ${$json.experience_tahun}th`
    : `Hanya ${score}%, perlu tingkatkan keahlian/edukasi/exp`;
  return [{ json: { ...$json, status, alasan }}];
  ```
- **Test:** Score 65 → Lolos, 64 → Tidak Lolos.

### Checkpoint 9: Postgres - Simpan Hasil
- **Node:** `n8n-nodes-base.postgres`
- **Table:** `screening_results` (buat dulu):
  ```sql
  CREATE TABLE IF NOT EXISTS screening_results (
    id SERIAL PRIMARY KEY,
    id_cv VARCHAR(20),
    nama_file_cv TEXT,
    nama_pengirim TEXT,
    posisi VARCHAR(20),
    total_score INT,
    status VARCHAR(20),
    alasan TEXT,
    keahlian JSONB,
    edukasi JSONB,
    experience_tahun INT,
    created_at TIMESTAMPTZ DEFAULT NOW()
  );
  ```
- **Operation:** `Insert` (atau `Upsert` jika id_cv sudah ada)
- **Columns mapping:** semua field dari node 8 + `keahlian`, `edukasi` as JSON.
- **Credentials:** Postgres yang sudah ada di n8n (cek list_credentials).
- **Test:** Insert 1 row, cek di DB.

### Checkpoint 10: Code - Output JSON Final
- **Node:** `n8n-nodes-base.code` atau `n8n-nodes-base.aggregate` + `code`
- **Tugas:** Gabungin hasil dari Posisi A/B/C (karena Switch misah, perlu Merge atau Aggregate).
- **Jika pakai 1 posisi dulu:** Cukup format.
- **Output:**
  ```json
  {
    "posisi": "Posisi A - Junior Architect",
    "total_cv": 5,
    "lolos": 2,
    "tidak_lolos": 3,
    "ranking": [
      { "id_cv": "cv_3", "nama_file_cv": "...", "total_score": 92, "status": "Lolos", "alasan": "..." },
      { "... sorted desc ..." }
    ]
  }
  ```
- **Logic:** Sort `ranking` by `total_score` desc.
- **Test:** 3 CV dengan score 70, 85, 60 → urutan 85,70,60.

### Checkpoint 11: Respond to Webhook / HTTP Request Balik ke Landing Page
- **Node:** `n8n-nodes-base.respondToWebhook` (prefer, lebih simple)
  - `respondWith`: `json`
  - `responseData`: `{{ $json }}` dari node 10
  - `responseCode`: 200
- **Alternative (sesuai gambaran Bang Rob):** `httpRequest` POST ke landing page URL (butuh landing page punya endpoint, jadi kurang efisien). Rekomendasi: pakai **Respond to Webhook** biar landing page langsung dapat response dari fetch.
- **Test:** Landing page fetch harus dapat JSON ranking.

---

## 4. Handling & Robustness

- **Array Handling:** Webhook → Code Extractor harus handle baik `body` array maupun `body.data`.
- **Error Ollama:** Jika Ollama timeout/offline, fallback ke regex parsing sederhana di Code Node (jangan fail total).
- **Max 5 CV:** Extractor `slice(0,5)` biar tidak overload.
- **Switch Fallback:** Output `fallback` untuk CV yang tidak match keyword (masuk manual review).
- **Postgres Fail:** Jika DB down, tetap lanjut ke Output JSON (jangan block response).

---

## 5. Kredensial & Dependensi

- **Ollama:** Pastikan `ollama serve` jalan + `qwen2.5:1.5b` sudah pull (`ollama pull qwen2.5:1.5b`). URL test: `curl http://localhost:11434/api/tags`.
- **Postgres:** Cek `list_credentials` untuk `postgres` credential ID.
- **n8n:** Workflow `Technical Test` sudah ada, tinggal update via `update_workflow`.

---

## 6. Estimasi & Urutan Implementasi (1 Node per 1 Node)

**Sesi 1 (Hari ini):** Checkpoint 1-5 (Webhook sampai Switch) → fondasi routing  
**Sesi 2:** Checkpoint 6-8 (Ollama + Scoring + Threshold) → otak penilaian  
**Sesi 3:** Checkpoint 9-11 (DB + Output + Response) → finishing

Setiap checkpoint: **addNode → validate → test → confirm Bang Rob → next.**

---

*Disusun oleh Udin - Sahabat Coding Bang Rob - Berdasarkan ide efisien Bang Rob (Predictable Rule-Based + Qwen Local)*
