# AGENTS.md

Instruksi permanen untuk semua AI coding agent yang bekerja di project ini.
Berlaku kapan pun, di sesi mana pun, dengan tool apa pun.

---

## Project

**Ngopi Bareng Teman** — website marketing untuk kedai kopi specialty Nusantara
(3 outlet: Jakarta, Bandung, Bali). Konten: brand story, katalog 11 menu beserta
harga, galeri, Instagram feed, lokasi outlet, kontak. Kanal konversi: WhatsApp.

**Ini project HTML statis, bukan aplikasi.** Nol backend, API, database,
autentikasi, payment, dan build tooling. Tujuh file `code.html` yang
self-contained, Tailwind CSS via CDN, vanilla JS inline.

Jangan mengarang arsitektur yang tidak ada. Kalau tidak ada di source code, berarti
tidak ada.

---

## Before Making Changes

**WAJIB, dalam urutan ini:**

1. Baca `AGENTS.md` (file ini).
2. Baca `AI_CONTEXT/PROJECT_CONTEXT.md`.
3. Baca `AI_CONTEXT/ARCHITECTURE.md`.
4. Baca `AI_CONTEXT/CURRENT_STATE.md`.
5. Baca `AI_CONTEXT/DECISIONS.md`.
6. Baca `AI_CONTEXT/TODO.md`.
7. Baca `AI_CONTEXT/HANDOFF.md`.
8. Inspeksi file source yang relevan.
9. Jalankan `git status`.
10. Pakai Graphify bila tersedia dan task relevan.

Jangan mulai mengedit sebelum langkah 1-7 selesai. Kalau ada bagian yang tidak
cocok dengan source code, **source code yang benar** dan perbaiki dokumentasinya.

---

## Graphify Rules

Untuk task yang menyentuh arsitektur, dependency, component relationship, atau
data consistency antar halaman:

1. Jalankan Graphify untuk memahami dependency.
2. Identifikasi impact area.
3. Periksa hubungan file/module terkait.
4. **Verifikasi hasil Graphify dengan membaca source code.**
5. Jangan melakukan perubahan besar sebelum memahami dependency relevan.

### Setup di environment ini

```
Interpreter : C:\Users\user\AppData\Roaming\uv\tools\graphifyy\Scripts\python.exe
Binary      : C:\Users\user\.local\bin\graphify.exe
Versi       : 0.9.67
Output      : graphify-out/  (gitignored)
```

Rebuild: `graphify E:\Ngobat` · Incremental: `graphify E:\Ngobat --update`

### Kalau Graphify tidak tersedia

- Lanjutkan dengan inspeksi source code langsung.
- **Jangan mengarang hasil Graphify.**
- Dokumentasikan keterbatasan itu di `CURRENT_STATE.md`.

### Cara membaca hasil Graphify

- `graphify-out/GRAPH_REPORT.md` — god nodes, suggested questions, ringkasan
- `graphify-out/graph.json` — 558 node, 910 edge, 27 community
- `graphify-out/graph.html` — visualisasi interaktif, buka di browser tanpa server

Ringkasan temuan yang sudah tersedia di `HANDOFF.md` bagian `GRAPHIFY NOTES` —
**baca itu dulu** sebelum membuka graph, jangan sampai mengulang analisis yang
sudah dilakukan.

### Cargo cult

Graphify adalah alat bantu, **bukan pengganti verifikasi source code**. Graph bisa
salah, bisa salah baca gambar, dan bisa salah menyimpulkan. Kalau hasil Graphify
bertentangan dengan source code, **source code yang menang**.

---

## Development Rules

- **Hormati arsitektur yang ada** kecuali diminta lain. Static single-file + Tailwind
  CDN adalah kondisi yang disengaja untuk tahap ini.
- **Reuse komponen dan pola yang sudah ada.** Jangan bikin pola baru kalau ada yang
  sudah bekerja.
- **Jangan tambah dependency tanpa alasan.** Nol `package.json` disengaja supaya
  setiap halaman bisa berdiri sendiri sebagai file statis.
