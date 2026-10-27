# Implementation Plan: Tugas Bertahap untuk AI-Assisted Coding

> Status: DRAFT v0.1 · Turunan dari `prd.md`, `content.md`, `design.md`, `tech-spec.md`, `quality.md`, `deployment.md`.
> Dirancang untuk dikerjakan lewat AI assistant di sidebar VS Code: **satu tugas = satu sesi chat = satu commit**.
> Ukuran: **S** (kecil, < 1 sesi pendek) · **M** (1 sesi) · **L** (bisa dipecah jadi 2 sesi).

---

## 1. Cara memakai dokumen ini

### Aturan kerja per tugas
1. **Satu tugas per sesi chat.** Mulai chat baru untuk tugas berikutnya agar konteks tetap bersih.
2. **Minta rencana dulu** untuk tugas M/L: agent menulis daftar file yang akan dibuat/diubah, kamu setujui, baru dikerjakan.
3. **Tinjau diff sebelum menerima.** Perhatikan terutama: dependensi baru, warna/spasi acak, teks hardcode (harus lewat `t()`), URL `wa.me` manual, harga, dan alamat.
4. **Verifikasi:** `npm run check` dan `npm run build` harus lolos. Untuk tugas UI, lihat hasilnya di browser pada lebar **360, 768, 1280 px** dan bandingkan dengan acuan visual.
5. **Commit per tugas** dengan pesan `T1.2: BaseLayout dan SEOHead`. Cabang per milestone (`m1-fondasi`, dst.); merge ke `main` setelah checkpoint lolos.
6. **Bila agent menulis API Astro/Tailwind dari ingatan** dan hasilnya error atau terasa usang, suruh ia membaca dokumentasi resmi (v7.x) sebelum mencoba lagi.
7. Agent **tidak mengubah dokumen di `project-docs/`**, kecuali **menambah entri baru** di `decisions.md` (dependensi atau keputusan baru). Perubahan dokumen lain dilakukan terpisah, dengan sadar.

### Templat prompt (tempel ke sidebar, isi bagian `< >`)

```
Baca `agents.md`, lalu: <dokumen relevan, mis. tech-spec.md §5, quality.md §3>.

Tugas: <ID + judul, mis. T1.2 BaseLayout dan SEOHead>.

Kerjakan hanya lingkup tugas ini (rincian ada di project-docs/implementation-plan.md).
1. Tulis rencana singkat: file yang akan dibuat/diubah. Tunggu persetujuanku.
2. Setelah disetujui, kerjakan.
3. Jalankan `npm run check` dan `npm run build`.
4. Laporkan: apa yang berubah, kriteria AC-xx terkait + statusnya,
   dan hal yang tidak bisa kamu verifikasi.

Batasan: jangan ubah project-docs/ (kecuali menambah entri di decisions.md
untuk dependensi/keputusan baru), jangan mengarang konten brand (pakai [TBD]).
Bila ragu soal API Astro v7 atau Tailwind, cek dokumentasi resmi dulu.
```

Untuk tugas kecil (S), langkah "rencana dulu" boleh dilewati.

### Tanda
- 🔒 **Butuh input** dari pemilik/kamu sebelum bisa final (lihat §3). Tugas tetap bisa dikerjakan dengan placeholder `[TBD]`.
- 🏁 **Checkpoint**: berhenti, uji, dan putuskan sebelum lanjut.

---

## 2. Peta milestone

| Milestone | Hasil | Tugas |
|---|---|---|
| **M0** Setup | Repo, Astro, Tailwind, token, font | T0.1-T0.4 |
| **M1** Fondasi | Layout, i18n, navigasi, tautan WA | T1.1-T1.4 |
| **M2** Lapisan konten | Skema, data contoh | T2.1-T2.3 |
| **M3** Halaman | Semua halaman ID+EN dengan data contoh | T3.1-T3.10 |
| **M4** Konten nyata | Konten dan foto asli, EN ditinjau | T4.1-T4.4 |
| **M5** SEO dan analytics | Sitemap, JSON-LD, OG, event klik WA | T5.1-T5.4 |
| **M6** Kualitas | Audit a11y, performa, uji perangkat, AC | T6.1-T6.4 |
| **M7** Rilis | Hosting, domain, rilis | T7.1-T7.3 |
| **M8** Serah-terima | Panduan admin, baseline | T8.1-T8.3 |

