# Quality: SEO, Aksesibilitas, Performa, dan Acceptance Criteria

> Status: DRAFT v0.1 · Bergantung pada `prd.md`, `content.md`, `design.md`, `tech-spec.md`.
> Angka bertanda **(usulan)** adalah target awal yang boleh disesuaikan setelah pengukuran nyata.
> Setiap kriteria punya ID (`AC-xx`) agar bisa dirujuk di tugas, PR, dan `agents.md`.

---

## 1. Definisi "selesai"

Situs dianggap siap rilis bila **semua kriteria bertanda Wajib lolos** pada build produksi (bukan hanya di dev server), dan daftar periksa rilis di bagian 8 sudah dicentang.

Prioritas: **Wajib** (blokir rilis) · **Sebaiknya** (perbaiki segera setelah rilis, tidak memblokir).

## 2. SEO

### Prinsip
Situs kecil dengan satu tujuan: ditemukan lewat **nama brand** dan **pencarian lokal/niche** (mis. "batik ecoprint" + daerah), lalu mengarahkan ke WhatsApp. Fokus pada dasar yang benar, bukan trik.

### Kebutuhan
| Area | Aturan |
|---|---|
| Title | Unik per halaman, ±50-60 karakter, memuat nama brand; template: `{Halaman} · {Brand}` |
| Meta description | Unik per halaman, ±120-155 karakter, menjelaskan isi dan ajakan |
| Heading | Satu `h1` per halaman; hierarki `h2`/`h3` berurutan |
| URL | Pendek, huruf kecil, kebab-case; slug dilokalkan (lihat `tech-spec.md`) |
| Canonical | Setiap halaman punya `rel="canonical"` ke URL produksi sendiri |
| Hreflang | `id`, `en`, dan `x-default`, saling berbalasan antar-halaman padanan |
| Sitemap | Otomatis lewat `@astrojs/sitemap`, memuat kedua bahasa; dirujuk di `robots.txt` |
| Open Graph / Twitter | Judul, deskripsi, gambar 1200×630 per halaman utama (penting untuk berbagi tautan di WA/IG) |
| Structured data | JSON-LD `LocalBusiness` (nama, URL, logo, kontak, tautan sosial, area/daerah). **Tanpa alamat jalan** (lihat `prd.md`) |
| Gambar | Nama file deskriptif, alt text bermakna (ID dan EN) |
| Tautan internal | Teks tautan deskriptif, bukan "klik di sini" |
| Bahasa | `<html lang>` benar per halaman; konten EN ditulis/ditinjau manusia |

### Di luar kode (sering lebih berdampak)
- **Google Business Profile:** buat/klaim profil usaha, cantumkan URL situs. Tentukan tingkat detail lokasi yang sesuai untuk usaha rumahan **[TBD, keputusan pemilik]**.
- **Google Search Console:** verifikasi domain, kirim sitemap, pantau indeks.
- **Bing Webmaster Tools** (opsional).
- **Konsistensi nama dan kontak** di Instagram, Google Maps, dan situs.
- Cantumkan URL situs di bio Instagram dan materi event.

### Kata kunci (awal, divalidasi pemilik)
ecoprint, batik daun / batik ecoprint, custom ecoprint, pelatihan ecoprint, plus nama daerah **[TBD]**. Dalam bahasa Inggris: ecoprint, natural leaf printing, eco-printed textiles, ecoprint workshop. Tulis wajar dalam kalimat; hindari menjejalkan kata kunci.

## 3. Aksesibilitas

**Target:** WCAG 2.2 level **AA**.

