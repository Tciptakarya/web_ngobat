# Current State

> Status per **2026-09-28**. Didokumentasikan dari inspeksi langsung terhadap source code,
> bukan dari conversation history.

## Current Development Status

**Fase: implementasi berjalan.** Halaman canonical sekarang memakai logo resmi dan
11 foto produk asli, dan sudah diverifikasi render di browser.

- Halaman canonical di-update 2026-09-28: logo resmi terpasang, 27 gambar lokal,
  0 hotlink, 0 error console.
- Enam halaman draft **belum** disentuh — masih hotlink, masih tanpa logo.
- Tidak ada build tooling. Verifikasi = buka di browser.
- Dua `screen.png` masih stub 28 byte. `design_specification.md` masih duplikat.

## Last Completed Work

**Backport 3 fitur + publish ke GitHub (2026-09-28).**

1. **Lightbox di-upgrade**: prev/next, counter `n / total`, badge kategori,
   deskripsi, keyboard ArrowLeft/ArrowRight. Counter mengikuti filter aktif.
2. **Filter region outlet**: 4 pill + `data-region` pada 3 kartu.
3. **Kanal kontak ketiga** (Kunjungi Langsung) dari data outlet terverifikasi.
4. **Filter bar menu jadi sticky** di bawah header.
5. `data-desc` ditambahkan ke 6 kartu galeri; tile ke-6 di-retitle karena
   judul lamanya tidak cocok dengan gambarnya.
6. **Di-push ke GitHub** `github.com/Tciptakarya/web_ngobat`, branch `main`.
7. **Dideploy ke Vercel** `https://web-ngobat.vercel.app/`. Semula 404 karena
   tidak ada `index.html` di root, lalu diperbaiki dengan memindahkan halaman
   utama ke root. `.vercelignore` ditambahkan untuk memangkas payload 24,3 MB
   menjadi 2,6 MB.

Sebelum itu, di entri sebelumnya: logo resmi terpasang di 3 titik dan 27 gambar
pindah dari hotlink ke aset lokal.

## Currently In Progress

## Currently In Progress

Tidak ada. Task migrasi aset selesai dan terverifikasi.

## Current Problems

### SOLVED — Issue 1: Logo resmi tidak dipakai

**Status: selesai.** 3 placeholder emoji (header, mobile drawer, footer) diganti
`assets/brand/logo-mark.png` dan `logo-lockup.png`.

Hambatan yang ditemukan dan penyelesaiannya:

| Hambatan | Penyelesaian |
|---|---|
| `24bpp RGB`, background putih `#FEFEFE` tertanam | Alpha dibuat dari luminance + saturasi, pinggiran di-feather |
| Lockup vertikal tidak muat di header `h-20` | Header pakai **mark saja** (40px) + teks HTML Quicksand; footer pakai **lockup penuh** (tinggi 48px) |
| Kuning logo `#FEE820` vs token sistem `#FFE600` | **Token `#FFE600` dipertahankan** — rationale di `DECISIONS.md` |

Varian yang sudah dibuat tapi belum dipakai: `logo-horizontal.png`,
`logo-wordmark.png`, `favicon-256.png`.

Catatan: line-art hitam pada logo praktis tidak terlihat di latar gelap. Logo hanya
dipakai di header dan footer yang berlatar putih — jangan dipakai di atas blok
hitam atau kuning tanpa perlakuan tambahan.

### SOLVED — Issue 2: Halaman canonical hanya punya 4 gambar unik

**Status: selesai.** 42 referensi gambar yang menunjuk ke 4 URL berbeda kini menunjuk
ke 22 aset lokal berbeda.

| Slot | Sebelum | Sesudah |
|---|---|---|
| 11 kartu produk | 2 foto (8 produk memakai foto yang sama) | 11 foto distinct dari papan resmi |
| 6 tile galeri | 4 foto (2 duplikat) | 6 foto distinct dari moodboard |
| 4 tile Instagram | 3 foto | 4 foto |
| Hero | foto barista | `barista-craft.jpg` 1000x1000 |
| OG / Twitter / JSON-LD | hotlink | `og-es-kopi-susu.jpg` 1200x630 |

Catatan: foto produk sumber hanya ~150x200 px (satu sel dari JPEG 1125x773). Cukup
untuk kartu produk (`h-48` = 192px) tapi **tidak** untuk lightbox besar. Karena itu
lightbox memakai foto moodboard resolusi tinggi, bukan foto produk.

