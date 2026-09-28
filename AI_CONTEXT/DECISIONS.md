# Decisions

> Aturan penulisan: **hanya keputusan yang benar-benar diambil.** Jika tidak ada
> keputusan, tidak ada entri. Hal yang masih terbuka dicatat di bagian
> "Open Decisions" dengan status `UNRESOLVED` — bukan diisi dengan tebakan.

Status entri:

- `CONFIRMED` — diputuskan dan disepakati oleh pemilik project
- `DERIVED` — kesimpulan yang diambil dari bukti di source code atau aset, bukan pilihan
 _diskusi_; bisa dipertanyakan tapi punya dasar
- `UNRESOLVED` — belum diputuskan, tunggu input client

---

## Decision: Halaman canonical adalah sumber kebenaran konten

**Status:** `CONFIRMED`

### Decision

`index.html` di root repository (sebelum 2026-09-28 berada di
`stitch_custom_design_implementation/ngopi_bareng_teman_official_website_consolidated_refined/code.html`)
adalah satu-satunya halaman yang dipakai sebagai referensi data dan konten. Enam
halaman draft lain **tidak boleh dipakai sebagai referensi data** — hanya sebagai
referensi fitur dan ide desain.

### Reason

Diputuskan pemilik project pada 2026-09-28 setelah data keenam halaman dibandingkan
langsung dengan papan harga resmi `menu-ngobat.jpeg`. Hasilnya:

- Halaman canonical cocok **11 dari 11** produk, harga, dan komposisi.
- Halaman `menu_harga` juga cocok 11/11, tapi datanya sedikit berbeda di bagian
  tambahan (addon susu) yang tidak ada di papan resmi.
- Halaman `beranda_profil_perusahaan` memuat **empat produk yang tidak ada sama sekali**
  di katalog resmi (Caramel Macchiato Rp 28.000, Iced Sea Salt Latte Rp 30.000,
  Matcha Espresso Fusion Rp 32.000) dan menyatakan harga yang berbeda.
- Enam halaman memakai placeholder WhatsApp `6281234567890` yang jelas bukan nomor asli.

Halaman canonical juga paling lengkap secara teknis: satu-satunya yang punya mobile
drawer, filter, 2 modal, IntersectionObserver, dan SEO JSON-LD sekaligus.

### Alternatives Considered

1. **Netral — semua 7 halaman setara**, biarkan agent berikutnya yang memutuskan.
   Ditolak: berisiko agent berikutnya memakai data draft yang salah, dan tidak ada
   dokumentasi yang memberi arah.
2. **Buang 6 halaman draft dari scope dokumentasi.**
   Ditolak: informasi fitur bagus di sana (lightbox keyboard, filter region, sticky
   filter, embedded map, 4 kanal kontak) akan hilang bersama detail implementasinya.

### Current Implementation

Belum ada implementasi. Keputusan ini berlaku sebagai panduan kerja, dicatat di
`HANDOFF.md` bagian `DO NOT CHANGE` dan di `ARCHITECTURE.md`.

Enam halaman draft **tidak dihapus** dari repo — hanya ditandai sebagai non-autoritatif
dalam dokumentasi.

### Important

**Ya — wajib dipertahankan.** Kalau nanti ada agent yang mulai menyalin konten dari
`beranda_profil_perusahaan` atau `cerita_kami`, itu pelanggaran keputusan ini.

---

## Decision: Papan harga cetak adalah sumber kebenaran katalog

**Status:** `DERIVED`

### Decision

`menu-ngobat.jpeg` adalah sumber kebenaran untuk nama produk, harga, dan komposisi.
Bukan halaman web mana pun.

### Reason

Ini bukan pilihan — ini fakta. `menu-ngobat.jpeg` adalah foto papan harga cetak yang
diberikan langsung oleh pemilik project sebagai aset brand. Papan ini merepresentasikan
apa yang sebenarnya dijual outlet.

Diverifikasi dengan cross-check 11 dari 11 item terhadap halaman canonical:

- Nama produk cocok
- Harga cocok
- Komposisi cocok
- Struktur 8 Cold + 3 Hot cocok dengan filter di kode

Kecocokan 100% ini tidak mungkin terjadi secara kebetulan untuk 33 nilai data
sekaligus. Sebaliknya, data draft berlawanan dengan purnama jelas.

### Alternatives Considered

Tidak ada alternatif yang masuk akal. Papan cetak adalah dokumen fisik dari bisnis.

### Current Implementation

Belum ada implementasi di kode. Saat ini papan ini hanya valuable sebagai **referensi
verifikasi** saat menulis dokumentasi, dan sebagai **sumber foto produk** untuk
mengganti hotlink.

### Important

