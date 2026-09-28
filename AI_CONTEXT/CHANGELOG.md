# Changelog

Format: `## [Tanggal]` → `Added` / `Changed` / `Fixed` / `Removed` / `Technical Notes`
/ `Important Decisions`.

---

## 2026-09-28 (4) — Halaman utama dipindahkan ke root agar bisa di-deploy

### Fixed

- **Vercel menyajikan `404 NOT_FOUND` di `web-ngobat.vercel.app`.** Deployment-nya
  sendiri sukses (status `Ready`), dan halamannya bisa diakses di URL dalam
  `stitch_custom_design_implementation/ngopi_bareng_teman_official_website_consolidated_refined/code.html`
  (HTTP 200, 85.567 byte). Penyebabnya: tidak ada `index.html` di root repository.
  Vercel menyajikan `/` dengan cara mencari `index.html` di root, dan tidak
  menemukannya — jadi file-nya ter-deploy, tapi tidak ada yang jadi halaman depan.

### Changed

- **Halaman produksi pindah ke root repository.**
  `stitch_custom_design_implementation/ngopi_bareng_teman_official_website_consolidated_refined/code.html`
  → `index.html`
- **Folder `assets/` pindah ke root**, di sebelah `index.html`. Dipakai `git mv`
  supaya git mencatatnya sebagai rename, bukan hapus + tambah.
- Path di dalam `index.html` **tidak berubah** — tetap `assets/...` dan tetap
  relatif. Karena `index.html` dan `assets/` kini berdua di root, path relatif ini
  masih resolve tanpa menyentuh satu baris pun di dalam HTML.
- Struktur direktori didokumentasikan ulang di `ARCHITECTURE.md`; referensi path
  di `AGENTS.md`, `CURRENT_STATE.md`, `DECISIONS.md`, `HANDOFF.md`, dan `TODO.md`
  semuanya diperbarui.

Folder `stitch_custom_design_implementation/ngopi_bareng_teman_official_website_consolidated_refined/`
tidak dihapus — `screen.png` (render desain dari Stitch) masih di sana dan berguna
sebagai referensi tampilan. Yang pindah hanya `code.html` dan `assets/`.

Entri changelog sebelumnya **sengaja tidak ditulis ulang**. Isinya tetap menggambarkan
kondisi pada saat itu, ketika halamannya memang masih berada di dalam foldernya.

### Technical Notes

**Kenapa bukan `vercel.json` rewrite.** URL aset di dalam HTML bersifat relatif, dan
browser me-resolve-nya terhadap URL dokumen, bukan terhadap path file aslinya di
server. Kalau `/` di-rewrite ke file yang berada di dalam folder, `assets/brand/logo-mark.png`
tetap diminta dari `/assets/brand/logo-mark.png` dan 404. Menghindarinya berarti
memindahkan `assets/` juga — hasilnya identik dengan memindahkan `index.html`, tapi
dengan path yang jauh lebih panjang dan rapuh kalau foldernya someday di-rename.

**Kenapa bukan redirect.** Butuh satu hop tambahan, dan yang dibuka tamu adalah
halaman kedua, bukan yang pertama.

**Yang masih ikut ter-deploy.** `stitch_custom_design_implementation_and_PRD.zip`
(10 MB), 5 foto moodboard (7,4 MB), 7 `screen.png`, dan 6 halaman draft — semuanya
naik ke Vercel karena belum ada `.vercelignore`. Tidaknya situs di root, jadi
`_headers`/path lama tidak lagi relevan, tapi file beratnya masih. Belum diperbaiki
karena di luar scope task ini — tercatat di `TODO.md`.

### Removed

Tidak ada. Tidak ada file yang dihapus; `git mv` menghasilkan riwayat rename.

---

## 2026-09-28 (3) — Backport 3 fitur + publish ke GitHub

### Added

- **Lightbox di-upgrade** di halaman canonical. Sebelumnya hanya tombol close.
  Sekarang punya: tombol sebelumnya / selanjutnya, counter `n / total`, badge
  kategori, deskripsi foto, dan navigasi keyboard panah kiri / kanan.
- **`data-desc` pada 6 kartu galeri** plus `data-desc`/perbaikan `data-title`/
  `aria-label`/caption pada tile komposit varian, karena judul lamanya
  ("Artisan Cangkir Kopi Hangat") tidak cocok dengan gambarnya.
