# Aturan Seksi & Validasi — Epic PRD

---

## Aturan per Seksi

### §1 Nama Epic
**PELANGGARAN (blokir):** Nama > 8 kata · Verb phrase bukan noun phrase
**PERINGATAN:** Nama mengandung codename internal · Tidak cocok dengan Jira/roadmap

### §2 Status Dokumen
**PELANGGARAN:** Status bukan salah satu dari: Draft / In Review / Disetujui / In Execution / Deprecated · Status In Review tapi tidak ada reviewer bernama · Status Disetujui tapi tidak ada approver bernama atau tanggal persetujuan
**PERINGATAN:** Tidak ada nomor versi · Penulis dan owner orang yang sama tanpa penjelasan

### §3 Latar Belakang
**PELANGGARAN:** Seksi dibuka dengan solusi bukan masalah · Tidak ada sumber bukti atau data yang dikutip · Cost of inaction tidak ada · Sinyal compliance dikonfirmasi tapi tidak ada sub-seksi Konteks Regulasi
**PERINGATAN:** Seksi < 3 paragraf · Seksi > 5 paragraf tanpa link ke dokumen pendukung

### §4 Tujuan & OKR
**PELANGGARAN:** Tujuan utama mendeskripsikan fitur bukan outcome bisnis · KR tidak punya baseline atau target numerik · Tidak ada link OKR perusahaan · Lebih dari satu tujuan utama
**PERINGATAN:** Kata "meningkatkan" muncul di KR tanpa qualifier terukur · Timeline tidak ada di satu atau lebih KR

### §5 Ruang Lingkup
**PELANGGARAN:** Seksi Di Luar Ruang Lingkup tidak ada · Item out-of-scope tidak punya alasan · Platform atau segmen pengguna tidak dinyatakan
**PERINGATAN:** Daftar In Scope hanya satu item · Tidak ada geografi untuk inisiatif regional

### §6 Hipotesis
**PELANGGARAN:** Tidak mengikuti template "Kami percaya bahwa [X] untuk [segmen] akan menghasilkan [Y] karena [Z]" · Satu atau lebih komponen template hilang · Tidak ada kondisi falsifikasi · Hipotesis tidak falsifiable
**PERINGATAN:** Tingkat keyakinan ada tapi tidak ada alasan tertulis · Owner retrospektif tidak disebutkan

### §7 User Journeys
**PELANGGARAN:** Tidak ada journey sama sekali · Journey tidak menunjukkan alur end-to-end (hanya daftar langkah tanpa konteks sebelum/sesudah)
**PERINGATAN:** Hanya ada satu journey untuk epic yang kompleks · Pain point tidak disebutkan

### §8 Kebutuhan Bisnis
**PELANGGARAN:** Tidak ada kebutuhan fungsional sama sekali · Kebutuhan non-fungsional tidak ada verification method
**PERINGATAN:** Kebutuhan terlalu teknis (level implementasi, bukan level bisnis) · Tidak ada prioritas MoSCoW

### §9 Metrik Keberhasilan
**PELANGGARAN:** Tidak ada leading indicator · Tidak ada lagging indicator · Seksi guardrail tidak ada · Baris metrik manapun tidak punya: baseline, target, metode pengukuran, atau owner
**PERINGATAN:** Metrik menggunakan vanity measure tanpa metrik kualitas berpasangan · Owner metrik adalah nama tim bukan individu

### §10 Nilai Bisnis
**PELANGGARAN:** Estimasi nilai tidak ada · Tidak ada justifikasi
**PERINGATAN:** Daftar fitur kosong (mungkin masih terlalu awal)

### §11 Arsitektur Teknis
**PELANGGARAN:** Tidak ada pendekatan teknis sama sekali · Tidak ada estimasi ukuran · Seksi Repo yang Terdampak tidak ada sama sekali
**PERINGATAN:** Tech stack tidak disebutkan · Tidak ada diagram arsitektur (di arch.md) · Repo yang disebutkan tidak ada di `repos/config.json` · Perubahan `API Contract` atau `Skema Database` ada tapi §14 Dependensi tidak aktif

### §12 FAQ
**PELANGGARAN:** Seksi sama sekali tidak ada
**PERINGATAN:** Open item tidak punya owner · FAQ kosong untuk draft yang sudah lama

---

## Rubrik Skor Validasi

Jalankan validasi ini setelah generate (Mode 1) atau saat review (Mode 2).
Skor Epic PRD dari 100. Laporkan skor, pelanggaran, dan peringatan secara terpisah.

