# AI HANDOFF

> Untuk AI coding agent yang mengambil alih project ini. Baca file ini dulu, lalu
> `CURRENT_STATE.md` untuk detail teknis.

## READ THESE FILES FIRST

1. `AGENTS.md`
2. `AI_CONTEXT/PROJECT_CONTEXT.md`
3. `AI_CONTEXT/ARCHITECTURE.md`
4. `AI_CONTEXT/CURRENT_STATE.md`
5. `AI_CONTEXT/DECISIONS.md`
6. `AI_CONTEXT/TODO.md`
7. `AI_CONTEXT/CHANGELOG.md` (kalau perlu konteks historis)

Tidak perlu file ini untuk mulai — ini ringkasannya.

## PROJECT

Website marketing untuk **Ngopi Bareng Teman**, kedai kopi specialty Nusantara dengan 3
outlet (Jakarta, Bandung, Bali). Isinya: brand story, katalog 11 menu beserta harga,
galeri, Instagram feed, lokasi outlet, kontak.

Kanal konversi: **WhatsApp**. Bukan e-commerce.

**Ini bukan aplikasi.** Nol backend, nol API, nol database, nol auth, nol payment.
Tujuh file HTML mandiri, Tailwind via CDN, vanilla JS inline. Nol build tooling —
loop dev = buka `code.html` di browser.

## CURRENT STATE

Fase implementasi. Halaman canonical memakai logo resmi dan 11 foto produk asli,
sudah diverifikasi render di browser.

- 27 gambar lokal, **0 hotlink**, 0 tanpa `alt`, 0 console error.
- Enam halaman draft **belum** disentuh — masih hotlink, masih tanpa logo.
- Tidak ada build tooling. Verifikasi = buka di browser.

## LAST COMPLETED

**Deploy diperbaiki + payload dipangkas (2026-09-28).**

- Halaman produksi dipindah ke root: `index.html` + `assets/`. Sebelumnya Vercel
  menyajikan 404 karena tidak ada `index.html` di root.
- `.vercelignore` dibuat: 24,3 MB -> 2,6 MB. 6 halaman draft, arsip ZIP, aset
  brand sumber, dan dokumentasi AI tidak lagi terkirim ke Vercel.
- Live di `https://web-ngobat.vercel.app/` (HTTP 200, title benar).
- `PROJECT_CONTEXT.md` bagian Deployment diisi; `AGENTS.md` dapat blok
  `Deployment Rules`.

Sebelum itu: lightbox + region filter + sticky filter bar, lalu push ke GitHub.

## CURRENTLY WORKING ON

**Tidak ada.** Tidak ada task yang sedang berjalan.

## KNOWN ISSUES

| # | Issue | Status |
|---|---|---|
| 1 | Logo resmi tidak dipakai | **SELESAI** - 3 titik terpasang |
| 2 | 42 referensi gambar hanya 4 URL unik | **SELESAI** - 22 aset lokal berbeda |
| 3 | Data katalog di 6 halaman draft bertentangan | Belum - terkunci keputusan |
| 4 | Addon susu (Oat/Soy) tidak ada di papan resmi | Belum - tunggu client |
| 5 | Badge Hot Kopi Saring Lawas ambigu | Belum - tunggu client |
| 6 | 2 `screen.png` stub 28 byte | Tidak bisa diperbaiki |
| 7 | `design_specification.md` duplikat | Belum - perlu konfirmasi hapus |
| 8 | 6 halaman tanpa mobile drawer | Bergantung keputusan fate draft |

## BLOCKERS

**4 pertanyaan yang harus dijawab client sebelum implementasi berikutnya:**

1. **Hot Kopi Saring Lawas** — badge-nya "New" (mengikuti tag hitam di papan resmi),
   "Signature" (status quo), atau "Best Seller" (mengikuti warna kuning di spec)?
2. **Susu alternatif Oat +Rp5.000 / Soy +Rp4.000** — benar-benar ada di outlet, atau
   konten generik yang harus dibuang?
3. **Warna kuning** — sistem pakai `#FFE600`, logo resmi `#FEE820`. Mana yang acuan?
4. **Enam halaman draft** — dipublish (perlu backport), atau dibuang (hanya canonical)?

Kamu **tidak perlu menunggu** untuk mengerjakan 3 task di `TODO.md` bagian `Next`
pertama (hapus duplikat, crop aset produk, backport 3 fitur). Semuanya aman dan tidak
bergantung pada jawaban di atas.

