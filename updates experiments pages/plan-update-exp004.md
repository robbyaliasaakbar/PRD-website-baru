# Plan Update EXP004. QC Bang Rob

Status: DRAFT PLAN revisi. Belum build. Nunggu QC plus LOCK bang rob.
Target: `public_html/exp004.html` (1213 baris, EN page)
Sumber diskusi: `../catatan-update-exp004.md` (7 dari 7 DONE, B2 sisa PR kecil)
Prinsip: portfolio bukan tutorial, jujur estimasi, tanpa sebut status izin atau nama PG, pisah Runtime dan Dev Tools.

## 0. Prinsip QC. Jangan dilanggar pas build

1. Tidak ada kata soal izin OJK, tidak ada nama PG teman. Framing resmi: `private internal adapter, interchangeable`.
2. Tidak ada tutorial bypass. Cukup high level plus `best practice remains official Meta API`.
3. Tidak ada angka ngarang. Angka yang boleh tampil: 40 to 50 adversarial, 4 hari, 1 bubble 5 niat paralel, temp 0 dan top p 0.1, listrik plus 400rb, fee 574, 11 per hari, 26 hari, sekitar 164164, total sekitar 564k, lebih dari 50M dari 5jt kali 9 to 10, 266 nodes dan 3 workflow, 4 dari 266, broadcast maksimal 20, backup 1 to 2 nomor, rest 1 to 2 jam, update tiap 2 minggu.
4. Pisah CAPEX yaitu sunk R dan D dan OPEX yaitu bulanan. Kiri OPEX manusia, kanan OPEX sistem.
5. Pisah Runtime yaitu jalan 24 per 7 dan Dev Tools yaitu pas ngebangun. Concurrent tanpa spill, cukup link exp001 dan exp002.
6. Living system: snapshot per tanggal, evidence lanjutan menyusul, jangan overclaim final.
7. Style wajib dari bang rob:
   * Tidak pakai emoji apapun di copy baru. Emoji lama di bagian What Failed dibiarkan apa adanya, tidak ditambah yang baru.
   * Tidak pakai emdash atau endash dalam kalimat. Pakai titik atau koma sebagai pengganti.
   * Tidak pakai strip di tengah kalimat untuk memotong kalimat. Strip hanya boleh untuk listing di awal baris dan untuk nama teknis yang memang ada stripnya seperti nama file model. Contoh nama file tetap ditulis apa adanya di dalam code ticks.
   * Tidak pakai semicolon di copy manapun. Pakai titik atau koma.
   * Section non technical wajib berasa ringan, bikin pembaca merasa pintar, pakai analogi warung yang natural walau nyeleneh.
   * Section Reliability dan Risk wajib tampil sebelum Experiment Details karena paling penting dan tidak boleh kelewat baca.

## 1. HEAD dan SEO. Edit kecil

Lokasi: head lines sekitar 43 sampai 44 dan 61.
Ubah:
* `article:modified_time` ke tanggal build nanti. Misal `2026-09-23T00:00:00+07:00`. Samakan dengan `dateModified` di JSON-LD Article.
* Tidak ubah title dan meta description dan canonical dan OG. Sudah kuat.
Alasan: living system tiap 2 minggu (B5). Modified harus fresh biar kredibel plus SEO.

## 2. SYSTEM REQUIREMENTS. Ubah plus tambah label. Tanpa tambah tile baru

Lokasi: section `System Requirements` yang isinya 9 tile.
Ganti isi 2 tile untuk typo fix:
* `ORNITH-1.0-25B-A3B-IQ4_NL-GGUF` ke `ORNITH-1.0-35B-A3B-IQ4_NL-GGUF`
* `Qwen2.5-1.5B-Instruct-Q8_0-GGUF` ke `qwen-2.5-1.5B-instruct-Q8_0.GGUF`

Tambah label grup. Tanpa ubah grid. Cuma tambah 2 subheading:
* Subheading di atas tile runtime: `Runtime. What serves customers every day.`
  Isi: CPU, RAM, GPU, OS, Model For Parsing yaitu qwen, Inference Engine yaitu Ollama only, Frontend UI yaitu Open WebUI dan n8n, Status. Catatan: Llama dot cpp pindah ke Dev Tools.
