# Architecture

## High Level Architecture

Project ini **tidak punya arsitektur aplikasi**. Tidak ada server, tidak ada API, tidak
ada database. Arsitekturnya hanya satu lapis:

```
Browser
  |
  +-- GET code.html            (static file, ~40-90 KB per halaman)
  |
  +-- <script> tailwind.config  (design tokens didefinisikan inline di HTML)
  |
  +-- fetch cdn.tailwindcss.com   -> CSS seluruh halaman
  +-- fetch fonts.googleapis.com  -> Quicksand, Plus Jakarta Sans, Material Symbols
  +-- fetch lh3.googleusercontent.com -> semua foto
  |
  v
Vanilla JS inline (dalam <body>)
  - baca data-* attribute dari DOM
  - toggle class Tailwind (hidden/flex, bg, text)
  - bangun URL wa.me lalu set href
  - tidak ada request ke server sendiri
```

Satu-satunya "data layer" adalah atribut `data-*` pada elemen HTML. Contoh dari halaman
canonical:

```html
<div class="menu-card" data-name="Es Kopi Susu Gula Aren"
     data-price="Rp18.000"
     data-desc="Espresso, susu, creamer, gula aren, es batu."
     data-category="cold"
     data-badge="Signature"
     data-image="https://lh3.googleusercontent.com/...">
```

JS membacanya saat user klik, lalu mengisi konten modal. Tidak ada fetch, tidak ada JSON,
tidak ada state management.

## Frontend Architecture

### Routing

**Tidak ada router.** Tujuh halaman terpisah, masing-masing file `code.html` yang
self-contained. Navigasi antar halaman pakai anchor (`#menu`, `#galeri`) di dalam satu
file, atau tidak ada sama sekali antar file (setiap file independen).

Routing yang ada hanya *in-page anchor* pada halaman canonical:

| Anchor | Section |
|---|---|
| `#beranda` | Hero |
| `#menu` | Menu dan harga |
| `#cerita` | Cerita Kami |
| `#galeri` | Galeri |
| `#instagram` | Instagram feed |
| `#lokasi` | Lokasi (3 outlet) |
| `#kontak` | Kontak (div di dalam `#lokasi`, bukan section terpisah) |

### Pages

Tujuh file independen. Hanya satu yang berlabel "official website".

| File | Baris | Peran |
|---|---|---|
| `ngopi_bareng_teman_official_website_consolidated_refined/code.html` | 1259 | **Canonical.** Satu halaman berisi 6 section + footer |
| `ngopi_bareng_teman_beranda_profil_perusahaan/code.html` | 548 | Draft awal. Body 100% identik dengan `global_navigation_footer_system` |
| `ngopi_bareng_teman_global_navigation_footer_system/code.html` | 548 | Eksperimen nav. Body identik dengan file di atasnya; header sticky + drawer |
| `ngopi_bareng_teman_menu_harga/code.html` | 516 | Draft fokus menu. Punya sticky filter + section susu alternatif |
| `ngopi_bareng_teman_galeri_instagram/code.html` | 454 | Draft fokus galeri. Punya lightbox dengan keyboard nav |
| `ngopi_bareng_teman_lokasi_kontak/code.html` | 444 | Draft fokus lokasi. Punya filter region, embedded map, 4 kanal kontak |
| `ngopi_bareng_teman_cerita_kami/code.html` | 345 | Draft fokus cerita. Tanpa JS sama sekali |

### Layouts

**Tidak ada layout terpisah.** Tidak ada template, tidak ada partial, tidak ada include.
Header dan footer di-copy-paste literal ke dalam setiap file. Konsekuensi: mengubah
header berarti mengedit 7 file.

Halaman canonical memakai container konsisten `max-w-[1240px] mx-auto px-4 md:px-8`.

### Components

**Tidak ada komponen dalam arti React/Vue.** Yang ada adalah pola class Tailwind yang
diulang. Pada halaman canonical ada 5 class sebagai titik genggam JS:

| Class | Dipakai untuk | Jumlah |
|---|---|---|
| `.menu-card` | Kartu produk | 11 |
| `.menu-filter-btn` | Tombol filter kategori | 3 |
| `.gallery-card` | Tile galeri | 6 |
| `.gallery-filter-btn` | Tombol filter galeri | 4 |
| `.nav-item` | Link navigasi desktop | 7 |