- **Jangan tulis ulang functionality yang sudah jalan** tanpa alasan.
- **Jangan ubah struktur folder** tanpa instruksi eksplisit.
- **Jangan modifikasi file yang tidak terkait** dengan task.
- **Jaga perubahan tetap fokus** pada yang diminta.
- **Jaga Bahasa Indonesia** di semua copy yang tampil ke user. Copy saat ini belum
  di-proofread native — perbaiki yang kamu sentuh.

---

## Data Integrity Rules

**Ini yang paling penting di project ini.**

`menu-ngobat.jpeg` adalah papan harga resmi yang dicetak untuk outlet. Itu sumber
kebenaran untuk katalog.

1. **Jangan pernah mengubah nama produk, harga, atau komposisi** tanpa cross-check
   ke `menu-ngobat.jpeg`.
2. **Jangan pernah menyalin data dari 6 halaman draft.** Lihat `DECISIONS.md` —
   halaman canonical dipilih sebagai sumber kebenaran setelah cocok 11/11 dengan papan
   resmi.
3. **Jangan memakai nomor `6281234567890`.** Itu placeholder dari generator. Nomor riil
   ada di halaman canonical dan `lokasi_kontak`.
4. **Jangan mengarang produk, harga, jam buka, atau alamat.** Kalau butuh data baru
   dan tidak ada sumbernya, tanyakan.
5. Kalau menemukan data yang saling bertentangan di source code, **dokumentasikan
   di `CURRENT_STATE.md`** — jangan diam-diam pilih satu.

### Aset brand

- `ngobat_logo.png` adalah logo resmi. Jangan ganti dengan placeholder.
- Kalau perlu membuat aset turunan (mis. varian horizontal), tetap harus berasal dari
  file ini, bukan digambar ulang.

---

## Security Rules

**Jangan pernah menuliskan** dalam kode maupun dokumentasi:

- API key, password, token, private key, secret, credential

Gunakan placeholder di dokumentasi:

```
WHATSAPP_BUSINESS_NUMBER=<required>
DATABASE_URL=<required>
```

Aturan tambahan:

- Jangan pernah commit `.env` yang berisi secret. `.gitignore` sudah mengaturnya.
- Kalau nanti ada input yang diproses, validasi selalu.
- Kalau nanti ada auth, jangan.route tanpa authorization check.
- Nomor WhatsApp dan alamat email yang ada di project ini adalah **kontak bisnis
  publik**, bukan credential. Boleh ditulis di dokumentasi.

---

## Testing Rules

Project ini **tidak punya** test runner, linter, atau typechecker. Nol `package.json`.
Jadi verifikasi berikut adalah reality:

### Yang bisa dijalankan otomatis

```powershell
git status
Select-String -Path (git ls-files) -Pattern 'BEGIN (RSA|OPENSSH) PRIVATE KEY|sk_live_|ghp_|AKIA'
```

Kalau kamu menambah tooling (linter, formatter, test runner), dokumentasikan di
`PROJECT_CONTEXT.md` dan `ARCHITECTURE.md` — jangan dibiarkan tidak tercatat.

### Yang wajib manual

1. Buka `code.html` di browser — desktop **dan** mobile width.
2. Uji setiap interaksi yang kamu sentuh (filter, modal, drawer, lightbox).
3. Cek Network tab: tidak ada 404 gambar, tidak ada error JS di Console.
4. Cek layout tidak pecah di 360px, 768px, 1280px.

### Aturan laporan

**Kalau sebuah command gagal, laporkan kegagalan itu apa adanya.** Jangan
menyembunyikan, jangan menulis "should work", jangan claiming berhasil tanpa
menjalankannya. Test yang tidak dijalankan dilaporkan sebagai tidak dijalankan.

---

## Documentation Rules — WAJIB SETIAP TASK SIGNIFIKAN

Project harus selalu menjaga dua hal sinkron:

```
SOURCE CODE  +  AI_CONTEXT
```

`AI_CONTEXT/` adalah portable memory project. Conversation history bukan satu-satunya
sumber context. AI agent baru harus bisa melanjutkan project hanya dengan source code,
`AGENTS.md`, dan `AI_CONTEXT/`.