### Issue 3: Data katalog di enam halaman draft bertentangan

#### Symptoms

Enam dari tujuh halaman berisi data yang tidak cocok dengan papan harga resmi
(`menu-ngobat.jpeg`). `index.html` (dulu `official_website_consolidated_refined`)
cocok **11 dari 11**.

#### Suspected Cause

Halaman-halaman draft dibuat pada iterasi desain yang berbeda, sebelum data disinkronkan
dengan papan harga cetak.

#### Investigation Already Done

Papan harga resmi `menu-ngobat.jpeg` dibaca langsung dan di-cross-check satu per satu
dengan setiap halaman. Katalog resmi:

| # | Produk | Kategori | Harga | Komposisi |
|---|---|---|---|---|
| 1 | Es Kopi Susu Gula Aren | Cold | 18K | Espresso, Susu, Creamer, Gula Aren, Es Batu |
| 2 | Es Kopi Susu Hazelnut | Cold | 18K | Espresso, Susu, Creamer, Hazelnut, Es Batu |
| 3 | Es Kopi Susu Caramel | Cold | 18K | Espresso, Susu, Creamer, Caramel, Es Batu |
| 4 | Es Kopi Susu Butter Scotch | Cold | 18K | Espresso, Susu, Creamer, Butter Scotch, Es Batu |
| 5 | Es Kopi Susu Mochaccino | Cold | 18K | Espresso, Susu, Creamer, SKM Coklat, Es Batu |
| 6 | Ice Coffee Berry | Cold | 15K | Espresso, Syrop Berry, Soda, Es Batu |
| 7 | Japanese Ice Coffee | Cold | 15K | Kopi dengan metode Japanese |
| 8 | Es Kopi Hitam | Cold | 15K | Espresso, Es Batu, Air |
| 9 | Hot Kopi Saring Lawas | **Hot** | 15K | Kopi dengan metode V60 |
| 10 | Hot Kopi Seduhan Klasik | **Hot** | 10K | Kopi Giling, Air Panas |
| 11 | Kopi Susu Jadoel | **Hot** | 18K | Espresso, Air, SKM |

8 Cold + 3 Hot. Struktur ini persis cocok dengan filter di halaman canonical.

**Kontradiksi per halaman draft:**

| Data | Halaman canonical | `beranda` | `lokasi_kontak` | `menu_harga` |
|---|---|---|---|---|
| WhatsApp | `628119700322` (riil) | `6281234567890` | `628119700322` | `6281234567890` |
| Harga kopi susu | Rp 18.000 | Rp 22.000–32.000 | — | Rp 18.000 (cocok) |
| Jam Senopati | 07.00–23.00 | 08.00–22.00 | 07.00–23.00 | 07.00–23.00 |
| Alamat Dago | Dago Heritage No. 18, Coblong | Jl. Ir. H. Juanda No. 128 | Dago Heritage No. 18 | — |
| Alamat Canggu | Batu Bolong No. 88 | Batu Bolong No. 58 | Batu Bolong No. 88 | — |
| Label outlet | "Bandung Hub" / "Bali Hub" | — | "Bandung Highland" / "Island Oasis" | — |

**Placeholder `6281234567890` muncul 3x** di masing-masing file `beranda`,
`cerita_kami`, `galeri_instagram`, `menu_harga`. File `global_navigation_footer_system`
punya 1x placeholder **dan** 5x nomor riil (terCampur dalam satu file).

#### Graphify Findings

Dua community khusus devoted pada kontradiksi ini:

- **C1 `Draft Data Contradictions (fake products)`** — 47 node, cohesion 0.061. Mengandung
  node produk fiktif seperti `Product: Caramel Macchiato - Rp 28.000`,
  `Product: Matcha Espresso Fusion - Rp 32.000 (does not exist in the consolidated
  11-item menu)`, dan edge AMBIGUOUS yang menyatakan nilai berbeda per halaman.
- **C12 `Outlet Address & Hours Contradictions`** — 20 node, cohesion 0.163. Menyimpan
  pasangan klaim jam buka dan alamat yang saling bertentangan antar halaman.

God node `menu_ngobat_price_board` (degree 17) adalah bridge antara katalog resmi dan
seluruh klaim produk di graph.

#### Current Status