Pola yang berulang (bukan komponen): pill button, icon tile, card grid, eyebrow pill,
arrow link, social icon row.

### Hooks

**Tidak ada.** Tidak ada custom hook, tidak ada composable.

### State Management

**Tidak ada library state.** State adalah status visibility yang diBake ke class Tailwind.

State yang ada:

| State | Representasi | Owner |
|---|---|---|
| Drawer mobile terbuka/tutup | class `hidden` pada `#mobile-menu-drawer` | `toggleMobileMenu()` |
| Filter menu aktif | class `bg-[#FFE600] text-[#111111]` pada tombol aktif + class `hidden` pada kartu non-cocok | script filter |
| Filter galeri aktif | class `bg-[#FFE600]` pada tombol aktif + class `hidden` pada tile non-cocok | script filter |
| Modal produk terbuka/tutup | class `hidden` pada `#product-modal` | `openProductModal()` / `closeProductModal()` |
| Lightbox terbuka/tutup | class `hidden` pada `#lightbox-modal` | `openLightbox()` / `closeLightbox()` |
| Scroll body terkunci | class `overflow-hidden` pada `<body>` | semua handler modal |
| Section nav aktif | class `bg-[#FFE600]` pada `.nav-item` via IntersectionObserver | observer |

Tidak ada localStorage, sessionStorage, IndexedDB, atau cookie.

### Form Handling

**Tidak ada form.** Nol `<form>`, nol `<input>` di seluruh project. Satu-satunya "form" adalah
search icon di spec yang tidak pernah diimplementasikan.

### Validation

**Tidak ada.** Tidak ada validasi client maupun server, karena tidak ada input yang
diproses.

## Backend Architecture

**Tidak ada.** Nol API route, nol server action, nol service, nol middleware, nol
utilitas, nol autentikasi, nol otorisasi.

### API Routes

Tidak ada.

### Server Actions

Tidak ada.

### Services

Tidak ada. Satu-satunya "service" adalah link keluar: `wa.me`, `maps.google.com`,
`instagram.com`.

### Middleware

Tidak ada.

### Authentication / Authorization

Tidak ada. Lihat `PROJECT_CONTEXT.md`.

## Database Architecture

**Tidak ada.** Tidak ada engine, tidak ada ORM, tidak ada schema, tidak ada model,
tidak ada relationship, tidak ada index, tidak ada enum, tidak ada migration.

### Relationship antar model

Tidak berlaku. Yang ada hanya **relasi antar elemen HTML**, direpresentasikan lewat
atribut `data-*`:

| Relasi | Diimplementasikan sebagai |
|---|---|
| Produk punya kategori | `data-category="cold"` atau `data-category="hot"` pada `.menu-card` |
| Produk punya badge | `data-badge="Signature"` (string kosong = tidak ada badge) |
| Produk punya komposisi | `data-desc` |
| Foto galeri punya kategori | `data-category` pada `.gallery-card` |
| Tombol filter memetakan ke kartu | `data-category` tombol dibandingkan dengan `data-category` kartu |
| Card ke modal | Halaman canonical: listener terpasang per-card. Draft: `onclick="openDetailModal(...)"` inline |

**Pola yang dipakai untuk relasi produk ke filter**: pencocokan string.

```js
if (cat === 'all' || card.getAttribute('data-category') === cat) {
  card.classList.remove('hidden');
} else {
  card.classList.add('hidden');
}
```

Halaman `galeri_instagram` memakai substring match, yang lebih longgar:

```js
item.dataset.category.includes(filter)
```

Ini karena `data-category` di sana bisa berisi beberapa nilai
(mis. `interior-eksterior suasana-cerita`). Konsekuensi: filter `space` akan cocok
dengan kartu yang kategori pertamanya `interior-eksterior`.

## Dependency Architecture

Tidak ada import/export di project ini — tidak ada modul yang saling mengimpor.
Ketergantungan hanya di tiga arah:

```
code.html
   |
   +--> CDN tailwindcss        (runtime, blocking page styling)
   +--> Google Fonts           (runtime)
   +--> lh3.googleusercontent  (runtime, per-gambar)
```

