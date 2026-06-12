---
name: writing-plans
description: >
  Use this skill BEFORE writing any code when you have a spec, PRD, ADR, or feature
  requirement. Creates an implementation plan that combines pseudocode specs with TDD
  task structure. Use whenever the user says "buat plan", "bikin implementation plan",
  "plan dulu sebelum coding", "buatkan ADR", "dokumentasi sebelum implementasi",
  "pseudocode for this feature", or when starting any multi-function feature. Also
  trigger when user shares a spec/requirement document and hasn't started implementing yet.
  Supports Go and Laravel.
---

# Implementation Plan (Pseudocode + TDD)

**Announce at start:** "Saya menggunakan writing-plans skill untuk membuat implementation plan."

**Save plans to:** `docs/plans/YYYY-MM-DD-<feature-name>.md`
(User preference override jika disebutkan)

---

## Proses

### 1. Elicitation (jika tidak ada PRD/spec/ADR)

Cek apakah user sudah melampirkan dokumen pendukung (PRD, ADR, user story, spec).

**Jika sudah ada** → lanjut ke Step 2, gunakan dokumen tersebut sebagai sumber CONSTRAINT dan GUARD.

**Jika belum ada** → jangan langsung tulis plan. Tanyakan empat hal berikut sebelum lanjut:

```
Sebelum saya buat plan-nya, saya butuh konfirmasi beberapa hal:

1. Business constraints — siapa yang boleh melakukan operasi ini, dan dalam kondisi apa?
   (contoh: "hanya verified user", "maksimal 10 item per order")

2. Rejection cases — apa yang harus ditolak sistem, bukan hanya apa yang harus berhasil?
   (contoh: "tolak jika stock kurang", "tolak duplicate request")

3. Idempotency — apakah operasi ini aman dipanggil ulang dengan input sama?
   (contoh: "ya, berdasarkan idempotency_key")

4. Side effects — apa yang terjadi selain DB write?
   (contoh: "kirim email notifikasi", "publish event", "invalidate cache")
```

Jawaban atas empat pertanyaan ini adalah **sumber langsung** untuk `[CONSTRAINT]` dan setiap `GUARD` di pseudocode. Tanpanya, plan akan berisi asumsi yang terlihat presisi tapi bisa salah.

**Minimum viable input jika user ingin skip elicitation:**

```markdown
## Mini-Spec
Siapa yang boleh  : <role/kondisi>
Tolak jika        : <daftar kondisi rejection>
Idempotent        : ya/tidak — berdasarkan <field>
Side effects      : <event/email/cache/dll, atau "-">
```

Setelah elicitation selesai atau mini-spec diterima → lanjut ke Step 2.

---

### 2. Scope Check

Sebelum mulai — jika spec mencakup lebih dari satu independent subsystem, pecah dulu menjadi sub-plan terpisah. Setiap plan harus menghasilkan software yang bisa berjalan dan di-test sendiri.

---

### 3. Plan Header

Tulis header plan sebelum apapun — Goal, Architecture, dan Tech Stack adalah keputusan yang harus dikunci sebelum dekomposisi file dan pseudocode.

```markdown
# [Feature Name] Implementation Plan

> **For agentic workers:** Execute task-by-task. Pseudocode spec dalam setiap task
> adalah **prescriptive contract** — setiap GUARD, FETCH, TRANSFORM wajib di-implement
> exactly, tidak ada improvisasi. Jika merasa ada yang perlu diubah, catat sebagai
> `// DEVIATION:` comment dan tetap ikuti spec. Jalankan maksimal 5–7 task per sesi;
> mulai sesi baru untuk task berikutnya dengan membaca state codebase terkini.

**Goal:** [Satu kalimat — apa yang dibangun]
**Architecture:** [2–3 kalimat — pendekatan teknis]
**Tech Stack:** [Bahasa, framework, DB, library utama]

---
```

---

### 4. File Structure

Petakan semua file yang akan dibuat atau dimodifikasi **sebelum** mendefinisikan tasks. Ini adalah tempat keputusan dekomposisi dikunci.

```markdown
## File Structure

| File | Action | Responsibility |
|---|---|---|
| `internal/feature/domain.go` | Create | Types, errors |
| `internal/feature/service.go` | Create | Business logic |
| `internal/feature/repository.go` | Create | DB interface |
| `internal/feature/service_test.go` | Create | Unit tests |
```

Prinsip:
- Satu file, satu responsibility yang jelas
- File yang sering berubah bersama → taruh bersama
- Interface/contract dideklarasi sebelum implementasi

---

### 5. Pseudocode Spec

Tulis spec untuk setiap non-trivial function **sebelum** membuat tasks. Ini adalah source of truth — bukan placeholder.

**Anatomy:**
```
// [CONSTRAINT] <business rule — kenapa fungsi ini ada>
// [IDEMPOTENT] <opsional — tandai jika safe dipanggil ulang>
//
// deps  : <interfaces yang dibutuhkan>
// reads : <tabel / service yang dibaca>
// writes: <tabel yang dimutasi>
// emits : <events, atau "-">

