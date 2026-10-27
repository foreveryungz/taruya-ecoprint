# Tech Spec: Stack, Struktur, dan Konvensi

> Status: DRAFT v0.1 · Bergantung pada `prd.md`, `content.md`, `design.md`.
> Dokumen ini adalah sumber untuk bagian teknis `agents.md`.
> Item bertanda **[VERIFIKASI]** harus dicocokkan dengan dokumentasi resmi versi yang terpasang sebelum dipakai.

---

## 1. Keputusan teknis ringkas

| Area           | Pilihan                                                                               | Alasan                                                |
| -------------- | ------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| Framework      | **Astro**, output **statis**                                                          | Situs konten, tanpa backend, performa terbaik         |
| Bahasa         | TypeScript (`strict`)                                                                 | Skema konten dan komponen aman tipe                   |
| Styling        | **Tailwind CSS** dengan design token dari `design.md`                                 | Cepat, konsisten, mudah dibantu AI                    |
| UI framework   | **Tidak ada** (komponen Astro saja)                                                   | Tidak perlu React/Vue; bundle JS minimal              |
| Konten         | **Astro Content Collections** (file YAML + Markdown)                                  | Terstruktur, tervalidasi, aman didelegasikan ke admin |
| i18n           | Routing i18n bawaan Astro (ID default, EN di `/en/`)                                  | Tanpa pustaka tambahan                                |
| Gambar         | `astro:assets`                                                                        | Ukuran responsif dan format modern otomatis           |
| Font           | Self-host (fitur Fonts bawaan Astro bila sesuai, atau `@fontsource`) **[VERIFIKASI]** | Performa dan privasi                                  |
| Interaktivitas | JavaScript vanilla kecil (lightbox, filter, menu mobile)                              | Tanpa dependensi besar                                |
| Hosting        | Hosting statis (Cloudflare Pages atau Netlify)                                        | Gratis, CDN, preview per branch                       |
| Analytics      | Privacy-friendly + event klik WA **[TBD produk]**                                     | Mengukur konversi utama                               |

## 2. Catatan versi (penting untuk AI-assisted coding)

- Per 26 Okt 2026, dokumentasi Astro menyebut rilis terbaru adalah **v7.x** (v7.4.2). Banyak materi dan memori model AI masih berbasis v3-v5, jadi **API bisa berbeda**.
- Aturan: pasang versi terbaru lewat `npm create astro@latest`, **kunci versi di `package.json`**, dan **cek `docs.astro.build` sebelum memakai API apa pun yang tidak pasti**. Jangan menebak dari ingatan.
- Dari dokumentasi v7 yang sudah diverifikasi: koleksi konten didefinisikan di `src/content.config.ts` dengan `defineCollection`, loader `glob()` dan `file()` dari `astro/loaders`, dan Zod diimpor dari `astro/zod` (Zod 4).
- Kunci versi Node.js di `.nvmrc` (LTS aktif) dan `engines` di `package.json`.

## 3. Struktur folder

```
/
├── agents.md
├── project-docs/                 # dokumen ini
├── public/
│   ├── favicon.svg
│   ├── robots.txt
│   └── og/                       # gambar Open Graph
├── src/
│   ├── assets/
│   │   ├── images/               # foto sumber (dioptimasi Astro)
│   │   └── logo/
│   ├── components/
│   │   ├── layout/               # Header, Footer, LanguageSwitcher
│   │   ├── ui/                   # Button, SectionHeading, dll.
│   │   ├── cards/                # ProductCard, ActivityCard, ServiceCard, CategoryCard
│   │   ├── sections/             # Hero, ProcessSteps, ContactBlock, PortfolioGrid
│   │   └── seo/                  # SEOHead, JsonLd
│   ├── config/
│   │   └── site.ts               # nama brand, kontak, URL sosial
│   ├── content/
│   │   ├── products/*.yaml
│   │   ├── portfolio/*.yaml
│   │   ├── activities/*.yaml
│   │   └── services/*.yaml
│   ├── content.config.ts         # definisi koleksi + skema
│   ├── i18n/
│   │   ├── ui.ts                 # kamus string UI (id, en)
│   │   ├── routes.ts             # peta rute per bahasa
│   │   └── utils.ts              # t(), getLangFromUrl(), localizedPath()
│   ├── layouts/
│   │   └── BaseLayout.astro
│   ├── lib/
│   │   └── whatsapp.ts           # buildWaLink(lang, message)
│   ├── pages/
│   │   ├── index.astro           # Beranda (ID)
│   │   ├── tentang.astro
│   │   ├── portofolio.astro
│   │   ├── katalog.astro
│   │   ├── layanan.astro
│   │   ├── kegiatan.astro
│   │   ├── kontak.astro
│   │   ├── 404.astro
│   │   └── en/                   # padanan bahasa Inggris
│   │       ├── index.astro
│   │       ├── about.astro
│   │       ├── portfolio.astro
│   │       ├── catalog.astro
│   │       ├── services.astro
│   │       ├── activities.astro
│   │       └── contact.astro
│   ├── scripts/                  # JS vanilla (lightbox, filter, menu)
│   └── styles/
│       └── global.css            # token desain + Tailwind
├── astro.config.mjs
├── tsconfig.json
├── package.json
└── .nvmrc
```