Artinya **impact area setiap perubahan** sangat lokal, dengan dua pengecualian besar:

### Pengecualian 1: Header dan Footer terduplikasi di 7 file

Mengubah satu baris nav berarti mengedit 7 file secara manual. Ini coupling terkuat di
project ini dan satu-satunya bagian yang layak diekstrak jadi shared partial.

Bukti duplikasi: `beranda_profil_perusahaan` dan `global_navigation_footer_system`
memiliki body yang identik 100%.

### Pengecualian 2: Data menu terduplikasi di beberapa file

11 produk, 4 titik filter, dan angka filter (11/8/3) muncul di minimal 2 file.oi
mengubah harga harus menyinkronkan semua kemunculannya.

## Payment Architecture

**Tidak ada.** Tidak ada provider, tidak ada checkout flow, tidak ada order flow, tidak
ada payment creation, tidak ada callback/webhook, tidak ada payment status handling.

Pengganti yang dipilih adalah link `wa.me` dengan pesan ter-encode:

```js
const encodedMsg = encodeURIComponent(
  `Halo Ngopi Bareng Teman, saya ingin memesan ${name}. Apakah masih tersedia?`
);
modalWaBtn.href = `https://wa.me/628119700322?text=${encodedMsg}`;
```

Konsekuensi arsitektur: konversi happening di luar aplikasi. Tidak ada data order yang
tersimpan, tidak ada riwayat, tidak ada tracking. Semua "order" terjadi di WhatsApp.

## Email Architecture

**Tidak ada.** Tidak ada verification, reset password, order confirmation, invoice,
maupun notification.

## File Structure

```
E:\Ngobat\
├── .gitignore
├── AGENTS.md
├── design_specification.md                 <- duplikat byte-identik (lihat catatan)
│
├── ngobat_logo.png                        <- logo resmi, BELUM dipakai
├── menu-ngobat.jpeg                       <- papan harga resmi, source of truth
├── stitch_custom_design_implementation_and_PRD.zip   <- arsip impor, tidak diedit
│
├── AI_CONTEXT\                            <- dokumentasi portable (file ini)
│   ├── PROJECT_CONTEXT.md
│   ├── ARCHITECTURE.md
│   ├── CURRENT_STATE.md
│   ├── DECISIONS.md
│   ├── TODO.md
│   ├── CHANGELOG.md
│   └── HANDOFF.md
│
├── graphify-out\                          <- output knowledge graph (gitignored)
│   ├── graph.html
│   ├── graph.json
│   ├── GRAPH_REPORT.md
│   ├── manifest.json
│   └── cache\
│
└── stitch_custom_design_implementation\
    ├── design_specification.md            <- duplikat byte-identik
    │
    ├── warm_social_cafe\
    │   └── DESIGN.md                      <- design token YAML (60+ token)
    │
    ├── ngopi_bareng_teman_official_website_consolidated_refined\
    │   ├── code.html                      <- CANONICAL, 1259 baris
    │   └── screen.png
    │
    ├── ngopi_bareng_teman_beranda_profil_perusahaan\
    │   ├── code.html
    │   └── screen.png
    ├── ngopi_bareng_teman_global_navigation_footer_system\
    │   ├── code.html
    │   └── screen.png
    ├── ngopi_bareng_teman_menu_harga\
    │   ├── code.html
    │   └── screen.png                     <- STUB 28 byte
    ├── ngopi_bareng_teman_galeri_instagram\
    │   ├── code.html
    │   └── screen.png                     <- STUB 28 byte
    ├── ngopi_bareng_teman_lokasi_kontak\
    │   ├── code.html
    │   └── screen.png
    ├── ngopi_bareng_teman_cerita_kami\
    │   ├── code.html
    │   └── screen.png
    │
    ├── warm_inviting_cozy_indonesian_coffee_shop_interior_modern_aesthetic_wooden\
    │   └── screen.png                     <- moodboard
    ├── warm_smiling_friendly_young_indonesian_barista_crafting_latte_art_in_ceramic\
    │   └── screen.png                     <- moodboard
    ├── aesthetic_indonesian_iced_coffee_drink_es_kopi_susu_with_layered_milk_espresso\
    │   └── screen.png                     <- moodboard
    ├── aesthetic_hot_manual_brew_black_coffee_in_ceramic_cup_v60_filter_coffee\
    │   └── screen.png                     <- moodboard
    └── minimalist_modern_exterior_of_cozy_indonesian_coffee_shop_named_ngopi_bareng\
        └── screen.png                     <- moodboard
