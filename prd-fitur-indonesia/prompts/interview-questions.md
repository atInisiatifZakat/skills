# Pertanyaan Wawancara — Feature PRD

Tanyakan secara percakapan — satu per satu, tidak sekaligus.

---

## §0 — Intake (Jalankan Sebelum Segalanya)

### §0.0 — Base Path (Tanyakan Pertama Kali)

Sebelum pertanyaan apapun, tanyakan:

> *"Di mana dokumen PRD disimpan di project ini?"*

Berikan contoh pilihan umum:
- `docs/plan` — konvensi standar
- `prd/nama-app` — jika project multi-app seperti `prd/mobile-app` atau `prd/backend`
- Path lain sesuai konvensi tim

**Aturan:**
- Catat jawaban sebagai `{base-path}` — gunakan di semua instruksi penyimpanan selanjutnya
- Jika pengguna tidak tahu → gunakan `docs/plan` sebagai default dan konfirmasi
- Jika Claude Code aktif → cek dulu apakah folder `prd/` atau `docs/plan/` sudah ada di filesystem
- **Jika ada Epic PRD parent** → gunakan base-path yang sama dengan Epic PRD tersebut, tidak perlu tanya ulang
- Path output final:
  - Dengan Epic: `{base-path}/{nama-epic}/{nama-fitur}/prd.md`
  - Standalone: `{base-path}/standalone/{nama-fitur}/prd.md`

---

### §0.1 — Identitas & Konteks Epic

1. Apa nama fitur ini?
2. Satu kalimat: apa yang dilakukan fitur ini dan untuk siapa?
3. Apakah fitur ini bagian dari Epic yang sudah ada?
   - Jika ya: "Apakah Anda punya link atau isi dari Epic PRD-nya? Saya akan gunakan sebagai konteks sehingga kita tidak perlu mengulang latar belakang yang sama."
   - Jika tidak: "Fitur ini akan diperlakukan sebagai standalone."

### §0.2 — Platform & Pengguna

4. Platform mana yang tersentuh? *(iOS / Android / Web / API / Semua)*
5. Siapa pengguna utama fitur ini? *(konsumen / staf internal / klien B2B / developer)*

### §0.3 — Sinyal Compliance

6. Apakah fitur ini menyentuh hal-hal berikut? *(konfirmasi satu per satu)*
   - Transaksi keuangan atau alur pembayaran → §11 Risiko otomatis aktif
   - Verifikasi identitas (eKYC, KYB, biometrik) → §11 Risiko otomatis aktif
   - Data pribadi atau PII di luar field profil dasar → §11 Risiko otomatis aktif
   - Persyaratan regulasi (OJK, UU PDP, GDPR, PCI-DSS)
   - Jika ada yang dikonfirmasi → jalankan identifikasi regulasi (tabel di bawah)

   | Sinyal | Regulasi yang Perlu Dipertimbangkan |
   |---|---|
   | Pembayaran / transaksi keuangan | PCI-DSS, OJK BI-SNAP, Peraturan BI |
   | eKYC / KYB | OJK POJK 12/2018, FATF AML/CFT |
   | Kredit / BNPL | OJK POJK 35/2018 |
   | Data pribadi / PII | UU PDP Indonesia, PDPA |
   | Data biometrik | UU PDP Pasal data sensitif |

### §0.4 — Dependensi

7. Apakah fitur ini bergantung pada API pihak ketiga, tim lain, atau fitur lain yang belum selesai?
   - Jika ya → §12 Dependensi otomatis aktif.

### §0.5 — Konfirmasi Seksi Opsional

> "Berdasarkan yang Anda ceritakan, berikut yang akan saya sertakan di luar 10 seksi inti:
> - §11 Risiko & Mitigasi: [Ya — alasan / Tidak]
> - §12 Dependensi: [Ya — alasan / Tidak]
>
> Sudah benar?"

---

## §1 — Nama Fitur

1. Apa nama fitur ini? (maks 8 kata, noun phrase)
2. Apakah nama ini muncul di Jira atau roadmap? Konfirmasi persis sama.

---

