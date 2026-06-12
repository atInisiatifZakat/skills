---
name: writing-plans-react
description: >
  Use this skill BEFORE writing any code for React projects when you have a spec, PRD,
  or feature requirement. Also use for bug fixes in React projects. Creates an
  implementation plan adapted for frontend context. Trigger when user says "buat plan",
  "bikin implementation plan", "plan dulu sebelum coding", "ada bug", "fix bug", or
  when starting any multi-component feature in a React project. Also trigger when user
  shares a spec/requirement for a frontend feature and hasn't started implementing yet.
  Supports React projects with Vite + TanStack Query/Router + React Hook Form +
  shadcn/Mantine + react-table + TypeScript. Do NOT trigger for Go or Laravel projects
  — use writing-plans instead.
compatibility: "Claude.ai web/mobile dan Claude Code"
---

# Writing Plans — React

**Announce at start:** "Saya menggunakan writing-plans-react skill untuk membuat implementation plan."

**Save plans to:** `docs/plans/YYYY-MM-DD-<feature-name>.md`
(User preference override jika disebutkan)

---

## Proses

### 1. Discovery Check

Cek apakah file `docs/plans/writing-plans-react/discovery.md` sudah ada.

**Jika sudah ada:**
Tampilkan ringkasan singkat isi discovery (UI framework, struktur utama, fitur referensi), lalu tanya:

```
Discovery sudah ada. Apakah ingin menjalankan ulang discovery, atau lanjut dengan hasil yang ada?
```

**Jika belum ada:**
Langsung jalankan Discovery tanpa tanya.

---

### 2. Discovery

Eksplorasi codebase secara otomatis. Ping user hanya untuk klarifikasi jika ada yang ambigu, dan minta konfirmasi di akhir sebelum menyimpan file.

#### 2a. UI Framework
Baca `package.json` — deteksi:
- UI framework: shadcn/ui atau Mantine (atau keduanya)
- Versi React, TanStack Query, TanStack Router, React Hook Form, Zod, react-table

#### 2b. Struktur Direktori
Scan `src/` sampai 2-3 level. Catat **pola**, bukan daftar semua file.

Contoh output:
```
src/
  features/          ← fitur dikelompokkan per domain
    [domain]/
      components/
      hooks/
      api/
  components/        ← shared/reusable components
  routes/            ← TanStack Router route definitions
  lib/               ← utils, config, helpers
  types/             ← shared types
```

#### 2c. Pattern API Call
Scan contoh file di `api/` atau sejenisnya — catat:
- HTTP client yang dipakai (axios, fetch, dll)
- Cara base URL dikonfigurasi
- Pattern penamaan function (camelCase, ada prefix `get`/`create`/`update`/`delete`?)
- Apakah ada error handling global

#### 2d. Pattern Project
Scan contoh hooks dan komponen — catat:
- Konvensi penamaan query key (array vs string, struktur nesting)
- Cara hook di-export dan dipakai di komponen
- Cara React Hook Form + Zod diintegrasikan
- Cara react-table dikonfigurasi (apakah ada wrapper component?)

#### 2e. Contoh Fitur Referensi
Scan codebase, identifikasi 2-3 fitur yang paling lengkap (punya route + query + komponen, idealnya juga ada form atau table). Tawarkan ke user:

```
Saya menemukan beberapa fitur yang bisa dijadikan referensi:
1. [nama fitur A] — punya table + form + query
2. [nama fitur B] — punya table + query
3. [nama fitur C] — punya form + mutation

Fitur mana yang paling representatif sebagai referensi?
```

Setelah user pilih, dokumentasikan:
- **Struktur file** lengkap fitur tersebut (semua file di dalam folder fitur)
- **Layout komposisi halaman** — komponen apa yang menyusun halaman utama fitur ini (PageHeader, Toolbar, Table, Dialog, dll)

#### 2f. Simpan Discovery
Konfirmasi ke user, lalu simpan ke `docs/plans/writing-plans-react/discovery.md`:

```markdown
# React Project Discovery

> Generated: YYYY-MM-DD

## UI Framework
- **Framework:** shadcn/ui | Mantine
- **Versi utama:** React X.X, TanStack Query X.X, TanStack Router X.X, RHF X.X, Zod X.X, react-table X.X

## Struktur Direktori
[scan hasil 2-3 level dari src/]

## Pattern API Call
- **HTTP client:** axios | fetch
- **Base URL:** [cara konfigurasi]
- **Naming:** [contoh: getUsers, createUser, updateUser, deleteUser]
- **Error handling:** [global interceptor / per-call]

## Pattern Project
- **Query key convention:** [contoh: ['users', filters] | 'users']
- **Hook pattern:** [contoh: export function useUsers(filters) { return useQuery(...) }]
- **Form pattern:** [contoh: useForm<Schema>({ resolver: zodResolver(schema) })]
- **Table pattern:** [contoh: useReactTable({ data, columns, getCoreRowModel() })]

## Fitur Referensi: [nama fitur]

### Struktur File
[daftar file di dalam folder fitur]

### Layout Komposisi Halaman
[PageHeader, Toolbar/FilterBar, DataTable, Sheet/Dialog, dll]
```

---

### 3. Mode Check

Tanya user (atau deteksi dari konteks):

```
Ini untuk:
1. Fitur baru
2. Bug fix
```

---

### 4a. Elicitation — Mode Feature

Cek apakah user sudah melampirkan cukup konteks (spec, PRD, user story).

**Jika sudah ada** → lanjut ke Step 5.

**Jika belum ada** → tanya empat hal berikut:

```
Sebelum saya buat plan-nya, konfirmasi beberapa hal:

1. Siapa yang bisa akses fitur ini?
   (contoh: "semua user", "hanya admin", "user dengan role tertentu")

2. Data dari mana?
   (contoh: endpoint yang sudah ada, perlu endpoint baru, atau data lokal saja)

3. Ada form / aksi write?
   (contoh: "ya, create + edit", "tidak, read-only")

4. Ada state yang perlu di-share antar komponen?
   (contoh: "tidak", "ya tapi cukup lewat props/URL params")
```

**Minimum viable input jika user ingin skip elicitation:**

```markdown
## Mini-Spec
Akses        : <role/kondisi>
Data dari    : <endpoint / lokal>
Ada form     : ya/tidak
Shared state : ya/tidak — via <props/URL params/lainnya>
```

---

### 4b. Elicitation — Mode Bug Fix

Tanya ke user:

```
Untuk membuat bug fix plan, saya butuh:

1. Symptom — apa yang terjadi vs yang diharapkan?
2. Reproduce — langkah untuk memunculkan bug?
3. Sudah coba fix apa? (opsional)
```

Setelah informasi terkumpul, eksplorasi codebase untuk mengidentifikasi root cause sebelum menulis plan.

---

### 5. Plan Header

```markdown
# [Feature/Bug Name] — React Implementation Plan

> **For agentic workers:** Execute task-by-task. Spec di setiap task adalah
> **prescriptive contract** — ikuti exactly, catat deviasi sebagai `// DEVIATION:`.
> Jalankan maksimal 5–7 task per sesi; mulai sesi baru untuk task berikutnya
> dengan membaca state codebase terkini.

**Goal:** [Satu kalimat — apa yang dibangun atau diperbaiki]
**Architecture:** [2–3 kalimat — pendekatan teknis]
**Tech Stack:** React, TanStack Query/Router, React Hook Form, Zod, [shadcn/ui | Mantine], react-table, TypeScript

---
```

---

### 6. File Structure

Petakan semua file yang akan dibuat atau dimodifikasi **sebelum** mendefinisikan tasks. Gunakan struktur yang konsisten dengan fitur referensi di discovery.

```markdown
## File Structure