- **Filter region outlet**: 4 pill (Semua Outlet 3 / Jakarta / Bandung / Bali)
  di atas grid outlet, plus `data-region` pada 3 kartu.
- **Kanal kontak ketiga "Kunjungi Langsung"** (Jakarta - Bandung - Bali),
  dibangun dari data outlet yang sudah terverifikasi.
- **`<link rel="icon">`** sudah ada (dari entri sebelumnya).

### Changed

- **Filter bar menu jadi sticky.** Semula ikut ter-scroll; sekarang `sticky top-20`
  dengan `backdrop-blur` dan border bawah, menempel di bawah header yang juga sticky.
  Alasannya: grid menu 11 kartu terlalu tinggi sehingga filter jadi tak terjangkau.
- Counter lightbox sekarang **mengikuti filter kategori yang aktif**. Kalau filter
  `barista` aktif, counter menunjukkan `1 / 2`, bukan `1 / 6`.

### Fixed

- Judul dan caption tile galeri ke-6 tidak lagi Infant dengan gambarnya.
- `alt` kosong pada `lightbox-img` kini selalu terisi karena `renderLightbox()`
  mengisinya dari `data-title` setiap kali berganti foto.

### Technical Notes

**Kenapa kanal email dan TikTok tidak ditambahkan.** `lokasi_kontak/code.html`
memiliki `hello@ngopibarengteman.com` dan `@ngopibarengteman.official` (TikTok),
dan file itu juga mencantumkan Instagram sebagai `@ngopibarengteman` (tanpa
`.official`) — tidak konsisten dengan halaman canonical. Karena `AGENTS.md`
melarang mengarang data kontak yang belum ada sumbernya, kedua kanal itu
dibiarkan dan ditandai sebagai TODO yang menunggu konfirmasi client. Kanal ketiga
yang ditambahkan memakai data outlet yang sudah terverifikasi di halaman ini.

**Deteksi kartu outlet.** Ada 7 elemen dengan class kartu yang identik di file ini
(3 kartu outlet + 4 kartu di section Cerita Kami). Penandaan `outlet-card` dilakukan
dengan mencari posisi relatif terhadap URL Google Maps masing-masing outlet, bukan
dengan mencocokkan class.

**Verifikasi browser** (Chrome, server lokal):

| Uji | Hasil |
|---|---|
| Filter region | 3 kartu, region jakarta/bandung/bali, filter ke 1 kota, kembali ke 3 |
| Filter menu sticky | `sticky` + `top-20` terpasang |
| Lightbox | buka di index kartu yang diklik, prev/next, wrap-around dua arah |
| Lightbox + filter | counter `1 / 2` saat filter barista aktif |
| Keyboard | ArrowRight/ArrowLeft navigasi, Escape menutup + unlock scroll |
| Kanal kontak | 3 (dari sebelumnya 2) |
| Regresi | filter menu 8/11, modal harga Rp18.000, 0 error JS, 0 request gagal |

**Tidak teruji:** screenshot visual lightbox dan region filter. Browser tool
menolak `screenshot` dengan pesan "needs a visible tab" karena window desktop
tidak sedang aktif. Verifikasi di atasseluruhnya lewat DOM/API, bukan pixel.

### Removed

Tidak ada.

---

## 2026-09-28 (2) — Logo resmi + 11 foto produk + galeri

Halaman canonical di-upgrade dari aset placeholder ke aset brand asli. **Ini commit
pertama yang mengubah source code aplikasi.**

### Added

**Aset logo** (`ngopi_bareng_teman_official_website_consolidated_refined/assets/brand/`,
8 file, 902 KB):

| File | Ukuran | Dipakai di |
|---|---|---|
| `logo-mark.png` | 256x256 | Header (40px), mobile drawer (36px) |
| `logo-lockup.png` | 300x597 | Footer (tinggi 48px) |
| `logo-wordmark.png` | 300x300 | Belum dipakai |
| `logo-horizontal.png` | 640x311 | Belum dipakai |
| `favicon-64/128/256.png` | 64/128/256 | `favicon-64` dan `favicon-128` dipakai di `<head>` |

**11 foto produk** (`assets/products/`, 267 KB total): masing-masing 600x600 JPEG,
di-crop dari `menu-ngobat.jpeg` dan dikomposit ke kartu krem `#FFFDF5` dengan tepi
di-feather.

