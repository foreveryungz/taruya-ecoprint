# Deployment & Maintenance

> Status: DRAFT v0.1 · Bergantung pada `tech-spec.md` dan `quality.md`.
> Item bertanda **[VERIFIKASI]** harus dicocokkan dengan dokumentasi hosting yang berlaku saat setup.

---

## 1. Ringkasan

Situs statis (`astro build` → folder `dist/`), tanpa server dan tanpa database. Deploy otomatis dari Git, dengan preview untuk setiap branch/PR.

## 2. Pilihan hosting

| Opsi | Catatan |
|---|---|
| **Cloudflare Workers (static assets)** | Dokumentasi Astro menyebut Cloudflare kini merekomendasikan Workers untuk proyek baru; Pages dipertahankan untuk proyek lama. Untuk situs statis, tidak perlu adapter Astro. |
| **Netlify** | Deploy statis dari Git, preview per branch, penanganan 404 mudah. |

Rekomendasi: pilih salah satu berdasarkan kemudahan serah-terima ke admin dan batas paket gratis saat ini **[VERIFIKASI]**. Keputusan dicatat di `decisions.md`.

**Catatan 404 di Cloudflare:** untuk proyek Workers, halaman 404 kustom memerlukan pengaturan `not_found_handling` pada konfigurasi aset (nilai `404-page`) **[VERIFIKASI]**, kalau tidak, 404 tidak tampil sebagaimana mestinya (AC-10).

## 3. Alur kerja Git

- Repositori: satu repo (GitHub/GitLab), `main` selalu siap rilis.
- Cabang fitur → PR → preview URL otomatis → merge ke `main` → deploy produksi otomatis.
- Pesan commit jelas dan kecil.
- Lockfile di-commit. Versi Node dikunci (`.nvmrc`, `engines`).

## 4. Build

| Langkah | Perintah |
|---|---|
| Pasang dependensi | `npm ci` (atau padanan pnpm) |
| Cek tipe & konten | `npm run check` (`astro check`) |
| Build | `npm run build` → `dist/` |
| Pratinjau lokal | `npm run preview` |

CI minimal: jalankan `check` dan `build` pada setiap PR; PR tidak boleh di-merge bila gagal (AC-60, AC-61). Lighthouse CI opsional.

## 5. Domain dan HTTPS

Domain **belum ada** (lihat `prd.md`).

1. Pilih dan beli domain (pertimbangkan `.id` atau `.com`; nama mudah dieja dan sesuai brand) **[TBD]**.
2. Pasang domain di hosting; HTTPS otomatis.
3. Perbarui `site` di `astro.config.mjs` ke URL produksi, lalu build ulang. Ini memengaruhi sitemap, canonical, dan hreflang.
4. Arahkan varian `www`/non-`www` ke satu versi saja (canonical).
5. Perbarui tautan di Instagram, Google Business Profile, dan materi cetak.

Sebelum domain ada, situs boleh live di subdomain bawaan hosting untuk uji, tetapi **jangan diindeks** (`noindex`) sampai domain final terpasang **[usulan]**, agar tidak ada dua URL bersaing di pencarian.

## 6. Lingkungan

| Lingkungan | Fungsi | URL |
|---|---|---|
| Lokal | Pengembangan | `localhost` |
| Preview | Tiap PR/branch | URL otomatis hosting |
| Produksi | Situs live | Domain final |

Tidak ada rahasia/environment variable di fase 1 (nomor WA dan tautan bersifat publik di `site.ts`). Bila nanti ada kunci analytics, simpan sebagai variabel lingkungan hosting, bukan di repo.

## 7. Rilis

Ikuti **daftar periksa rilis** di `quality.md` bagian 8. Ringkas:
1. Semua AC Wajib lolos di build produksi
2. Konten tanpa `[TBD]`
3. Domain dan `site` benar, HTTPS aktif
4. Setelah rilis: Search Console, Google Business Profile, uji ulang tautan WA

## 8. Backup dan pemulihan

- Sumber kebenaran adalah **repositori Git**. Konten (YAML) dan kode ada di sana.
- Foto master asli disimpan terpisah (drive pemilik/penyimpanan cadangan), bukan hanya di repo.
- Pemulihan: deploy ulang dari commit yang baik (rollback lewat hosting atau `git revert`).
- Catat akses penting (akun hosting, domain, Search Console, Google Business Profile) di tempat aman milik pemilik usaha. **Kepemilikan akun domain dan hosting sebaiknya atas nama pemilik usaha**, bukan pribadi developer, agar tidak jadi masalah saat serah-terima **[usulan]**.

## 9. Pemeliharaan rutin

| Frekuensi | Tugas |
|---|---|
| Setiap menambah konten | Cek build di preview sebelum merge |
| Bulanan | Cek tautan rusak, uji ulang tautan WA; tinjau Search Console |
| Per kuartal | Perbarui dependensi (patch/minor), jalankan ulang Lighthouse |
| Per rilis major Astro | Baca panduan upgrade resmi; naikkan versi di branch terpisah |
| Tahunan | Perpanjang domain; periksa akun dan akses; tinjau konten usang (kegiatan lama, info pelatihan) |

Pembaruan dependensi dilakukan bertahap dan terukur, tidak sekaligus saat ada rilis besar.

## 10. Serah-terima ke admin

Direncanakan beberapa bulan setelah live, setelah situs stabil. Persiapan:

**Akses dan alat**
- Akun GitHub (atau alur CMS bila nanti dipilih) untuk admin
- Panduan satu halaman: *cara menambah produk/kegiatan*

**Panduan tugas admin (isi singkat, bergambar bila perlu)**
1. Menyiapkan foto (ukuran, format, nama file)
2. Menambah entri produk/portofolio/kegiatan (salin file YAML contoh, isi field ID dan EN, alt text)
3. Mengecek pratinjau, lalu mempublikasikan
4. Mengubah nomor WA atau tautan kontak (`site.ts`)
5. Apa yang **tidak** boleh diubah tanpa developer (komponen, konfigurasi)

**Keputusan sebelum serah-terima**
- Apakah admin nyaman mengedit file YAML + Git, atau lebih baik memakai CMS ringan berbasis Git (mis. Pages CMS/Decap)? Putuskan lewat uji coba singkat dengan admin, lalu catat di `decisions.md`.
- Siapa yang menerima notifikasi bila build gagal.
- Siapa yang menanggapi chat WA dan seberapa cepat (memengaruhi konversi, bukan teknis, tapi perlu jelas).

Kriteria siap serah-terima: admin bisa menambah satu produk lengkap (ID+EN, foto, alt) dan melihatnya live tanpa bantuan.

## 11. Insiden sederhana

| Kejadian | Langkah |
|---|---|
| Situs tidak bisa diakses | Cek status hosting dan DNS; cek domain belum kedaluwarsa |
| Build gagal setelah menambah konten | Baca pesan error (biasanya skema konten: field kosong, alt hilang); perbaiki file; push ulang |
| Tampilan rusak setelah update | Rollback ke deploy sebelumnya, lalu selidiki di branch |
| Nomor WA berganti | Ubah di `site.ts`; uji ulang semua CTA (AC-04) |

## 12. Keputusan terbuka

1. Hosting: Cloudflare Workers atau Netlify **[TBD]**.
2. Domain: nama, registrar, dan atas nama siapa **[TBD]**.
3. CMS atau edit YAML+Git untuk admin **[TBD, uji sebelum serah-terima]**.
4. Penyimpanan cadangan foto master **[TBD]**.
5. Siapa penerima notifikasi build gagal dan penanggung jawab chat WA.
