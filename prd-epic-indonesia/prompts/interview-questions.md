# Pertanyaan Wawancara — Epic PRD

Tanyakan secara percakapan — satu per satu, tidak sekaligus.
Validasi setiap jawaban sebelum lanjut ke seksi berikutnya.

---

## §0 — Intake (Jalankan Sebelum Segalanya)

Selesaikan semua pertanyaan §0 sebelum memulai wawancara seksi.
Gunakan jawaban §0 untuk konfigurasi seksi opsional dan menyesuaikan pertanyaan berikutnya.

---

### §0.0 — Base Path (Tanyakan Pertama Kali)

Sebelum pertanyaan apapun, tanyakan:

> *"Di mana dokumen PRD disimpan di project ini?"*

Berikan contoh pilihan umum:
- `docs/plan` — konvensi standar
- `prd/nama-app` — jika project multi-app seperti `prd/mobile-app` atau `prd/backend`
- Path lain sesuai konvensi tim

**Aturan:**
- Catat jawaban sebagai `{base-path}` — gunakan di semua instruksi penyimpanan selanjutnya
- Jika pengguna tidak tahu → gunakan `docs/plan` sebagai default dan konfirmasi: *"Saya akan gunakan `docs/plan` sebagai default. Bisa diubah nanti."*
- Jika Claude Code aktif → cek dulu apakah folder `prd/` atau `docs/plan/` sudah ada di filesystem, tawarkan yang ditemukan sebagai pilihan utama
- Path output final: `{base-path}/{nama-epic}/epic.md` dan `{base-path}/{nama-epic}/arch.md`

---

### §0.1 — Identitas Inisiatif

1. Apa nama kerja epic ini? (akan dirapikan menjadi noun phrase di §1 — nama apapun boleh untuk sekarang)
2. Satu kalimat: apa yang ingin dicapai epic ini, dan untuk siapa?

---

### §0.2 — Konteks Produk & Platform

3. Tipe inisiatif apa ini?
   *(fitur baru / redesign / tools internal / API atau platform / marketplace / B2B SaaS / backend saja / lainnya)*

4. Platform mana yang tersentuh?
   *(iOS / Android / Web / backend saja / lintas platform)*
   - Jika lintas platform: "Apakah ada perbedaan per platform, atau experience-nya sama?"

5. Siapa pengguna utamanya?
   *(konsumen / staf internal / klien B2B / developer / lainnya)*

---

### §0.3 — Domain & Konteks Compliance

6. Domain produk apa ini?
   *(mis. pembayaran, onboarding, notifikasi, pencarian, pengaturan, konten, growth, infrastruktur)*

7. Apakah epic ini menyentuh hal-hal berikut? *(konfirmasi satu per satu)*
   - Transaksi keuangan atau alur pembayaran
   - Verifikasi identitas (eKYC, KYB, biometrik)
   - Data pribadi atau PII di luar field profil dasar
   - Persyaratan regulasi (OJK, UU PDP, GDPR, PCI-DSS, dll.)
   - Produk kredit, pinjaman, atau asuransi
   - *Jika ada yang dikonfirmasi:* §13 Risiko & Mitigasi otomatis disertakan.

---

### §0.3b — Identifikasi Regulasi (Wajib Jika Ada Sinyal Compliance)

**Jalankan segera setelah §0.3 jika ada sinyal compliance. Jangan skip.**

Berdasarkan domain, geografi, dan sinyal yang dikonfirmasi, identifikasi regulasi yang berlaku:

| Sinyal | Regulasi yang Perlu Dipertimbangkan |
|---|---|
| Transaksi keuangan / pembayaran | PCI-DSS, OJK BI-SNAP, Peraturan BI terkait |
| Verifikasi identitas / eKYC / KYB | OJK POJK 12/2018, FATF AML/CFT |
| Kredit / pinjaman / BNPL | OJK POJK 35/2018 |
| Data pribadi / PII | UU PDP Indonesia, PDPA (jika ada operasi di Thailand/SG) |
| Data biometrik | UU PDP Pasal khusus data sensitif |
| Data lintas batas | UU PDP Bab transfer data internasional |
| Anak-anak / data minor | UU PDP ketentuan khusus anak |