## §2 — Status Dokumen

1. Status saat ini? (Draft / In Review / Disetujui / In Execution / Deprecated)
2. Siapa penulis dan owner-nya?
3. Siapa yang perlu me-review? Minimal satu engineering lead dan satu stakeholder bisnis — nama individu.
4. Siapa yang berwenang menyetujui? Nama individu, bukan tim.

---

## §3 — Latar Belakang

1. Masalah apa yang fitur ini selesaikan? Siapa yang terdampak?
   - Probe jika terlalu solusi: "Mari tahan solusinya dulu — apa pain yang dialami pengguna?"
   - Jika ada Epic parent: "Bagian mana dari masalah Epic yang spesifik diselesaikan oleh fitur ini?"

2. Bukti apa yang mendukung bahwa masalah ini nyata?
   - Jika "kami pikir" tanpa data: "Saya catat sebagai `[Keyakinan tim — belum divalidasi: ___]`."
   - **Aturan:** Jangan tulis klaim apapun sebagai fakta tanpa angka atau sumber bernama.

3. Apa yang hilang jika fitur ini tidak dibangun?

4. *(Jika sinyal compliance dikonfirmasi)* Persyaratan regulasi spesifik apa yang diimposekan?
   - Jika tidak tahu: "Saya tulis `[TBD — perlu review compliance: nama-regulasi]`."

---

## §4 — Tujuan & Metrik

1. Seperti apa kesuksesan fitur ini dalam satu kalimat? (outcome, bukan fitur)
2. Apa metrik utama yang bergerak lebih dulu? (leading indicator)
3. Apa metrik yang mengkonfirmasi kesuksesan jangka panjang? (lagging indicator)
4. Untuk setiap metrik: baseline, target, metode pengukuran, owner (individu), timeline.
   - Jika tidak ada baseline: "Saya tulis `[TBD — baseline diperlukan]`. Siapa yang akan mendapatkan angka ini?"
   - **Aturan:** Jangan estimasi baseline. Jika tidak ada angka → `[TBD]`.
5. Metrik apa yang tidak boleh turun? (guardrail)

---

## §5 — Ruang Lingkup

1. Apa yang secara eksplisit masuk dalam ruang lingkup fitur ini?
2. Apa yang secara eksplisit di luar ruang lingkup?
   - Untuk setiap item out-of-scope: "Alasannya apa — ditunda, dikerjakan tim lain, fase berikutnya?"
3. Platform mana yang dicakup?
4. Segmen pengguna mana yang berlaku?
5. Apakah ini berfase?

---

## §6 — Hipotesis

1. Lengkapi kalimat ini:
   "Kami percaya bahwa [melakukan X] untuk [pengguna/segmen] akan menghasilkan [outcome Y], karena [bukti/alasan Z]."
2. Seberapa yakin? (Tinggi / Sedang / Rendah) — dan mengapa?
3. Apa yang akan memberitahu Anda hipotesis ini salah?

---

## §7 — Kebutuhan (User Stories)

Untuk setiap user story, tanyakan urutan berikut. Ulangi sampai pengguna bilang "cukup".

1. Siapa pengguna dalam story ini? (peran spesifik — bukan hanya "pengguna")
2. Apa yang ingin mereka lakukan?
3. Mengapa — manfaat apa yang mereka dapat?
   → Susun: "Sebagai [peran], saya ingin [aksi], sehingga [manfaat]."
4. Apa happy path — skenario sukses standar?
   → Tulis sebagai Gherkin: Diberikan / Ketika / Maka
5. Apa edge case atau skenario kegagalan?
   → Tulis sebagai Gherkin: Diberikan / Ketika / Maka
6. Prioritasnya? (Wajib / Sebaiknya / Bisa / Tidak fase ini)
7. Apakah story ini bergantung pada story lain atau sistem eksternal?
8. Ada catatan, constraint, atau pertanyaan terbuka spesifik untuk story ini?