### Setelah menyelesaikan task, update:

| File | Kapan wajib |
|---|---|
| `CURRENT_STATE.md` | **Setiap task selesai.** Wajib pertahankan `Currently In Progress` dan `Exact Next Step` akurat |
| `TODO.md` | **Setiap task selesai.** Pindahkan ke `Completed`, tambah task baru hanya jika benar-benar ditemukan |
| `CHANGELOG.md` | **Setiap task selesai.** Entri bertanggal dengan Added/Changed/Fixed/Removed |
| `DECISIONS.md` | Hanya kalau ada keputusan teknis baru |
| `ARCHITECTURE.md` | **Hanya** kalau sentuh arsitektur, database, API, auth, payment, folder structure, external service, atau dependency relationship |
| `HANDOFF.md` | **Wajib setiap task selesai** |
| `AGENTS.md` | Hanya kalau aturan project berubah |

### Jangan mengarang

- Jangan tulis task yang tidak berasal dari source code, konfigurasi, dokumentasi
  yang ada, atau investigasi aktual.
- Jangan tulis keputusan yang tidak pernah diambil. Kalau belum diputuskan, tulis
  di bagian `Open Decisions` dengan status `UNRESOLVED`.
- Jangan tulis changelog historis yang tidak ada buktinya. Untuk project ini,
  sejarah sebelum 2026-09-28 memang tidak ada — itu jujur dan harus ditulis begitu.

### Sebelum menyatakan selesai

1. Baca ulang source code yang berubah.
2. Pastikan dokumentasi cocok dengan source code.
3. Cek tidak ada informasi obsolete.
4. Cek tidak ada secret.
5. Cek semua path yang dirujuk benar-benar ada.
6. Cek tidak ada kontradiksi antar file `AI_CONTEXT/`.

---

## Git

```powershell
git status
git diff
```

Tampilkan perubahannya. **Jangan pernah** menjalankan `git reset`,
`git checkout --`, `git clean`, atau menghapus perubahan tanpa izin eksplisit.

Kalau diminta commit, commit harus mencakup:

- Source code yang relevan
- `AI_CONTEXT` yang relevan

dalam satu commit yang koheren. Format commit yang dipakai:

```
<type>: <deskripsi singkat>

<body: kenapa, jika perlu>
```

Type yang dipakai: `feat`, `fix`, `docs`, `chore`, `refactor`, `style`.

---

## Project-Specific Gotchas

Temuan yang akan membuat agent berikutnya salah kalau tidak tahu:

1. **`code.html` bukan file build output.** Edit langsung, tidak ada yang meng-generate
   ulang. Ng changes survive.
2. **Tailwind Config inline.** Setiap file punya `<script id="tailwind-config">` sendiri.
   Tidak ada shared config.
3. **Warna hardcoded.** `#FFE600` muncul 75x dan `#111111` 160x di halaman canonical
   sebagai arbitrary value, padahal sudah ada di config sebagai token. Kalau diubah,
   harus find-replace — atau lebih baik, refactor ke token dulu.
4. **`::-webkit-scrollbar { display: none }`** ada di halaman canonical. Render akan
   terlihat "menghilang" kalau CSS gagal load.
5. **Logo sekarang masih emoji `☕`** di 3 tempat. Itu placeholder, bukan keputusan
   desain.
6. **Semua gambar masih hotlink** ke `lh3.googleusercontent.com`. Dan halaman
   canonical hanya punya **4 gambar unik untuk 42 referensi** — 11 kartu produk
   sebenarnya cuma memakai 2 foto. Jangan tambah hotlink baru sebelum aset lokal siap.
7. **2 `screen.png` adalah stub 28 byte.** Bukan gambar valid. Jangan buka atau
   analisis sebagai gambar.
8. **Header/footer ada di 7 file secara terpisah.** Edit navigasi berarti edit semua.
9. **Dukungan browser tidak ada.** Ini websitetj-public, bukan proyek privacy-sensitive.
   Tetap saja jangan defecate data pribadi ke client tanpa alasan.
