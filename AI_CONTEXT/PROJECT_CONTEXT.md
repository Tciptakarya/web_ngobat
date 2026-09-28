# Project Context

## Project Name

Ngopi Bareng Teman — website kedai kopi specialty Nusantara dengan 3 outlet (Jakarta, Bandung, Bali).

## Project Purpose

Marketing / company-profile website untuk kedai kopi "Ngopi Bareng Teman". Isinya
menampilkan brand, katalog menu beserta harga, cerita perusahaan, galeri, kanal sosial,
lokasi outlet, dan kontak. Tujuan akhirnya konversi pengunjung menjadi pelanggan lewat
WhatsApp — **bukan** e-commerce.

## Business Goal

Membangun keberadaan digital kedai agar calon pelanggan bisa:
1. Mengenali brand (kenapa "Ngopi Bareng Teman").
2. Melihat menu dan harga tanpa harus datang.
3. Mencari outlet terdekat.
4. Menghubungi via WhatsApp untuk pesan, tanya kapasitas, booking event, atau kolaborasi.

Kanal konversi tunggal saat ini: **WhatsApp**. Tidak ada keranjang belanja, checkout,
pembayaran, atau sistem order online.

## Current Project Status

**Fase: prototype desain / handoff.** Bukan aplikasi produksi.

- Tidak ada build tooling sama sekali — nol `package.json`, nol bundler, nol test runner.
- Semua halaman adalah file HTML tunggal mandiri (Tailwind via CDN, vanilla JS inline).
- Tujuh halaman sudah ada. Satu dianggap canonical, enam masih draft dengan data yang
  saling bertentangan.
- Aset logo resmi sudah tersedia di repo tapi belum dipakai di halaman mana pun.
- Git history baru satu commit (impor).
- Dokumentasi AI di folder `AI_CONTEXT/` dibuat pada 2026-09-28.

## Technology Stack

| Lapisan | Teknologi | Catatan |
|---|---|---|
| Markup | HTML5 | Satu file per halaman, self-contained |
| Styling | Tailwind CSS via `cdn.tailwindcss.com` | Runtime JIT, bukan build. Config inline `<script id="tailwind-config">` |
| Ikon | Material Symbols Outlined (Google Fonts) | Ikon filled, bukan SVG line-art seperti di spec |
| Font | Google Fonts: Quicksand, Plus Jakarta Sans | Heading = Quicksand, body = Plus Jakarta Sans |
| Interaksi | Vanilla JavaScript inline | Tanpa framework, tanpa build step |
| Animasi | Tailwind `transition-*`, `animate-pulse`, `animate-ping` | CSS saja |
| Aset | `<img>` eksternal ke `lh3.googleusercontent.com` | Semua gambar masih hotlink |
| Server | Tidak ada | Cukup static file server |

**Tidak ada sama sekali:** backend, API, database, ORM, auth, session, payment gateway,
layanan email, CI/CD, container, package manager, linter, test runner.

## Frameworks

Tidak ada framework aplikasi. Yang berframework hanya Tailwind CSS, dan itu di-pull
dari CDN saat runtime — bukan dependency yang di-install.

## Libraries

Semua via CDN, tanpa `node_modules`:

| Library | Cara | Dipakai untuk |
|---|---|---|
| `tailwindcss` | `cdn.tailwindcss.com?plugins=forms,container-queries` | Seluruh styling |
| `Material Symbols Outlined` | Google Fonts variable font | Semua ikon |
| `Quicksand` | Google Fonts 500/600/700 | Heading (`font-headline`, `font-display`) |
| `Plus Jakarta Sans` | Google Fonts 400/500/600/700 | Body (`font-body`, `font-sans`) |

Plugin Tailwind yang aktif: `forms`, `container-queries`.

## Database

**Tidak ada.** Tidak ada database engine, tidak ada ORM, tidak ada schema, tidak ada
migration. Seluruh data (produk, outlet, kontak) di-hardcode sebagai literal HTML dan
atribut `data-*`.

Konsekuensi yang harus dipahami agent berikutnya: menambah produk atau outlet berarti
mengedit markup secara manual, bukan insert ke database.

## Authentication

**Tidak ada.** Tidak ada login, tidak ada sesi, tidak ada token, tidak ada user account.
Seluruh konten publik dan statis.

