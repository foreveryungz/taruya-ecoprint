# Decisions: Catatan Keputusan dan Pertanyaan Terbuka

> Status: DRAFT v0.1 · Dokumen hidup. Tambah entri setiap ada keputusan baru; **jangan hapus entri lama**, ubah statusnya.
> AI agent: baca dokumen ini sebelum mengusulkan perubahan arsitektur atau menambah dependensi. Bila sebuah keputusan sudah ada di sini, jangan membukanya kembali tanpa diminta.

**Status:** Diputuskan · Usulan (menunggu persetujuan) · Terbuka · Digantikan

---

## 1. Keputusan yang sudah diambil

| ID | Tanggal | Keputusan | Alasan | Sumber |
|---|---|---|---|---|
| D-01 | 2026-10-27 | Tanpa e-commerce (tanpa keranjang, checkout, akun) | Skala usaha rumahan; harga beragam; konversi lewat WhatsApp | `prd.md` |
| D-02 | 2026-10-27 | Harga tidak ditampilkan, pakai kalimat standar "tanya via WA" | Harga bergantung bahan, produk, dan jumlah | `prd.md`, `content.md` |
| D-03 | 2026-10-27 | Dua bahasa: Indonesia (default) dan Inggris (`/en/`) | Ada audiens luar saat event offline | `prd.md` |
| D-04 | 2026-10-27 | Lokasi hanya nama daerah + tautan Google Maps, tanpa alamat lengkap, termasuk di metadata/JSON-LD | Privasi usaha rumahan | `prd.md` |
| D-05 | 2026-10-27 | Konversi utama = klik WhatsApp dengan pesan terisi sesuai konteks | Tujuan situs sesuai jawaban pemilik | `prd.md`, `content.md` |
| D-06 | 2026-10-27 | Framework: Astro, output statis, tanpa backend/adapter | Situs konten sederhana | `tech-spec.md` |
| D-07 | 2026-10-27 | Tanpa UI framework (React/Vue); JavaScript vanilla kecil | Bundle minimal, kompleksitas rendah | `tech-spec.md` |
| D-08 | 2026-10-27 | Konten via Astro Content Collections (YAML), field dua bahasa dalam satu entri, alt text dua bahasa wajib | Terstruktur, tervalidasi, aman untuk admin | `tech-spec.md` |
| D-09 | 2026-10-27 | Struktur dokumen ramping: `prd`, `content`, `design`, `tech-spec`, `quality`, `deployment`, `decisions` + `agents.md` | Hindari tumpang tindih dari rencana 12 dokumen | percakapan perencanaan |
| D-10 | 2026-10-27 | Fase 1 tanpa: mode gelap, CMS, blog, filter kompleks, carousel otomatis, pengalihan bahasa otomatis | Jaga scope kecil | `prd.md`, `design.md`, `tech-spec.md` |
| D-11 | 2026-10-27 | Halaman detail produk/kegiatan opsional, tidak dibuat di fase 1 | Grid + CTA WA sudah cukup untuk mulai | `content.md` |
| D-12 | 2026-10-27 | Pengelolaan awal oleh developer; delegasi ke admin beberapa bulan setelah live | Arah pemilik/pengelola | `prd.md`, `deployment.md` |

### Keputusan implementasi T0.1

| ID | Tanggal | Keputusan | Alasan dan alternatif yang ditolak | Sumber |
|---|---|---|---|---|
| D-13 | 2026-10-27 | Scaffold minimal, Astro dipatok ke 7.4.2, TypeScript strict, output statis tanpa integrasi | T0.1 menyetujui U-01. Versi diverifikasi melalui npm view astro version; Boutique dan integrasi tambahan ditolak karena di luar scope | T0.1; https://docs.astro.build/en/install-and-setup/ |
| D-14 | 2026-10-27 | npm dengan package-lock.json; Node 24.22.1 LTS di .nvmrc dan engines >=22.23.4 <25 | Menjawab Q-16. Node awal 22.12.0 di bawah minimum Astro >=22.23.4; Node 26 masih Current saat verifikasi. pnpm ditolak agar satu package manager | T0.1; https://nodejs.org/en/about/previous-releases; https://nodejs.org/dist/index.json |
| D-15 | 2026-10-27 | @astrojs/check dan typescript sebagai devDependencies untuk astro check | Memeriksa berkas .astro dan TypeScript; tsc saja tidak memeriksa .astro, build saja tidak memeriksa tipe. Formatter dan linter ditunda dari T0.1 | T0.1; https://docs.astro.build/en/guides/typescript/ |

### Keputusan implementasi T0.2

| ID | Tanggal | Keputusan | Alasan dan alternatif yang ditolak | Sumber |
|---|---|---|---|---|
| D-16 | 2026-10-27 | Tailwind CSS 4.3.3 dan @tailwindcss/vite 4.3.3 dipasang melalui CLI Astro; token CSS memakai @theme di global.css | Panduan resmi Styles and CSS untuk Astro terpasang 7.4.2 memakai plugin Vite. Integrasi lama @astrojs/tailwind dan konfigurasi Tailwind v3 ditolak karena bukan jalur yang dianjurkan | T0.2 disetujui; https://docs.astro.build/en/guides/styling/#add-tailwind-4; https://tailwindcss.com/docs/theme |
| D-17 | 2026-10-27 | Palet usulan design.md dipusatkan di global.css; halaman uji sementara memuat contoh ID/EN melalui t() lokal | Memverifikasi token tanpa mendahului komponen, font, routing i18n, atau halaman produksi. Nilai warna/spasi di halaman dan pemasangan font ditolak karena di luar scope; halaman diganti di T3.2 | T0.2 disetujui; design.md §2–4; tech-spec.md §8 |