| Area | Aturan |
|---|---|
| Struktur | Landmark (`header`, `nav`, `main`, `footer`), urutan heading logis, `skip link` ke konten utama |
| Bahasa | `lang` pada `<html>`; tautan pengalih bahasa memiliki `lang`/`hreflang` pada tautannya dan teks yang jelas ("English", "Bahasa Indonesia") |
| Keyboard | Seluruh fitur bisa dipakai dengan keyboard; urutan fokus logis; tidak ada jebakan fokus |
| Fokus | Indikator fokus selalu terlihat (kontras cukup); tidak dihapus |
| Kontras | Teks biasa ≥ 4,5:1, teks besar dan komponen UI ≥ 3:1 (lihat catatan `accent` di `design.md`) |
| Target sentuh | Standar internal ≥ 44×44px (WCAG 2.2 AA mensyaratkan minimum lebih kecil; kita memakai standar yang lebih ketat) |
| Gambar | Alt text bermakna untuk gambar informatif; `alt=""` untuk dekoratif |
| Lightbox | Memakai `<dialog>`; fokus berpindah ke dalamnya saat buka dan kembali ke pemicu saat tutup; Esc menutup; tombol tutup berlabel |
| Tautan | Teks tautan bermakna di luar konteks (mis. label tombol WA menyebut produk: "Tanya via WhatsApp tentang {produk}"); tautan yang buka tab baru diberi tahu |
| Informasi | Tidak hanya lewat warna (status ketersediaan juga berupa teks) |
| Gerak | Hormati `prefers-reduced-motion`; tidak ada konten berkedip; tidak ada carousel otomatis |
| Zoom | Teks bisa dibesarkan 200% tanpa kehilangan fungsi; viewport tidak mengunci zoom |
| Tanpa JS | Konten inti tetap terbaca dan tautan WA tetap berfungsi |

## 4. Performa

**Target (usulan)**, diukur pada profil mobile dengan jaringan 4G terbatas:

| Metrik | Target | Catatan |
|---|---|---|
| Lighthouse Performance (mobile) | ≥ 90 | dari `prd.md` |
| LCP | ≤ 2,5 detik | biasanya foto hero |
| CLS | ≤ 0,1 | wajib tentukan dimensi gambar |
| INP | ≤ 200 ms | JS minimal membantu |
| JavaScript total per halaman | ≤ ~50 KB terkompresi (usulan) | tanpa UI framework |
| Berat awal Beranda | ≤ ~1,5 MB (usulan) | gambar adalah faktor utama |
| Hero | ≤ ~200 KB (usulan) | format modern, ukuran responsif |

**Praktik:**
- Output statis dan CDN dari hosting
- `astro:assets` untuk semua foto; `lazy` kecuali hero; hero memakai `fetchpriority="high"`
- Font self-host, maksimal 2 keluarga, 2-3 bobot, `font-display: swap`, fallback system font
- Tanpa skrip pihak ketiga kecuali analytics yang ringan
- Header cache panjang untuk aset ber-hash dari hosting
- Tanpa video otomatis; embed Google Maps **tidak** dimuat otomatis (gunakan tautan, atau muat setelah klik) karena berat

**Catatan pengukuran:** data lapangan (CrUX) belum akan ada di awal karena trafik rendah, jadi andalkan Lighthouse dan WebPageTest/PageSpeed lab data sampai trafik cukup.

## 5. Konten, privasi, dan legal ringan

> Saya bukan penasihat hukum; bagian ini daftar periksa praktis, bukan nasihat hukum.

- Situs **tidak mengumpulkan data pribadi** sendiri (tanpa formulir, tanpa akun). Interaksi berpindah ke WhatsApp/Instagram, yang diatur kebijakan masing-masing.
- Analytics dipilih yang tanpa cookie pelacak bila memungkinkan, agar tidak perlu banner persetujuan. Jika menambah alat berbasis cookie atau formulir, tinjau kewajiban perlindungan data pribadi yang berlaku di Indonesia (UU PDP) dan di negara audiens luar **[TBD]**.
- Halaman singkat "Privasi" cukup (satu paragraf: tidak ada data pribadi dikumpulkan, analytics apa yang dipakai) **(sebaiknya)**.
- **Klaim brand** ("ramah lingkungan", "alami", "tidak ada dua yang sama") hanya yang benar-benar bisa dipertanggungjawabkan pemilik.
- **Hak foto:** pastikan foto milik sendiri atau berizin. Foto orang di event/pelatihan sebaiknya dengan persetujuan.
- Tidak ada harga di situs, jadi risiko klaim harga minimal; tetap pakai kalimat standar di `content.md` bagian 8.

## 6. Acceptance criteria