**Belum dikerjakan.** Tidak ada perbaikan data di kode.

#### Recommended Next Investigation

Jangan coba memperbaiki enam halaman draft. ajudam lihat `DECISIONS.md` — halaman itu
sudah diputuskan tidak boleh dipakai sebagai referensi data.

---

### Issue 4: Angka addon susu tidak ada di papan resmi

#### Symptoms

File `menu_harga/code.html` punya bento "Seduh Sesuai Selera Kamu" yang mencantumkan:

- Tingkat kawan poin gula: Normal 100% / Less 50% / No Sugar 0%
- Susu alternatives: **Oat +Rp5.000**, **Soy +Rp4.000**
- 100% Kopi Nusantara: Flores Bajawa, Gayo, Temanggung

#### Suspected Cause

Konten generik yang ditambahkan untuk memperkaya halaman, tanpa diverifikasi ke katalog.

#### Investigation Already Done

Papan harga resmi diperiksa ulang. Tidak ada satu pun penyebutan susu nabati, oat, soy,
maupun level gula di 11 item. Papan hanya menampilkan harga fix per item.

#### Graphify Findings

Edge `..._screen_addon_pricing_tier` → `menu_ngobat_product_catalogue` ditandai
**AMBIGUOUS confidence 0.2** — hipotes Graphify bahwa pricing tier ini Bertentangan
dengan katalog resmi. Ininal.

#### Current Status

**Belum diverifikasi ke client.** Perlu dikonfirmasi apakah addon susu ini benar-benar
ditawarkan outlet atau tidak.

#### Recommended Next Investigation

Tanyakan ke client. Jangan ditayangkan sebelum dikonfirmasi.

---

### Issue 5: Badge item 9 ambigu di papan resmi

#### Symptoms

Di `menu-ngobat.jpeg`, 10 dari 11 produk memakai tag harga **kuning**. Hanya satu yang
berbeda:

**Hot Kopi Saring Lawas (15K) memakai tag harga HITAM.**

#### Suspected Cause

Ini keputusan highlight yang disengaja. Tapi toolbar design spec menetapkan arti warna
tag yang berbeda:

| Tag | Warna spec | Arti |
|---|---|---|
| Best Seller | Kuning `#FFE600` + teks hitam | Best Seller |
| New | Hitam `#111111` + teks putih | New |

#### Investigation Already Done

Halaman canonical dan `menu_harga` sama-sama menandai **dua** item sebagai
`data-badge="Signature"`, yaitu:
- `Es Kopi Susu Gula Aren` (Cold, 18K)
- `Hot Kopi Saring Lawas` (Hot, 15K)

Jadi implementasi saat ini memakai label "Signature" yang **tidak ada di design spec
mana pun**, dan mengabaikan warna hitam yang ada di papan resmi.

#### Graphify Findings

Community **C4 `Official Price Board & Product Catalogue`** (35 node, cohesion 0.175)
mengandung node `menu_ngobat_black_price_tag_highlight` yang secara eksplisit mencatat
bahwa item ini satu-satunya dengan tag non-kuning, dan mengaitkannya dengan
`menu_ngobat_yellow_price_tag_style` sebagai pembeda yang disengaja.

#### Current Status

**Belum diputuskan.** Butuh konfirmasi client.

#### Recommended Next Investigation

Tanyakan: apakah item 9 itu mau ditandai "New", "Signature", atau "Best Seller"?

---

### Issue 6: Dua file `screen.png` adalah stub rusak

#### Symptoms

Dua file screenshot 0-byte yang terindeteksi paling awal:

```
28 bytes  ngopi_bareng_teman_galeri_instagram\screen.png
28 bytes  ngopi_bareng_teman_menu_harga\screen.png
```

Keduanya file PNG yang tidak valid (bukan 0 byte, tapi 28 byte — signature PNG tanpa payload).

#### Suspected Cause

Screenshot gagal di-render saat proses export Stitch.

#### Investigation Already Done

Dikonfirmasi ukuran persisnya lewat `Get-ChildItem`. Kedua halaman punya `code.html`
yang utuh, jadi tidak ada data yang hilang — hanya visual referensinya.

#### Graphify Findings