Langkah-langkah:
1. Sampaikan: "Berdasarkan yang Anda ceritakan, regulasi berikut kemungkinan berlaku: [daftar]. Apakah benar? Ada yang perlu ditambah atau dihapus?"
2. Tunggu konfirmasi pengguna.
3. Catat daftar regulasi yang dikonfirmasi → digunakan di §3 (Konteks Regulasi), §5 (Ruang Lingkup), §13 (Risiko).
4. Jika pengguna tidak yakin: "Saya akan tandai sebagai `[TBD — tim legal/compliance perlu konfirmasi regulasi yang berlaku]` dan tambahkan sebagai open item di §12 FAQ."

---

### §0.4 — Konteks Tim & Eksekusi

8. Berapa tim yang terlibat dalam epic ini?
   *(hanya tim kita / 2 tim / 3 tim atau lebih / lintas organisasi)*
   - Jika 3+ tim → tandai §16 Peta Stakeholder.
   - Jika 2+ tim → tandai §14 Dependensi.

9. Apakah epic ini saat ini diblokir oleh, atau memblokir, pekerjaan tim lain?
   - Jika ya → tandai §14 Dependensi.

10. Apakah ada ketergantungan pada API pihak ketiga, vendor, atau layanan eksternal?
    - Jika ya → tandai §14 Dependensi.

11. Apakah butuh sign-off eksekutif atau leadership sebelum rilis?
    - Jika ya → tandai §16 Peta Stakeholder.

---

### §0.5 — Konteks Rilis

12. Bagaimana epic ini akan diluncurkan?
    *(rilis penuh / feature flag / staged rollout / A/B test / regional)*
    - Jika staged, A/B, atau canary → tandai §15 Rencana Rilis.

13. Apakah butuh hal-hal berikut sebelum rilis?
    - Koordinasi marketing atau komunikasi
    - Sign-off legal atau compliance
    - Enablement customer support
    - Notifikasi partner atau vendor
    - Jika ada yang dikonfirmasi → tandai §15 Rencana Rilis.

---

### §0.6 — Kematangan Discovery

14. Di mana discovery untuk epic ini sekarang?
    *(baru ide / sudah validasi dengan pengguna / berbasis data / sudah ada desain / siap dibangun)*
    - Jika "baru ide": "Latar Belakang dan Hipotesis perlu dikembangkan lebih dalam — saya akan probe lebih jauh di sana. Siapkan untuk menggunakan banyak placeholder [TBD]."
    - Jika "sudah ada desain" atau "siap dibangun": "Apakah ada link desain atau dokumentasi? Share sekarang, saya akan lampirkan ke §11 Arsitektur."

15. Apakah ada artefak yang bisa dijadikan input?
    *(laporan riset, analisis kompetitif, dokumen OKR, Jira epic, file Figma, PRD sebelumnya)*
    - Jika ya → "Paste temuan kunci atau link-nya. Saya akan gunakan sebagai bukti nyata di §3 dan §6."

---

### §0.7 — Konfirmasi Seksi Opsional

Setelah §0.3–§0.5, terapkan logika trigger dan sajikan ringkasan:

> "Berdasarkan yang Anda ceritakan, berikut yang akan saya sertakan di luar 12 seksi inti:
> - §13 Risiko & Mitigasi: [Ya — alasan / Tidak]
> - §14 Dependensi: [Ya — alasan / Tidak]
> - §15 Rencana Rilis: [Ya — alasan / Tidak]
> - §16 Peta Stakeholder: [Ya — alasan / Tidak]
>
> Apakah sudah benar? Anda bisa tambah atau hapus sebelum kita mulai."

Tunggu konfirmasi. Jangan lanjut ke §1 sebelum pengguna setuju.

---

## §1 — Nama Epic

1. Apa nama epic ini? (maks 8 kata, noun phrase)
   - Probe jika terlalu panjang: "Bisa kita ringkas jadi 8 kata atau kurang?"
   - Probe jika verb phrase: "Mari ubah jadi noun phrase — apa yang sedang dibangun?"
2. Apakah nama ini muncul di Jira, Confluence, atau roadmap? Jika ya, konfirmasi persis sama.

---

## §2 — Status Dokumen

1. Apa status saat ini? (Draft / In Review / Disetujui / In Execution / Deprecated)
2. Siapa penulisnya? Siapa owner-nya saat ini?
3. Siapa yang perlu me-review? Sebutkan minimal satu engineering lead dan satu stakeholder bisnis.
   - Jika hanya nama tim: "Bisa sebutkan nama individu spesifik, bukan hanya tim?"
4. Siapa yang berwenang menyetujui PRD ini?
   - Jika hanya nama tim: "Bisa sebutkan orangnya secara spesifik?"