### A. Fungsi dan konten
| ID | Kriteria | Cara verifikasi | Prioritas |
|---|---|---|---|
| AC-01 | Semua halaman di sitemap (`content.md`) ada dalam ID dan EN dan terbuka tanpa error | Buka satu per satu pada build produksi; periksa 404 | Wajib |
| AC-02 | Pengalih bahasa di setiap halaman mengarah ke padanan **halaman yang sama** di bahasa lain | Uji manual semua halaman | Wajib |
| AC-03 | Tidak ada teks UI yang tertinggal berbahasa Indonesia di halaman EN (dan sebaliknya) | Review manual atau pencarian string | Wajib |
| AC-04 | Setiap CTA WhatsApp membuka WA dengan nomor benar dan pesan terisi sesuai konteks dan bahasa | Uji di Android dan iOS (WA terpasang) dan desktop (WA Web) | Wajib |
| AC-05 | Tautan Instagram dan Google Maps benar dan terbuka | Klik manual | Wajib |
| AC-06 | Tidak ada harga yang tampil; kalimat standar harga konsisten | Pencarian string; review katalog | Wajib |
| AC-07 | Katalog menampilkan kategori dan ketersediaan (Ready/Pre-Order/Custom) yang benar | Cocokkan dengan data | Wajib |
| AC-08 | Filter portofolio bekerja; tanpa JS semua item tetap tampil | Uji dengan JS dimatikan | Wajib |
| AC-09 | Lokasi hanya menampilkan nama daerah + tautan Maps, **tanpa alamat lengkap** di mana pun (termasuk metadata/JSON-LD) | Pencarian di sumber halaman | Wajib |
| AC-10 | Halaman 404 ada, berbahasa sesuai, dan memberi jalan kembali | Buka URL sembarang | Wajib |
| AC-11 | Tidak ada tautan rusak internal maupun eksternal | Pemeriksa tautan | Wajib |
| AC-12 | Konten placeholder `[TBD]`/lorem tidak tersisa | Pencarian string `TBD`, `lorem` | Wajib |

### B. SEO
| ID | Kriteria | Cara verifikasi | Prioritas |
|---|---|---|---|
| AC-20 | Title dan meta description unik di setiap halaman, ID dan EN | Audit sumber/alat crawl | Wajib |
| AC-21 | Tepat satu `h1` per halaman | Audit | Wajib |
| AC-22 | Canonical dan hreflang (`id`, `en`, `x-default`) benar dan saling berbalasan | Audit sumber | Wajib |
| AC-23 | `sitemap` dan `robots.txt` tersedia; sitemap memuat kedua bahasa dengan domain produksi | Buka URL-nya | Wajib |
| AC-24 | Open Graph/Twitter card tampil benar saat tautan dibagikan (WA, IG, minimal satu pratinjau uji) | Alat pratinjau/berbagi nyata | Wajib |
| AC-25 | JSON-LD `LocalBusiness` valid dan tanpa alamat jalan | Rich Results Test / validator schema | Wajib |
| AC-26 | Situs terverifikasi di Search Console dan sitemap terkirim | Cek akun | Sebaiknya |

### C. Aksesibilitas
| ID | Kriteria | Cara verifikasi | Prioritas |
|---|---|---|---|
| AC-30 | Lighthouse Accessibility ≥ 95 pada semua halaman utama (mobile) | Lighthouse | Wajib |
| AC-31 | Tanpa pelanggaran "serious/critical" dari alat otomatis (axe atau setara) | axe DevTools | Wajib |
| AC-32 | Seluruh fitur bisa dipakai hanya dengan keyboard, termasuk menu, pengalih bahasa, filter, lightbox | Uji manual Tab/Shift+Tab/Enter/Esc | Wajib |
| AC-33 | Fokus selalu terlihat; ada skip link yang berfungsi | Uji manual | Wajib |
| AC-34 | Kontras semua pasangan warna teks/latar lolos AA | Contrast checker | Wajib |
| AC-35 | Semua gambar informatif punya alt bermakna di kedua bahasa; dekoratif `alt=""` | Audit | Wajib |
| AC-36 | Lightbox: fokus masuk dan kembali, Esc menutup, ada label tombol tutup | Uji manual | Wajib |
| AC-37 | Teks diperbesar 200% tanpa kehilangan konten/fungsi; tidak ada scroll horizontal pada 360px | Uji zoom | Wajib |
| AC-38 | `prefers-reduced-motion` dihormati | Uji dengan pengaturan OS | Wajib |
| AC-39 | Uji singkat dengan pembaca layar (TalkBack atau VoiceOver) pada Beranda dan Katalog | Uji manual | Sebaiknya |