Urutan sengaja: **bangun kerangka dengan data contoh dulu (M0-M3)**, jadi tidak menunggu konten pemilik. Konten nyata masuk di M4.

---

## 3. Input yang dibutuhkan (kapan dan dari siapa)

| Input | Dari | Dibutuhkan sebelum | Rujukan |
|---|---|---|---|
| Warna logo (hex), file logo SVG | Kamu | T0.2 (bisa pakai palet usulan dulu) | Q-10 |
| 3-5 foto asli untuk data contoh | Kamu | T2.2 | `content.md` §5 |
| Acuan visual (3-5 tangkapan layar) | Kamu | T3.2 | `design.md` |
| Tagline, cerita brand | Pemilik | T4.2 | Q-01 |
| Gaya sapaan (kami/Anda atau kamu) | Pemilik | T4.2 | Q-02 |
| Detail pelatihan, alur pre-order/custom | Pemilik | T4.2 | Q-03, Q-04 |
| Daftar kegiatan | Pemilik | T4.2 | Q-05 |
| Jam respons WA | Pemilik | T3.10 (final di T4.2) | Q-06 |
| Domain | Kamu/pemilik | T7.2 | Q-14 |
| Produk analytics | Kamu | T5.4 | Q-15 |
| Hosting | Kamu | T7.1 | Q-13 |

---

## M0: Setup

### T0.1 Inisialisasi repo dan Astro (S)
- **Baca:** `tech-spec.md` §1-3, §13
- **Kerjakan:** buat proyek dengan `npm create astro@latest` (template minimal, TypeScript `strict`). Pasang `.nvmrc` dan `engines`. Salin `agents.md` ke root dan `project-docs/` ke repo. `.gitignore`, README singkat, `git init`.
- **Selesai bila:** `dev`, `check`, `build` berjalan; struktur folder dasar sesuai `tech-spec.md` §3 (folder kosong boleh).
- **Di luar scope:** styling, halaman.

### T0.2 Tailwind dan design token (M)
- **Baca:** `design.md` §2-4, `tech-spec.md` §8
- **Kerjakan:** pasang Tailwind sesuai panduan Astro untuk versi terpasang **(V-03)**. Definisikan token warna, tipografi, dan spasi di `src/styles/global.css` sesuai `design.md` dan petakan ke tema Tailwind. Halaman uji sementara menampilkan palet dan skala tipografi.
- 🔒 Warna final menunggu logo (Q-10); mulai dengan palet usulan.
- **Selesai bila:** kelas seperti `bg-bg`, `text-ink`, `bg-primary` bekerja; tidak ada nilai warna hardcode di komponen.

### T0.3 Font self-host dan gaya dasar (S)
- **Baca:** `design.md` §3, `tech-spec.md` §1
- **Kerjakan:** pasang font (fitur Fonts bawaan Astro atau `@fontsource`, **V-04**), maksimal 2 keluarga dan 2-3 bobot, `font-display: swap`, fallback system font. Gaya dasar: body, heading, tautan, fokus.
- **Selesai bila:** tipografi sesuai skala di `design.md`; tidak ada request font ke pihak ketiga (AC-44).

### T0.4 Tooling dan CI minimal (S)
- **Baca:** `tech-spec.md` §13, `deployment.md` §3-4
- **Kerjakan:** Prettier + `prettier-plugin-astro`; skrip `dev`, `build`, `preview`, `check`. Workflow CI yang menjalankan `check` dan `build` pada PR (opsional bila belum ada hosting Git).
- **Selesai bila:** `npm run check` dan `build` lolos bersih (AC-60).

---

## M1: Fondasi