* Subheading di atas tile dev: `Dev Tools. What helped me build it.`
  Isi: `ORNITH-1.0-35B` plus `Llama dot cpp dan opencode plus MCP n8n`
  Tambah satu baris kecil di bawahnya: `Not part of 266 nodes. Concurrent execution on the same machine uses learnings from exp001 and exp002. Develop it yourself. See links.`

QC note: B3. No spill. Cuma link. Jangan tulis cara offload atau VRAM split.

## 3. SCREENSHOTS. Tahan dulu. Tidak diubah di batch ini

Tetap 3 gambar. Evidence baru yaitu log sensor plus WA sensor masuk batch lanjutan setelah bang rob siapin file. Jangan tambah placeholder kosong.

## 4. VISUALIZATION. Tahan. Tidak diubah

Gradient AI First vs Deterministic First plus radar tetap. Tidak perlu angka baru.

## 5. NON TECHNICAL SUMMARY. Rewrite total dengan analogi warung. Wajib ringan

Lokasi: `.summary-callout` lines sekitar 502 sampai 516.
Judul section tetap: `For those unable to interpret the data from a technical standpoint, here is the conclusion`

Ganti total 8 paragraf dengan copy baru di bawah. Copy ini EN, siap tempel, tanpa emoji, tanpa emdash, tanpa strip di tengah kalimat, tanpa semicolon. Sengaja dibuat seperti cerita warung supaya pembaca non technical merasa pintar.

Paragraf 1:
`Think of my business like a small food stall. Every day there are customers asking where we deliver, what they want to wash, when we can come, and what their house number is. This system is the stall assistant that never sleeps. It is live and I update it every 2 weeks from real notes.`

Paragraf 2:
`At first I gave all the keys to a smart intern and told him to guess everything. He guessed the price. He guessed the area. He even guessed if people already paid. He was confident but often wrong. That was the AI first phase. Hundreds of tries failed.`

Paragraf 3:
`Then I changed the job. The intern now only shouts short keywords. Like Jakarta. Like sofa. Like today at 2. Like number 11. That is all. The boss which is my hardcoded rules plus my database decides the price and the schedule and the reply. If the shout is unclear, we simply ask again. No guessing.`

Paragraf 4:
`I tested it like a stall owner tests a new helper. First I tried 40 to 50 tricky customers myself. Then my staff tried to trick it. Then my non technical friends tried their own weird requests. It took 4 days to call it ready. Total hundreds of chats with the same steady answers. It still runs today.`

Paragraf 5:
`In daily work it covers the boring repeat jobs of 9 to 10 people in 5 spots. Like 4 to 5 admin sellers from first chat to closing. Like 2 to 3 customer service for complaints. Like 1 ads helper that sends leads to Meta and Google. Like 1 data note taker that saves failed inputs for research. Like 1 finance helper for invoices linked to my exp006. Smart decisions stay with humans.`

Paragraf 6:
`About money. This is estimation, not promise. Paying 9 to 10 people with Bekasi minimum simplified to 5 million each plus allowances easily passes 50 million a month. I run at about 564 thousand a month. It is plus 400 thousand electricity measured at home plus about 164 thousand gateway fees. The fee is 574 flat per paid transaction times about 11 a day times 26 work days. Hardware is already owned. Internet uses the existing home line. Maintenance is a few hours a week by one person.`

Paragraf 7:
`About payment. Each deal gets its own unique virtual number or QR code. It lives for 3 minutes and we make a new one if it expires. We only wait for a succeed status. We never guess from transfer screenshots. The webhook road uses Tailscale Funnel so there is no direct public IP. History with personal data is wiped. Only order notes stay for finance. Gateway name is hidden on purpose and you can swap it with any licensed one.`

Paragraf 8:
`About crowd. Orders did not drop. Revenue is stable. There is a small lift but nothing crazy. Same as similar stalls run by humans. What clearly changed is spending and how easy it is to scale and maintain. The machine is modest. For daily run I use auto start, periodic health checks, scheduled rest of 1 to 2 hours at quiet time, spare WA numbers, and a human backup when the machine rests. Unofficial WA tools carry ban risk from Meta. Best practice remains the official Meta API.`

QC note: B1 B2 B4 B6 B7 masuk semua tapi dalam bahasa warung. Minta bang rob cek analogi intern dan warung cukup natural atau mau lebih nyeleneh lagi. Angka 400rb dan 574 dan 11 dan 26 dan 164164 dan 564k wajib LOCK. Kata MENGGANTIKAN repetitif sudah terwakili dengan `covers the boring repeat jobs`.