| File | Action | Responsibility |
|---|---|---|
| `src/features/[feature]/types.ts` | Create | API response types |
| `src/features/[feature]/api/[feature].ts` | Create | API functions |
| `src/features/[feature]/hooks/use[Feature].ts` | Create | Query hook |
| `src/features/[feature]/components/[Feature]Table.tsx` | Create | Table component |
| `src/features/[feature]/components/[Feature]Form.tsx` | Create | Form component |
| `src/routes/[feature].tsx` | Create | Route + page composition |
```

---

### 7. Spec Format

Gunakan format yang sesuai dengan tipe file.

#### Logic Files (hooks, api, lib) — Pseudocode

```
// [CONSTRAINT] <business rule — kenapa fungsi ini ada>
//
// QUERY: <nama> — GET /api/path, query key: ['key', params]
// GUARD: <kondisi yang blokir eksekusi> → return / throw / render fallback
// TRANSFORM: <input> → <output> — detail mapping atau kalkulasi
// MUTATION: <nama> — POST/PUT/DELETE /api/path, invalidate: ['key']
// FORM: schema Zod — field list + validasi
// COLUMNS: definisi kolom react-table — accessor, header, cell renderer
// SIDE_EFFECT: setelah mutation sukses — redirect / toast / refetch
```

#### Component Files — Component Spec

```
// Props: { prop1: Type, prop2: Type, onAction: (id: string) => void }
// Renders: [deskripsi singkat UI — komponen apa yang dikomposisi]
// Empty state: [kondisi + komponen yang ditampilkan]
// Loading state: [skeleton / spinner]
// Handlers: [event apa → aksi apa]
```

#### Page/Route Files — Kombinasi

```
// Queries: useFeature(filters), useFeatureStats()
// Composes: <FeatureTable />, <FeatureFilters />, <PageHeader />
// Layout: [urutan komponen di halaman]
// Guards: [kondisi redirect atau permission check]
```

---

### 8. Task Structure

#### Mode Feature

````markdown
### Task N: [Nama Komponen/Layer]

**Spec:**
```
[pseudocode atau component spec sesuai tipe file]
```

**Files:**
- Create/Modify: `exact/path/to/file.tsx`

- [ ] **Step 1: Definisikan types dari API response** (`types.ts`)
- [ ] **Step 2: Buat API function** (`api/[feature].ts`) — ikuti spec di atas
- [ ] **Step 3: Buat query/mutation hook** (`hooks/use[Feature].ts`) — ikuti spec di atas
- [ ] **Step 4: Buat form schema** (`hooks/use[Feature]Form.ts`) — jika ada form, skip jika tidak
- [ ] **Step 5: Buat komponen UI** — component spec sebagai contract, tidak ada placeholder
- [ ] **Step 6: Wiring di halaman/route** — komposes semua komponen sesuai layout spec
- [ ] **Step 7: Commit** `git commit -m "feat([scope]): message"`
````

Step yang tidak relevan di-skip (contoh: fitur read-only skip Step 4).

#### Mode Bug Fix

Bug fix plan harus **ringkas** — tidak ada File Structure table, tidak ada spec pseudocode panjang. Pseudocode hanya untuk menunjukkan perubahan spesifik (before/after), maksimal 3–5 baris per file.

Jumlah step mengikuti jumlah file yang diubah:
- **1 file** = 4 step (reproduce → fix → verifikasi → commit)
- **2+ file** = satu step fix per file, lalu verifikasi dan commit

````markdown
### Bug Fix: [Judul Bug]

**Symptom:** [apa yang terjadi vs yang diharapkan]
**Reproduce:** [langkah reproduksi — numbered list]
**Root Cause:** [file, baris, dan alasan teknis mengapa terjadi]
**Fix Approach:** [ringkasan perubahan yang akan dilakukan]

- [ ] **Step 1: Reproduksi** — ikuti langkah Reproduce di atas, konfirmasi symptom terjadi

- [ ] **Step 2: Fix `path/to/file.tsx`**
  ```
  // BEFORE: [kondisi yang menyebabkan bug — 1-2 baris]
  // AFTER:  [perubahan yang memperbaiki — 1-2 baris]
  ```

- [ ] **Step N: Fix `path/to/other-file.tsx`** ← tambahkan jika ada file lain, hapus jika tidak perlu

- [ ] **Step N+1: Verifikasi** — [langkah manual: buka halaman X, lakukan Y, pastikan Z]

- [ ] **Step N+2: Commit** `git commit -m "fix([scope]): message"`
````

---

### 9. Self-Review

Setelah plan selesai ditulis, review dengan checklist ini sebelum diserahkan:

**Spec coverage:**
- Goal, Architecture, Tech Stack sudah diisi konkret (bukan placeholder)?
- Setiap API endpoint yang dipakai sudah ada query/mutation hook-nya?
- Setiap form sudah ada Zod schema-nya?
- Setiap komponen dengan table sudah ada definisi kolom-nya?

**Consistency:**
- Nama query key konsisten di semua task yang pakai query yang sama?
- Tipe dari API response dipakai ulang, tidak didefinisikan ulang di tiap file?
- Nama komponen di spec identik dengan nama file yang akan dibuat?
- Struktur file mengikuti pola fitur referensi di discovery?

**Placeholder scan:**
- Tidak ada: "TBD", "TODO", "add validation", "handle error", "similar to above"
- Component spec punya Props, Renders, dan Handlers yang eksplisit
- Pseudocode punya keyword QUERY/GUARD/TRANSFORM/MUTATION yang konkret
- Setiap task punya spec inline — bukan di section global terpisah