```

Direktori dengan awalan `aesthetic_`, `minimalist_`, `warm_` berisi **foto referensi
art direction**, bukan deliverable. Belum dipakai di halaman mana pun.

## Important Files

| File | Purpose | Dependency / Relationship | Important? |
|---|---|---|---|
| `stitch_custom_design_implementation/ngopi_bareng_teman_official_website_consolidated_refined/code.html` | Halaman utama canonical. 6 section, 5 fungsi JS, 11 produk, 6 foto galeri, mobile drawer, 2 modal, SEO JSON-LD | Sumber kebenaran untuk konten & data. Berisi semua data yang dirujuk halaman lain | **Ya — sentral** |
| `menu-ngobat.jpeg` | Papan harga cetak resmi. 11 produk, harga, komposisi, Packaging shot | Sumber kebenaran terverifikasi untuk data katalog. Cross-check 11/11 cocok dengan halaman canonical | **Ya — otoritatif** |
| `ngobat_logo.png` | Logo resmi brand. 1227x1282 px, RGB tanpa alpha, bg putih | Belum dipakai. Harus menggantikan 3 kemunculan emoji di halaman canonical | **Ya — belum terpasang** |
| `stitch_custom_design_implementation/warm_social_cafe/DESIGN.md` | 60+ design token (warna, tipografi, radius, spacing) dalam YAML frontmatter | Sumber token yang di-copy ke `tailwind.config` tiap halaman | Ya |
| `design_specification.md` (root) | Spec desain asli dari Planshet: warna, tipografi, layout desktop, responsive, daftar aset | Kontrak desain. Menjelaskan gap G1-G6 | Ya |
| `stitch_custom_design_implementation/design_specification.md` | **Duplikat byte-identik** dari file di root (hash SHA-256 sama) | Tidak menambah informasi. Kandidat dihapus | Rendah |
| `.../ngopi_bareng_teman_galeri_instagram/code.html` | Lightbox terbaik: prev/next, counter, keyboard Esc/←/→, `updateVisibleList()` | Sumber fitur untuk backport F9. **Jangan pakai datanya** (WA placeholder) | Sedang |
| `.../ngopi_bareng_teman_lokasi_kontak/code.html` | Filter region + embedded map + 4 kanal kontak | Sumber fitur untuk backport F13/F14/F16. **Jangan pakai datanya** | Sedang |
| `.../ngopi_bareng_teman_menu_harga/code.html` | Sticky filter bar + bento "Seduh Sesuai Selera Kamu" | Sumber fitur untuk backport. **Jangan pakai addon harganya** (tidak ada di papan resmi) | Sedang |
| `.../ngopi_bareng_teman_global_navigation_footer_system/code.html` | Sticky header + mobile drawer markup | Body identik dengan `beranda_profil_perusahaan`. Sumber drawer markup | Rendah |
| `.../ngopi_bareng_teman_beranda_profil_perusahaan/code.html` | Draft awal. Body identik dengan file nav | Produk fiktif (Rp 28.000/22.000/32.000/30.000). **Jangan dipakai** | Rendah |
| `.../ngopi_bareng_teman_cerita_kami/code.html` | Draft fokus cerita. Tanpa JS | Nyaris duplikat dari section `#cerita` halaman canonical | Rendah |
| `stitch_custom_design_implementation_and_PRD.zip` | Arxiv export Stitch asli | Sumber file yang sudah diekstrak. Jangan diedit, jangan dihapus | Sedang (arsip) |
| `graphify-out/graph.json` | Knowledge graph: 558 node, 910 edge, 27 community | Hasil analisis dependency untuk handoff ini | Sedang |
| `graphify-out/GRAPH_REPORT.md` | Laporan audit graph + god nodes + suggested questions | SQLite-like ringkasan analisis | Rendah |