```
es-kopi-susu-gula-aren      es-kopi-susu-butter-scotch  japanese-ice-coffee
es-kopi-susu-hazelnut       es-kopi-susu-mochaccino     es-kopi-hitam
es-kopi-susu-caramel        ice-coffee-berry             hot-kopi-saring-lawas
hot-kopi-seduhan-klasik     kopi-susu-jadoel
```

**9 foto lain** (`assets/gallery/` 6 file 1014 KB, `assets/hero/` 3 file 569 KB):
6 tile galeri, 1 image hero 1000x1000, 1 wide band 1600x900, 1 OG image 1200x630.
Semuanya diturunkan dari 5 foto moodboard resolusi tinggi (1200x896 / 1376x768 /
896x1200), bukan dari stok CDN.

Total `assets/`: 28 file, 2.751 MB.

### Changed

- **Halaman canonical** `.../ngopi_bareng_teman_official_website_consolidated_refined/code.html`:
  89.226 → 78.451 byte (turun 12%, karena URL hotlink yang sangat panjang diganti path
  lokal yang pendek)
  - 3 placeholder emoji `☕` diganti logo asli (header, drawer, footer)
  - 27 `<img>` diganti aset lokal
  - 11 `data-image` (modal produk) diganti
  - 6 `data-src` (lightbox) diganti
  - 3 image URL di `<head>` (OG, Twitter Card, JSON-LD) diganti
  - 2 `<link rel="icon">` ditambahkan
  - 2 `alt` yang tidak cocok dengan gambarnya diperbaiki (hero, tile flavour-range)
- **Lokasi aset**: `assets/` dipindahkan dari root project ke
  `stitch_custom_design_implementation/ngopi_bareng_teman_official_website_consolidated_refined/assets/`
  supaya path relatif resolve dan folder halaman bisa di-deploy mandiri
- Dokumentasi `AI_CONTEXT/`: `CURRENT_STATE.md`, `DECISIONS.md`, `TODO.md`,
  `HANDOFF.md` diperbarui

### Fixed

- **Logo tidak pernah dipakai** — 3 titik sudah terpasang
- **11 kartu produk hanya menampilkan 2 foto berbeda** — sekarang 11 foto distinct
- **42 referensi gambar menunjuk ke 4 URL hotlink** — sekarang 22 aset lokal berbeda
- **2 `alt` salah** — hero menyebut "Es Kopi Susu" padahal gambarnya barista;
  tile galeri menyebut "Cangkir keramik kopi hangat" padahal gambarnya komposit
  4 varian susu

### Removed

Tidak ada file yang dihapus dari repo. Dua file turunan yang sempat dibuat lalu
dibuang karena tidak berguna: `assets/gallery/outlet-corner.jpg` (duplikat komposisi dari
`exterior.jpg` — centering tidak menghasilkan komposisi berbeda) dan
`assets/brand/favicon-512.png` (terlalu besar, 235 KB).

### Technical Notes

**Deteksi bounding box produk.** Grid 6x2 di `menu-ngobat.jpeg` dideteksi otomatis
dengan skor `max(saturasi, 2 x gelapness)` per kolom, lalu gap < 14 piksel di-merge.
Hasil: 6 blob di row 1, 5 blob di row 2 = 11 produk. Row 2 ternyata melompati kolom 4,
terbukti dari posisi teks produk yang tidak centered di kolom 4.

**Batas Y diperbaiki dua kali.** Crop pertama masih menyertakan tag harga kuning di
atas setiap produk, karena tag itu juga saturasi tinggi. Tag price terdeteksi di
y 63-98 (row 1) dan y 422-459 (row 2) lewat filter kuning; batas crop digeser ke
y 114 dan y 474.

**Alpha logo.** Background `#FEFEFE` dihapus dengan alpha gabungan luminance dan
saturasi, supaya tepi kuning blob tetap opaque sementara garis hitam yang samar
di luar jadi transparan. Semua varian diverifikasi dengan superimposisi pada tiga
latar: putih, `#111111`, dan `#FEE820`.

**Warna kuning dipertahankan.** Diukur tiga sumber: spec `#FFE600`, logo `#FEE820`
(median 72.342 piksel), tag papan `#FCDC17`. Papan adalah foto cetak sehingga warnanya
sudah tergeser. Token `#FFE600` dipertahankan — rationale lengkap di `DECISIONS.md`.

**Verifikasi browser** (Chrome, `python -m http.server` lokal):