## 6. RELIABILITY dan RISK. Section penting. Wajib sebelum Experiment Details

Alasan pindah: perintah bang rob. Ini paling penting dan tidak boleh kelewat baca. Jadi urutan page nanti: Visualization lalu Non Technical Summary lalu Reliability dan Risk lalu Experiment Details. Bukan di bawah lagi.

Layout: 1 section penuh `py-20` dengan eyebrow `Reliability and Risk`. Judul H2: `Important to read before the details`. Tiga cards plus satu quote. Responsive 1 kolom di mobile dan 3 kolom di desktop. Pakai class existing. Tanpa JS baru.

Card 1. Private road:
`Webhooks use Tailscale Funnel. Public yes but with no direct public IP. The private callback road is separated from the public customer pages. History with personal data is wiped after the JSON reaches the trigger. Only order notes stay for finance.`

Card 2. WA numbers can be banned:
`All unofficial WA tools carry the same ban risk from Meta if detected. I do not hide this. I keep 1 to 2 spare WA numbers ready. I use a separate funnel, human like typing pauses, throttled broadcast a few times a week with max 20 contacts per batch and randomized pattern. Best practice remains the official Meta API.`

Card 3. Machine needs rest plus human backup:
`Ollama auto starts with system service. n8n runs in Docker with always restart. Health is checked with periodic terminal checks. This is not workstation hardware with no ECC memory. So I rest the PC 1 to 2 hours at quiet time to keep it durable. When the system rests or dies, a human takes over. That is the human in the loop fallback.`

Quote bawah:
`Read this first. If you skip this part, the rest of the page will look like magic. It is not magic. It is maintenance plus fallback.`

QC note: B6. High level only. Jangan tulis kata spoofing di publik. Pakai `typing pauses`. Jangan tulis langkah bypass. Minta bang rob cek kata `human in the loop` tanpa strip sudah oke.

## 7. EXPERIMENT DETAILS. Edit per sub bagian. Posisi setelah Reliability

### 7a. Problem dan Specifications dan Experiment dan What Failed dan Iteration. TAHAN
Tidak diubah. Sudah kuat. Emoji lama di What Failed dibiarkan. Jangan tambah emoji baru di tempat lain.

### 7b. Hypothesis point 6 yaitu payment gateway. UBAH TOTAL
Lama: `I cannot disclose... due to credibility and legal...`
Baru EN siap tempel:
`6. A payment gateway adapter reachable with status check plus webhook verification. I use a private internal adapter. Name is withheld for privacy. You can swap it with any licensed gateway such as Midtrans or Xendit or DOKU. It issues unique VA plus QRIS per transaction and returns a succeed status. No customer data entry. No screenshot guessing. See Cost and Reliability sections for fees and safeguards. Invoicing links to my exp006 invoice generator.`

QC note: B2. Tanpa sebut izin. Tanpa nama. Ada fee di section Cost. Ada link exp006.

### 7c. Evidence. TAMBAH 2 paragraf metodologi. Tanpa ubah 8 tile angka
Sisipkan di bawah kalimat `These are the figures...` EN siap tempel:
`How tested. 40 to 50 tricky scenarios by me. Then staff testing with their own ideas. Then non technical friends testing theirs. 4 days to fully tested. Hundreds of chats total with identical outputs. Decoding locked at temp 0 and top p 0.1 with greedy sampling. One bubble with up to 5 intents is split by hardcoded logic and parsed in parallel. No exact lab count. Layered validation before public deploy. Stable since.`
`How guarded. Regex cleans before the model. JS plus Postgres checks after the model. Unknown means ask the customer. Never guess.`

QC note: B1. Jujur tidak ada angka eksak ratusan tapi ada 40 to 50 plus 4 hari plus 3 fase.

### 7d. Result dan Learned dan Practical Implication. TAMBAH 1 kalimat each
* Result tambah: `Failed inputs are kept as research data for the 2 week iteration cycle.`
* Learned tambah: `Portfolio not tutorial. Concurrent learnings from exp001 and exp002 are referenced. Develop it yourself.`
* Practical tambah: `Start with keyword parsing plus templates. Then expand to parallel splitter plus deterministic checks.`

## 8. KEYWORD PARSING. Section baru. Tambah setelah Evidence, sebelum Result