## Authorization

**Tidak ada.** Tidak ada peran, tidak ada pemeriksaan permission. Konsekuensinya: apa pun
yang ditulis ke `code.html` akan terlihat oleh siapa pun yang membuka halamannya.

## Payment

**Tidak ada integrasi payment.** Tidak ada Midtrans/Xendit/QRIS dinamis, tidak ada
checkout, tidak ada webhook, tidak ada status pembayaran.

Satu-satunya penyebutan pembayaran bersifat informasional di section FAQ:
`QRIS, Kartu Debit, Transfer Bank` plus uang tunai.

## Email

**Tidak ada pengiriman email.** Ada satu alamat email yang ditampilkan sebagai teks
kontak di file `lokasi_kontak`, tapi tidak ada mailto automation dan tidak ada service.

## External Services

Semua dependency eksternal di-load saat runtime dari browser:

| Service | Dipakai untuk | Risiko |
|---|---|---|
| `cdn.tailwindcss.com` | CSS seluruh site | Site tidak ter-style sama sekali jika CDN down |
| `fonts.googleapis.com` / `fonts.gstatic.com` | Quicksand, Plus Jakarta Sans, Material Symbols | Typography hilang |
| `lh3.googleusercontent.com` | Semua foto produk, galeri, dan hero | Gambar hilang. Hotlink ke CDN Google, bukan aset milik project |
| `wa.me` | Link WhatsApp | External handoff |
| `maps.google.com` | Link "Buka di Google Maps" | External handoff |
| `instagram.com` | Link profil | External handoff |

Tidak ada API key, tidak ada token, tidak ada credential apa pun di project ini.

## Deployment

**Sudah live di Vercel.** `index.html` di root repository adalah entry point, dan
Vercel otomatis men-deploy setiap kali ada push ke branch `main`.

```
https://web-ngobat.vercel.app/
```

Repo GitHub: `github.com/Tciptakarya/web_ngobat` (branch `main`).

| Aspek | Nilai |
|---|---|
| Hosting | Vercel, static, tanpa build step |
| Trigger | otomatis, setiap push ke `main` |
| Build command | tidak ada. Nol `package.json`, nol framework |
| Entry point | `index.html` (wajib ada di root, kalau tidak Vercel 404) |

**`.vercelignore`** membatasi apa yang terkirim: hanya `index.html`, `assets/`, dan
`README.md` (2,6 MB dari total repo 24,3 MB). Yang disembunyikan: 6 halaman draft,
arsip ZIP, aset brand sumber, dan dokumentasi AI. Alasannya tertulis di dalam
`.vercelignore` itu sendiri.

Tidak ada `vercel.json`, `netlify.toml`, `_headers`, `CNAME`, GitHub Actions,
maupun `Dockerfile`. Tidak perlu satu pun untuk static site tanpa build step.

## Development Environment

| Item | Nilai |
|---|---|
| OS | Windows 11 |
| Shell | PowerShell 7 (`pwsh`) |
| Node | v26.7.0 (terpasang, tapi project tidak memakainya) |
| npm | 12.0.2 (terpasang, tapi project tidak memakainya) |
| Python | 3.14.4 (`py -3`) |
| Graphify | 0.9.67, di-install via uv tool |
| Editor | bebas; HTML statis tanpa build |

Karena nol build tooling, loop pengembangan = buka `code.html` di browser. Tidak ada
`npm run dev`, tidak ada hot reload.

## Important Dependencies

Tidak ada `package.json`, jadi tidak ada dependency yang di-declare. Seluruh dependency
berupa URL CDN yang hardcoded di dalam setiap `code.html`. Konsekuensi: tidak ada cara
mengaudit versi, dan tidak ada lockfile.

## Important Environment Variables

**Tidak ada sama sekali.** Tidak ada `.env`, tidak ada `process.env`, tidak ada
substitusi saat build.

`.gitignore` sudah dikonfigurasi mengabaikan `.env`, `.env.*`, `*.pem`, `*.key` jika nanti
project butuh secret, tapi saat ini tidak ada satu pun file tersebut di repo.

Kalau suatu saat nanti ditambahkan backend, gunakan placeholder di dokumentasi — jangan
pernah tulis nilai asli:

```
WHATSAPP_BUSINESS_NUMBER=<required>
DATABASE_URL=<required>
```

---

## Main Features

Fitur yang benar-benar ada di source code:

| # | Feature | Ada di | Status |
|---|---|---|---|
| F1 | Sticky header, 7 link navigasi, tombol WhatsApp | consolidated, semua halaman | Working |
| F2 | Mobile navigation drawer (toggle, `aria-expanded`, body scroll lock, close on Escape) | consolidated, `global_navigation_footer_system` | Working |
| F3 | Hero dengan headline dua warna | consolidated | Working |
| F4 | Katalog menu: grid 11 produk (8 cold, 3 hot) | consolidated, `menu_harga` | Working |
| F5 | Filter kategori menu (Semua / Cold / Hot) | consolidated, `menu_harga` | Working |
| F6 | Product detail modal dengan tombol order WhatsApp per produk | consolidated, `menu_harga` | Working |
| F7 | Galeri foto: 6 tile plus filter kategori | consolidated, `galeri_instagram` | Working |
| F8 | Lightbox foto | consolidated, `galeri_instagram` | Working |
| F9 | Lightbox dengan prev/next, counter, dan navigasi keyboard (Esc / panah kiri / kanan) | `galeri_instagram` saja | Working, belum di-backport |
| F10 | Instagram feed grid | consolidated, `galeri_instagram` | Working |
| F11 | Section Cerita Kami (4 Nilai + 4 Pillar) | consolidated | Working |
| F12 | 3 kartu outlet plus link Google Maps | consolidated, `lokasi_kontak` | Working |
| F13 | Filter berdasarkan region (Semua / Jakarta / Bandung / Bali) | `lokasi_kontak` saja | Working, belum di-backport |
| F14 | Embedded Google Map | `lokasi_kontak` saja | Working, belum di-backport |
| F15 | FAQ 3 item (statis, bukan accordion) | consolidated, `lokasi_kontak` | Working |
| F16 | 4 kanal kontak (WA, IG, email, TikTok) | `lokasi_kontak` | Working |
| F17 | Footer 12 kolom | semua halaman | Working |
| F18 | Mobile sticky action bar (Menu / Lokasi / WhatsApp) | consolidated | Working |
| F19 | Active-section nav indicator via IntersectionObserver | consolidated saja | Working |
| F20 | SEO: Open Graph, Twitter Card, Schema.org JSON-LD `CafeOrCoffeeShop` | consolidated saja | Working |
| F21 | Skip-to-content link dan focus-visible ring | consolidated | Working |

Fitur yang ada di spec tapi belum ada di kode (scope gap):

| # | Feature | Sumber spec | Kenapa belum ada |
|---|---|---|---|
| G1 | Shopping cart dan tombol "Order Now" | `design_specification.md` bab 2A/2B | Diganti "Chat via WhatsApp" di semua halaman |
| G2 | Section "Our Favourite" carousel | `design_specification.md` bab 2D | Diimplementasikan sebagai grid statis `#menu-grid` |
| G3 | Floating category bar yang overlap hero | `design_specification.md` bab 2C | Tidak ada. Kategori jadi filter pill row di dalam section menu |
| G4 | Kategori: Flavour / Non Coffee / Signature / Cold Drinks / Pastry | `design_specification.md` bab 2C | Diganti Cold Coffee (8) dan Hot Coffee (3) |
| G5 | Ikon SVG line-art | `design_specification.md` bab 4 | Diganti Material Symbols (filled) |
| G6 | Nav label "Flavour" | `design_specification.md` bab 2A | Tidak ada di nav mana pun |

## User Roles

**Tidak ada role.** Project ini fully public static site tanpa autentikasi. Siapa pun yang
membuka URL bisa melihat dan menginteraksi dengan seluruh konten.

Satu-satunya peran yang bisa diidentifikasi adalah peran pengunjung:

| Peran | Kemampuan |
|---|---|
| Pengunjung (anonim) | Scroll halaman, filter menu, filter galeri, buka modal produk, buka lightbox, buka drawer mobile, klik link WhatsApp / Maps / Instagram |

Tidak ada editor, admin, staff, owner, atau manager role di kode.