| Uji | Hasil |
|---|---|
| Filter menu | Semua 11 / Cold 8 / Hot 3, nama hot benar |
| Filter galeri | Semua 6 / barista 2 |
| Modal produk | Judul, harga, komposisi, foto, link WA `628119700322` benar |
| Badge | Tampil untuk Gula Aren & Saring Lawas, disembunyikan untuk yang lain |
| Lightbox | Buka dengan gambar + judul yang benar, tutup bersih |
| Mobile drawer | Buka, `aria-expanded` true, body scroll lock, 7 link, tutup bersih |
| Gambar | 27 `<img>`, 27 lokal, 0 hotlink, 0 tanpa `alt` |
| Console | 0 error |
| Network | 0 request gagal |

**Tidak teruji:** render mobile sesungguhnya. Browser tool yang tersedia tidak punya
fungsi resize viewport, jadi hanya logika drawer dan class responsif yang diverifikasi
(`xl:hidden` pada hamburger, `hidden xl:flex` pada nav desktop, `md:hidden` pada sticky
bar) — bukan tampilan visualnya.

### Important Decisions

- **Logo: mark di header, lockup penuh di footer.** Lockup asli vertikal (rasio 0.957)
  tidak muat di header `h-20`; teks brand jadi HTML Quicksand agar tajam dan
  aksesibel. Detail di `DECISIONS.md`
- **Token `#FFE600` dipertahankan** despite logo `#FEE820` — 75 kemunculan, keuntungan
  visual minimal, dan koreksi yang benar sebenarnya ada di sisi logo. Detail di
  `DECISIONS.md`
- **Logo hanya untuk latar terang.** Garis hitamnya praktis hilang di atas `#111111`,
  sudah diverifikasi secara visual

---

## 2026-09-28 (1) — Handoff dokumentasi awal

Hari pertama project ini masuk version control. **Tidak ada perubahan pada kode
aplikasi** — seluruh commit ini adalah implementasi (impor) dan dokumentasi.

### Added

- `AGENTS.md` di root — instruksi permanen untuk AI coding agent
- `AI_CONTEXT/PROJECT_CONTEXT.md` — tujuan project, stack, fitur, role
- `AI_CONTEXT/ARCHITECTURE.md` — arsitektur aktual, struktur file, tabel file penting
- `AI_CONTEXT/CURRENT_STATE.md` — 8 issue dengan gejala / suspected cause / investigasi
  / temuan Graphify / status / next investigation
- `AI_CONTEXT/DECISIONS.md` — 5 keputusan (`CONFIRMED` / `DERIVED`) + 6 open decisions
- `AI_CONTEXT/TODO.md` — task di Critical / Next / Planned / Bugs / Technical Debt
- `AI_CONTEXT/CHANGELOG.md` — file ini
- `AI_CONTEXT/HANDOFF.md` — ringkasan untuk agent berikutnya
- `.gitignore` — mengecualikan `graphify-out/`, OS/editor noise, dan pola secret
- Git repository diinisialisasi, commit pertama `5b5e871` (26 file)
- `stitch_custom_design_implementation/` — hasil ekstrak ZIP Stitch, 22 file
- `graphify-out/` — knowledge graph hasil analisis:
  - `graph.html` (710 KB) — visualisasi interaktif, buka di browser tanpa server
  - `graph.json` (940 KB) — 558 node, 910 edge, 44 hyperedge
  - `GRAPH_REPORT.md` (64 KB) — laporan audit, god nodes, suggested questions
  - `manifest.json` — untuk incremental `--update`
  - `cache/stat-index.json` — cache ekstraksi semantik

### Changed

Tidak ada file source yang diubah.

Satu perubahan struktural: `stitch_custom_design_implementation_and_PRD.zip` tadinya
satu-satunya sumber code; sekarang isinya juga ada sebagai folder di working tree.
**File ZIP asli tidak disentuh dan tidak dihapus.**

### Fixed

Tidak ada.

### Removed

Tidak ada. Tidak ada file yang dihapus.

### Technical Notes

**Pipeline Graphify.** Dijalankan penuh terhadap 24 file yang terdeteksi
(10 dokumen, 14 gambar, 0 file kode — `code.html` diklasifikasikan sebagai dokumen):