// Go:
func (s *XxxService) MethodName(ctx context.Context, req RequestType) (ReturnType, error) {

// Laravel:
public function methodName(RequestDTO $dto): ReturnType {

    // GUARD: <precondition> → return nil / throw ErrXxx
    // FETCH: <data> from <repo/service>
    // TRANSFORM: <input> → <output>
    //   - detail kalkulasi atau mapping
    // GUARD: <post-fetch condition> → return nil / throw ErrXxx
    // ATOMIC (tx):
    //   - <operasi 1>
    //   - <operasi 2>
    // EMIT: EventName{fields}
    // return result
}
```

**Keywords yang wajib digunakan:**
| Keyword | Artinya |
|---|---|
| `GUARD` | Kondisi yang menghentikan eksekusi → error spesifik |
| `FETCH` | Ambil data dari repo/service eksternal |
| `TRANSFORM` | Konversi/kalkulasi data, tanpa side effect |
| `ATOMIC (tx)` | Batas database transaction |
| `EMIT` | Event publish — selalu di luar ATOMIC kecuali transactional outbox |

**Kapan tulis full code vs pseudocode — whitelist eksplisit:**

| Kategori | Format | Contoh |
|---|---|---|
| `type`, `struct`, `interface` | ✅ Full code | `type Order struct { ... }` |
| `var Err...`, `errors.New` | ✅ Full code | `var ErrNotFound = errors.New(...)` |
| Test code | ✅ Full code | Selalu — no placeholders |
| Dependency wiring | ✅ Full code | Constructor calls + route registration |
| Test infrastructure schema | ⚠️ Schema notation | Lihat contoh di bawah |
| Function dengan conditional / loop / error handling | ❌ Pseudocode | Gunakan GUARD/FETCH/ATOMIC |

**Test infrastructure schema — gunakan notation ringkas, bukan full Go:**
```
// ❌ Jangan: 40 baris Go+SQL boilerplate untuk setiap stub table
// ✅ Gunakan schema notation:
//
// TestStubs → 2 migrations (ID prefix 00000000000003/4)
// employees : id TEXT PK, name TEXT, branch_id TEXT nullable
// branches  : id TEXT PK, name TEXT, is_head_office BOOLEAN
```
Agent yang baik bisa menulis Go wrapper dari spec ini. Tidak perlu Sonnet menulisnya lengkap.

**Pseudocode ≠ placeholder:**
- ❌ Placeholder: `// add validation` (vague)
- ✅ Pseudocode: `// GUARD: user.Status != Verified → return nil, ErrUnverifiedUser` (presisi)

---

### 6. Task Structure (TDD)

Setiap task mengikuti struktur ini. Step implementasi **mengacu ke spec pseudocode** yang sudah ditulis di dalam task yang sama — tidak perlu repeat full code, dan spec **tidak boleh** dipindah ke section global terpisah.

**❌ Salah — spec dipisah ke section global:**
```
## Pseudocode Spec        ← jangan lakukan ini
### FindByID: ...

### Task 2:
- [ ] Step 1: implementasi  ← agent tidak punya referensi inline, akhirnya tulis full code
```

**✅ Benar — spec inline di dalam task:**
```
### Task 2: UserRepository

**Spec:**                  ← spec ada di dalam task
// GUARD: ...
// FETCH: ...

- [ ] Step 4: Implementasi (ikuti spec di atas — jangan tulis ulang full code)
```

````markdown
### Task N: [Nama Komponen]

**Spec:**
```
[pseudocode spec di sini]
```

**Files:**
- Create/Modify: `exact/path/to/file`
- Test: `exact/path/to/file_test`

- [ ] **Step 1: Definisikan types & errors** ← full code, selalu
- [ ] **Step 2: Tulis failing tests** ← full code, selalu, no placeholders
- [ ] **Step 3: Run → pastikan FAIL** `<test command>`
- [ ] **Step 4: Implementasi** (spec adalah contract — ikuti exactly, catat deviasi sebagai `// DEVIATION:`)
- [ ] **Step 5: Run → pastikan PASS** `<test command>`
- [ ] **Step 6: Commit** `git commit -m "feat(scope): message"`
````

**Aturan absolute untuk test code (No Placeholders):**
- Tulis test code lengkap — tidak ada "add more test cases", "test the sad path", "similar to above"
- Setiap GUARD dalam pseudocode spec wajib punya test case sendiri
- Happy path + setiap error path = test terpisah

**Referensi template lengkap per bahasa:**
- Go → baca `references/go.md`
- Laravel → baca `references/laravel.md`

---

### 7. Self-Review

Setelah plan selesai ditulis, review dengan checklist ini sebelum diserahkan:

**Spec coverage:**
- Goal, Architecture, dan Tech Stack sudah diisi konkret (bukan template placeholder)?
- Setiap requirement di spec/PRD bisa di-trace ke task mana?
- Setiap GUARD punya test case?
- ATOMIC scope sudah benar (EMIT di luar tx)?

**Consistency:**
- Error name di pseudocode identik dengan yang dideklarasi di domain/types task?
- Method name di spec identik dengan yang ada di interface?
- Tidak ada `processOrder()` di Task 2 tapi `createOrder()` di Task 5?

**Placeholder scan:**
- Tidak ada: "TBD", "TODO", "add validation", "handle edge cases", "similar to Task N"
- Pseudocode dengan GUARD/FETCH/TRANSFORM eksplisit = ✅ bukan placeholder
- Spec pseudocode ada **inline di dalam setiap task** — bukan di section global terpisah
- Step implementasi tidak mengandung full code untuk fungsi yang sudah ada pseudocode spec-nya
