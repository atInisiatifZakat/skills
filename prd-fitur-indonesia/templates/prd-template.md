# [Nama Fitur]

> **Satu kalimat: apa fitur ini dan mengapa penting.**

**Epic Parent:** `[Link ke docs/plan/{nama-epic}/epic.md]` *(hapus jika standalone)*

---

# SEKSI INTI

---

## 1. Nama Fitur

`[Nama]`

**Aturan:** Maksimal 8 kata · Noun phrase · Cocokkan dengan Jira/roadmap · Jika bagian dari Epic, link ke Epic parent

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
| **Approver** | [Nama] |
| **Tanggal Disetujui** | — |
| **Epic Parent** | [Nama Epic / Link] *(hapus jika standalone)* |

---

## 3. Latar Belakang

### Pernyataan Masalah
[Deskripsikan pain pengguna atau kebutuhan bisnis. Mulai dari masalah — bukan solusi. Jika ada Epic parent, cukup referensikan Epic PRD — tidak perlu ulangi konteks yang sama.]

### Bukti & Data
[Kutip data, riset, tiket support, atau feedback. Tidak ada klaim tanpa sumber.]

### Biaya Tidak Bertindak
[Apa yang hilang jika fitur ini tidak dibangun?]

### Konteks Regulasi
*(Sertakan hanya jika sinyal compliance dikonfirmasi. Hapus dan catat "Tidak ada persyaratan regulasi" jika tidak ada.)*

| Regulasi | Yurisdiksi | Kewajiban untuk Fitur Ini | Status |
|---|---|---|---|
| [Nama regulasi] | [Indonesia] | [Kewajiban spesifik] | `Dikonfirmasi` / `[TBD]` |

---

## 4. Tujuan & Metrik

### Tujuan Utama
> [Satu outcome statement. Bukan fitur, bukan deliverable — hasil nyata yang dihasilkan.]

### Metrik Utama

| Metrik | Tipe | Baseline | Target | Metode Pengukuran | Timeline | Owner |
|---|---|---|---|---|---|---|
| [Metrik] | Leading | [Nilai] | [Nilai] | [Metode] | [Tanggal] | [Nama] |
| [Metrik] | Lagging | [Nilai] | [Nilai] | [Metode] | [Tanggal] | [Nama] |

### Metrik Guardrail

| Metrik | Baseline | Ambang Peringatan | Owner |
|---|---|---|---|
| [Metrik] | [Nilai] | [Ambang] | [Nama] |

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

---

## 7. Kebutuhan

> User stories: *"Sebagai [peran spesifik], saya ingin [aksi], sehingga [manfaat]."*
> Acceptance criteria: Gherkin, minimal 2 skenario per story.

---

### Kebutuhan Fungsional

#### US-001 — [Judul]

**Prioritas:** `Wajib` / `Sebaiknya` / `Bisa` / `Tidak fase ini`

**User Story:**
> Sebagai **[peran spesifik]**, saya ingin **[aksi]**, sehingga **[manfaat]**.

**Acceptance Criteria:**
```gherkin
Skenario: [Happy path]
  Diberikan [prasyarat]
  Ketika [aksi]
  Maka [hasil yang diharapkan]

Skenario: [Edge case atau kegagalan]
  Diberikan [prasyarat]
  Ketika [aksi]
  Maka [hasil yang diharapkan]
```

**Dependensi:** [Tidak ada / US-XXX / Sistem eksternal]
**Catatan:** [Constraint, pertanyaan terbuka]

---

#### US-002 — [Judul]

**Prioritas:** `Wajib`

**User Story:**
> Sebagai **[peran spesifik]**, saya ingin **[aksi]**, sehingga **[manfaat]**.

**Acceptance Criteria:**
```gherkin
Skenario: [Happy path]
  Diberikan [prasyarat]
  Ketika [aksi]
  Maka [hasil yang diharapkan]

Skenario: [Edge case atau kegagalan]
  Diberikan [prasyarat]
  Ketika [aksi]
  Maka [hasil yang diharapkan]
```

**Dependensi:** [Tidak ada]
**Catatan:**

---

### Kebutuhan Non-Fungsional

| ID | Kategori | Kebutuhan | Prioritas | Metode Verifikasi |
|---|---|---|---|---|
| KNF-001 | Performa | [Kebutuhan] | `Wajib` | [Metode] |
| KNF-002 | Keamanan | [Kebutuhan] | `Wajib` | [Metode] |
| KNF-003 | Compliance | [Kebutuhan] | `Wajib` | [Metode] |

---

## 8. Solusi

