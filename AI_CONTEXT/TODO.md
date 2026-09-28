# TODO

Semua task di bawah berasal dari inspeksi source code, `design_specification.md`,
atau aset brand. Tidak ada task yang Based on asumsi.

---

## Critical

- [ ] **Jawab 4 pertanyaan client yang memblokir** (lihat `HANDOFF.md` →
      `BLOCKERS`): badge item Hot Kopi Saring Lawas, addon susu alternatif, warna kuning
      acuan, dan fate 6 halaman draft
- [ ] **Pasang logo + aset lokal ke 6 halaman draft.** Halaman canonical sudah selesai;
      enam halaman draft masih hotlink dan tanpa logo
- [ ] **Putuskan warna kuning.** Token `#FFE600` dipakai sementara. Kalau client bilang
      `#FEE820` yang benar, perbaikannya di 75 kemunculan canonical **plus**
      `tailwind.config` di 7 file (konsekuensi lihat `DECISIONS.md`)

## In Progress

Tidak ada. Task migrasi aset selesai dan terverifikasi di browser.

## Next

- [ ] **Hapus `design_specification.md` yang duplikat.** File di root dan di
      `stitch_custom_design_implementation/` identik (SHA-256 sama). Sisakan satu
- [ ] **Backport lightbox prev/next + counter + navigasi keyboard** dari
      `galeri_instagram/code.html` ke `#lightbox-modal` di halaman canonical.
      Versi canonical sekarang hanya punya tombol close
- [ ] **Backport filter region + 4 kanal kontak** dari `lokasi_kontak/code.html`.
      Tambah section `#kontak` terpisah (sekarang `#kontak` cuma div di dalam
      `#lokasi`) dengan WhatsApp, Instagram, email, TikTok
- [ ] **Backport sticky filter bar** dari `menu_harga/code.html` ke `#menu`.
      Sekarang filter ikut ter-scroll; di sana `position: sticky` di `top-20`
- [ ] **Backport bento "Seduh Sesuai Selera Kamu"** dari `menu_harga/code.html`
      — **hanya bagian level gula dan asal biji, JANGAN bagian harga susu alternatif**
      (belum diverifikasi ke client)

## Planned

- [ ] **Konsolidasikan warna hardcoded.** `#FFE600` muncul 75x dan `#111111` 160x di
      halaman canonical sebagai arbitrary value (`bg-[#FFE600]`), padahal keduanya
      sudah ada di `tailwind.config` sebagai `primary-container` dan `text-primary`.
      Ganti ke token agar perubahan brand cukup di satu tempat
- [ ] **Pecah halaman canonical jadi multi-page** (`index.html`, `menu.html`,
      `about.html`, `galeri.html`, `lokasi.html`) dengan shared header/footer —
      hanya jika tooling build disetujui
- [ ] **Tambah filter kategori sesuai spec** (Flavour / Non Coffee / Signature /
      Cold Drinks / Pastry) atau update `design_specification.md` supaya sesuai
      implementasi. Saat ini spec dan kode berbeda
- [ ] **Tambah `favicon`** dari mark logo
- [ ] **Siapkan konfigurasi deploy** (`netlify.toml` atau similar) saat proyek
      siap tayang
- [ ] **Pindahkan foto moodboard ke `assets/moodboard/`** supaya 5 folder dengan nama
      panjang itu tidak memicu kebingungan
- [ ] **Update `design_specification.md`** agar mendokumentasikan keputusan yang
      sudah diambil (WA sebagai ganti cart, kategori cold/hot, Material Symbols
      sebagai ganti SVG line-art)

## Bugs

Tidak ada bug fungsional. Semua interaksi halaman canonical sudah diuji di browser
pada 2026-09-28 dan lolos: filter 3/8/11, filter galeri 2/6, modal, lightbox,
drawer, scroll lock, 0 console error, 27 gambar / 0 hotlink / 0 tanpa alt.

Yang tersisa:

- [ ] **Periksa ulang `alt=""` yang kosong.** Ada di halaman canonical:
      `<img alt="" id="modal-img">` dan `<img alt="" id="lightbox-img">`. Keduanya
      sengaja dikosongkan karena diisi runtime oleh JS, tapi kalau JS gagal atau
      `data-*`-nya kosong, alt akan tetap kosong