## IMPORTANT DECISIONS

Detail di `DECISIONS.md`. Yang wajib diketahui:

**1. Halaman canonical adalah sumber kebenaran.**
```
index.html   (root repository)
```
Enam halaman draft **tidak boleh dipakai sebagai referensi data** — hanya fitur.
Alasannya: canonical cocok 11/11 dengan papan harga resmi; `beranda` memuat 4 produk
fiktif (Rp 28.000/22.000/32.000/30.000) yang tidak ada di katalog.

**2. `menu-ngobat.jpeg` adalah sumber kebenaran katalog.**
Nama, harga, komposisi. Cross-check 11/11.

**3. `ngobat_logo.png` adalah logo resmi.** Belum terpasang.

**4. Arsitektur static single-file dipertahankan.** Tidak ada migrasi framework atau
build step pada tahap ini.

## DO NOT CHANGE

- **Jangan pakai data dari 6 halaman draft.** Copy dari `beranda_profil_perusahaan`,
  `cerita_kami`, atau `menu_harga` akan memasukkan harga dan nomor WA yang salah.
- **Jangan hapus `stitch_custom_design_implementation_and_PRD.zip`** - arsip
  tidak termodifikasi dari export Stitch.
- **Jangan ubah harga, komposisi, atau nama produk** tanpa cross-check ke
  `menu-ngobat.jpeg`.
- **Jangan pakai nomor `6281234567890`** - placeholder, bukan nomor asli.
- **Jangan kembalikan logo ke placeholder emoji** - `assets/brand/` sudah jadi
  sumbernya. Kalau butuh varian baru, turunkan dari `ngobat_logo.png`, jangan
  gambar ulang.
- **`index.html` dan `assets/` ada di root dan harus berpindah bersama.**
  Path aset relatif (`assets/...`). Kalau dipisah, semua gambar rusak.
- **Jangan tambah dependency atau tooling tanpa alasan.**
- **Jangan reorganisasi struktur folder** tanpa instruksi eksplisit.
- **Jangan ubah warna `#FFE600`** tanpa konfirmasi client - sudah diputuskan
  sementara untuk dipertahankan, rationale di `DECISIONS.md`.

## GRAPHIFY NOTES

Graphify 0.9.67, output di `graphify-out/` (gitignored — regenerate dengan
`/graphify E:\Ngobat` bila perlu). Interpreter:
`C:\Users\user\AppData\Roaming\uv\tools\graphifyy\Scripts\python.exe`

**558 node, 910 edge, 44 hyperedge, 27 community.**

> **Graph sudah BASI.** Graph ini dibangun sebelum aset dimigrasi, jadi tidak
> merepresentasikan folder `assets/` maupun 28 file baru di dalamnya. Jalankan
> `graphify E:\Ngobat --update` kalau butuh data dependency yang akurat. Ringkasan
> di bawah tetap berguna sebagai peta relationship antar halaman, tapi jangan
> dipakai untuk menghitung file atau aset.

### God nodes (degree tertinggi)

| Node | Degree | Artinya |
|---|---|---|
| `..._warm_social_cafe_design_warm_social_cafe_design_system` | 37 | Design token set adalah pusat gravitasi project — semua halaman meng-copy-nya |
| `..._official_website_consolidated_refined_code_page` | 20 | Halaman canonical, bridge ke semua halaman lain |
| `..._ngopi_bareng_teman_menu_harga_code_menu_grid` | 17 | Grid 11 produk |
| `menu_ngobat_price_board` | 17 | **Papan harga resmi — bridge antara katalog dan seluruh klaim produk** |
| `..._beranda_profil_perusahaan_screen_render` | 15 | Draft homepage |
| `..._official_website_consolidated_refined_code_menu_grid` | 13 | Katalog di halaman canonical |
| `..._beranda_profil_perusahaan_code_page` | 12 | Draft page |
| `menu_ngobat_product_catalogue` | — | Katalog resmi |

### Community yang paling berguna

