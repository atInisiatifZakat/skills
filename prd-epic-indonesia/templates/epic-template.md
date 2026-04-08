# [Nama Epic]

> **Satu kalimat: apa inisiatif ini dan mengapa penting.**

---

## Cara Menggunakan Template Ini

- Isi semua **Seksi Inti**. Jangan skip atau tinggalkan placeholder.
- Tambahkan **Seksi Opsional** hanya saat kondisi pemicu terpenuhi.
- Dokumen ini adalah satu-satunya sumber kebenaran untuk inisiatif ini.
- Jika tidak ditulis di sini, maka tidak ada.

---

# SEKSI INTI

---

## 1. Nama Epic

`[Nama]`

**Aturan:** Maksimal 8 kata · Noun phrase · Cocokkan dengan Jira/roadmap · Jika berfase, tambahkan nomor fase

---

## 2. Status Dokumen

| Field | Detail |
|---|---|
| **Status** | `Draft` |
| **Versi** | v0.1 |
| **Terakhir Diperbarui** | YYYY-MM-DD |
| **Penulis** | [Nama] |
| **Owner** | [Nama] |
| **Reviewer** | [Engineering Lead], [Stakeholder Bisnis] |
| **Approver** | [Nama], [Nama] |
| **Tanggal Disetujui** | — |

**Aturan:** Status hanya: Draft → In Review → Disetujui → In Execution → Deprecated · Reviewer harus nama individu, bukan tim · Tidak ada development sebelum Disetujui

---

## 3. Latar Belakang

### Pernyataan Masalah
[Deskripsikan pain pengguna atau kebutuhan bisnis. Siapa yang terdampak? Seberapa sering? Seberapa parah? Mulai dari masalah — bukan solusi.]

### Bukti & Data
[Kutip data, riset, tiket support, feedback NPS, atau insiden. Tidak ada klaim tanpa sumber.]

### Konteks & Riwayat
[Pekerjaan sebelumnya, percobaan yang gagal, atau inisiatif terkait. Tautkan ke PRD atau post-mortem sebelumnya.]

### Biaya Tidak Bertindak
[Apa yang hilang jika ini tidak dikejar — pendapatan, pengguna, compliance, posisi kompetitif?]

### Konteks Regulasi
*(Sertakan sub-seksi ini saat sinyal compliance dikonfirmasi di intake. Hapus dan catat eksplisit "Tidak ada persyaratan regulasi yang teridentifikasi" jika tidak ada.)*

| Regulasi | Yurisdiksi | Kewajiban Utama untuk Inisiatif Ini | Status |
|---|---|---|---|
| [Nama regulasi, mis. UU PDP Pasal 20] | [Indonesia] | [Kewajiban spesifik] | `Dikonfirmasi` / `[TBD — perlu review compliance]` |

**Owner Review Compliance:** [Nama individu]
**Deadline Review:** [Tanggal atau `[TBD]`]

---

## 4. Tujuan & OKR

### Tujuan Utama
> [Satu pernyataan outcome. Tidak ada deskripsi fitur. Tidak ada "meningkatkan" tanpa qualifier terukur.]

### Key Results

| # | Key Result | Baseline | Target | Timeline |
|---|---|---|---|---|
| KR1 | [Outcome terukur] | [Nilai] | [Nilai] | [Tanggal] |
| KR2 | [Outcome terukur] | [Nilai] | [Nilai] | [Tanggal] |
| KR3 | [Outcome terukur] | [Nilai] | [Nilai] | [Tanggal] |

### Keselarasan OKR Perusahaan
[Sebutkan OKR perusahaan induk. Jika tidak ada, eskalasi sebelum lanjut.]

---

## 5. Ruang Lingkup

### Dalam Ruang Lingkup
- [Item]
- [Item]

### Di Luar Ruang Lingkup

| Item | Alasan | Rencana ke Depan |
|---|---|---|
| [Item] | [Alasan] | [Rencana atau tidak ada] |

### Cakupan Platform & Segmen

| Dimensi | Cakupan |
|---|---|
| **Platform** | [iOS / Android / Web / API / Semua] |
| **Segmen Pengguna** | [Segmen] |
| **Geografi** | [Geografi] |
| **Fase** | [Fase] |

---

## 6. Hipotesis

> *"Kami percaya bahwa **[X]** untuk **[segmen]** akan menghasilkan **[Y]**, karena **[Z]**."*

[Tulis hipotesis di sini.]

### Tingkat Keyakinan
`Tinggi` / `Sedang` / `Rendah`

**Alasan:** [Mengapa tingkat ini?]

### Kondisi Falsifikasi
[Hasil apa yang akan membuktikan hipotesis ini salah?]

### Rencana Pembelajaran Pasca-Rilis
[Siapa yang memiliki retrospektif? Kapan?]

---

## 7. User Journeys

*Deskripsikan alur besar pengguna dari awal hingga akhir — bukan user story individual. Gunakan narasi atau diagram alur sederhana.*

