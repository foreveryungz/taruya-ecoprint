# Design: Arah Visual dan Design System

> Status: DRAFT v0.1 · Bergantung pada `prd.md` dan `content.md`.
> Nilai warna dan font di sini adalah **usulan awal**. Warna final harus diselaraskan dengan logo brand **[TBD]**.

---

## 1. Arah visual

**Kata kunci:** hangat · alami · buatan tangan · editorial · tenang

Karya ecoprint sudah kaya pola dan warna. Desain situs harus menjadi **bingkai yang tenang**, bukan bersaing dengan foto.

**Lakukan:**

- Ruang kosong yang lapang, foto besar, teks singkat
- Palet warna bumi yang diambil dari pewarna dan daun alami
- Tipografi berkarakter untuk judul, sangat terbaca untuk isi
- Foto dengan warna asli, tanpa filter berat

**Hindari:**

- Banyak warna cerah yang bersaing dengan foto produk
- Dekorasi daun/ornamen berlebihan (karya aslinya sudah jadi ornamen)
- Efek animasi yang mengganggu atau memperlambat
- Tampilan "toko online" (badge diskon, harga, keranjang), karena situs ini bukan e-commerce

### Referensi: template Boutique

**Diambil sebagai inspirasi:** tata letak editorial, galeri ala lookbook, navigasi sederhana, CTA kontak yang menonjol, rasa butik kecil yang personal.
**Diubah:**
| Template Boutique | Situs ini |
|---|---|
| Satu halaman dengan anchor | Multi-halaman dengan struktur di `content.md` |
| Bagian "Brands" | Diganti "Proses" dan "Kategori produk" |
| Tema gelap/pekat | Dasar terang hangat |
| Lokasi toko banyak | Satu lokasi rumahan, hanya nama daerah + Maps |
| Satu bahasa | ID dan EN dengan pengalih bahasa |

## 2. Warna

Usulan palet (diturunkan dari daun, tanah, dan pewarna alami). Sesuaikan dengan warna logo.

| Token                   | Hex       | Fungsi                                  |
| ----------------------- | --------- | --------------------------------------- |
| `--color-bg`            | `#F7F3EC` | Latar halaman (krem hangat)             |
| `--color-surface`       | `#FFFFFF` | Kartu, panel                            |
| `--color-ink`           | `#2B2A26` | Teks utama                              |
| `--color-muted`         | `#6B665A` | Teks sekunder                           |
| `--color-border`        | `#E4DDCF` | Garis tipis                             |
| `--color-primary`       | `#4F6B4A` | Hijau daun: tombol utama, tautan        |
| `--color-primary-ink`   | `#FFFFFF` | Teks di atas primary                    |
| `--color-accent`        | `#B5653A` | Terakota: aksen dekoratif               |
| `--color-accent-strong` | `#9A4F2A` | Terakota gelap: bila dipakai untuk teks |

**Aturan penggunaan:**

- Tombol WhatsApp memakai **primary**. Jangan memakai hijau WhatsApp bawaan, supaya tetap selaras dengan brand (ikon WA tetap dikenali).
- `accent` hanya untuk elemen dekoratif atau teks besar. Untuk teks kecil pakai `accent-strong`.
- Perkiraan kontras: `primary` di atas `bg` sekitar 5:1 dan `accent` di atas `bg` sekitar 3,9:1, sehingga `accent` tidak lolos untuk teks kecil. **Verifikasi semua pasangan warna dengan contrast checker** sebelum dikunci.
- Mode gelap: tidak di fase 1 (dicatat di `decisions.md`).

```css
:root {
  --color-bg: #f7f3ec;
  --color-surface: #ffffff;
  --color-ink: #2b2a26;
  --color-muted: #6b665a;
  --color-border: #e4ddcf;
  --color-primary: #4f6b4a;
  --color-primary-ink: #ffffff;
  --color-accent: #b5653a;
  --color-accent-strong: #9a4f2a;
}
```

## 3. Tipografi

| Peran           | Usulan                                                           | Catatan                                |
| --------------- | ---------------------------------------------------------------- | -------------------------------------- |
| Judul (display) | Serif berkarakter, mis. **Fraunces** atau **Cormorant Garamond** | Memberi kesan hangat dan buatan tangan |
| Isi dan UI      | Sans-serif bersih, mis. **Inter** atau **DM Sans**               | Terbaca di layar kecil                 |

- Font di-host sendiri (mis. paket `@fontsource`) demi performa dan privasi, bukan lewat CDN pihak ketiga.
- Muat maksimal 2 keluarga font dan 2-3 bobot. Gunakan `font-display: swap` dan fallback system font.
- Skala (fluid dengan `clamp`):

| Level   | Ukuran (mobile → desktop) | Pakai untuk                |
| ------- | ------------------------- | -------------------------- |
| Display | 2,25rem → 4rem            | Judul hero                 |
| H1      | 2rem → 3rem               | Judul halaman              |
| H2      | 1,5rem → 2,25rem          | Judul section              |
| H3      | 1,25rem → 1,5rem          | Judul kartu                |
| Body    | 1rem (16px)               | Teks isi, tinggi baris 1,6 |
| Small   | 0,875rem                  | Keterangan, label          |

