# PRD: Website Profil Usaha Ecoprint

> Status: DRAFT v0.1 · Sumber kebenaran untuk tujuan, audiens, dan scope.
> Dokumen turunan: `content.md`, `design.md`, `tech-spec.md`, `quality.md`, `deployment.md`, `decisions.md`.
> Item bertanda **[TBD]** belum diputuskan. Lihat bagian 12.

---

## 1. Ringkasan

Website profil statis untuk usaha ecoprint (batik dengan daun alam) yang memproduksi fashion & aksesoris: baju, kain, sepatu, gelas, tumbler, dan produk lain. Situs berfungsi sebagai **portofolio utama brand** dan **pintu masuk ke WhatsApp**. Tidak ada e-commerce.

- **Nama brand:** TARUYA ECOPRINT
- **Bahasa:** Indonesia (default) dan Inggris
- **Aset:** logo, nama brand, dan foto sudah tersedia.

## 2. Tujuan

### Tujuan bisnis

1. Menjadi portofolio utama yang menampilkan kualitas dan karakter karya.
2. Mengarahkan pengunjung untuk menghubungi via WhatsApp (konversi utama).
3. Membangun kredibilitas di depan audiens baru, termasuk audiens luar negeri yang bertemu brand di event offline.
4. Menjadi satu referensi tautan untuk Instagram, kartu nama, dan materi event.

### Indikator sukses (usulan, target angka [TBD])

| Indikator                                                     | Cara ukur                          |
| ------------------------------------------------------------- | ---------------------------------- |
| Klik tombol WhatsApp                                          | Event analytics (privacy-friendly) |
| Kunjungan halaman Portofolio dan Katalog                      | Analytics                          |
| Skor Lighthouse mobile ≥ 90 (Performance, SEO, Accessibility) | Lighthouse CI / manual             |
| Situs muncul di pencarian nama brand dan Google Maps          | Pengecekan manual                  |

## 3. Audiens

> Persona di bawah adalah **asumsi awal** dan perlu divalidasi oleh pemilik usaha.

| Persona                                                           | Kebutuhan                                                | Tindakan yang diharapkan                   |
| ----------------------------------------------------------------- | -------------------------------------------------------- | ------------------------------------------ |
| **Pembeli personal** (menemukan brand lewat Instagram atau teman) | Melihat kualitas dan ragam produk, tahu cara memesan     | Chat WA untuk tanya produk dan harga       |
| **Pemesan custom / grup** (souvenir, seragam, hadiah, event)      | Bukti portofolio, alur custom order, estimasi waktu      | Chat WA dengan detail kebutuhan dan jumlah |
| **Calon peserta pelatihan / komunitas**                           | Info jenis pelatihan, format, dan kontak                 | Chat WA untuk jadwal                       |
| **Audiens internasional** (kenal brand di event offline)          | Konten berbahasa Inggris, cerita brand, cara menghubungi | Chat WA atau Instagram                     |

Perilaku umum: mayoritas akses dari **mobile**, datang dari Instagram, pencarian, atau tautan di materi cetak.

## 4. Scope

### Dalam scope (fase 1)

- Pengenalan brand dan cerita proses ecoprint
- Portofolio karya
- Katalog sederhana (tanpa harga, tanpa keranjang)
- Aktivitas / kegiatan (event, pameran, workshop)
- Layanan: Shop, Custom Order, Pre-Order, Jasa Pelatihan
- Kontak: WhatsApp, Instagram, Google Maps
- Dua bahasa: Indonesia dan Inggris
- SEO dasar, SEO lokal, aksesibilitas, performa

### Non-goals (tidak dikerjakan di fase 1)

- Keranjang, checkout, pembayaran, atau akun pengguna
- Menampilkan harga
- Formulir order dengan backend (semua lewat WhatsApp)
- Blog / artikel rutin
- Fitur pencarian dan filter yang kompleks
- Dashboard admin atau CMS (lihat bagian 9)

## 5. Layanan dan alur konversi

Semua layanan berujung pada **WhatsApp**. Setiap CTA membuka WA dengan pesan terisi otomatis yang sesuai konteks (nama produk atau jenis layanan).