**Ya.** Kalau nanti ada agent yang menambahkan produk, mengubah harga, atau
mengubah komposisi, harga itu harus dicek dulu ke `menu-ngobat.jpeg`. JanganVice versa.

---

## Decision: Logo resmi adalah `ngobat_logo.png`

**Status:** `DERIVED`

### Decision

`ngobat_logo.png` adalah logo resmi brand dan harus dipakai menggantikan placeholder
emoji di halaman canonical.

### Reason

Diberikan langsung oleh pemilik project sebagai aset brand, berdampingan dengan
papan harga. Penampilannya konsisten dengan brand: line-art hitam + aksen kuning,
sesuai dengan `design_specification.md` yang menyebut "the line-art logo featuring two
faces, a yellow circle accent, and hand-drawn text".

Saat ini halaman canonical justru menampilkan emoji:

```html
<div class="w-10 h-10 rounded-full bg-[#FFE600] ...">☕</div>
```

Ini jelas placeholder, bukan keputusan desain.

### Alternatives Considered

Tidak ada. Logo adalah aset tunggal yang tersedia.

### Current Implementation

**Sudah diimplementasikan (2026-09-28).** Ketiga hambatan diselesaikan:

| Hambatan | Penyelesaian |
|---|---|
| `24bpp RGB`, background putih `#FEFEFE` tertanam | Alpha dibuat dari luminance + saturasi, pinggiran di-feather. Tersimpan di `assets/brand/` |
| Lockup vertikal tidak muat di header `h-20` (80px) | **Header pakai mark saja** (40x40) + teks HTML Quicksand. **Footer pakai lockup penuh** (tinggi 48px), di mana orientasi vertikal memang wajar. Varian `logo-horizontal.png` sudah dibuat sebagai alternatif |
| Kuning logo `#FEE820` vs token sistem `#FFE600` | **Token `#FFE600` dipertahankan** — lihat keputusan terpisah di bawah |

Logo terpasang di 3 titik: header, mobile drawer, footer.

### Important

**Ya untuk asset identity.** Kalau nanti ada agent yang mengganti logo dengan
placeholder, atau menggambar ulang logo dari nol, itu pelanggaran keputusan ini.

**Belum untuk warna** — lihat `UNRESOLVED: Warna acuan` di `Open Decisions`.

---

## Decision: Ekstrak ZIP ke working tree dan inisialisasi git

**Status:** `CONFIRMED`

### Decision

Isi `stitch_custom_design_implementation_and_PRD.zip` diekstrak ke dalam working tree
sebagai folder `stitch_custom_design_implementation/`, lalu repository di-`git init`
dengan satu commit impor. File ZIP asli tetap ada dan tidak disentuh.

### Reason

Diputuskan pemilik project pada 2026-09-28. Awalnya source code seluruhnya terkunci
di dalam ZIP, sehingga:

- Graphify tidak bisa menganalisis apa pun (butuh file di disk)
- Dokumentasi tidak boleh mereferensikan path yang tidak ada
- Tidak ada version control sama sekali, padahal 7 file 4.114 baris akan diubah

### Alternatives Considered

1. **Jangan ekstrak**, analisis dari folder temp di luar project.
   Ditolak: path di dokumentasi jadi tidak bisa dibuka agent berikutnya.
2. **Ekstrak tanpa git.**
   Ditolak: tidak ada safety net saat refactor 7 file besar.

### Current Implementation

Sudah dilakukan.

```
stitch_custom_design_implementation/   <- hasil ekstrak (22 file)
.gitignore                              <- baru dibuat
commit 5b5e871                          <- 26 file terlacak
```

`.gitignore` mengecualikan `graphify-out/`, OS/editor noise, dan pola secret
(`.env`, `*.pem`, `*.key`).

### Important

**Ya untuk struktur.** ZIP asli **tidak boleh dihapus** — itu satu-satunya arsip
tidak termodifikasi dari hasil export Stitch.

---

## Decision: Tetap static single-file HTML dengan Tailwind CDN

**Status:** `DERIVED`

### Decision

Arsitektur saat ini (file HTML mandiri, Tailwind via CDN, vanilla JS inline, nol build
tooling) **dipertahankan** untuk tahap berikutnya. Tidak ada migrasi ke framework
ataupun setup build step.

### Reason

Ini bukan keputusan yang diambil eksplisit — ini Deskripsi kondisi yang ada, dan
keputusan untuk **tidak** mengubahnya dalam handoff ini.

Alasan yang relevan: saat ini nol `package.json`, nol bundler, nol test runner. Semua 7
halaman sudah self-contained dan bisa langsung dipublish ke static host apa pun.
Menambahkan build step berarti kamu harus mengonversi 7 file sekaligus, yang
justru menambah risiko pada handoff.