- [ ] **Proofread teks Indonesia yang bercampur bahasa Inggris.** Contoh terverifikasi
      di halaman canonical: pesan WhatsApp default berbunyi
      `...bertanya mengenai menu dan informasi coffee shop.`, dan section
      "Come. Sit. Stay Awhile." memakai headline Inggris di antara body Bahasa
      Indonesia. Perlu ditinjau apakah disengaja (tone cosmopolitan) atau kelalaian
- [ ] **Dua `screen.png` yang stub** (28 byte, tidak valid) di `menu_harga` dan
      `galeri_instagram`. Tidak bisa di-regenerate tanpa tooling Stitch

## Technical Debt

- [ ] **Header dan footer terduplikasi di 7 file.** Mengubah satu baris nav berarti
      mengedit 7 file. `beranda_profil_perusahaan` dan
      `global_navigation_footer_system` punya body identik 100% - kandidat dihapus
- [ ] **`tailwind.config` inline di 7 file.** Ubah token = edit 7 file
- [ ] **6 halaman draft masih pakai hotlink dan tanpa logo.** Kalau akan dipublish,
      perlu di-update dengan `assets/` juga
- [ ] **Nol test, nol lint, nol typecheck.** Verifikasi hanya manual: buka di browser
- [ ] **Nol `package.json`, nol lockfile.** Versi CDN tidak ter-pin, tidak bisa diaudit
- [ ] **`::-webkit-scrollbar { display: none }`** disembunyikan global di halaman
      canonical. Keputusan desain yang disengaja, tapi punya konsekuensi: di browser
      yang tidak mendukung properti tersebut scrollbar tetap tampil, dan affordance
      scroll berkurang untuk pengguna mouse
- [ ] **40 dangling-endpoint edge di `graphify-out/graph.json`.** Muncul dari chunk
      ekstraksi yang sengaja mereferensikan node lintas-chunk (`brand_identity`,
      `canonical_page`, `header_logo_placement`) yang didefinisikan di chunk lain.
      Expected, bukan corruption - tapi berarti ada node konseptual yang hanya muncul
      sebagai target edge, bukan sebagai node utuh
- [ ] **`graphify-out/` di-gitignore.** Kalau agent berikutnya butuh grafnya, harus
      regenerate dengan `/graphify`
- [ ] **Foto produk resolusi rendah.** Sumber ~150x200 px. Cukup untuk kartu 192px,
      tidak untuk lightbox besar. Kalau lightbox produk dibutuhkan nanti, perlu foto
      resolusi tinggi dari client

## Completed

- [x] Ekstrak `stitch_custom_design_implementation_and_PRD.zip` ke working tree
- [x] `git init` + `.gitignore` + commit impor (`5b5e871`, 26 file)
- [x] Inspeksi penuh 7 file `code.html` (4.114 baris)
- [x] Cross-check data menu 11 item terhadap `menu-ngobat.jpeg` - 11/11 cocok
- [x] Pipeline Graphify penuh: 558 node, 910 edge, 27 community
- [x] Dokumentasi `AI_CONTEXT/` (7 file) + `AGENTS.md`
- [x] Verifikasi tidak ada secret di repo
- [x] **Logo resmi diproses**: alpha, crop tight, mark / wordmark / lockup /
      horizontal, favicon 64/128/256 -> `assets/brand/`
- [x] **11 foto produk di-crop** dari `menu-ngobat.jpeg` -> `assets/products/`,
      600x600 JPEG di kartu krem `#FFFDF5`
- [x] **6 tile galeri + hero + OG image** dari 5 foto moodboard -> `assets/gallery/`,
      `assets/hero/`
- [x] **Logo terpasang** di header, mobile drawer, footer (3 titik)
- [x] **27 `<img>` diganti aset lokal, 0 hotlink tersisa** di halaman canonical
- [x] Favicon ditambahkan, 2 `alt` yang tidak cocok diperbaiki
- [x] **Aset dipindahkan ke dalam folder halaman** supaya path relatif resolve dan
      halaman bisa di-deploy mandiri
- [x] **Verifikasi browser**: filter 3/8/11, filter galeri 2/6, modal produk,
      lightbox, mobile drawer, scroll lock, 0 console error, 27 gambar / 0 hotlink /
      0 tanpa alt