| Layanan        | Deskripsi singkat                        | CTA                       |
| -------------- | ---------------------------------------- | ------------------------- |
| Shop           | Produk siap beli                         | "Tanya produk ini via WA" |
| Custom Order   | Pesanan sesuai bahan, desain, dan jumlah | "Diskusi custom order"    |
| Pre-Order      | Produk dengan periode pemesanan          | "Ikut pre-order"          |
| Jasa Pelatihan | Workshop / pelatihan ecoprint            | "Tanya jadwal pelatihan"  |

Harga tidak ditampilkan: tulis "Harga menyesuaikan bahan, produk, dan jumlah. Hubungi kami via WhatsApp." Hal ini dijelaskan sekali dan konsisten di seluruh situs.

## 6. Halaman (ringkas, detail di `content.md`)

Beranda · Tentang / Proses · Portofolio · Katalog · Layanan · Kegiatan · Kontak

Struktur final, hierarki, dan model konten ditetapkan di `content.md`.

## 7. Bahasa

- Indonesia sebagai bahasa default, Inggris sebagai bahasa kedua.
- Pemilihan bahasa lewat URL (mis. `/` dan `/en/`) dan ada pengalih bahasa yang jelas.
- Konten Inggris harus ditulis atau ditinjau manusia, bukan terjemahan mentah.
- Detail implementasi (i18n routing, `hreflang`) ada di `tech-spec.md` dan `quality.md`.

## 8. Kontak dan lokasi

- WhatsApp: +6285877704021
- Instagram: @taru_ecoprint
- Google Maps: https://maps.app.goo.gl/6vswDnatLpSuACRA7
- Lokasi berupa tempat usaha **rumahan**. Alamat lengkap dan jam kunjungan perlu keputusan: ditampilkan penuh, atau "kunjungan dengan janji via WA". Privasi pemilik jadi pertimbangan.

## 9. Pengelolaan konten

- **Fase awal:** dikelola oleh developer (kamu), konten berupa file di repo.
- **Setelah beberapa bulan live:** didelegasikan ke admin. Admin perlu cara menambah produk, foto, dan kegiatan tanpa menyentuh kode.
- Konsekuensi arsitektur: model konten dirancang terstruktur sejak awal (Astro Content Collections). Keputusan memakai CMS ringan dicatat di `decisions.md` dan ditinjau sebelum delegasi. Keputusan ini **tidak** memblokir fase 1.

## 10. Asumsi dan batasan

- Brand, logo, dan foto sudah tersedia. Kualitas dan kelengkapan foto per kategori perlu dicek di `content.md`.
- Tidak ada anggaran server: hosting statis gratis atau murah.
- Domain: belum ada. asumsi akan menggunakan nama taruya
- Tidak ada backend. Seluruh interaksi lewat tautan eksternal (WA, IG, Maps).
- Tidak ada data pribadi pengunjung yang disimpan.

## 11. Fase

| Fase                | Isi                                                                                     |
| ------------------- | --------------------------------------------------------------------------------------- |
| 0. Dokumen          | `prd`, `content`, `design`, `tech-spec`, `quality`, `deployment`, `decisions`, `agents` |
| 1. Build & konten   | Setup Astro, komponen, halaman, konten ID + EN                                          |
| 2. Kualitas & rilis | SEO, aksesibilitas, performa, deploy, domain                                            |
| 3. Stabilisasi      | Monitoring, perbaikan, persiapan delegasi ke admin                                      |

## 12. Risiko

| Risiko                                     | Mitigasi                                                                     |
| ------------------------------------------ | ---------------------------------------------------------------------------- |
| Foto tidak seragam kualitas atau ukurannya | Panduan foto singkat di `content.md`, optimasi otomatis lewat `astro:assets` |
| Konten Inggris kurang natural              | Tinjau manusia, tulis ringkas dan sederhana                                  |
| Konten jadi usang setelah delegasi         | Model konten sederhana, panduan admin di `deployment.md`                     |
| Scope melebar (harga, checkout, blog)      | Daftar non-goals di atas, keputusan baru dicatat di `decisions.md`           |
| Privasi alamat rumahan                     | Putuskan tingkat detail alamat di bagian 8                                   |

## 13. Pertanyaan terbuka

1. Nama brand, nomor WA, akun Instagram, dan tautan Maps (isi di bagian 2, 8).
2. Alamat ditampilkan penuh atau berbasis janji?
3. Apakah domain sudah dimiliki?
4. Target angka untuk indikator sukses.
5. Apakah ada tagline atau cerita brand yang sudah ditulis?