## 4. Layout, spasi, bentuk

- **Grid:** kontainer maksimal ±1200px, padding sisi 16px (mobile) sampai 32px (desktop). Galeri 2 kolom (mobile), 3-4 kolom (desktop).
- **Breakpoint:** mengikuti default Tailwind (`sm 640`, `md 768`, `lg 1024`, `xl 1280`). Desain **mobile-first**.
- **Spasi:** skala kelipatan 4px; jarak antar-section besar (64-96px) supaya terasa lapang.
- **Sudut:** radius kecil-sedang (`6-12px`), konsisten. Foto boleh persegi atau radius kecil.
- **Bayangan:** minimal; lebih baik garis `border` tipis.
- **Tap target:** minimal 44×44px untuk semua tombol dan tautan penting.

## 5. Gambar

| Konteks          | Rasio                                     | Catatan                                                 |
| ---------------- | ----------------------------------------- | ------------------------------------------------------- |
| Hero             | 16:9 desktop, 4:5 mobile (crop terarah)   | Satu foto terbaik, teks dengan kontras cukup di atasnya |
| Kartu produk     | 4:5                                       | Seragam untuk seluruh katalog                           |
| Portofolio       | Campuran, grid masonry sederhana atau 1:1 | Hindari tampilan berantakan                             |
| Kegiatan         | 3:2                                       |                                                         |
| Detail pola daun | 1:1                                       | Cocok untuk close-up                                    |

- Gunakan `astro:assets` untuk ukuran responsif dan format modern. `loading="lazy"` kecuali hero.
- Teks alt wajib (ID dan EN). Gambar dekoratif murni diberi `alt=""`.

## 6. Komponen UI

Daftar komponen yang dibutuhkan. Spesifikasi props dan struktur file ada di `tech-spec.md`.

| Komponen           | Fungsi                                 | Catatan                                 |
| ------------------ | -------------------------------------- | --------------------------------------- |
| `Header`           | Logo, navigasi, pengalih bahasa        | Sticky, menu ringkas di mobile          |
| `LanguageSwitcher` | Pindah ID ↔ EN di halaman yang sama    | Tautan biasa, bukan JS                  |
| `Footer`           | Kontak singkat, tautan, hak cipta      |                                         |
| `Button`           | Varian: primary, secondary, text       | Satu gaya, fokus jelas                  |
| `WhatsAppButton`   | CTA ke WA dengan pesan terisi          | Menerima `message` dan `lang`           |
| `Hero`             | Judul, tagline, CTA, foto              |                                         |
| `SectionHeading`   | Judul section + deskripsi singkat      |                                         |
| `CategoryCard`     | Pintu ke kategori produk               |                                         |
| `ProductCard`      | Foto, nama, ketersediaan, CTA WA       | Tanpa harga                             |
| `PortfolioGrid`    | Galeri dengan filter kategori          | Filter sederhana                        |
| `Lightbox`         | Perbesar foto                          | Dapat diakses keyboard, Esc untuk tutup |
| `ProcessSteps`     | 3-4 langkah proses dengan foto         |                                         |
| `ServiceCard`      | Ringkasan satu layanan                 |                                         |
| `ActivityCard`     | Kegiatan: foto, judul, tanggal, tempat |                                         |
| `ContactBlock`     | WA, IG, tautan Maps                    |                                         |
| `SEOHead`          | Meta, Open Graph, `hreflang`, schema   |                                         |

Kemungkinan tambahan: tombol WA mengambang di mobile (lihat bagian 8).

## 7. Interaksi dan motion

- Animasi halus dan fungsional: transisi hover/focus, fade-in lembut saat scroll (opsional).
- Hormati `prefers-reduced-motion`. Tanpa animasi besar yang menunda konten.
- Tidak ada carousel otomatis.

## 8. Aksesibilitas visual

- Kontras teks minimal WCAG AA (4,5:1 teks biasa, 3:1 teks besar)
- Indikator fokus terlihat jelas (outline 2px dengan offset), jangan dihapus
- Informasi tidak hanya lewat warna (mis. status "Pre-Order" juga berupa teks)
- Urutan heading logis (satu `h1` per halaman)
- Semua kontrol bisa dioperasikan dengan keyboard

## 9. Keputusan terbuka

1. **Warna final:** cocokkan dengan logo. Kirim warna logo (hex), lalu palet di atas disesuaikan **[TBD]**.
2. **Pilihan font final** setelah dicoba dengan teks dan foto sebenarnya **[TBD]**.
3. **Tombol WA mengambang di mobile:** meningkatkan konversi, tapi menutupi konten. Usulan: aktifkan dan uji **[TBD]**.
4. **Portofolio:** grid seragam atau masonry **[TBD]**.
