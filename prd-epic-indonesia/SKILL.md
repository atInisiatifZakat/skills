---
name: prd-epic-indonesia
description: >
  Gunakan skill ini untuk membuat, mengulas, atau memperbarui dokumen Epic PRD dan Arsitektur Teknis
  dalam Bahasa Indonesia. Picu skill ini setiap kali pengguna menyebut "epic", "buat epic PRD",
  "rencanakan epic", "arsitektur epic", "dokumen inisiatif", "perencanaan produk besar", atau ingin
  mendefinisikan inisiatif lintas tim yang membutuhkan dokumen visi bisnis dan arsitektur teknis.
  Skill ini menghasilkan dua dokumen: epic.md (PRD level Epic) dan arch.md (Arsitektur Teknis),
  seluruhnya dalam Bahasa Indonesia — siap digunakan tim produk, engineering, dan stakeholder.
compatibility: "Claude.ai web/mobile dan Claude Code"
---

# PRD Epic Indonesia

Skill interaktif untuk membuat **dokumen Epic PRD** dan **Arsitektur Teknis** berkualitas
production dalam Bahasa Indonesia. Menghasilkan dua file:
- `{base-path}/{nama-epic}/epic.md` — visi bisnis, kebutuhan, metrik
- `{base-path}/{nama-epic}/arch.md` — arsitektur teknis, diagram sistem, estimasi

`{base-path}` ditentukan saat intake — bisa `docs/plan`, `prd/app-name`, atau path lain sesuai struktur project.

---

## Output & Penyimpanan

### Deteksi Base Path (wajib di §0 sebelum generate)

Tanyakan kepada pengguna di awal intake:
> *"Di mana dokumen PRD disimpan di project ini? Contoh: `docs/plan`, `prd/nama-app`, atau path lain?"*

Aturan penentuan `{base-path}`:
1. **Jika pengguna menyebutkan path** → gunakan path tersebut persis
2. **Jika Claude Code & ada struktur project terdeteksi** → cek apakah folder `prd/` atau `docs/plan/` sudah ada, gunakan yang ditemukan
3. **Jika tidak ada informasi** → gunakan default `docs/plan` dan informasikan ke pengguna

Path output final:
- `{base-path}/{nama-epic}/epic.md`
- `{base-path}/{nama-epic}/arch.md`

### Claude Code (filesystem tersedia)
Claude langsung menulis file ke path yang sudah ditentukan. Folder dibuat otomatis jika belum ada.

### Claude.ai Web / Mobile (tanpa filesystem)
Claude generate dokumen lengkap di chat. Di akhir output, Claude menyertakan instruksi:
> *"Simpan file ini ke `{base-path}/{nama-epic}/epic.md` di project Anda."*
> *(ganti `{base-path}` dengan path aktual yang sudah dikonfirmasi di awal)*

---

## Aturan Anti-Halusinasi

Berlaku **wajib** di semua mode. Tidak boleh dilanggar.

- **JANGAN** mengarang metrik, angka baseline, atau target yang tidak diberikan pengguna → tulis `[TBD — baseline diperlukan]`
- **JANGAN** membuat link Figma, Confluence, atau Jira yang tidak ada → tulis `[Belum ada link desain — status: Draft]`
- **JANGAN** mencantumkan nama orang sebagai reviewer/approver/owner jika tidak disebutkan → tulis `[TBD — owner: ]`
- **JANGAN** menulis klaim sebagai fakta tanpa sumber → tulis `[Keyakinan tim — belum divalidasi: ___]`
- **JANGAN** mengarang persyaratan regulasi → jika tidak tahu, tulis `[TBD — perlu review legal/compliance: nama-regulasi]`
- **JANGAN** mengisi checkbox sign-off sebagai selesai kecuali pengguna konfirmasi eksplisit
- Jika pengguna tidak tahu → tulis `[TBD]`, jangan mengarang jawaban yang "masuk akal"

---

## Versi & Tanggal

| Pemicu | Aturan | Contoh |
|---|---|---|
| Epic baru dibuat | Set ke `v0.1` | — |
| Seksi ditambah/diedit | Naikkan angka kecil | `v0.2 → v0.3` |
| Status Draft → In Review | Naikkan angka kecil | `v0.3 → v0.4` |
| Status In Review → Disetujui | Naikkan angka besar, reset kecil | `v0.4 → v1.0` |
| Status Disetujui → In Execution | Naikkan angka besar | `v1.0 → v2.0` |

`Terakhir Diperbarui` selalu diisi dengan tanggal hari ini format `YYYY-MM-DD`.

---

## Alur Kerja

### Mode 1: Buat Baru (default)
Digunakan saat pengguna ingin membuat Epic PRD dari awal.

