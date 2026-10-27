# Taru Ecoprint

Website profil statis dua bahasa (Indonesia dan Inggris) untuk usaha ecoprint: portofolio brand dan pintu kontak WhatsApp. Bukan e-commerce.

T0.1 menyiapkan Astro minimal dan TypeScript strict. Halaman bawaan masih berupa scaffold; konten, i18n, Tailwind, dan styling dikerjakan pada tugas berikutnya.

## Menjalankan

Gunakan Node **24.22.1 LTS** (lihat `.nvmrc`) dan npm. Dengan fnm di PowerShell:

```powershell
fnm env --shell powershell | Out-String | Invoke-Expression
fnm install
fnm use
npm ci
npm run dev
```

Dev server: http://localhost:4321. Hentikan dengan `q` lalu Enter.

```sh
npm run check    # pemeriksaan tipe Astro/TypeScript
npm run build    # build statis ke dist/
npm run preview  # pratinjau hasil build
```

## Dokumentasi

Baca [aturan agent](agents.md), [spesifikasi teknis](project-docs/tech-spec.md), [kriteria kualitas](project-docs/quality.md), dan [catatan keputusan](project-docs/decisions.md). Dokumen lengkap ada di [project-docs](project-docs/).

Setup mengikuti [Getting Started Astro](https://docs.astro.build/en/install-and-setup/) dan [panduan TypeScript](https://docs.astro.build/en/guides/typescript/).