### Journey 1: [Nama Journey]
**Persona:** [Siapa pengguna ini?]

[Langkah 1] → [Langkah 2] → [Langkah 3] → [Outcome]

**Pain point saat ini:** [Apa yang susah sekarang?]
**Kondisi ideal setelah epic ini:** [Bagaimana experience-nya nanti?]

### Journey 2: [Nama Journey]
*(Ulangi format di atas untuk setiap journey utama)*

---

## 8. Kebutuhan Bisnis

### Kebutuhan Fungsional
*(Level bisnis — apa yang sistem harus lakukan, bukan bagaimana caranya)*

- **KF-001:** [Sistem harus...] — Prioritas: `Wajib` / `Sebaiknya` / `Bisa` / `Tidak fase ini`
- **KF-002:** [Sistem harus...]
- **KF-003:** [Sistem harus...]

### Kebutuhan Non-Fungsional

| ID | Kategori | Kebutuhan | Prioritas | Metode Verifikasi |
|---|---|---|---|---|
| KNF-001 | Performa | [Kebutuhan] | `Wajib` | [Metode] |
| KNF-002 | Keamanan | [Kebutuhan] | `Wajib` | [Metode] |
| KNF-003 | Compliance | [Kebutuhan] | `Wajib` | [Metode] |

---

## 9. Metrik Keberhasilan

### Metrik Utama

| Metrik | Tipe | Baseline | Target | Metode Pengukuran | Timeline | Owner |
|---|---|---|---|---|---|---|
| [Metrik] | Leading | [Nilai] | [Nilai] | [Metode] | [Tanggal] | [Nama] |
| [Metrik] | Lagging | [Nilai] | [Nilai] | [Metode] | [Tanggal] | [Nama] |

### Metrik Guardrail

| Metrik | Baseline | Ambang Peringatan | Metode Pengukuran | Owner |
|---|---|---|---|---|
| [Metrik] | [Nilai] | [Ambang] | [Metode] | [Nama] |

---

## 10. Nilai Bisnis

### Estimasi Nilai
`Tinggi` / `Sedang` / `Rendah`

**Justifikasi:** [Mengapa estimasi ini? Dampak pendapatan, efisiensi, atau risiko yang dimitigasi?]

### Daftar Fitur yang Dihasilkan
*Fitur-fitur berikut akan dijabarkan masing-masing dalam dokumen prd-fitur-indonesia:*

| Fitur | Prioritas | Path PRD |
|---|---|---|
| [Nama Fitur 1] | `Wajib` | `{base-path}/{nama-epic}/{nama-fitur-1}/prd.md` |
| [Nama Fitur 2] | `Sebaiknya` | `{base-path}/{nama-epic}/{nama-fitur-2}/prd.md` |

---

## 11. Arsitektur Teknis (Ringkasan)

*Detail lengkap ada di `arch.md`. Seksi ini hanya ringkasan untuk pembaca non-teknis.*

### Pendekatan Teknis
[1–2 kalimat tentang pendekatan arsitektur keseluruhan.]

### Tech Stack Utama
[Daftar teknologi kunci yang akan digunakan.]

### Estimasi Ukuran
`S` / `M` / `L` / `XL`

**Alasan:** [Mengapa ukuran ini?]

### Repo yang Terdampak

*Daftar semua repo yang perlu diubah, ditambahkan, atau disesuaikan untuk merealisasikan epic ini.
Nama repo merujuk ke daftar repo yang dikenal di project ini.*

| Repo | Jenis Perubahan | Komponen / Service | Estimasi Dampak | Catatan |
|---|---|---|---|---|
| [nama-repo-1] | `Fitur Baru` | [Service/modul yang diubah] | `Besar` / `Sedang` / `Kecil` | [Catatan singkat] |
| [nama-repo-2] | `Modifikasi` | [Service/modul yang diubah] | `Sedang` | [Catatan singkat] |
| [nama-repo-3] | `API Contract` | [Endpoint / interface baru] | `Kecil` | [Catatan singkat] |

**Jenis Perubahan yang valid:**
`Fitur Baru` · `Modifikasi` · `API Contract` · `Skema Database` · `Konfigurasi` · `Infrastruktur` · `Tidak Ada Perubahan Kode`

**Aturan:**
- Setiap repo yang disebutkan harus konsisten dengan yang digunakan di seluruh dokumen PRD project ini
- Repo dengan jenis `API Contract` atau `Skema Database` otomatis memicu §14 Dependensi
- Jika epic tidak menyentuh repo manapun secara langsung, tulis: *"Epic ini tidak memerlukan perubahan kode — hanya konfigurasi atau operasional."*

---

## 12. FAQ