**Pola agar halaman ID dan EN tidak duplikat logika:** setiap halaman Inggris tipis, hanya memanggil komponen halaman bersama dengan prop `lang="en"`. Isi halaman berada di komponen (mis. `components/pages/`), bukan disalin dua kali. Detail pola ini diputuskan saat implementasi.

## 4. Konfigurasi situs (`src/config/site.ts`)

Satu tempat untuk data yang dipakai di banyak halaman:

| Field           | Keterangan                                                                 |
| --------------- | -------------------------------------------------------------------------- |
| `name`          | Nama brand (dari `prd.md`)                                                 |
| `url`           | Domain produksi **[TBD, belum punya domain]**; sementara pakai URL hosting |
| `whatsapp`      | Nomor format internasional tanpa `+` atau spasi                            |
| `instagram`     | URL akun                                                                   |
| `mapsUrl`       | Tautan Google Maps                                                         |
| `locationLabel` | Nama daerah (tanpa alamat lengkap)                                         |
| `defaultLang`   | `id`                                                                       |

Tidak ada rahasia di sini; semua nilai bersifat publik.

## 5. Internasionalisasi

- **Bahasa:** `id` (default, tanpa prefiks) dan `en` (prefiks `/en/`). Gunakan konfigurasi `i18n` bawaan Astro dan modul `astro:i18n` untuk membuat URL lokal. **[VERIFIKASI]** opsi persisnya di dokumentasi v7.
- **Slug dilokalkan** (`/tentang` ↔ `/en/about`), dipetakan di `src/i18n/routes.ts`: kunci halaman → path per bahasa. `LanguageSwitcher` memakai peta ini supaya pengalih bahasa selalu mengarah ke padanan halaman yang sama.
- **String UI** (menu, tombol, label) ada di `src/i18n/ui.ts` sebagai objek `{ id: {...}, en: {...} }` dengan fungsi `t(lang, key)`. Jika kunci EN kosong, build gagal atau memperingatkan; **tidak** diam-diam jatuh ke bahasa Indonesia.
- **Konten** (produk, portofolio, dll.) berisi field dua bahasa langsung di dalam entri (`{ id, en }`), sehingga satu entri = satu file, dua bahasa.
- **SEO:** setiap halaman punya `<link rel="alternate" hreflang>` untuk `id`, `en`, dan `x-default`, plus `<html lang>` yang benar.
- **Tanpa pengalihan otomatis berdasarkan bahasa browser** di fase 1. Pengunjung memilih sendiri. Ini menghindari masalah SEO dan hasil yang membingungkan.

## 6. Content Collections

Didefinisikan di `src/content.config.ts`. Format data: **YAML** (lebih ramah admin dibanding JSON). Skema memakai Zod. Bagian di bawah adalah **sketsa**; sesuaikan bentuk helper gambar dengan dokumentasi v7 **[VERIFIKASI]**.

