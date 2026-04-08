# Aturan Seksi & Validasi — Feature PRD

---

## Aturan per Seksi

### §1 Nama Fitur
**PELANGGARAN:** Nama > 8 kata · Verb phrase bukan noun phrase
**PERINGATAN:** Tidak cocok dengan Jira/roadmap · Tidak ada link ke Epic parent padahal bagian dari Epic

### §2 Status Dokumen
**PELANGGARAN:** Status bukan salah satu dari: Draft / In Review / Disetujui / In Execution / Deprecated · Status In Review tapi reviewer tidak bernama individu · Status Disetujui tapi tidak ada approver bernama atau tanggal persetujuan
**PERINGATAN:** Tidak ada nomor versi

### §3 Latar Belakang
**PELANGGARAN:** Seksi dibuka dengan solusi bukan masalah · Tidak ada bukti atau sumber yang dikutip · Cost of inaction tidak ada · Sinyal compliance dikonfirmasi tapi tidak ada sub-seksi Konteks Regulasi
**PERINGATAN:** Seksi < 2 paragraf (mungkin kurang berkembang)

### §4 Tujuan & Metrik
**PELANGGARAN:** Tujuan utama mendeskripsikan fitur bukan outcome · KR/metrik tidak punya baseline atau target numerik · Tidak ada leading indicator · Tidak ada lagging indicator · Tidak ada guardrail · Baris metrik manapun tidak lengkap fieldnya
**PERINGATAN:** Owner metrik adalah nama tim bukan individu · Timeline tidak ada

### §5 Ruang Lingkup
**PELANGGARAN:** Seksi Di Luar Ruang Lingkup tidak ada · Item out-of-scope tidak punya alasan · Platform tidak dinyatakan
**PERINGATAN:** In Scope hanya satu item

### §6 Hipotesis
**PELANGGARAN:** Tidak mengikuti template "Kami percaya bahwa [X] untuk [segmen] akan menghasilkan [Y] karena [Z]" · Satu atau lebih komponen template hilang · Tidak ada kondisi falsifikasi · Hipotesis tidak falsifiable
**PERINGATAN:** Tingkat keyakinan ada tapi tidak ada alasan tertulis

### §7 Kebutuhan (User Stories)
**PELANGGARAN:** Kebutuhan apapun tidak ditulis sebagai user story · Peran pengguna hanya "pengguna" tanpa spesifikasi · User story punya < 2 skenario Gherkin · Klausa "Maka" pada skenario Gherkin ambigu atau tidak deterministik · User story tidak punya prioritas MoSCoW · Kebutuhan non-fungsional tidak punya verification method
**PERINGATAN:** User story tampak terlalu besar untuk satu sprint · Dependensi antar story dideskripsikan dalam prosa bukan di field Dependensi

### §8 Solusi
**PELANGGARAN:** Tidak ada link artefak desain sama sekali · Tidak ada peta cakupan user story · Tidak ada seksi alternatif yang dipertimbangkan · Alternatif ada tapi tidak ada alasan penolakan · Seksi Repo Target tidak ada sama sekali
**PERINGATAN:** Link desain ada tapi masih Draft · Satu atau lebih user story dari §7 tidak ada di peta cakupan · Repo yang disebutkan tidak konsisten dengan dokumen PRD lain di project · Perubahan `API Contract` atau `Skema Database` ada tapi §12 Dependensi tidak aktif · Fitur bagian dari Epic tapi repo di sini tidak subset dari repo di Epic PRD parent

### §9 Kriteria Penerimaan
**PELANGGARAN:** Seksi tidak ada sama sekali · Tidak ada checklist untuk satu atau lebih user story Wajib
**PERINGATAN:** Sign-off QA tidak ada · Kondisi penerimaan tidak bisa diuji secara objektif

### §10 FAQ
**PELANGGARAN:** Seksi sama sekali tidak ada
**PERINGATAN:** Open item tidak punya owner · FAQ kosong untuk draft yang sudah lama

---

## Rubrik Skor Validasi