### Pendekatan yang Diusulkan
[Deskripsi level produk — bagaimana cara kerjanya dari sudut pandang pengguna. Referensikan desain — jangan deskripsikan UI dalam prosa.]

### Artefak Desain

| Artefak | Link | Status |
|---|---|---|
| User Flow | [Link] | `Draft` / `Final` |
| Wireframe | [Link] | `Draft` / `Final` |
| Mockup | [Link] | `Draft` / `Final` |

### Peta Cakupan User Story

| User Story | Komponen Solusi | Referensi Desain |
|---|---|---|
| US-001 | [Komponen] | [Screen / link] |
| US-002 | [Komponen] | [Screen / link] |

### Alternatif yang Dipertimbangkan

| Opsi | Deskripsi | Alasan Ditolak |
|---|---|---|
| Opsi A | [Deskripsi] | [Alasan] |

### Keputusan & Constraint Teknis
[Keputusan arsitektur atau constraint yang membentuk solusi ini.]

### Repo Target

*Daftar repo spesifik yang perlu diubah untuk mengimplementasikan fitur ini.
Nama repo merujuk ke entri di `repos/config.json`.*

| Repo | Jenis Perubahan | File / Modul / Endpoint | Deskripsi Perubahan |
|---|---|---|---|
| [nama-repo-1] | `Fitur Baru` | [path/file atau nama modul] | [Apa yang perlu ditambahkan atau diubah] |
| [nama-repo-2] | `Modifikasi` | [path/file atau nama modul] | [Apa yang perlu ditambahkan atau diubah] |

**Jenis Perubahan yang valid:**
`Fitur Baru` · `Modifikasi` · `API Contract` · `Skema Database` · `Konfigurasi` · `Tidak Ada Perubahan Kode`

**Aturan:**
- Setiap repo yang disebutkan harus terdaftar di `repos/config.json`
- Jika fitur hanya menyentuh satu repo, tetap tulis tabelnya — eksplisit lebih baik dari implisit
- Repo dengan jenis `API Contract` atau `Skema Database` otomatis memicu §12 Dependensi
- Jika fitur ini adalah subset dari Epic, pastikan daftar repo di sini merupakan subset dari daftar repo di Epic PRD parent

---

## 9. Kriteria Penerimaan

*Checklist "Selesai" per user story — digunakan untuk sign-off QA sebelum rilis.*

### US-001 — [Judul]
- [ ] [Kondisi selesai 1]
- [ ] [Kondisi selesai 2]
- [ ] [Kondisi edge case tercakup]

### US-002 — [Judul]
- [ ] [Kondisi selesai 1]
- [ ] [Kondisi selesai 2]

### Sign-off QA
- [ ] Pending — [Nama QA Engineer]
- [ ] Disetujui — [Nama], [Tanggal]

---

## 10. FAQ

| # | Pertanyaan | Jawaban | Tanggal | Dijawab Oleh | Status |
|---|---|---|---|---|---|
| 1 | [Pertanyaan] | [Jawaban] | YYYY-MM-DD | [Nama] | `Selesai` |
| 2 | [Pertanyaan terbuka] | — | — | [Owner] | `Terbuka` |

---

---

# SEKSI OPSIONAL

---

## 11. Risiko & Mitigasi
*(Sertakan saat: ada sinyal compliance atau risiko teknis signifikan)*

### Risiko Regulasi
*(Wajib jika sinyal compliance dikonfirmasi.)*

| ID | Regulasi | Deskripsi Risiko | Kemungkinan | Dampak | Mitigasi | Kontingensi | Owner |
|---|---|---|---|---|---|---|---|
| RR-001 | [Regulasi] | [Risiko spesifik] | `T/S/R` | `T/S/R` | [Pencegahan] | [Respons] | [Nama] |

**Sign-off Compliance:**
- [ ] Pending
- [ ] Disetujui — [Nama], [Tanggal]

### Risiko Teknis & Operasional

| ID | Risiko | Kemungkinan | Dampak | Mitigasi | Kontingensi | Owner |
|---|---|---|---|---|---|---|
| R-001 | [Risiko] | `T/S/R` | `T/S/R` | [Pencegahan] | [Respons] | [Nama] |

---

## 12. Dependensi
*(Sertakan saat: bergantung pada API eksternal, tim lain, atau US dari fitur lain)*

| ID | Dependensi | Tim / Vendor | Yang Dibutuhkan | Tanggal Perkiraan | Dikonfirmasi | Risiko jika Terlambat |
|---|---|---|---|---|---|---|
| DEP-001 | [Nama] | [Tim] | [Deliverable] | YYYY-MM-DD | Ya / Tidak | [Dampak] |

---

*— Feature PRD Template — prd-fitur-indonesia skill —*