| ID | Nama | n | Cohesion | Kenapa penting |
|---|---|---|---|---|
| **C1** | Draft Data Contradictions | 47 | 0.061 | **Semua data fiktif & harga salah.** Node produk yang "does not exist in the consolidated 11-item menu" |
| **C12** | Outlet Address & Hours Contradictions | 20 | 0.163 | **Pasangan klaim alamat/jam yang bertentangan** antar halaman |
| **C4** | Official Price Board & Product Catalogue | 35 | 0.175 | 11 produk + node `menu_ngobat_black_price_tag_highlight` (anomali badge) |
| **C11** | Official Logo Asset Constraints | 20 | 0.147 | Constraint teknis logo: alpha, aspect, clearspace, min-size |
| **C0** | Brand Photography & Art Direction | 55 | 0.051 | 5 moodboard → konsep `brand_photography_style` dll |
| **C19** | Global Navigation & Footer System | 10 | 0.244 | Markup drawer mobile siap backport |
| **C22** | Shared Footer Component | 7 | 0.286 | Footer byte-identik di 4 halaman draft |
| **C20** | Duplicated Spec Tokens | 8 | — | Sumber `design_specification.md` yang terduplikasi |

### Impact area sebelum perubahan besar

Karena tidak ada import/export, impact ordinarily sangat lokal. **Tiga pengecualian:**

1. **Header/footer terduplikasi di 7 file.** Edit satu baris nav = edit 7 file.
2. **Data katalog terduplikasi.** Harga muncul di minimal 2 file + filter count
   hardcoded di label tombol (`Semua (11)`, `Cold (8)`, `Hot (3)`).
3. **`tailwind.config` inline di setiap file.** Ubah token = edit 7 file.

### Catatan integritas graph

40 dangling-endpoint edge. Ini **expected, bukan corruption**: chunk ekstraksi sengaja
mereferensikan node lintas-chunk (`brand_identity`, `canonical_page`,
`header_logo_placement`) yang hanya terdefinisi di chunk lain. fearing dampaknya,
beberapa node konseptual hanya muncul sebagai target edge, bukan sebagai node utuh.
0 missing-endpoint, 0 self-loop, 2 collapsed undirected edge.

## NEXT ACTION

Deploy sudah beres. Sisa pekerjaan:

```
1. Hapus design_specification.md yang duplikat (SHA-256 identik)

2. Kanal kontak email + TikTok - TERTAHAN.
   Data hanya ada di halaman draft dan tidak konsisten.
   Butuh konfirmasi client dulu.

3. Opsional: buang 5 aset tak terpakai (~885 KB)
```

Kalau nanti menambah halaman produksi baru (mis. `menu.html`), taruh di root
bersama `index.html` dan tambahkan ke navigasi. Jangan taruh di dalam
`stitch_custom_design_implementation/` - folder itu sudah dikecualikan di
`.vercelignore`.

## VERIFICATION

Karena nol tooling, verifikasi ini yang realistis:

### Otomatis (yang ada)

```powershell
# 1. Sisa file untracked
git status

# 2. Tidak ada secret yang bocor
Select-String -Path (git ls-files) -Pattern 'BEGIN (RSA|OPENSSH) PRIVATE KEY|sk_live_|ghp_|AKIA'
Select-String -Path (git ls-files) -Pattern '6281234567890'   # harus kosong di canonical

# 3. Hitung asset hotlink
Select-String -Path '.../code.html' -Pattern 'lh3\.googleusercontent\.com' | Measure-Object

# 4. Regenerate graph kalau struktur berubah
graphify E:\Ngobat --update
```

### Manual (wajib — tidak ada test runner)

1. Buka `code.html` di browser, **desktop dan mobile width**.
2. Klik filter menu → pastikan jumlah kartu benar (8 / 3 / 11).
3. Klik kartu produk → pastikan modal terbuka, isi benar, link WhatsApp terisi nama
   produk.
4. Klik filter galeri → buka lightbox → coba tombol ← → dan Escape.
5. Di mobile: buka hamburger drawer, klik link, pastikan body scroll terkunci dan
   drawer tertutup.
6. Pastikan tidak ada 404 di Network tab untuk gambar.

### Setelah perubahan

- Update `CURRENT_STATE.md` (status, masalah baru, next step)
- Pindahkan task selesai di `TODO.md` ke `Completed`
- Tambah entri di `CHANGELOG.md`
- Tambah entri di `DECISIONS.md` **hanya** kalau ada keputusan teknis baru
- Update `ARCHITECTURE.md` **hanya** kalau menyentuh arsitektur, folder structure,
  atau dependency
- Update `HANDOFF.md` — **wajib setiap kali task selesai**
- `git status`, lalu tampilkan perubahannya

**Jangan pernah** `git reset`, `git checkout --`, atau menghapus perubahan tanpa izin.

Kalau diminta commit, commit harus mencakup source code **dan** `AI_CONTEXT` yang
relevan dalam satu commit.