```
Langkah 1 — Intake (§0)
  Jalankan seluruh pertanyaan §0 dari prompts/interview-questions.md.
  JANGAN mulai wawancara seksi sebelum §0 selesai.
  Gunakan jawaban §0 untuk menentukan seksi opsional yang akan disertakan.
  Jika ada sinyal compliance di §0.3 → jalankan §0.3b identifikasi regulasi SEBELUM lanjut.

Langkah 2 — Wawancara Seksi per Seksi
  Kerjakan setiap seksi inti secara berurutan (§1–§12).
  Tanyakan satu pertanyaan sekaligus, tidak sekaligus dump.
  Validasi jawaban terhadap section-rules.md sebelum lanjut ke seksi berikutnya.
  Tampilkan output seksi yang sudah selesai sebelum pindah ke seksi berikutnya.

Langkah 3 — Seksi Opsional
  Konfirmasi daftar seksi opsional yang ditentukan di intake:
  "Berdasarkan yang Anda ceritakan, saya akan menyertakan: [daftar]. Sudah benar?"
  Generate setiap seksi opsional yang dikonfirmasi secara berurutan.

Langkah 4 — Arsitektur (arch.md)
  Setelah epic.md selesai, tawarkan untuk lanjut ke dokumen arsitektur:
  "Epic PRD sudah selesai. Mau saya lanjutkan buat dokumen arsitektur teknisnya sekarang?"
  Jika ya → jalankan wawancara seksi arsitektur (A1–A6) dari interview-questions.md.

Langkah 5 — Output
  Set versi ke v0.1 dan tanggal hari ini.
  Susun dan tulis dokumen lengkap.
  Jalankan validasi kualitas (lihat prompts/validation-rules.md).
  Laporkan skor validasi dan peringatan.
```

### Mode 2: Review
Digunakan saat pengguna berkata "review epic PRD saya" atau memberikan file yang sudah ada.

```
Langkah 1 — Baca file epic.md yang ada.
Langkah 2 — Jalankan validasi terhadap semua section rules.
Langkah 3 — Laporkan: skor, pelanggaran, seksi yang perlu diperbaiki.
Langkah 4 — Tawarkan perbaikan seksi tertentu secara interaktif.
  Setiap perbaikan: terapkan aturan versi (edit konten atau perubahan status).
```

### Mode 3: Perbarui
Digunakan saat pengguna ingin merevisi seksi tertentu dari Epic PRD yang sudah ada.

```
Langkah 1 — Identifikasi seksi target.
Langkah 2 — Tampilkan konten seksi saat ini.
Langkah 3 — Tanyakan pertanyaan terfokus untuk mengumpulkan konten baru.
Langkah 4 — Tulis ulang seksi dan perbarui file. Terapkan aturan versi sebelum simpan.
Langkah 5 — Jalankan validasi ulang pada seksi yang diperbarui.
```

---

## Struktur Epic PRD

**12 Seksi Inti** (selalu wajib):
1. Nama Epic
2. Status Dokumen
3. Latar Belakang
4. Tujuan & OKR
5. Ruang Lingkup
6. Hipotesis
7. User Journeys
8. Kebutuhan Bisnis
9. Metrik Keberhasilan
10. Nilai Bisnis
11. Arsitektur Teknis *(ringkasan di epic.md, detail di arch.md)*
12. FAQ

**4 Seksi Opsional** (sertakan hanya saat dipicu):
- §13 Risiko & Mitigasi — saat ada sinyal compliance, pembayaran, atau dependensi lintas tim
- §14 Dependensi — saat bergantung pada atau memblokir tim/sistem lain
- §15 Rencana Rilis — saat butuh rollout bertahap, staged release, atau gate compliance
- §16 Peta Stakeholder — saat inisiatif melibatkan 3+ tim atau butuh visibilitas eksekutif

---

## Skor Validasi

Setelah generate, PRD diberi skor dari 100:

| Seksi | Poin | Cek Utama |
|---|---|---|
| Nama Epic | 3 | ≤8 kata, noun phrase |
| Status Dokumen | 5 | Status valid, reviewer bernama |
| Latar Belakang | 10 | Pain-led, evidence dikutip, cost of inaction ada |
| Tujuan & OKR | 10 | KR numerik, link OKR perusahaan |
| Ruang Lingkup | 8 | In + Out scope ada, Out items punya alasan |
| Hipotesis | 8 | Template dipakai, falsifiable, confidence level |
| User Journeys | 8 | Minimal 2 journey, alur end-to-end jelas |
| Kebutuhan Bisnis | 12 | FR dan NFR ada, spesifik dan terukur |
| Metrik Keberhasilan | 10 | Leading + lagging + guardrail, semua field lengkap |
| Nilai Bisnis | 6 | Estimasi nilai ada, justifikasi ada |
| Arsitektur Teknis | 10 | Overview ada, tech stack ada, T-shirt size ada |
| FAQ | 5 | Min satu entri, open items punya owner |
| **Total** | **95** | *5 poin bonus untuk diagram arsitektur (Mermaid)* |

Band skor: **90–100** Siap In Review · **75–89** Perbaiki peringatan · **60–74** Perlu kerja · **< 60** Jangan diedarkan

---

## Referensi File

| File | Fungsi |
|---|---|
| `SKILL.md` | File ini — entry point dan alur kerja |
| `templates/epic-template.md` | Template kosong Epic PRD |
| `templates/arch-template.md` | Template kosong Arsitektur Teknis |
| `prompts/interview-questions.md` | Pertanyaan wawancara per seksi |
| `prompts/section-rules.md` | Aturan lengkap per seksi untuk validasi |
| `prompts/validation-rules.md` | Rubrik skor dan format laporan validasi |