---

## §3 — Latar Belakang

1. Masalah apa yang kita selesaikan? Siapa yang terdampak, dan seberapa sering atau parah?
   - Probe jika terlalu solusi: "Mari tahan solusinya dulu — apa pain yang dialami pengguna atau bisnis?"

2. Bukti apa yang Anda punya bahwa masalah ini nyata?
   - Sumber yang diterima: data kuantitatif, riset pengguna, volume tiket support, NPS verbatim, data insiden.
   - Jika "kami pikir" atau "kami percaya" tanpa data: "Saya akan catat sebagai keyakinan tim yang belum divalidasi — `[Keyakinan tim — belum divalidasi: ___]`. Bisa tambahkan angka kasar atau referensi? Jika tidak, saya tandai `[TBD — perlu data]`."
   - **Aturan:** Jangan tulis klaim apapun sebagai fakta tanpa angka atau sumber bernama dari pengguna.

3. Apakah ini pernah dicoba sebelumnya? Ada pekerjaan sebelumnya yang perlu diakui?

4. Apa yang hilang jika kita tidak melakukan ini? Apa biaya tidak bertindak?

5. *(Jalankan hanya jika sinyal compliance dikonfirmasi di §0.3b)* Berdasarkan regulasi yang diidentifikasi — [daftar dari §0.3b] — apa persyaratan spesifik yang diimposekan pada inisiatif ini?
   - Jika tidak tahu: "Saya tulis `[TBD — perlu review compliance: nama-regulasi]` dan tandai sebagai open item."

---

## §4 — Tujuan & OKR

1. Seperti apa kesuksesan dalam satu kalimat? Harus outcome, bukan fitur.
   - Probe jika itu fitur: "Itu yang kita bangun — tapi hasil bisnis apa yang dihasilkan?"
2. Apa 2–3 key result yang terukur? Untuk masing-masing: metrik, baseline saat ini, target, dan timeline.
   - Jika tidak ada baseline: "Ada angka awal? Bahkan estimasi? Jika tidak, saya tulis `[TBD — baseline diperlukan]`."
3. OKR perusahaan mana yang didukung epic ini?

---

## §5 — Ruang Lingkup

1. Apa yang secara eksplisit masuk dalam ruang lingkup?
2. Apa yang secara eksplisit di luar ruang lingkup?
   - Untuk setiap item out-of-scope: "Apa alasannya — ditunda, dikerjakan tim lain, fase berikutnya?"
3. Platform mana yang dicakup?
4. Segmen pengguna mana yang berlaku?
5. Geografi atau region mana yang disertakan?
6. Apakah ini berfase? Jika ya, apa yang ada di Fase 1 vs. berikutnya?

---

## §6 — Hipotesis

1. Mari bangun hipotesis bersama. Lengkapi kalimat ini:
   "Kami percaya bahwa [melakukan X] untuk [pengguna/segmen] akan menghasilkan [outcome Y], karena [bukti/alasan Z]."
2. Seberapa yakin Anda dengan hipotesis ini? (Tinggi / Sedang / Rendah) — dan mengapa?
3. Apa yang akan memberitahu Anda hipotesis ini salah?
4. Siapa yang akan memiliki retrospektif pasca-rilis?

---

## §7 — User Journeys

1. Siapa saja persona pengguna utama yang terpengaruh oleh epic ini?
2. Untuk setiap persona: ceritakan alur mereka dari awal hingga akhir saat ini (kondisi sekarang, sebelum epic ini).
3. Bagaimana alur itu berubah setelah epic ini selesai?
4. Apa pain point terbesar di setiap journey saat ini?
5. Apakah ada journey edge case atau pengguna sekunder yang perlu dicakup?

---

## §8 — Kebutuhan Bisnis

1. Apa yang harus dilakukan sistem ini dari perspektif bisnis? (bukan bagaimana caranya)
   - Untuk setiap kebutuhan: "Apakah ini Wajib, Sebaiknya, Bisa, atau Tidak fase ini?"
2. Apakah ada kebutuhan performa? (mis. waktu respons, throughput)
3. Apakah ada kebutuhan keamanan atau compliance?
4. Apakah ada kebutuhan aksesibilitas?
5. Apakah ada kebutuhan ketersediaan atau uptime?
6. Bagaimana masing-masing akan diverifikasi atau diuji?

---

## §9 — Metrik Keberhasilan