| # | Pertanyaan | Jawaban | Tanggal | Dijawab Oleh | Status |
|---|---|---|---|---|---|
| 1 | [Pertanyaan] | [Jawaban] | YYYY-MM-DD | [Nama] | `Selesai` |
| 2 | [Pertanyaan terbuka] | — | — | [Owner] | `Terbuka` |

---

---

# SEKSI OPSIONAL

> Sertakan hanya saat kondisi pemicu terpenuhi. Hapus blok instruksi ini di dokumen final.

---

## 13. Risiko & Mitigasi
*(Sertakan saat: ada sinyal compliance, pembayaran, data diatur, atau dependensi lintas tim)*

### Risiko Regulasi
*(Wajib saat sinyal compliance dikonfirmasi. Satu baris per regulasi yang dikonfirmasi.)*

| ID | Regulasi | Deskripsi Risiko | Kemungkinan | Dampak | Mitigasi | Kontingensi | Owner | Fase |
|---|---|---|---|---|---|---|---|---|
| RR-001 | [Nama regulasi] | [Risiko spesifik] | `T/S/R` | `T/S/R` | [Pencegahan] | [Respons jika terjadi] | [Nama] | Pra-rilis |

**Sign-off Legal/Compliance atas Risiko Regulasi:**
- [ ] Pending — [Nama, Jabatan yang perlu review]
- [ ] Disetujui — [Nama, Jabatan], [Tanggal]

### Risiko Operasional & Teknis

| ID | Risiko | Kemungkinan | Dampak | Mitigasi | Kontingensi | Owner | Fase |
|---|---|---|---|---|---|---|---|
| R-001 | [Risiko] | `T/S/R` | `T/S/R` | [Pencegahan] | [Respons] | [Nama] | Pra-rilis |

---

## 14. Dependensi
*(Sertakan saat: bergantung pada atau memblokir tim lain, atau mengandalkan sistem pihak ketiga)*

### Dependensi Upstream

| ID | Dependensi | Tim / Vendor | Yang Dibutuhkan | Tanggal Perkiraan | Dikonfirmasi | Risiko jika Terlambat | Eskalasi |
|---|---|---|---|---|---|---|---|
| DEP-001 | [Nama] | [Tim] | [Deliverable] | YYYY-MM-DD | Ya / Tidak | [Dampak] | [Jalur] |

### Dependensi Downstream

| ID | Inisiatif Dependen | Tim | Yang Mereka Butuhkan | Tanggal Perkiraan |
|---|---|---|---|---|
| DEP-D01 | [Nama] | [Tim] | [Yang mereka butuhkan] | YYYY-MM-DD |

---

## 15. Rencana Rilis
*(Sertakan saat: rollout terkoordinasi, staged release, atau compliance gate diperlukan)*

### Strategi Rollout
`[Rilis Penuh / Feature Flag / Rollout Bertahap / Regional / Invite-Only]`

[Alasan strategi yang dipilih.]

### Kriteria Go / No-Go

| Tahap | Kondisi Go | Kondisi No-Go | Pengambil Keputusan |
|---|---|---|---|
| Internal | [Kondisi] | [Kondisi] | [Nama] |
| Rollout Terbatas | [Kondisi] | [Kondisi] | [Nama] |
| Rilis Penuh | [Kondisi] | [Kondisi] | [Nama] |

### Rencana Komunikasi

| Audiens | Channel | Ringkasan Pesan | Timing | Owner |
|---|---|---|---|---|
| Tim internal | [Channel] | [Ringkasan] | [Timing] | [Nama] |
| Pengguna akhir | [Channel] | [Ringkasan] | [Timing] | [Nama] |
| Customer support | [Channel] | [Yang perlu ditangani] | [X hari sebelum] | [Nama] |

### Rencana Rollback

| Field | Detail |
|---|---|
| **Pemicu Rollback** | [Kondisi observable spesifik] |
| **Pengambil Keputusan** | [Nama] |
| **Waktu Eksekusi** | [Durasi] |
| **Langkah Rollback** | [Langkah-langkah atau link runbook] |

**Launch DRI:** [Nama — bukan PM]

---

## 16. Peta Stakeholder
*(Sertakan saat: >3 tim terlibat, butuh visibilitas eksekutif, atau ada pihak eksternal)*

### Matriks RACI

| Nama | Jabatan / Peran | Tim / Org | RACI | Frekuensi Komunikasi | Channel |
|---|---|---|---|---|---|
| [Nama] | [Jabatan] | [Tim] | `A` | Mingguan | [Channel] |
| [Nama] | [Jabatan] | [Tim] | `R` | Harian saat eksekusi | [Channel] |
| [Nama] | [Jabatan] | [Tim] | `C` | Saat milestone | [Channel] |
| [Nama] | [Jabatan] | [Tim] | `I` | Saat milestone | [Channel] |

> `R` Responsible · `A` Accountable · `C` Consulted · `I` Informed
> Tepat satu Accountable. Nama individu, bukan tim.

---

*— Epic PRD Template — prd-epic-indonesia skill —*