| Seksi | Poin Maks | Cara Menilai |
|---|---|---|
| §1 Nama Fitur | 3 | 3 = noun phrase valid ≤8 kata. 1 = ada tapi bermasalah. 0 = tidak ada. |
| §2 Status Dokumen | 5 | 5 = status valid + reviewer bernama + approver bernama + versi. Kurangi 1 per field yang hilang. |
| §3 Latar Belakang | 8 | 8 = pain-led, evidence dikutip, cost of inaction ada. Kurangi 3 jika solution-led. Kurangi 3 jika tidak ada evidence. Kurangi 2 jika tidak ada cost of inaction. |
| §4 Tujuan & Metrik | 12 | 12 = tujuan outcome + leading + lagging + guardrail + semua baris lengkap. Kurangi 3 jika tujuan berbasis fitur. Kurangi 2 per tipe metrik yang hilang. Kurangi 1 per baris tidak lengkap. |
| §5 Ruang Lingkup | 8 | 8 = In + Out ada, Out punya alasan, platform ada. Kurangi 3 jika Out tidak ada. Kurangi 2 jika Out tidak ada alasan. Kurangi 2 jika platform tidak ada. |
| §6 Hipotesis | 8 | 8 = template dipakai, falsifiable, confidence + falsification ada. Kurangi 3 jika template tidak diikuti. Kurangi 3 jika tidak falsifiable. Kurangi 1 masing-masing untuk yang hilang. |
| §7 Kebutuhan | 25 | 25 = semua user story dengan role spesifik, ≥2 Gherkin per story, MoSCoW, NFR dengan verifikasi. Kurangi 3 per story yang bukan user story. Kurangi 2 per story dengan role generik. Kurangi 3 per story dengan < 2 skenario Gherkin. Kurangi 1 per story tanpa MoSCoW. Kurangi 2 per NFR tanpa verification method. |
| §8 Solusi | 10 | 10 = design link ada, peta cakupan lengkap, alternatif ada dengan alasan. Kurangi 3 jika tidak ada design link. Kurangi 3 jika tidak ada peta cakupan. Kurangi 2 jika tidak ada alternatif. Kurangi 1 per alternatif tanpa alasan. |
| §9 Kriteria Penerimaan | 11 | 11 = checklist ada per US Wajib, kondisi bisa diuji, sign-off QA ada. Kurangi 4 jika seksi tidak ada. Kurangi 2 per US Wajib tanpa checklist. Kurangi 2 jika tidak ada sign-off QA. Kurangi 1 per kondisi yang tidak bisa diuji. |
| §10 FAQ | 5 | 5 = min satu entri, open items punya owner. 3 = ada tapi tidak ada owner. 0 = tidak ada. |
| **Total** | **95** | *+5 bonus jika ada link ke Epic PRD parent* |

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
LAPORAN VALIDASI FEATURE PRD
Fitur: [Nama]
Epic Parent: [Nama / Standalone]
Skor: [X]/100 — [Band]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PELANGGARAN (wajib diperbaiki sebelum In Review):
  §7 Kebutuhan — US-002 hanya punya 1 skenario Gherkin (minimum 2)
  §7 Kebutuhan — US-003 role pengguna adalah "pengguna" — harus peran spesifik
  §9 Kriteria Penerimaan — Tidak ada checklist untuk US-001 (prioritas Wajib)

PERINGATAN (sebaiknya diperbaiki, tidak memblokir):
  §4 Metrik — Owner metrik adalah "Tim Produk" — sebutkan individu
  §8 Solusi — Artefak desain masih Draft — tandai untuk reviewer

SEKSI LULUS:
  ✓ §1 Nama Fitur (3/3)
  ✓ §2 Status Dokumen (5/5)
  ✓ §6 Hipotesis (8/8)
  ...

Langkah Selanjutnya:
  1. Perbaiki semua PELANGGARAN di atas
  2. Jalankan ulang validasi: "review PRD saya"
  3. Atasi PERINGATAN sebelum diedarkan ke stakeholder
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Validasi Seksi Opsional

Jika seksi opsional ada, validasi terhadap section-rules di atas.
Pelanggaran pada seksi opsional **memblokir** PRD dari status `Disetujui`.
