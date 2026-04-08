---
name: prd-fitur-indonesia
description: >
  Gunakan skill ini untuk membuat, mengulas, atau memperbarui dokumen Feature PRD dalam Bahasa
  Indonesia. Picu skill ini setiap kali pengguna menyebut "buat PRD", "tulis PRD fitur",
  "dokumentasi fitur", "spesifikasi fitur", "user stories", "acceptance criteria", "PRD untuk
  [nama fitur]", atau ingin mendefinisikan kebutuhan fitur spesifik dalam format terstruktur.
  Skill ini menghasilkan satu dokumen prd.md per fitur — lengkap dengan user stories, Gherkin
  acceptance criteria, dan metrik — seluruhnya dalam Bahasa Indonesia. Bisa digunakan standalone
  atau sebagai turunan dari dokumen Epic PRD (prd-epic-indonesia).
compatibility: "Claude.ai web/mobile dan Claude Code"
---

# PRD Fitur Indonesia

Skill interaktif untuk membuat **Feature PRD** berkualitas production dalam Bahasa Indonesia.
Menghasilkan satu file:
- `{base-path}/{nama-epic}/{nama-fitur}/prd.md`

Jika fitur bukan bagian dari Epic (standalone):
- `{base-path}/standalone/{nama-fitur}/prd.md`

`{base-path}` ditentukan saat intake — bisa `docs/plan`, `prd/app-name`, atau path lain sesuai struktur project.

---

## Output & Penyimpanan

### Deteksi Base Path (wajib di §0 sebelum generate)

Tanyakan kepada pengguna di awal intake:
> *"Di mana dokumen PRD disimpan di project ini? Contoh: `docs/plan`, `prd/nama-app`, atau path lain?"*

Aturan penentuan `{base-path}`:
1. **Jika pengguna menyebutkan path** → gunakan path tersebut persis
2. **Jika Claude Code & ada struktur project terdeteksi** → cek apakah folder `prd/` atau `docs/plan/` sudah ada, gunakan yang ditemukan
3. **Jika ada Epic PRD parent** → gunakan base-path yang sama dengan Epic PRD tersebut
4. **Jika tidak ada informasi** → gunakan default `docs/plan` dan informasikan ke pengguna

Path output final:
- Dengan Epic: `{base-path}/{nama-epic}/{nama-fitur}/prd.md`
- Standalone: `{base-path}/standalone/{nama-fitur}/prd.md`

### Claude Code (filesystem tersedia)
Claude langsung menulis file ke path yang sudah ditentukan. Folder dibuat otomatis jika belum ada.

### Claude.ai Web / Mobile (tanpa filesystem)
Claude generate dokumen lengkap di chat. Di akhir output, Claude menyertakan instruksi:
> *"Simpan file ini ke `{base-path}/{nama-epic}/{nama-fitur}/prd.md` di project Anda."*
> *(ganti `{base-path}` dengan path aktual yang sudah dikonfirmasi di awal)*

---

## Aturan Anti-Halusinasi

Berlaku **wajib** di semua mode. Tidak boleh dilanggar.

- **JANGAN** mengarang metrik, angka baseline, atau target → tulis `[TBD — baseline diperlukan]`
- **JANGAN** membuat link Figma, Confluence, atau Jira yang tidak ada → tulis `[Belum ada link desain — status: Draft]`
- **JANGAN** mencantumkan nama orang jika tidak disebutkan → tulis `[TBD — owner: ]`
- **JANGAN** menulis klaim sebagai fakta tanpa sumber → tulis `[Keyakinan tim — belum divalidasi: ___]`
- **JANGAN** mengarang persyaratan regulasi → tulis `[TBD — perlu review legal/compliance: nama-regulasi]`
- **JANGAN** mendeskripsikan UI dalam prosa jika tidak ada link desain → tulis `[Design pending — link akan ditambahkan]`
- **JANGAN** mengisi checkbox sign-off kecuali pengguna konfirmasi eksplisit
- Jika pengguna tidak tahu → tulis `[TBD]`, jangan mengarang jawaban

---

## Versi & Tanggal

| Pemicu | Aturan | Contoh |
|---|---|---|
| PRD baru dibuat | Set ke `v0.1` | — |
| Seksi ditambah/diedit | Naikkan angka kecil | `v0.2 → v0.3` |
| Status Draft → In Review | Naikkan angka kecil | `v0.3 → v0.4` |
| Status In Review → Disetujui | Naikkan angka besar, reset kecil | `v0.4 → v1.0` |