## 2. Usulan (menunggu persetujuan)

| ID | Usulan | Alasan | Keputusan di |
|---|---|---|---|
| U-01 | Bangun dari template `minimal` Astro; Boutique hanya referensi visual | Struktur Boutique (satu halaman, bagian "Brands") tidak cocok; menghindari membongkar tema. Bila ada kode yang disalin, cek lisensi dulu | `tech-spec.md` §16 |
| U-02 | Palet warna: krem, hijau daun, terakota, lalu disesuaikan dengan logo | Latar tenang agar foto jadi pusat perhatian | `design.md` §2 |
| U-03 | Tombol WA mengambang di mobile | Meningkatkan konversi; perlu diuji agar tidak menutupi konten | `design.md` §9 |
| U-04 | Situs di subdomain hosting diberi `noindex` sampai domain final terpasang | Hindari dua URL bersaing di pencarian | `deployment.md` §5 |
| U-05 | Akun domain dan hosting atas nama pemilik usaha | Hindari masalah saat serah-terima | `deployment.md` §8 |
| U-06 | Halaman Privasi singkat | Transparansi; analytics tanpa cookie | `quality.md` §5 |

## 3. Pertanyaan terbuka (terkumpul dari semua dokumen)

**Butuh keputusan pemilik usaha**
| # | Pertanyaan | Sumber |
|---|---|---|
| Q-01 | Tagline dan cerita brand (dikonfirmasi) | `prd.md`, `content.md` |
| Q-02 | Gaya sapaan bahasa Indonesia: kami/Anda atau kami/kamu | `content.md` §6 |
| Q-03 | Detail jasa pelatihan: format, durasi, jumlah peserta | `content.md` §3 |
| Q-04 | Alur pre-order dan custom order | `content.md` §3 |
| Q-05 | Daftar 3-5 kegiatan terakhir (judul, tanggal, tempat, foto) | `content.md` §9 |
| Q-06 | Jam respons WhatsApp dan siapa penanggung jawab chat | `content.md`, `deployment.md` |
| Q-07 | Tingkat detail lokasi di Google Business Profile | `quality.md` §2 |
| Q-08 | Kelengkapan dan kualitas foto per kategori (audit inventori) | `content.md` §5 |

**Butuh keputusan developer**
| # | Pertanyaan | Sumber |
|---|---|---|
| Q-10 | Warna logo (hex) untuk menyesuaikan palet | `design.md` §9 |
| Q-11 | Pilihan font final | `design.md` §9, `tech-spec.md` |
| Q-12 | Portofolio: grid seragam atau masonry | `design.md` §9 |
| Q-13 | Hosting: Cloudflare Workers atau Netlify | `deployment.md` §2 |
| Q-14 | Domain: nama, registrar, atas nama siapa | `deployment.md` §5 |
| Q-15 | Produk analytics (tanpa cookie) dan event yang dilacak | `tech-spec.md`, `quality.md` |
| Q-16 | Package manager: pnpm atau npm | `tech-spec.md` |
| Q-17 | Daerah/kata kunci lokal untuk SEO | `quality.md` §2 |
| Q-18 | Target angka indikator sukses (setelah baseline diukur) | `prd.md` |
| Q-19 | CMS untuk admin atau edit YAML+Git (uji sebelum serah-terima) | `prd.md` §9, `deployment.md` §10 |
| Q-20 | Penyimpanan cadangan foto master | `deployment.md` §8 |

**Perlu verifikasi dokumentasi saat setup**
| # | Hal | Sumber |
|---|---|---|
| V-01 | Bentuk helper `image()` pada skema koleksi Astro v7 | `tech-spec.md` §6 |
| V-02 | Opsi persis konfigurasi i18n Astro v7 | `tech-spec.md` §5 |
| V-03 | Cara integrasi Tailwind untuk versi yang terpasang | `tech-spec.md` §8 |
| V-04 | Fitur Fonts bawaan Astro vs `@fontsource` | `tech-spec.md` §1 |
| V-05 | Konfigurasi 404 kustom pada hosting yang dipilih | `deployment.md` §2 |
| V-06 | Batas paket gratis hosting saat ini | `deployment.md` §2 |

## 4. Format entri baru

Salin dan isi:

```
| D-xx | YYYY-MM-DD | <keputusan, satu kalimat> | <alasan> | <dokumen sumber> |
```

Bila sebuah keputusan berubah, ubah status lamanya menjadi **Digantikan** dengan rujukan ke ID baru; jangan menimpa.

## 5. Aturan kerja dengan dokumen ini

1. Keputusan baru yang memengaruhi scope, arsitektur, dependensi, atau konten dicatat di sini **sebelum** atau **bersamaan dengan** perubahan kode.
2. Dependensi baru wajib punya entri (alasan, alternatif yang ditolak).
3. Pengecualian terhadap kriteria di `quality.md` dicatat di sini.
4. Pertanyaan terbuka diberi nomor dan, bila terjawab, dipindahkan ke tabel Keputusan.