| Seksi | Poin Maks | Cara Menilai |
|---|---|---|
| §1 Nama Epic | 3 | 3 = noun phrase valid ≤8 kata. 1 = ada tapi bermasalah. 0 = tidak ada atau tidak valid. |
| §2 Status Dokumen | 5 | 5 = status valid + reviewer bernama + approver bernama + versi. Kurangi 2 untuk reviewer tidak bernama. Kurangi 1 per field yang hilang. |
| §3 Latar Belakang | 10 | 10 = pain-led, evidence dikutip, cost of inaction ada. Kurangi 3 jika solution-led. Kurangi 3 jika tidak ada evidence. Kurangi 2 jika tidak ada cost of inaction. |
| §4 Tujuan & OKR | 10 | 10 = outcome statement + KR numerik + link OKR. Kurangi 4 jika tujuan berbasis fitur. Kurangi 2 per KR yang tidak ada baseline/target. Kurangi 2 jika tidak ada link OKR. |
| §5 Ruang Lingkup | 8 | 8 = In + Out scope ada, Out punya alasan, platform/segmen ada. Kurangi 3 jika Out scope tidak ada. Kurangi 2 jika Out items tidak ada alasan. Kurangi 2 jika tidak ada platform/segmen. |
| §6 Hipotesis | 8 | 8 = template dipakai, falsifiable, confidence level + falsification condition. Kurangi 3 jika template tidak diikuti. Kurangi 3 jika tidak falsifiable. Kurangi 1 masing-masing untuk confidence atau falsification yang hilang. |
| §7 User Journeys | 8 | 8 = minimal 2 journey, alur end-to-end jelas, pain point disebutkan. Kurangi 4 jika tidak ada journey. Kurangi 2 per journey yang tidak ada pain point. |
| §8 Kebutuhan Bisnis | 12 | 12 = FR dan NFR ada, spesifik, prioritas ada, NFR punya verification. Kurangi 4 jika tidak ada FR. Kurangi 3 jika tidak ada NFR. Kurangi 1 per NFR tanpa verification method. |
| §9 Metrik Keberhasilan | 10 | 10 = leading + lagging + guardrail, semua baris lengkap. Kurangi 3 jika tidak ada leading. Kurangi 3 jika tidak ada lagging. Kurangi 2 jika tidak ada guardrail. Kurangi 1 per baris metrik yang tidak lengkap. |
| §10 Nilai Bisnis | 6 | 6 = estimasi ada + justifikasi ada + daftar fitur ada. Kurangi 2 per komponen yang hilang. |
| §11 Arsitektur Teknis | 10 | 10 = overview + tech stack + T-shirt size ada. Kurangi 3 jika tidak ada overview. Kurangi 3 jika tidak ada T-shirt size. Kurangi 2 jika tidak ada tech stack. Bonus +5 jika arch.md memiliki diagram Mermaid lengkap. |
| §12 FAQ | 5 | 5 = minimal satu entri, open items punya owner. 3 = ada tapi tidak ada owner untuk open items. 0 = tidak ada. |
| **Total** | **95** | *+5 bonus untuk diagram Mermaid di arch.md* |

---

## Band Skor

| Skor | Band | Rekomendasi |
|---|---|---|
| 90–100 | **Sangat Baik** | Siap pindah ke `In Review` |
| 75–89 | **Baik** | Perbaiki peringatan sebelum diedarkan |
| 60–74 | **Perlu Kerja** | Beberapa seksi tidak lengkap — revisi sebelum berbagi |
| < 60 | **Belum Siap** | Gap besar — jangan diedarkan |

---

## Format Laporan Validasi

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
LAPORAN VALIDASI EPIC PRD
Epic: [Nama]
Skor: [X]/100 — [Band]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PELANGGARAN (wajib diperbaiki sebelum In Review):
  §3 Latar Belakang — Tidak ada bukti yang dikutip untuk klaim masalah
  §9 Metrik — Tidak ada leading indicator

PERINGATAN (sebaiknya diperbaiki, tidak memblokir):
  §9 Metrik — Owner metrik adalah "Tim Data" — sebutkan individu
  §11 Arsitektur — Tidak ada diagram Mermaid di arch.md

SEKSI LULUS:
  ✓ §1 Nama Epic (3/3)
  ✓ §2 Status Dokumen (5/5)
  ...

Langkah Selanjutnya:
  1. Perbaiki semua PELANGGARAN di atas
  2. Jalankan ulang validasi: "review epic PRD saya"
  3. Atasi PERINGATAN sebelum diedarkan ke stakeholder
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Validasi Seksi Opsional

Jika seksi opsional ada, validasi terhadap section-rules di atas.
Tambahkan pelanggaran dan peringatan mereka ke laporan.
Seksi opsional tidak berkontribusi ke skor 100 poin,
tetapi pelanggarannya **memblokir** PRD dari status `Disetujui`.