1. Apa metrik utama yang dioptimalkan epic ini?
2. Adakah leading indicator — metrik yang bergerak lebih dulu?
3. Adakah lagging indicator — metrik yang mengkonfirmasi kesuksesan?
4. Untuk setiap metrik: baseline, target, metode pengukuran, owner (individu), tipe.
   - Jika tidak ada baseline: "Saya tulis `[TBD — baseline diperlukan]`. Siapa yang akan mendapatkan angka ini, dan kapan?"
   - **Aturan:** Jangan estimasi atau perkirakan baseline. Jika pengguna tidak berikan angka → `[TBD]`.
5. Metrik apa yang tidak boleh turun sebagai akibat epic ini? (guardrail)

---

## §10 — Nilai Bisnis

1. Seberapa besar nilai bisnis dari epic ini? (Tinggi / Sedang / Rendah)
2. Apa justifikasinya? Pertimbangkan dampak pendapatan, efisiensi operasional, atau risiko yang dimitigasi.
3. Fitur-fitur apa saja yang akan dihasilkan dari epic ini? (untuk daftar di §10)

---

## §11 — Arsitektur Teknis (untuk epic.md dan arch.md)

*Pertanyaan ringkasan untuk epic.md:*
1. Apa pendekatan teknis keseluruhan dalam 1–2 kalimat?
2. Apa tech stack utama yang akan digunakan?
3. Berapa estimasi ukuran keseluruhan epic? (S/M/L/XL — dan mengapa)

*Pertanyaan repo yang terdampak:*
4. Repo mana saja yang perlu diubah atau disesuaikan untuk epic ini?
   - Untuk setiap repo: nama repo, jenis perubahan, komponen/service yang terdampak, estimasi dampak
   - Jika tidak tahu nama repo pastinya: "Saya akan tulis `[TBD — konfirmasi nama repo di repos/config.json]`"
   - Probe jika ada perubahan API atau skema: "Apakah ada perubahan API contract atau skema database? Jika ya, saya akan tandai §14 Dependensi otomatis aktif."
   - Jika tidak ada perubahan kode: "Apakah epic ini murni konfigurasi atau operasional tanpa perubahan kode?"

*Pertanyaan detail untuk arch.md:*
5. Komponen sistem apa saja yang terlibat? (untuk diagram Mermaid)
6. Apa saja fitur tingkat tinggi yang akan dibangun?
7. Apa saja technical enablers baru yang dibutuhkan (service/library/infrastruktur baru)?
8. Apa keputusan arsitektur penting yang sudah diambil dan mengapa?
9. Apa risiko teknis utama?
10. Alternatif teknis apa yang sudah dipertimbangkan dan ditolak?

---

## §12 — FAQ

Sebelum bertanya secara terbuka, ajukan probe kontekstual berdasarkan jawaban §0:

- Jika domain pembayaran: "Adakah yang bertanya apa yang terjadi jika pembayaran gagal di tengah alur, atau jika retry menyebabkan duplikat charge?"
- Jika produk consumer: "Adakah yang bertanya bagaimana experience ini di OS lama atau perangkat low-end?"
- Jika tools internal: "Adakah yang bertanya bagaimana masa transisi saat alur lama dan baru berjalan bersamaan?"
- Jika 3+ tim terlibat: "Adakah yang bertanya siapa yang memiliki otoritas keputusan akhir saat tim tidak sepakat?"
- Jika ada compliance: "Sudahkah ada review legal atau compliance? Jika belum, apakah perlu jadi open item?"

Untuk setiap open item → tanyakan owner individu dan deadline resolusi.

---

## §13 — Risiko & Mitigasi (Opsional)

*(Jalankan hanya saat §13 dipicu di intake. Gunakan daftar regulasi dari §0.3b.)*

**Mulai dari risiko regulasi:**

Untuk setiap regulasi yang dikonfirmasi dari §0.3b:
1. Apa risiko spesifik yang ditimbulkan regulasi ini?
2. Seberapa besar kemungkinannya? (Tinggi / Sedang / Rendah)
3. Apa dampak bisnisnya jika terjadi?
4. Apa rencana mitigasinya?
5. Apa rencana kontingensinya jika risiko tetap terjadi?
6. Siapa owner risiko ini? (nama individu)
7. Apakah ini risiko pra-rilis atau pasca-rilis?

**Lalu risiko operasional & teknis:**
8. Apa 3 risiko operasional teratas yang bukan regulasi?
9. Untuk masing-masing: kemungkinan, dampak, mitigasi, kontingensi, owner, fase.