### T1.1 Konfigurasi situs dan inti i18n (M)
- **Baca:** `tech-spec.md` §4-5, `content.md` §2, §7
- **Kerjakan:** `src/config/site.ts`; konfigurasi `i18n` Astro (**V-02**); `src/i18n/ui.ts` (kamus ID/EN untuk menu dan label dasar), `routes.ts` (peta slug per bahasa sesuai sitemap), `utils.ts` (`t()`, `getLangFromUrl()`, `localizedPath()`).
- 🔒 Nama brand, nomor WA, IG, Maps (sudah diisi di PRD; salin ke `site.ts`).
- **Selesai bila:** `t('id', key)` dan `t('en', key)` bekerja; kunci EN yang hilang memunculkan error atau peringatan jelas, bukan fallback diam-diam.

### T1.2 BaseLayout dan SEOHead (M)
- **Baca:** `tech-spec.md` §5, §12, `quality.md` §2-3
- **Kerjakan:** `BaseLayout.astro` (`<html lang>`, skip link, landmark), `SEOHead` (title, description, canonical, hreflang `id`/`en`/`x-default`, Open Graph/Twitter dasar).
- **Selesai bila:** sumber halaman uji memuat canonical dan hreflang yang benar dan berbalasan (AC-22 sebagian).

### T1.3 Header, Footer, LanguageSwitcher (M)
- **Baca:** `design.md` §6-8, `quality.md` §3
- **Kerjakan:** header sticky dengan menu ringkas di mobile (JS kecil, atribut ARIA), footer dengan kontak singkat, pengalih bahasa yang memakai `routes.ts` agar mengarah ke padanan halaman yang sama.
- **Selesai bila:** menu bisa dipakai dengan keyboard; fokus terlihat; pengalih bahasa benar di semua halaman uji (AC-02, AC-32 sebagian).

### T1.4 Tautan WhatsApp dan Button (S)
- **Baca:** `content.md` §7, `tech-spec.md` §11
- **Kerjakan:** `src/lib/whatsapp.ts` (`buildWaLink(lang, message)` dengan `encodeURIComponent`); komponen `Button` (primary/secondary/text) dan `WhatsAppButton`. Template pesan di `ui.ts`.
- **Selesai bila:** tombol membuka `wa.me` dengan nomor dan pesan benar untuk kedua bahasa; label menyebut konteks (AC-04 sebagian).

### 🏁 Checkpoint M1
Halaman uji ID dan EN: header, footer, pengalih bahasa, tombol WA berfungsi. `check` dan `build` lolos. Merge ke `main`.

---

## M2: Lapisan konten

### T2.1 Skema Content Collections (M)
- **Baca:** `tech-spec.md` §6, `content.md` §4
- **Kerjakan:** `src/content.config.ts` untuk `products`, `portfolio`, `activities`, `services` dengan loader `glob()` dan Zod dari `astro/zod`. Bentuk helper `image()` dicek di dokumentasi v7 **(V-01)**. Field dua bahasa wajib; alt dua bahasa wajib.
- **Selesai bila:** entri yang melanggar skema (alt kosong, kategori salah) menggagalkan build (AC-61).

### T2.2 Data contoh (S)
- **Baca:** `content.md` §3-4
- **Kerjakan:** 3 produk, 3 portofolio, 2 kegiatan, 4 layanan (shop, custom-order, pre-order, training) dalam YAML dengan 3-5 foto asli. Semua teks contoh diberi tanda jelas (mis. awalan `[CONTOH]`) agar tidak lolos ke produksi.
- 🔒 Foto asli dari kamu.
- **Selesai bila:** semua entri lolos validasi; foto diproses `astro:assets`.

### T2.3 Helper konten (S)
- **Kerjakan:** `getLocalized(field, lang)`, helper pengurutan (`order`, `date`), helper pengelompokan per kategori. Tanpa mengandalkan urutan `getCollection()`.
- **Selesai bila:** helper dipakai konsisten (belum di halaman; dites lewat halaman uji).

### 🏁 Checkpoint M2
Data contoh tampil di halaman uji untuk ID dan EN; build gagal bila skema dilanggar.

---

## M3: Komponen dan halaman