`Terakhir Diperbarui` selalu diisi dengan tanggal hari ini format `YYYY-MM-DD`.

---

## Alur Kerja

### Mode 1: Buat Baru (default)
Digunakan saat pengguna ingin membuat Feature PRD dari awal.

```
Langkah 1 — Intake (§0)
  Jalankan pertanyaan §0 dari prompts/interview-questions.md (~8 pertanyaan).
  JANGAN mulai wawancara seksi sebelum §0 selesai.
  Gunakan jawaban §0 untuk menentukan seksi opsional dan konteks Epic parent.
  Jika ada sinyal compliance di §0.3 → jalankan identifikasi regulasi SEBELUM lanjut.

Langkah 2 — Wawancara Seksi per Seksi
  Kerjakan setiap seksi inti secara berurutan (§1–§10).
  Tanyakan satu pertanyaan sekaligus.
  Tampilkan output seksi yang sudah selesai sebelum pindah ke seksi berikutnya.
  Untuk §7 (User Stories): kumpulkan satu story sekaligus, ulangi sampai pengguna bilang "cukup".

Langkah 3 — Seksi Opsional
  Konfirmasi seksi opsional yang ditentukan di intake dan generate secara berurutan.

Langkah 4 — Output
  Set versi ke v0.1 dan tanggal hari ini.
  Susun dan tulis dokumen lengkap.
  Jalankan validasi kualitas (lihat prompts/validation-rules.md).
  Laporkan skor dan peringatan.
```

### Mode 2: Review
Digunakan saat pengguna berkata "review PRD saya" atau memberikan file yang sudah ada.

```
Langkah 1 — Baca file prd.md yang ada.
Langkah 2 — Jalankan validasi terhadap semua section rules.
Langkah 3 — Laporkan: skor, pelanggaran, seksi yang perlu diperbaiki.
Langkah 4 — Tawarkan perbaikan seksi tertentu secara interaktif.
```

### Mode 3: Perbarui
Digunakan saat pengguna ingin merevisi seksi tertentu.

```
Langkah 1 — Identifikasi seksi target.
Langkah 2 — Tampilkan konten seksi saat ini.
Langkah 3 — Tanyakan pertanyaan terfokus untuk konten baru.
Langkah 4 — Tulis ulang seksi. Terapkan aturan versi sebelum simpan.
Langkah 5 — Jalankan validasi ulang pada seksi yang diperbarui.
```

---

## Struktur Feature PRD

**10 Seksi Inti** (selalu wajib):
1. Nama Fitur
2. Status Dokumen
3. Latar Belakang
4. Tujuan & Metrik
5. Ruang Lingkup
6. Hipotesis
7. Kebutuhan (User Stories + Gherkin)
8. Solusi
9. Kriteria Penerimaan
10. FAQ

**2 Seksi Opsional** (sertakan hanya saat dipicu):
- §11 Risiko & Mitigasi — saat ada sinyal compliance atau risiko teknis signifikan
- §12 Dependensi — saat bergantung pada API eksternal atau tim lain

---

## Skor Validasi

| Seksi | Poin | Cek Utama |
|---|---|---|
| Nama Fitur | 3 | ≤8 kata, noun phrase |
| Status Dokumen | 5 | Status valid, reviewer bernama |
| Latar Belakang | 8 | Pain-led, evidence dikutip |
| Tujuan & Metrik | 12 | Leading + lagging + guardrail, semua field lengkap |
| Ruang Lingkup | 8 | In + Out scope, Out punya alasan |
| Hipotesis | 8 | Template dipakai, falsifiable |
| Kebutuhan (User Stories) | 25 | Role spesifik, ≥2 Gherkin per story, MoSCoW |
| Solusi | 10 | Design link ada, coverage map ada, alternatif ada |
| Kriteria Penerimaan | 11 | Checklist lengkap per US, QA sign-off ada |
| FAQ | 5 | Min satu entri, open items punya owner |
| **Total** | **95** | *+5 bonus untuk link Epic PRD parent* |

Band skor: **90–100** Siap In Review · **75–89** Perbaiki peringatan · **60–74** Perlu kerja · **< 60** Jangan diedarkan

---

## Referensi File

| File | Fungsi |
|---|---|
| `SKILL.md` | File ini — entry point dan alur kerja |
| `templates/prd-template.md` | Template kosong Feature PRD |
| `prompts/interview-questions.md` | Pertanyaan wawancara per seksi |
| `prompts/section-rules.md` | Aturan lengkap per seksi untuk validasi |
| `prompts/validation-rules.md` | Rubrik skor dan format laporan validasi |