Tujuan: B1 butuh tabel bukti. Skeptis butuh contoh konkret. Ini section teknis jadi boleh pakai istilah teknis singkat. Tetap tanpa emoji tanpa emdash tanpa strip tengah kalimat tanpa semicolon.

Layout: 1 section `bg-paper py-20`, grid 2 kolom di desktop dan 1 kolom di mobile. 4 cards A B C D plus satu badge decoding. Pakai class existing yaitu `border rounded-xl p-6`. Tanpa JS baru.

Copy per card EN siap tempel:

* A Location. Input `aku dijakarta kak` becomes `jakarta`. Other examples `cikarang` becomes `cikarang` and `BSD` becomes `BSD`. Cleaned before the model. Then code checks province to village level. Then database checks coverage. Then template reply is prepared.
* B Item. Input `saya mau cuci kasur ukuran 180 2 pcs` becomes `kasur, 180, 2 pcs`. Then hardcoded price estimate follows. Input `saya mau cuci sofa` becomes `sofa, ukuran unknown, qty unknown`. Then we ask details first. No model after this point.
* C Schedule. Input `saya mau hari ini jam 2 siang` becomes `hari ini, jam 2 siang`. Then hardcoded scheduler follows.
* D House number. Input `griya asri 2 blok e5 nomor 11` becomes `11`. If unreadable it becomes `unknown`. Then we ask again so field team never gets lost.

Badge di atas grid:
`Decoding locked at temp 0 and top p 0.1 with greedy sampling. Clean before. Check after. Unknown means ask.`

QC note: Minta bang rob cek contoh typo `dijakarta` lawan `di jakarta` plus `BSD` plus `180 2 pcs` plus `e5 nomor 11` sudah sesuai asli. Jangan tambah contoh baru tanpa izin.

## 9. BUSINESS IMPACT. Edit kecil plus link

* Judul plus 5 cards tetap.
* Tambah di card Finance kalimat link: `Invoicing links to exp006 invoice generator.`
* Tambah note di bawah grid yang sudah ada dan dipertajam: `Repeat rule based tasks only. This system is designed to REPLACE those repeat roles. Smart and creative decisions stay human.`
QC note: B4 B7. Kata REPLACE wajib versi halus tapi tegas sesuai sikap bang rob yang tidak mau nutupin.

## 10. COST IMPLICATION. Rewrite berat. Kiri tahan, kanan plus bar plus disclaimer ubah total

Kiri yaitu If I hired 9 to 10 people. TAHAN angka `lebih dari Rp 50.000.000 per month` plus 3 bullet. Tambah 1 bullet: `Office rent plus leave plus turnover drama. Benchmark Bekasi minimum simplified to Rp 5jt per person. Estimation not guarantee.`

Kanan yaitu What I run now. GANTI TOTAL:
* Badge tetap `Stable`
* Angka: `about Rp 564.000 per month` plus sub `cash out measured`
* Bullets baru. Semua bullet adalah listing jadi boleh pakai strip di awal baris:
  * `Electricity plus Rp 400k per month. Measured at home. 24 per 7 existing PC.`
  * `Gateway about Rp 164k per month. Rp 574 flat times about 11 paid per day times 26 work days.`
  * `Hardware owned. Internet existing. No server rent. Build cost excluded as sunk CAPEX.`
  * `Maintenance a few hours a week by 1 person for logs plus failed input iteration.`
  * `Postgres plus Evolution API plus qwen model via Ollama. Nama file lengkap ada di System Requirements.`

Bar visualization. GANTI:
* Human `100 percent` label `lebih dari Rp 50M`
* System width dari `12 percent` ke `2 percent`. Alasan: 564k dibagi 50M sekitar 1.1 percent. Dibulatkan visual 2 percent biar kelihatan. Label `about Rp 0.56M which is 1.1 percent`.
* Footnote baru: `Estimation uses Bekasi minimum simplified to Rp 5jt per person for 9 to 10 headcount. System OPEX measured as plus 400k electricity plus about 164k gateway fees. Build and research is sunk CAPEX excluded. Your numbers will differ. Owner mentality means business keeps running and takes profit.`

QC note: B4. Minta bang rob LOCK angka 400rb dan 574 dan 11 dan 26. Bar 2 percent lawan 12 percent tolong QC visual.