> Setiap halaman dibuat **dua bahasa sekaligus** lewat komponen halaman bersama (`lang` sebagai prop); file di `src/pages/en/` tipis. Gunakan teks contoh bertanda `[CONTOH]`/`[TBD]`.

### T3.1 Komponen dasar (M)
- **Baca:** `design.md` §4-6
- **Kerjakan:** `Container`, `SectionHeading`, `Hero`, `ProcessSteps`.
- **Selesai bila:** hanya memakai token desain; heading mengikuti hierarki.

### T3.2 Beranda (L) 🏁
- **Baca:** `content.md` §3 (Beranda), `design.md` seluruhnya
- **Kerjakan:** hero, pengantar, sorotan kategori, proses singkat, portofolio pilihan, ringkasan layanan, kegiatan terbaru, CTA penutup.
- 🔒 Acuan visual (3-5 tangkapan layar) dari kamu, dilampirkan ke chat agent.
- **Selesai bila:** tampil benar di 360/768/1280 px; LCP tidak terhambat hero (AC-40/41 pemeriksaan awal).
- **Checkpoint visual:** nilai Beranda dengan jujur. Bila terasa generik, putuskan apakah perlu iterasi desain (atau mencoba alat bantu desain di branch terpisah) **sebelum** membangun sisa halaman, karena perbaikan di sini murah dan di akhir mahal.

### T3.3 Kartu: Product, Category, Service, Activity (M)
- **Baca:** `design.md` §6, `tech-spec.md` §9
- **Kerjakan:** keempat komponen kartu; `ProductCard` menampilkan ketersediaan sebagai teks (bukan hanya warna) dan CTA WA kontekstual, **tanpa harga**.
- **Selesai bila:** AC-06 dan AC-07 terpenuhi untuk data contoh.

### T3.4 Halaman Layanan (M)
- **Baca:** `content.md` §3 (Layanan), §5-8
- **Kerjakan:** empat bagian layanan (deskripsi, cocok untuk siapa, langkah, CTA WA).
- 🔒 Detail pelatihan dan alur (Q-03, Q-04): pakai `[TBD]` dulu.

### T3.5 Halaman Katalog (M)
- **Baca:** `content.md` §3 (Katalog)
- **Kerjakan:** grid per kategori dari koleksi `products`, tanpa halaman detail (D-11).

### T3.6 Halaman Portofolio dengan filter (M)
- **Baca:** `content.md` §3 (Portofolio), `tech-spec.md` §10
- **Kerjakan:** grid dengan filter kategori berbasis `data-category` + JS kecil; **tanpa JS semua item tetap tampil**.
- **Selesai bila:** AC-08 lolos.

### T3.7 Lightbox (M)
- **Baca:** `tech-spec.md` §10, `quality.md` §3
- **Kerjakan:** `<dialog>` native + JS kecil; Esc menutup, fokus masuk dan kembali ke pemicu, tombol tutup berlabel; dipakai di Katalog dan Portofolio.
- **Selesai bila:** AC-36 lolos.

### T3.8 Halaman Kegiatan (S)
- **Baca:** `content.md` §3 (Kegiatan)
- **Kerjakan:** daftar kronologis (terbaru di atas) dari koleksi `activities`.

### T3.9 Halaman Tentang dan Proses (S)
- **Baca:** `content.md` §3 (Tentang)
- **Kerjakan:** struktur cerita brand, penjelasan ecoprint, langkah proses, perawatan produk; isi `[TBD]` untuk cerita yang belum ada (jangan mengarang).

### T3.10 Halaman Kontak dan 404 (S)
- **Baca:** `content.md` §3 (Kontak), `prd.md` §8
- **Kerjakan:** blok kontak (WA, IG, tautan Maps), lokasi hanya nama daerah, **tanpa embed Maps otomatis**; halaman 404 dua bahasa dengan jalan kembali.
- **Selesai bila:** AC-05, AC-09, AC-10 lolos untuk data yang ada.

### 🏁 Checkpoint M3
Seluruh sitemap ada dalam ID dan EN dengan data contoh (AC-01, AC-02). Cek cepat di perangkat nyata. Merge ke `main`.

---