Chunk ekstraksi untuk kedua gambar ini **menyatakan terus terang** bahwa file PNG-nya
adalah stub dan isinya direkonstruksi dari `code.html` di direktori yang sama, bukan dari
inspeksi piksel. Node `..._screen_render` di kedua chunk diberi label yang menyebut
limitation tersebut secara eksplisit, dan confidence diturunkan ke INFERRED/AMBIGUOUS.
Ini perilaku yang benar dan tidak seharusnya dianggap sebagai analisis visual.

#### Current Status

**Tidak memperbaiki.** Sumber render PNG-nya tidak dapat diregenerasi tanpa tooling Stitch.

#### Recommended Next Investigation

Biarkan. Kalau butuh referensi visual, buka `code.html` langsung di browser.

---

### Issue 7: `design_specification.md` terduplikasi

#### Symptoms

File yang sama ada di dua tempat:

```
design_specification.md                                    (root)
stitch_custom_design_implementation/design_specification.md
```

#### Suspected Cause

File ada di dalam paket Stitch, lalu diekstrak keluar juga.

#### Investigation Already Done

Hash kedua file dibandingkan dengan `Get-FileHash`. **Identik** (SHA-256 sama persis,
prefix `D55C0F999850D667`).

#### Graphify Findings

Community **C20 `Duplicated Spec Tokens`** (8 node) terbentuk persis dari pengulangan
ini. Graphify melaporkan `semantically_similar_to` edge pada 4 konsep identik
(`Hero Section`, `Category Menu Floating Bar`, `Mobile Responsive Variations`,
`UI/UX Design Specification`) yang menghubungkan kedua file sebagai "surprising connection".

#### Current Status

**Belum dihapus.** File tidak dihapus karena instruksi handoff melarang mengubah source.

#### Recommended Next Investigation

Hapus salah satu (disarankan yang di dalam `stitch_custom_design_implementation/`),
dengan konfirmasi dulu karena mengubah isi repo.

---

### Issue 8: Enam halaman tidak punya kontrol navigasi mobile

#### Symptoms

Lima dari tujuh halaman tidak punya tombol hamburger maupun drawer mobile:

| Halaman | Mobile drawer? |
|---|---|
| `index.html` (root) | Ya |
| `global_navigation_footer_system` | Ya |
| `menu_harga` | **Tidak** |
| `galeri_instagram` | **Tidak** |
| `lokasi_kontak` | **Tidak** |
| `cerita_kami` | **Tidak** |
| `beranda_profil_perusahaan` | **Tidak** |

Pada empat halaman tanpa drawer, di lebar layar mobile **tidak ada cara untuk berpindah
section** — link nav desktop disembunyikan oleh `hidden xl:flex`, dan tidak ada
alternatifnya.

#### Suspected Cause

Halaman-halaman ini adalah draft fokus, dibuat tanpaBRIEFING mobile yang sama seperti
halaman canonical.

#### Investigation Already Done

`beranda_profil_perusahaan` dan `cerita_kami` dikonfirmasi **nol baris JavaScript**
sepatinya. Halaman `menu_harga`, `galeri_instagram`, dan `lokasi_kontak` punya JS tapi
tidak ada markup drawer.

#### Graphify Findings

Community **C19 `Global Navigation & Footer System`** (10 node, cohesion 0.244)
mengisolasi `JS function toggleMobileMenu(open)`, `MOBILE NAVIGATION DRAWER DIALOG
#mobile-menu-drawer`, dan `Global sticky header` sebagai referensi markup yang bisa
di-backport.

Community **C22 `Shared Footer Component (draft pages)`** (7 node, cohesion 0.286)
mendokumentasikan bahwa empat halaman draft memakai komponen footer yang byte-identik
satu sama lain.

#### Current Status

**Belum dikerjakan.** Tidak menjadi trade-off kalau halaman-halaman ini memang tidak akan
dipublish — tapi itu keputusan yang belum diambil.

#### Recommended Next Investigation

Tentukan dulu: apakah enam halaman draft akan dipublish, atau dibuang dan hanya
canonical yang dilanjutkan? Jawabannya menentukan apakah backport ini perlu.

---

## Working Features

Fitur yang terbukti berfungsi, diverifikasi lewat inspeksi source:

| # | Feature | Verifikasi |
|---|---|---|
| 1 | Mobile drawer: `toggleMobileMenu()`, `aria-expanded`, body scroll lock, close on backdrop/Escape/link click | Kode lengkap, alur konsisten |
| 2 | Filter menu 3 kategori | `data-category` match + toggle class `hidden`, 11 card ter-cover |
| 3 | Filter galeri 4 kategori | Sama seperti filter menu, 6 tile ter-cover |
| 4 | Product detail modal | `openProductModal()` mengisi 5 field dari `data-*`, `closeProductModal()` mengembalikan state |
| 5 | Lightbox | `openLightbox()`/`closeLightbox()`, gallery card punya `role="button"` dan `tabindex="0"` plus handler keyboard Enter/Space |
| 6 | Navigasi keyboard global | Listener `keydown` untuk Escape menutup drawer, modal, dan lightbox sekaligus |
| 7 | Active nav via IntersectionObserver | `rootMargin: '-20% 0px -70% 0px'`, observes `main > section` |
| 8 | Aksesibilitas dasar | Skip-to-content link, `aria-label` di tombol ikon, `aria-expanded`/`aria-controls` di drawer, `role="dialog"` + `aria-modal` di modal, focus-visible ring utility |
| 9 | SEO | Open Graph lengkap, Twitter Card, JSON-LD `CafeOrCoffeeShop` dengan 3 outlet sebagai `department` |
| 10 | Dynamic year | `document.getElementById('current-year').textContent = new Date().getFullYear()` |
| 11 | Responsive | Breakpoint `sm` / `md` / `lg` / `xl` dipakai konsisten; mobile sticky bar muncul hanya di `< md` |

## Broken Features

Tidak ada fitur yang terbukti rusak secara fungsional. Semua interaksi di halaman
canonical sudah diuji langsung di browser pada 2026-09-28 (filter 3/8/11, filter
galeri 2/6, modal produk, lightbox, mobile drawer, scroll lock) dan lolos.

Yang tersisa bersifat substantif, bukan crash:

| Item | Jenis | Status |
|---|---|---|
| 6 halaman draft | Konten tidak tervalidasi + masih hotlink | Belum disentuh |
| `screen.png` stub | Artefak export Stitch | Tidak bisa diperbaiki tanpa tooling Stitch |
| Foto produk resolusi rendah | Sumber ~150x200 px | Cukup untuk kartu, tidak untuk lightbox |

## Current Blockers

**Dua pertanyaan yang masih menunggu client:**

1. **Item Hot Kopi Saring Lawas** - badge-nya "New" (mengikuti warna hitam di
   papan), "Signature" (status quo), atau "Best Seller"?
2. **Susu alternatif Oat +Rp5.000 / Soy +Rp4.000** - benar-benar ada di outlet,
   atau konten generik yang harus dibuang?

**Blocker baru: alamat email dan handle TikTok.** Keduanya hanya ada di halaman
draft, dan file draft itu sendiri tidak konsisten soal handle Instagram. Jadi tidak
dipasang di halaman canonical. Butuh data resmi dari client.

Dua pertanyaan lama (warna kuning, fate 6 halaman draft) tidak lagi memblokir
pekerjaan teknis, tapi tetap perlu konfirmasi.

## Exact Next Step

## Exact Next Step

Aset dan logo sudah selesai. Berikutnya adalah backport fitur.

**Langkah 1 (tidak butuh jawaban client): Hapus duplikat.**
`design_specification.md` di root dan di `stitch_custom_design_implementation/` identik
(SHA-256 sama). Hapus yang duplikat, sisakan satu. Tidak mengubah perilaku apa pun.

**Langkah 2 (tidak butuh jawaban client): Backport 3 fitur dari draft ke canonical.**

| Dari file | Fitur | Kondisi di canonical sekarang |
|---|---|---|
| `galeri_instagram` | Lightbox prev/next + counter + keyboard (Esc/←/→) | `#lightbox-modal` cuma punya tombol close |
| `lokasi_kontak` | Filter region + 4 kanal kontak | `#kontak` cuma div di dalam `#lokasi`, 2 kanal saja |
| `menu_harga` | Sticky filter bar saat scroll | `#menu` filter-nya ikut ter-scroll |

Semuanya peningkatan murni — tidak mengubah data produk, tidak mengubah scope.

**Langkah 3 (tergantung jawaban client):** pasang logo + aset ke 6 halaman draft,
tentukan badge item 9, putuskan addon susu, putuskan warna kuning.

Alasan urutan: langkah 1-2 aman dan tidak bergantung pada keputusan bisnis, sementara
langkah 3 semuanya bergantung pada jawaban client.