### D. Performa
| ID | Kriteria | Cara verifikasi | Prioritas |
|---|---|---|---|
| AC-40 | Lighthouse Performance ≥ 90 (mobile) pada Beranda, Katalog, Portofolio | Lighthouse pada build produksi | Wajib |
| AC-41 | LCP ≤ 2,5 s, CLS ≤ 0,1 pada profil mobile terbatas (lab) | Lighthouse/PageSpeed | Wajib |
| AC-42 | JS per halaman dalam anggaran (usulan ≤ ~50 KB terkompresi) | Analisis bundle build | Sebaiknya |
| AC-43 | Semua `<img>` punya dimensi; tidak ada layout shift akibat gambar/font | Audit CLS | Wajib |
| AC-44 | Tidak ada skrip/font pihak ketiga selain yang tercatat di `decisions.md` | Audit tab Network | Wajib |

### E. Responsif dan lintas-perangkat
| ID | Kriteria | Cara verifikasi | Prioritas |
|---|---|---|---|
| AC-50 | Tata letak benar pada lebar 360, 390, 768, 1280 px | Uji viewport | Wajib |
| AC-51 | Berfungsi di Chrome Android, Safari iOS, dan peramban desktop utama (Chrome, Safari, Firefox/Edge) | Uji manual | Wajib |
| AC-52 | Tidak ada konten terpotong oleh area aman (notch) atau bilah browser mobile | Uji perangkat nyata | Sebaiknya |

### F. Keandalan dan kualitas kode
| ID | Kriteria | Cara verifikasi | Prioritas |
|---|---|---|---|
| AC-60 | `astro check` dan build produksi lolos tanpa error/peringatan relevan | CI/lokal | Wajib |
| AC-61 | Skema konten memvalidasi semua entri; entri tidak valid menggagalkan build | Uji dengan data salah sengaja | Wajib |
| AC-62 | Tanpa warna/spasi di luar token desain di komponen | Review kode | Sebaiknya |
| AC-63 | Setiap dependensi tercatat dan dibutuhkan | Review `package.json` | Sebaiknya |

## 7. Anggaran konten (agar performa tidak rusak oleh admin)

Panduan yang nanti masuk ke `deployment.md` (bagian serah-terima admin):
- Foto diunggah dengan sisi terpanjang ±2400px; hero ±2000px
- Format JPG/PNG/WebP dari kamera atau editor, tanpa filter berat
- Maksimal jumlah foto per entri produk (usulan 6) dan per halaman galeri agar halaman tetap ringan
- Alt text wajib diisi dua bahasa; build gagal bila kosong

## 8. Daftar periksa rilis

**Sebelum rilis**
- [ ] Semua AC Wajib di bagian 6 lolos pada build produksi
- [ ] Tidak ada `[TBD]` tersisa di konten maupun dokumen yang memengaruhi situs
- [ ] Nomor WA, akun IG, dan tautan Maps diverifikasi oleh pemilik
- [ ] Konten EN ditinjau
- [ ] Domain dan `site` di `astro.config.mjs` diperbarui; HTTPS aktif
- [ ] Favicon, gambar OG, dan logo benar
- [ ] Pratinjau berbagi tautan diuji

**Setelah rilis**
- [ ] Verifikasi Search Console, kirim sitemap
- [ ] Hubungkan situs di Google Business Profile dan bio Instagram
- [ ] Uji ulang semua tautan WA dari perangkat nyata pada situs live
- [ ] Ukur ulang Lighthouse pada URL produksi
- [ ] Catat hasil awal sebagai baseline (untuk menetapkan target indikator sukses di `prd.md`)

## 9. Pemeliharaan kualitas

- Jalankan ulang Lighthouse dan pemeriksa tautan setelah setiap perubahan besar dan sebelum serah-terima ke admin.
- Setiap perubahan yang melanggar kriteria di atas harus diperbaiki atau dicatat sebagai pengecualian di `decisions.md`.

## 10. Keputusan terbuka

1. Daerah/kata kunci lokal untuk SEO **[TBD]**.
2. Tingkat detail lokasi di Google Business Profile untuk usaha rumahan **[TBD]**.
3. Produk analytics (tanpa cookie) dan jenis event yang dilacak **[TBD]**.
4. Apakah halaman Privasi dibuat di fase 1.
5. Apakah target performa/anggaran di atas disetujui atau perlu disesuaikan setelah pengukuran awal.