## M4: Konten nyata

### T4.1 Audit dan persiapan foto (M) 🔒
- **Baca:** `content.md` §5, `quality.md` §7
- **Kerjakan:** cocokkan foto yang ada dengan inventori; tandai yang kurang. Siapkan foto (sisi terpanjang ±2400px, nama deskriptif). Agent boleh membantu membuat skrip ringan untuk resize/rename di luar repo atau di `scripts/`; tanpa dependensi baru tanpa entri `decisions.md`.

### T4.2 Konten nyata Bahasa Indonesia (L) 🔒
- **Baca:** `content.md` seluruhnya
- **Kerjakan:** ganti semua data contoh dengan konten asli (produk, portofolio, kegiatan, layanan, cerita brand, tagline) mengikuti gaya sapaan yang dipilih. Agent hanya **memformat dan memasukkan** konten dari pemilik; ia tidak mengarang klaim brand.
- 🔒 Q-01 sampai Q-06.

### T4.3 Konten Bahasa Inggris (L)
- **Kerjakan:** terjemahan/penulisan EN untuk semua konten dan string UI; kalimat pendek dan jelas untuk pembaca non-native. **Tinjau oleh manusia** sebelum dianggap selesai.
- **Selesai bila:** AC-03 lolos.

### T4.4 Pembersihan placeholder (S)
- **Kerjakan:** cari dan hapus semua `[TBD]`, `[CONTOH]`, `lorem` dari konten dan kode.
- **Selesai bila:** AC-12 lolos.

### 🏁 Checkpoint M4
Pemilik meninjau situs lokal/preview: konten benar, foto tepat, klaim bisa dipertanggungjawabkan.

---

## M5: SEO dan metadata

### T5.1 Sitemap, robots, dan kontrol indeks (M)
- **Baca:** `quality.md` §2, `deployment.md` §5
- **Kerjakan:** `@astrojs/sitemap` dengan i18n; `robots.txt`; mekanisme `noindex` yang aktif selama domain sementara dan mudah dimatikan saat domain final (U-04).
- **Selesai bila:** AC-23 lolos; sitemap memakai URL domain yang benar (`site` di config).

### T5.2 JSON-LD LocalBusiness (S)
- **Baca:** `quality.md` §2, `prd.md` §8
- **Kerjakan:** data terstruktur `LocalBusiness` **tanpa alamat jalan**; hanya daerah, tautan sosial, kontak.
- **Selesai bila:** AC-25 dan AC-09 lolos.

### T5.3 Gambar OG, favicon, ikon (S)
- **Kerjakan:** gambar OG 1200×630 untuk halaman utama, favicon, ikon.
- **Selesai bila:** AC-24 lolos (uji pratinjau berbagi tautan).

### T5.4 Analytics dan event klik WhatsApp (S) 🔒
- **Baca:** `tech-spec.md` §10, `quality.md` §5
- **Kerjakan:** pasang analytics tanpa cookie pilihanmu (**Q-15**) dan satu listener untuk elemen `[data-track="wa"]` yang mencatat klik WhatsApp beserta konteksnya (halaman/produk). Tanpa banner persetujuan bila tidak diperlukan. Catat produk dan alasannya di `decisions.md`.
- **Selesai bila:** klik WA tercatat di dasbor; AC-44 tetap lolos (skrip pihak ketiga tercatat); skor performa tidak turun di bawah target.

---

## M6: Kualitas

> Pendekatan: agent **mengaudit dan memperbaiki**, kamu **memverifikasi di perangkat/alat nyata**. Jangan percaya klaim "sudah lolos" tanpa bukti (skor Lighthouse, tangkapan layar, hasil uji keyboard).

### T6.1 Audit aksesibilitas (M)
- **Baca:** `quality.md` §3, AC-30 sampai AC-39
- **Kerjakan:** audit komponen terhadap daftar §3; perbaiki. Kamu uji keyboard penuh dan (sebaiknya) pembaca layar singkat.