## 11. HARDWARE. Rewrite ringan karena Reliability sudah pindah ke atas

Judul tetap tapi tanpa emdash: `Hardware and Reliability. Modest concurrent maintainable`
Subjudul: `Same machine for build and run. Rested on schedule. Checked periodically. With human fallback. Details on risk are in the Reliability section above.`

Kolom kiri yaitu Current What I run today. Update list:
* CPU i5 11400F, RAM 16GB DDR4 3200, GPU RX 6700 XT, OS Ubuntu 26.04 LTS. TAHAN. Catatan: di copy publik tulis tanpa strip tengah kalimat. Untuk nama CPU tulis apa adanya karena nama produk dikecualikan dari aturan strip.
* Inference runtime: `Ollama with qwen model. Nama file ada di System Requirements.`
* Dev build: `Llama dot cpp with ORNITH 35B plus opencode plus MCP n8n. Not in 266 nodes.`
* Frontend: Open WebUI dan n8n. TAHAN.
* Tambah baris: `Concurrency uses learnings from exp001 and exp002. Not spoon fed. See links.`

Kolom kanan yaitu If you want 24 per 7. Ubah 4 bullets jadi:
* `Ollama auto run with system service. n8n with Docker always restart. Health with periodic terminal checks.`
* `Scheduled rest 1 to 2 hours at quiet hours. Non ECC memory lasts longer with rest.`
* `NVMe backup plus UPS plus stable internet.`
* `Human in the loop fallback when system is down for rest or maintenance.`

Quote bawah ganti jadi: `The question was never can it run. The question is can it rest and recover and hand over to human without drama.`

QC note: B3 B6. Tanpa spill trik concurrent. Tanpa tulis perintah systemd atau docker detail.

## 12. OPERATIONAL EFFICIENCY. Edit judul plus quote

* 4 cards TAHAN.
* Paragraf pembuka tambah: `Orders stable. Revenue stable. Small lift but nothing crazy. Same as human run peers.`
* Quote bawah ganti: `Demand did not drop and did not explode. What changed was cost to serve at about Rp 564k lawan more than Rp 50M plus how easily it scales and is maintained.`
QC note: B7. Jujur flat sama dengan stabil. Gain di cost plus scalability.

## 13. FAQ. Update 7 plus tambah 2 jadi 9. Plus sinkron JSON-LD

Update wording 7 existing. EN singkat siap tempel. Tanpa emoji baru. Tanpa emdash. Tanpa strip tengah kalimat. Tanpa semicolon.

1. Why AI first failed. Jawab tambah `cost, coverage, payment verification` yang sudah ada. TAHAN.
2. What is deterministic first. Ganti jadi: `Clean first. Then Qwen 1.5B extracts 1 to 3 keywords at 4 points with temp 0 and top p 0.1. Then JS plus Postgres decides. Only 4 of 266 nodes. One bubble with up to 5 intents split in parallel. Unknown means ask. Never guess.`
3. Can beginner build. Tambah: `See exp001 and exp002 first. Develop it yourself. Portfolio not tutorial.`
4. 9 to 10 people. Tambah: `Repeat rule based only. Designed to REPLACE those roles with 4 to 5 admin plus 2 to 3 CS plus 1 analyst plus 1 ads plus 1 finance. Strategy stays human. Invoicing links exp006.`
5. More than 50M guaranteed. Ganti jadi: `No. Estimation uses Bekasi minimum simplified to Rp 5jt times 9 to 10 plus allowances. I run at about 564k with 400k electricity plus about 164k gateway. Your numbers will differ.`
6. Expensive hardware for 24 per 7. Ganti jadi: `No. Same i5 plus 16GB plus 6700XT for Ornith 35B for dev plus Qwen 1.5B for runtime concurrently with exp001 and exp002 learnings. For daily run use auto start plus health checks plus 1 to 2 hours rest plus UPS and backup plus human fallback.`
7. Demand plus missed inputs. Ganti jadi: `Flat stable means orders and revenue stable with small lift. Same as human run peers. Missed inputs saved to private DB for 2 week iteration plus market research plus promos.`