```ts
import { defineCollection } from "astro:content";
import { glob } from "astro/loaders";
import { z } from "astro/zod";

const bilingual = z.object({ id: z.string().min(1), en: z.string().min(1) });
const category = z.enum([
  "pakaian",
  "kain",
  "sepatu",
  "gelas-tumbler",
  "lainnya",
]);

const products = defineCollection({
  loader: glob({ pattern: "**/*.yaml", base: "./src/content/products" }),
  schema: (
    { image }, // [VERIFIKASI] bentuk helper image()
  ) =>
    z.object({
      category,
      title: bilingual,
      description: bilingual,
      availability: z.enum(["ready", "pre-order", "custom"]),
      images: z.array(z.object({ src: image(), alt: bilingual })).min(1),
      materials: bilingual.optional(),
      featured: z.boolean().default(false),
      order: z.number().default(0),
    }),
});
```

Koleksi lain (`portfolio`, `activities`, `services`) mengikuti pola sama dengan field dari `content.md` bagian 4.

**Aturan:**

- `id` entri dari nama file (kebab-case), unik dan stabil; jangan diganti setelah dipublikasikan.
- Urutan: `getCollection()` tidak menjamin urutan, jadi **selalu urutkan sendiri** (`order`, atau `date` untuk kegiatan).
- Alt text dua bahasa **wajib**, supaya admin tidak melewatkannya. Longgarkan hanya bila terbukti menyulitkan.
- Halaman detail produk/kegiatan tidak dibuat di fase 1 (lihat `content.md`). Struktur data sudah siap bila nanti ditambahkan lewat dynamic route + `getStaticPaths()`.

## 7. Gambar

- Foto sumber disimpan di `src/assets/images/` (atau diacu dari entri konten) dan diproses lewat `astro:assets` (`<Image>` / `<Picture>`).
- Format keluaran modern (WebP/AVIF) dengan ukuran responsif (`widths`, `sizes`).
- Hero: `loading="eager"` dan `fetchpriority="high"`; gambar lain `lazy`.
- Selalu tentukan `width`/`height` atau rasio untuk mencegah layout shift.
- Master foto **tidak** di-commit mentah dalam ukuran sangat besar. Batasi sisi terpanjang ±2400px sebelum masuk repo supaya repo dan waktu build tetap ringan. Bila volume foto besar, pertimbangkan penyimpanan terpisah (keputusan nanti).
- Gambar Open Graph per halaman (1200×630) disiapkan manual untuk fase 1.

## 8. Styling

- Tailwind CSS, dipasang sesuai panduan "Styles and CSS" di dokumentasi Astro **[VERIFIKASI]** (cara integrasi berubah antar-versi Tailwind).
- Token dari `design.md` didefinisikan sebagai CSS variable di `src/styles/global.css` lalu dipetakan ke tema Tailwind, sehingga kelas seperti `bg-bg`, `text-ink`, `bg-primary` memakai nilai token.
- **Dilarang** nilai warna/spasi acak (`#abc123`, `mt-[13px]`) di komponen. Tambahkan token dulu, baru pakai.
- Gaya khusus hanya untuk hal yang tidak praktis lewat utility (mis. lightbox).
- Mobile-first. Breakpoint default Tailwind.

## 9. Komponen

Daftar mengikuti `design.md` bagian 6. Kontrak props ringkas:

| Komponen                                        | Props utama                                                            |
| ----------------------------------------------- | ---------------------------------------------------------------------- |
| `BaseLayout`                                    | `lang`, `title`, `description`, `ogImage?`, `pageKey` (untuk hreflang) |
| `Header`                                        | `lang`, `pageKey`                                                      |
| `LanguageSwitcher`                              | `lang`, `pageKey`                                                      |
| `Button`                                        | `href`, `variant: 'primary' \| 'secondary' \| 'text'`, `external?`     |
| `WhatsAppButton`                                | `lang`, `message`, `label?`, `variant?`                                |
| `Hero`                                          | `title`, `tagline`, `image`, `cta`                                     |
| `ProductCard`                                   | `entry` (CollectionEntry), `lang`                                      |
| `PortfolioGrid`                                 | `items`, `lang`, `filterable?`                                         |
| `Lightbox`                                      | dipasang sekali per halaman; membuka gambar dari grid                  |
| `ActivityCard` / `ServiceCard` / `CategoryCard` | `entry` atau data ringkas, `lang`                                      |
| `ContactBlock`                                  | `lang` (data dari `site.ts`)                                           |
| `SEOHead`                                       | `title`, `description`, `lang`, `pageKey`, `ogImage?`                  |

Aturan komponen: tanpa logika bisnis di dalam; teks UI selalu lewat `t()`; setiap komponen yang menampilkan gambar menerima `alt` dari data, bukan hardcode.