### Alternatives Considered

1. Migrate ke Astro/Next.js/Vite.
   Ditolak untuk sekarang: mengubahnya berarti menulis ulang 7 halaman, bukan
   memperbaiki site. Effort-nya tidak sebanding dengan masalah yang ada (logo, aset,
   backport fitur).
2. Ekstrak shared partial untuk header/footer.
   **Belum diputuskan.** Lihat Open Decisions.

### Current Implementation

Sudah ada, tidak disentuh. Halaman canonical dan 6 draft tetap seperti apa adanya.

### Important

**Ya, sampai ada instruksi lain.** Kalau nanti perlu break shared partial, itu
memang perbaikan arsitektur yang benar (header/footer terduplikasi di 7 file), tapi
karena setiap halaman harus bisa berdiri sendiri, tooling build (atau generator statis)
jadi prasyarat. Jangan initiatespartial tanpa peningkatan tooling juga.

---

## Decision: Logo dipakai sebagai mark di header, lockup penuh di footer

**Status:** `DERIVED`

### Decision

Halaman canonical memakai dua varian logo sesuai konteksnya:

| Lokasi | Varian | Ukuran |
|---|---|---|
| Header | `logo-mark.png` (lingkaran saja) + teks HTML Quicksand | 40x40 |
| Mobile drawer | `logo-mark.png` | 36x36 |
| Footer | `logo-lockup.png` (mark + wordmark 3 baris) | tinggi 48px |

### Reason

Logo asli adalah lockup **vertikal** dengan rasio 0.957 — mark di atas, wordmark
3 baris di bawah. Header canonical setinggi `h-20` (80px), jadi lockup penuh tidak
bisa muat dengan wordmark yang masih terbaca.

Memakai mark saja di header danتقدمدkan teks brand sebagai HTML adalah pilihan
standar: teks jadi tajam di semua DPI, bisa diseleksi, terbaca screen reader, dan
tetap bisa di-styling. Memakai gambar wordmark di header akan يجعل teks kecil
buram.

Di footer, orientasi vertikal adalah hal wajar — ruang vertikal lega, dan tampilannya
lebih "brand".

### Alternatives Considered

1. **Horizontal lockup di header** (`logo-horizontal.png` sudah dibuat).
   Alternatif yang layak, tapi teks brand jadi gambar buram. Belum dipakai.
2. **Perbesar tinggi header** supaya lockup vertikal muat. Merusak proporsi layout
   dan membuat header mengambil banyak viewport height.
3. **Gunakan Quicksand sebagai pengganti wordmark di seluruh situs.** Menolak —
   wordmark marker adalah bagian identitas brand yang membedakan produk ini.

### Current Implementation

Sudah diterapkan. Ketiga titik sudah memakai `<img>` dengan `assets/brand/`.

`logo-horizontal.png` dan `logo-wordmark.png` sudah tersedia kalau nanti diperlukan.

### Important

**Ya.** Kalau mengganti logo di header, jangan pakai lockup penuh — ukurannya tidak
cukup. Dan jangan pakai logo di atas latar gelap: line-art hitamnya praktis tidak
terlihat (sudah diverifikasi dengan superimposisi pada background `#111111`).

---

## Decision: Token kuning `#FFE600` dipertahankan

**Status:** `DERIVED`

### Decision

Warna aksen di seluruh kode tetap `#FFE600`. Tidak diubah ke `#FEE820` (warna logo)
atau `#FCDC17` (warna tag papan harga).

### Reason

Tiga sumber warna diukur dan berbeda-beda:

| Sumber | Nilai | Keterangan |
|---|---|---|
| `design_specification.md` | `#FFE600` | Nilai yang dispesifikasikan designer |
| `tailwind.config` (7 file) | `#FFE600` | Sudah jadi token `primary-container` |
| `ngobat_logo.png` | `#FEE820` | Median 72.342 piksel yellow, sampling |
| `menu-ngobat.jpeg` (tag harga) | `#FCDC17` | Median ~3.000 piksel per tag |

Papan harga adalah **foto papan cetak**, jadi warnanya sudah tergeser oleh cahaya,
kamera, dan profil warna — itu menjelaskan `#FCDC17`. Logo adalah aset digital
paling bersih, tapi masih bisa punya Intentional warm shift.

Mengubah `#FFE600` → `#FEE820` berarti find-replace di 75 kemunculan pada halaman
canonical plus `tailwind.config` di 7 file, demi mengejar selisih yang secara visual
hampir tak terlihat (channel biru 0 vs 32 dari 255). Risiko dokumentasi dan
kemungkinan salah-replace lebih besar dari keuntungan yang didapat.

