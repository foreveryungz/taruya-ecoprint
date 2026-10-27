# agents.md

Aturan kerja untuk AI agent di repo ini. Singkat sengaja: detail ada di `project-docs/`, jangan disalin ke sini.

## Proyek

Website profil statis dua bahasa (Indonesia default, Inggris di `/en/`) untuk usaha **ecoprint** (batik daun alami; fashion & aksesoris). Fungsi: portofolio brand dan pintu ke **WhatsApp**. **Bukan e-commerce.**

Stack: Astro (output statis), TypeScript strict, Tailwind CSS, Astro Content Collections (YAML), JavaScript vanilla kecil. Tanpa backend, tanpa UI framework.

## Baca dulu (sumber kebenaran)

| Butuh | Baca |
|---|---|
| Tujuan, scope, non-goals | `project-docs/prd.md` |
| Halaman, model konten, nada bahasa, pesan WA | `project-docs/content.md` |
| Token warna/tipografi, komponen UI | `project-docs/design.md` |
| Struktur folder, i18n, skema konten, konvensi | `project-docs/tech-spec.md` |
| Kriteria selesai (`AC-xx`), SEO, a11y, performa | `project-docs/quality.md` |
| Build, deploy, serah-terima | `project-docs/deployment.md` |
| Keputusan yang sudah diambil, pertanyaan terbuka | `project-docs/decisions.md` |

Bila dokumen bertentangan dengan permintaan di chat, **tanyakan**, jangan diam-diam memilih salah satu.

## Versi dan dokumentasi

- Astro yang dipakai adalah versi terbaru (v7.x saat dokumen ini ditulis). **Pengetahuanmu mungkin berbasis v3-v5.** Sebelum memakai API Astro, Tailwind, atau integrasi yang tidak pasti, **cek `docs.astro.build` / dokumentasi resmi**. Jangan menebak sintaks dari ingatan.
- Item bertanda `[VERIFIKASI]` di dokumen proyek harus dicocokkan dengan dokumentasi sebelum diimplementasikan.

## Perintah

Gunakan package manager yang lockfile-nya ada di repo.

```
npm run dev       # pengembangan
npm run check     # astro check (tipe + skema konten)
npm run build     # build produksi → dist/
npm run preview   # pratinjau build
```

Sebelum menyatakan tugas selesai: `check` dan `build` harus lolos.

## Aturan keras (jangan dilanggar)

1. **Tidak ada harga** di mana pun. Pakai kalimat standar dari `content.md` §8.
2. **Tidak ada alamat lengkap.** Lokasi hanya nama daerah + tautan Google Maps (termasuk di metadata dan JSON-LD).
3. **Tidak ada fitur e-commerce**: keranjang, checkout, akun, formulir dengan backend.
4. **Dua bahasa selalu lengkap.** Setiap teks UI lewat `t()`; setiap konten punya `id` dan `en`; alt text dua bahasa wajib. Jangan membiarkan teks Indonesia muncul di halaman Inggris.
5. **Semua CTA kontak lewat `buildWaLink()`** dengan pesan sesuai konteks dan bahasa. Jangan menulis URL `wa.me` manual.
6. **Gunakan token desain.** Tanpa warna/spasi acak (`#abc123`, `mt-[13px]`). Tambah token dulu bila perlu.
7. **Tidak menambah dependensi** tanpa entri di `project-docs/decisions.md` (alasan + alternatif yang ditolak).
8. **Tidak ada skrip atau font pihak ketiga** selain yang tercatat di `decisions.md`.
9. **Tidak ada rahasia di repo.** Nomor WA dan tautan sosial publik hidup di `src/config/site.ts`.
10. **Jangan mengarang konten brand** (tagline, klaim "ramah lingkungan", cerita, detail pelatihan). Bila belum ada, tulis placeholder `[TBD]` dan laporkan.

## Konvensi kode

- Komponen `PascalCase.astro`; utilitas `camelCase.ts`; file konten `kebab-case.yaml`.
- Struktur folder mengikuti `tech-spec.md` §3. Halaman EN tipis: panggil komponen bersama dengan `lang="en"`, jangan menyalin logika.
- Konten diurutkan secara eksplisit (`order` / `date`); jangan mengandalkan urutan `getCollection()`.
- Gambar lewat `astro:assets`, dimensi jelas, `lazy` kecuali hero.
- Konten inti dan tautan WA harus tetap berfungsi **tanpa JavaScript**. JS hanya untuk peningkatan (menu, filter, lightbox).
- Aksesibilitas bukan tambahan: heading berurutan, fokus terlihat, kontrol bisa dipakai dengan keyboard (`quality.md` §3).

## Cara bekerja

- Kerjakan **satu perubahan kecil dan terfokus** per langkah. Jangan merombak di luar permintaan.
- Bila ada hal ambigu yang memengaruhi hasil, tanyakan **satu** pertanyaan ringkas, bukan menebak.
- Setelah selesai, laporkan: apa yang diubah, kriteria `AC-xx` yang terkait dan statusnya, serta hal yang tidak bisa diverifikasi (mis. perlu uji di perangkat nyata).
- Keputusan baru yang memengaruhi scope, arsitektur, atau dependensi: tambahkan ke `decisions.md` dalam perubahan yang sama.
- Jangan menghapus atau menimpa dokumen di `project-docs/` tanpa diminta; usulkan perubahannya.