| Tahap | Hasil |
|---|---|
| Detect | 24 file, ~423k token kata, 0 `skipped_sensitive` |
| Chunk 1 | 3 design spec `.md` → 81 node, 156 edge, 3 hyperedge |
| Chunk 2 | 7 `code.html` → 141 node, 225 edge, 3 hyperedge |
| Chunk 3 | `ngobat_logo.png` → 20 node, 39 edge, 4 hyperedge |
| Chunk 4 | `menu-ngobat.jpeg` → 25 node, 93 edge, 4 hyperedge |
| Chunk 5-11 | 7 `screen.png` render → 236 node, 340 edge, 27 hyperedge |
| Chunk 12 | 5 moodboard → 55 node, 99 edge, 3 hyperedge |
| **Total** | **558 node, 952 edge, 44 hyperedge** |
| Build | 558 node, **910 edge**, 27 community (undirected) |
| AST | Kosong — tidak ada file kode yang terdeteksi |

**Graph health check.** Ditemukan 40 dangling-endpoint edge, 0 missing-endpoint, 0
self-loop, 2 collapsed undirected edge. Ini **expected**: chunk ekstraksi sengaja
mereferensikan node lintas-chunk (`brand_identity`, `canonical_page`,
`header_logo_placement`) yang hanya terdefinisi di chunk lain. Ini menghasilkan
"surprising connections" di community `Draft Data Contradictions` dan
`Outlet Address & Hours Contradictions` — justru berguna, karena itulah cara kontradiksi
antar halaman terdeteksi. Tidak dianggap corruption.

**Cost tracking.** 0 input / 0 output token tercatat — ekstraksi semantik dijalankan
lewat subagent, bukan lewat API key, jadi `graphify` tidak mengukur biayanya.
Tidak ada `GEMINI_API_KEY` / `GOOGLE_API_KEY` di environment.

**Sampling warna logo.** Diamilk dengan `System.Drawing.Bitmap.GetPixel()` pada grid
5x6 di area kuning. Hasil: `#FEE820` (rentang `#FDE71A`–`FFE920`). Ini sampling, bukan
color picker, jadi ada kemungkinan bias. Perlu konfirmasi client sebelum dijadikan
token.

**Verifikasi nol secret.** Repository diperiksa: tidak ada `.env`, `*.pem`, `*.key`,
`credentials*`, `*secret*`, `*token*`. Tidak ada `Authorization` header, tidak ada
password di markup. Nomor WhatsApp yang ada adalah kontak bisnis publik, bukan
credential.

### Important Decisions

Lihat `DECISIONS.md` untuk detail lengkap. Ringkas:

- **Halaman canonical adalah sumber kebenaran** untuk konten dan data
  (`CONFIRMED`). Enam halaman draft hanya boleh dipakai sebagai referensi fitur.
- **Papan harga `menu-ngobat.jpeg` adalah sumber kebenaran katalog** (`DERIVED`).
  Cross-check 11/11 cocok dengan halaman canonical.
- **`ngobat_logo.png` adalah logo resmi** (`DERIVED`), belum terpasang.
- **Ekstrak ZIP + git init** (`CONFIRMED`) — supaya dokumentasi bisa mereferensikan
  path yang valid dan ada version control.
- **Arsitektur static single-file dipertahankan** untuk tahap ini (`DERIVED`).
  Tidak ada migrasi framework atau build step.

6 hal masih `UNRESOLVED` dan menunggu client — lihat `DECISIONS.md` bagian
`Open Decisions`.

---

## [Sebelum 2026-09-28] — Sejarah yang tidak direkam

Tidak ada riwayat sebelum commit ini. Project tidak pernah berada di version
control, jadi tidak ada changelog, tidak ada commit, dan tidak ada jejak perubahan
sebelumnya.

Yang bisa direkonstruksi dari artefak yang ada:

| Fakta | Bukti |
|---|---|
| Papan harga dicetak dan difoto | `menu-ngobat.jpeg` |
| Logo resmi dibuat | `ngobat_logo.png` |
| Spec desain ditulis di luar repo | `design_specification.md` |
| Ekspor dari Google Stitch | `stitch_custom_design_implementation_and_PRD.zip` |
| 7 halaman di-render | 7 folder dengan `code.html` + `screen.png` |
| 2 render gagal | `screen.png` 28 byte di `menu_harga` dan `galeri_instagram` |

**Tidak diketahui** dan tidak boleh diasumsikan: siapa yang membuat halaman, urutan
pengerjaannya, kapan spec dulu incompatible, atau mengapa 2 screenshot gagal. Jangan
mengarang detail ini.