### T6.2 Audit performa (M)
- **Baca:** `quality.md` §4, AC-40 sampai AC-44
- **Kerjakan:** analisis bundle JS, ukuran gambar, `sizes`/`widths`, dimensi gambar, font. Perbaiki yang melewati anggaran.
- **Catatan:** angka anggaran adalah usulan; sesuaikan dengan hasil ukur.

### T6.3 Uji lintas perangkat dan tautan (M) 🔒
- **Baca:** AC-04, AC-05, AC-11, AC-50 sampai AC-52
- **Kerjakan (manual, kamu):** uji tautan WA di Android dan iOS, uji di peramban utama dan lebar 360/390/768/1280. Agent boleh membuat skrip pemeriksa tautan.

### T6.4 Jalankan seluruh acceptance criteria (M)
- **Kerjakan:** buat tabel hasil per `AC-xx` (lolos/gagal/bukti) dan perbaiki yang gagal. Pengecualian dicatat di `decisions.md`.
- **Selesai bila:** semua AC **Wajib** lolos.

### 🏁 Checkpoint M6
Siap rilis secara teknis. Daftar periksa "Sebelum rilis" di `quality.md` §8 tercentang.

---

## M7: Rilis

### T7.1 Hosting dan deploy preview (M)
- **Baca:** `deployment.md` §2-4, §6
- **Kerjakan:** hubungkan repo ke hosting (Workers static assets atau Netlify; **Q-13**), deploy otomatis dari `main`, preview per branch. Cek 404 kustom di hosting terpilih **(V-05)**; catat batas paket gratis **(V-06)**.
- **Selesai bila:** push ke `main` men-deploy; PR punya preview.

### T7.2 Domain dan HTTPS (S) 🔒
- **Baca:** `deployment.md` §5
- **Kerjakan:** pasang domain (**Q-14**), HTTPS, perbarui `site` di `astro.config.mjs`, matikan `noindex`, arahkan `www`/non-`www` ke satu versi. Akun domain atas nama pemilik usaha (U-05).

### T7.3 Rilis dan pasca-rilis (S)
- **Baca:** `quality.md` §8
- **Kerjakan:** jalankan daftar periksa rilis; Search Console + kirim sitemap; hubungkan situs di Google Business Profile dan bio Instagram; uji ulang tautan WA di situs live; ukur Lighthouse di URL produksi.

---

## M8: Stabilisasi dan serah-terima

### T8.1 Panduan admin (M)
- **Baca:** `deployment.md` §10
- **Kerjakan:** panduan satu halaman bergambar: menyiapkan foto, menambah produk/kegiatan (salin file YAML contoh, isi ID dan EN, alt), cek preview, publikasi, mengubah kontak.

### T8.2 Uji admin dan keputusan CMS (M) 🔒
- **Kerjakan:** minta admin menambah satu produk lengkap tanpa bantuan. Dari hasilnya putuskan: tetap YAML+Git atau pakai CMS ringan (**Q-19**). Catat di `decisions.md`.
- **Selesai bila:** admin berhasil menambah satu produk (ID+EN, foto, alt) dan melihatnya live.

### T8.3 Baseline metrik (S)
- **Kerjakan:** catat hasil awal (klik WA, kunjungan halaman, Lighthouse) dan tetapkan target di `prd.md` **(Q-18)**.

---

## 4. Lacak kemajuan

Salin ke catatan pribadi atau issue tracker:

- [ ] M0: T0.1 · T0.2 · T0.3 · T0.4
- [ ] M1: T1.1 · T1.2 · T1.3 · T1.4 🏁
- [ ] M2: T2.1 · T2.2 · T2.3 🏁
- [ ] M3: T3.1 · T3.2 🏁 · T3.3 · T3.4 · T3.5 · T3.6 · T3.7 · T3.8 · T3.9 · T3.10 🏁
- [ ] M4: T4.1 · T4.2 · T4.3 · T4.4 🏁
- [ ] M5: T5.1 · T5.2 · T5.3 · T5.4
- [ ] M6: T6.1 · T6.2 · T6.3 · T6.4 🏁
- [ ] M7: T7.1 · T7.2 · T7.3
- [ ] M8: T8.1 · T8.2 · T8.3