Tambah 2 FAQ baru:
8. `How is payment verified without storing sensitive data` Jawab: `Unique VA plus QRIS per transaction with 3 minute window. New code if expired. Status check plus webhook. Never screenshot. With Tailscale Funnel. PII history wiped. Order notes kept. Name withheld. You can swap with licensed gateways. Fee Rp 574 flat per transaction.`
9. `Is WhatsApp ban risk hidden. What if system is down` Jawab: `No. Unofficial API can be banned by Meta. Mitigated with isolated funnel plus typing pauses plus throttled randomized broadcast max 20 plus spare numbers. Ollama auto start plus n8n always restart plus scheduled rest plus human fallback. Best practice is official Meta API. Evidence is living and updated every 2 weeks with deeper pages to follow.`

JSON-LD FAQPage: sinkronkan 9 Q dan A di atas kata per kata dengan visible details. Jangan lupa update `article:modified_time` juga.

QC note: B2 B5 B6. Cek kata `spoofing` jangan muncul. Pakai `typing pauses`. Cek fee 574 plus tailscale funnel plus living system masuk.

## 14. FOOTER CTA plus FOOTER. Tahan

Tidak diubah.

## 15. Verifikasi pas build. Checklist Udin

* Responsive: 1 kolom mobile ke 2 kolom sm ke 3 atau 4 kolom lg untuk grid baru yaitu System labels dan Keyword 2 kolom dan Cost 2 kolom dan Hardware 2 kolom dan Reliability 3 cards. Test 360px dan 768px dan 1280px.
* Semantic: 1 kali h1 tetap. Section baru pakai h2. Cards pakai p plus eyebrow. Jangan tambah h1 atau h3 ngaco.
* Alt text: tidak tambah img baru di batch ini kecuali badge tanpa img. Screenshot lama alt tetap.
* Title dan meta: title plus description tahan. Canonical tahan. OG tahan. Update modified time plus dateModified saja.
* Kelas Tailwind existing saja seperti `border-black/10 rounded-2xl p-6 bg-paper` dan lain lain. Tanpa CDN baru. Tanpa JS baru. File charts dot js plus main dot js tetap.
* Wording EN konsisten untuk nama teknis di dalam code ticks: `qwen-2.5-1.5B-instruct-Q8_0.GGUF` dan `ORNITH-1.0-35B-A3B` dan `Rp 574` dan `about Rp 564k` dan `more than Rp 50M` dan `temp 0 and top p 0.1` dan `Tailscale Funnel` dan `human in the loop` dan `exp006` dan `exp001 and exp002`.
* Style wajib: tidak ada emoji baru. Tidak ada emdash. Tidak ada strip di tengah kalimat prosa. Strip hanya untuk listing dan nama teknis. Tidak ada semicolon.
* Larangan: tidak sebut status izin. Tidak sebut nama PG. Tidak spill trik concurrent atau regex list atau prompt full. Tidak klaim growth wah. Tidak janji tanggal evidence.

## 16. Yang SENGAJA tidak dikerjakan di plan ini. Biar QC jelas

* Tidak export workflow n8n atau repo karena IP.
* Tidak tambah video atau log asli. Nunggu evidence lanjutan B5.
* Tidak ubah 3 screenshot. Tunggu file baru.
* Tidak ubah radar atau gradient numbers.
* Tidak buat halaman experiments lanjutan. Masuk backlog PRD terpisah.

## Minta QC Bang Rob. Jawab LOCK per nomor biar bisa build 1 per 1

1. Typo fix 35 plus nama Qwen bener. LOCK.
2. Split Runtime dan Dev Tools plus link exp001 dan exp002 tanpa spill. LOCK.
3. Summary warung 8 paragraf. Analogi cukup natural dan bikin merasa pintar atau mau lebih nyeleneh lagi. LOCK.
4. Reliability dan Risk pindah sebelum Experiment Details dengan 3 cards plus quote. LOCK.
5. Tabel A B C D plus badge decoding. Contoh input output sudah sesuai asli. LOCK.
6. Cost kanan about 564k dari 400k plus 164k plus bar 2 percent plus footnote CAPEX lawan OPEX plus kata REPLACE. LOCK.
7. Hardware concurrent plus auto start plus rest 1 to 2 jam plus human fallback. LOCK.
8. FAQ 9 yaitu 7 update plus 2 baru plus JSON-LD sinkron. LOCK.
9. Style bersih yaitu tanpa emoji baru tanpa emdash tanpa strip tengah kalimat tanpa semicolon. LOCK.

Kalau 9 dari 9 LOCK, gue build 1 section per 1 batch sesuai aturan.