## 10. Interaktivitas (JavaScript)

Hanya yang diperlukan, dan harus tetap **berfungsi tanpa JS** untuk konten dasar (progressive enhancement):

| Fitur             | Pendekatan                                                                |
| ----------------- | ------------------------------------------------------------------------- |
| Menu mobile       | Tombol + atribut ARIA, JS kecil                                           |
| Filter portofolio | Atribut `data-category` + JS kecil; tanpa JS semua item tetap tampil      |
| Lightbox          | Elemen `<dialog>` native + JS kecil; Esc menutup, fokus kembali ke pemicu |
| Tombol WA         | Tautan biasa (`<a href>`), tanpa JS                                       |
| Event analytics   | Satu listener untuk klik `[data-track="wa"]` **[TBD produk analytics]**   |

Kalau kebutuhan lightbox melebar (zoom, geser, pinch), pertimbangkan pustaka ringan; catat di `decisions.md` sebelum menambah dependensi.

## 11. Tautan WhatsApp

`src/lib/whatsapp.ts` menyediakan `buildWaLink(lang, message)`:

- Basis: `https://wa.me/<nomor>?text=<pesan di-encode>`
- Nomor dari `site.ts`; pesan dari template di `content.md` bagian 7 (disimpan di `ui.ts`).
- Pesan dienkode dengan `encodeURIComponent`.
- Tautan eksternal memakai `rel="noopener noreferrer"` dan `target="_blank"` bila dibuka di tab baru.

## 12. SEO teknis (ringkas, detail di `quality.md`)

- `@astrojs/sitemap` dengan dukungan i18n, `robots.txt`, canonical per halaman
- `hreflang` dua arah, JSON-LD `LocalBusiness` dengan **lokasi sebatas daerah** (tanpa alamat jalan, sesuai `prd.md`)
- Open Graph dan Twitter card per halaman

## 13. Tooling dan konvensi kode

| Hal             | Aturan                                                                              |
| --------------- | ----------------------------------------------------------------------------------- |
| Package manager | Satu saja (pnpm atau npm); simpan lockfile **[TBD]**                                |
| TypeScript      | `strict`; jalankan `astro check` sebelum commit                                     |
| Format          | Prettier + `prettier-plugin-astro`                                                  |
| Lint            | ESLint opsional; jangan menambah bila tidak dipakai                                 |
| Penamaan        | Komponen `PascalCase.astro`, utilitas `camelCase.ts`, file konten `kebab-case.yaml` |
| Dependensi      | Tambah seminimal mungkin; setiap dependensi baru dicatat di `decisions.md`          |
| Commit          | Pesan jelas dan kecil per perubahan (Conventional Commits opsional)                 |
| Skrip           | `dev`, `build`, `preview`, `check`                                                  |

## 14. Build dan deploy (ringkas, detail di `deployment.md`)

- Output statis (`astro build`), tidak memerlukan adapter server.
- Hosting statis dengan deploy otomatis dari branch `main` dan preview untuk branch/PR.
- Domain belum ada; sementara gunakan subdomain bawaan hosting. Variabel `site` di `astro.config.mjs` harus diperbarui saat domain final dipasang (memengaruhi sitemap dan canonical).

## 15. Hal yang sengaja tidak dipakai

Backend/API, database, adapter SSR, UI framework (React/Vue), CMS (di fase 1), mode gelap, pustaka animasi besar, dan pelacak iklan. Perubahan atas daftar ini dicatat di `decisions.md`.

## 16. Keputusan terbuka

1. **Bangun dari nol vs memakai starter Boutique.** Rekomendasi: mulai dari template `minimal` Astro, dan Boutique hanya sebagai referensi visual. Jika ada kode Boutique yang disalin, **periksa lisensinya** dulu.
2. Pustaka font final: fitur Fonts bawaan Astro atau `@fontsource` **[VERIFIKASI]**.
3. Cara integrasi Tailwind yang berlaku di versi terpasang **[VERIFIKASI]**.
4. Produk analytics (dan apakah perlu banner persetujuan).
5. Package manager (pnpm atau npm).
6. Hosting: Cloudflare Pages atau Netlify.
7. CMS untuk admin: ditinjau sebelum delegasi (lihat `prd.md` bagian 9).