Setelah setiap story, sebelum tanya lebih lanjut, tampilkan gap check ini:
> "Sebelum lanjut — sudahkah kita cover:
> 1. State error atau kegagalan untuk flow ini?
> 2. Versi admin atau staf internal dari flow ini?
> 3. Variasi berdasarkan role atau level permission?
> 4. Skenario abandonmen atau penyelesaian parsial?"

Lalu tanya: "Ada story lagi? Sebelum jawab tidak — sudahkah kita cover alur onboarding atau first-run?"

Lanjut sampai pengguna bilang "tidak ada lagi story" setelah gap-check prompt.

**Kebutuhan Non-Fungsional:**
9. Ada kebutuhan performa? (mis. waktu respons, throughput)
10. Ada kebutuhan keamanan atau compliance?
11. Ada kebutuhan aksesibilitas?
12. Ada rate limit, API quota, atau constraint konkurensi?
13. Ada kebutuhan retensi atau penghapusan data?
    - Jika fitur tangani PII (dari §0.3): tanyakan ini tanpa syarat.

---

## §8 — Solusi

1. Bagaimana solusi yang diusulkan bekerja dari sudut pandang pengguna?

2. Apakah ada artefak desain — flow, wireframe, mockup? Share link-nya.
   - Jika tidak ada: "Saya tulis `[Belum ada link desain — status: Draft]`."
   - **Aturan:** Jangan deskripsikan UI dalam prosa jika tidak ada link.

3. Untuk setiap user story, bagian solusi mana yang mengatasinya?

4. Alternatif apa yang sudah dipertimbangkan dan mengapa ditolak?
   - Probe jika tidak ada: "Selalu ada trade-off — apa yang dipertimbangkan dan diputuskan tidak dipakai?"

5. Ada constraint teknis atau keputusan arsitektur yang membentuk solusi ini?

6. Repo mana saja yang perlu diubah untuk mengimplementasikan fitur ini?
   - Untuk setiap repo: nama repo (sesuai `repos/config.json`), jenis perubahan, file/modul/endpoint yang terdampak, deskripsi singkat perubahannya
   - Jika ada Epic parent: "Apakah repo-repo ini subset dari yang sudah disebutkan di Epic PRD?"
   - Jika tidak tahu nama repo pastinya: "Saya tulis `[TBD — konfirmasi nama repo di repos/config.json]`"
   - Probe jika ada perubahan API atau skema database: "Apakah ada perubahan API contract atau skema database? Jika ya, §12 Dependensi otomatis aktif."
   - **Aturan:** Jangan skip pertanyaan ini walau fitur terlihat kecil — satu fitur bisa menyentuh banyak repo sekaligus

---

## §9 — Kriteria Penerimaan

1. Untuk setiap user story, kondisi "selesai" apa yang perlu diverifikasi QA?
   - Pastikan setiap kondisi bisa diuji secara objektif.
2. Siapa QA engineer yang akan sign-off?

---

## §10 — FAQ

Sebelum bertanya secara terbuka, ajukan probe kontekstual:

- Jika domain pembayaran: "Adakah yang bertanya apa yang terjadi jika pembayaran gagal di tengah alur?"
- Jika produk consumer: "Adakah yang bertanya tampilan di OS lama atau perangkat low-end?"
- Jika tools internal: "Adakah yang bertanya masa transisi saat alur lama dan baru berjalan bersamaan?"
- Jika ada compliance: "Sudahkah ada review legal? Jika belum, apakah perlu jadi open item?"

Untuk open item → tanya owner individu dan deadline resolusi.

---

## §11 — Risiko & Mitigasi (Opsional)

Untuk setiap risiko regulasi yang dikonfirmasi:
1. Risiko spesifik apa yang ditimbulkan regulasi ini untuk fitur ini?
2. Kemungkinan (Tinggi / Sedang / Rendah) dan dampak?
3. Rencana mitigasi? (pencegahan sebelum rilis)
4. Rencana kontingensi? (respons jika tetap terjadi)
5. Siapa owner-nya? (nama individu)

Untuk risiko teknis:
6. Apa 1–3 risiko teknis utama fitur ini?
7. Untuk masing-masing: kemungkinan, dampak, mitigasi, kontingensi, owner.