Koreksi yang benar sebenarnya ada di sisi lain: idealnya logo yang disesuaikan ke
`#FFE600`, atau client memberi panduan warna resmi dari brand book. Itu keputusan
client, bukan keputusan teknis.

### Alternatives Considered

1. **Ubah semua ke `#FEE820`** agar cocok dengan logo. Ditolak: 75 kemunculan,
   risiko diff besar, keuntungan visual minimal.
2. **Jadikan dua token terpisah** (`#FFE600` untuk UI, `#FEE820` untuk logo).
   Ditolak: mustard yellow di dalam logo sudah menyatu dengan kuning di
   `#FFE600` — memisahkan keduanya justru membuat nilainya kabur.

### Current Implementation

Belum ada perubahan warna. Token `#FFE600` dipakai apa adanya.

### Important

**Ya, sementara.** Sampai client mengonfirmasi warna resmi. Kalau client ternyata
bilang `#FEE820` yang benar, perbaikannya adalah find-replace — dan itu harus
dilakukan di SEMUA 7 file, bukan hanya canonical, karena `tailwind.config` inline
terduplikasi di semua file.



Hal-hal berikut **belum diputuskan**. Jangan diasumsikan.

### UNRESOLVED: Badge item Hot Kopi Saring Lawas

**Konteks:** Papan harga resmi menandai 10 dari 11 produk dengan tag kuning, tapi
Hot Kopi Saring Lawas (15K) memakai tag **hitam**. Design spec menetapkan hitam =
tag "New". Implementasi sekarang memakai label "Signature" yang tidak ada di spec
mana pun, dan menandai 2 item sekaligus.

**Pilihan yang ada:**
1. Ubah ke "New" — mengikuti warna hitam di papan resmi
2. Pertahankan "Signature" — mengikuti implementasi sekarang
3. Ubah ke "Best Seller" — mengikuti warna kuning di spec
4. Hapus badge dari item ini

**Perlu:** konfirmasi client.

### UNRESOLVED: Susu alternatif Oat +Rp5.000 / Soy +Rp4.000

**Konteks:** Ada di bento "Seduh Sesuai Selera Kamu" pada `menu_harga/code.html`.
Tidak ada di papan harga resmi.

**Pilihan:** pertahankan (tanyakan ke client apakah outlet memang menawarkan),
atau hapus.

**Perlu:** konfirmasi client.

### UNRESOLVED: Kuning acuan — `#FFE600` atau `#FEE820`

**Konteks:** Design system dan seluruh kode memakai `#FFE600`. Logo resmi_samples
menunjukkan `#FEE820`. Selisihnya kecil tapi nyata (kuning lebih terang/kehijauan).

**Pilihan:**
1. Ubah token sistem ke `#FEE820` agar match logo
2. Pertahankan `#FFE600` dan pasang logo apa adanya
3. Jadikan dua warna terpisah: `#FFE600` untuk UI, `#FEE820` khusus logo

**Perlu:** konfirmasi client.

### UNRESOLVED:-Level logo di header

**Konteks:** Logo asli adalah lockup vertikal (mark + wordmark 3 baris) dengan rasio
0.957 dan padding besar. Header canonical setinggi `h-20` (80px). Lockup penuh tidak muat
dengan wordmark yang masih terbaca.

**Pilihan:**
1. Buat varian horizontal (mark + teks 1 baris) — perlu desain ulang dari logo asli
2. Pakai mark saja di header, wordmark jadi teks HTML di sebelahnya
3. Perbesar tinggi header

**Perlu:** keputusan teknis, bisa diambil sendiri tapi lebih baikvia client.

### UNRESOLVED: Fate enam halaman draft

**Konteks:** Enam halaman tidak punya mobile drawer (empat di antaranya tidak punya
JS sama sekali). Kalau dipublish, halaman-halaman itu tidak bisa dinavigasi di mobile.

**Pilihan:**
1. Publish semua — perlu backport drawer + perbaiki data
2. Publish hanya canonical — hapus/arsipkan enam draft
3. Pecah canonical jadi multi-page (`index`, `menu`, `about`, ...) dengan shared shell

**Perlu:** keputusan pemilik project.

### UNRESOLVED: Spec gap G1 (Shopping Cart)

**Konteks:** `design_specification.md` meminta shopping cart dan tombol "Order Now".
Semua 7 halaman mengimplementasikan "Chat via WhatsApp" sebagai gantinya. Ada
ketidaksesuaian scope yang belum pernah diselesaikan.

**Pilihan:** sudah putuskan tidak ada cart (WA adalah kanal), atau memang masih
dipertimbangkan.

**Perlu:** konfirmasi client.
