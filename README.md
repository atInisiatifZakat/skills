# Skills — Claude Code

Kumpulan skill interaktif untuk Claude Code. Setiap skill mendefinisikan alur kerja terstruktur yang dijalankan Claude secara interaktif — mulai dari wawancara, pemrosesan, hingga menghasilkan output sesuai konteks.

## Daftar Skill

| Skill | Deskripsi Singkat | Kata Kunci Pemicu |
|---|---|---|
| [`prd-epic-indonesia`](prd-epic-indonesia/SKILL.md) | Buat Epic PRD + Arsitektur Teknis dalam Bahasa Indonesia | "epic", "buat epic PRD", "arsitektur epic" |
| [`prd-fitur-indonesia`](prd-fitur-indonesia/SKILL.md) | Buat Feature PRD dengan User Stories & Gherkin dalam Bahasa Indonesia | "buat PRD", "PRD fitur", "user stories" |

---

## Cara Menggunakan

Setiap skill berjalan langsung di Claude Code. Cukup ketik kata kunci pemicunya di chat, lalu Claude akan menjalankan alur kerja yang sudah didefinisikan di skill tersebut.

---

## Struktur Direktori Skill

Setiap skill mengikuti struktur berikut (disesuaikan dengan kebutuhan skill):

```
{nama-skill}/
  SKILL.md        # Entry point: deskripsi, alur kerja, dan aturan skill
  prompts/        # Prompt pendukung (pertanyaan, aturan validasi, dll.)
  templates/      # Template output yang dihasilkan skill
```

File wajib hanya `SKILL.md`. Direktori `prompts/` dan `templates/` bersifat opsional — tambahkan sesuai kebutuhan.

---

## Menambah Skill Baru

1. Buat direktori baru: `{nama-skill}/`
2. Tulis `SKILL.md` dengan frontmatter minimal:
   ```yaml
   ---
   name: nama-skill
   description: >
     Kapan dan bagaimana skill ini dipicu.
   compatibility: "Claude.ai web/mobile dan Claude Code"
   ---
   ```
3. Definisikan alur kerja, mode operasi, dan aturan perilaku di dalam `SKILL.md`
4. Tambahkan file pendukung di `prompts/` dan `templates/` jika diperlukan
5. Daftarkan skill di pengaturan Claude Code agar bisa dipicu via kata kunci
