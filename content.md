# Content: Strategi, Sitemap, dan Model Konten

> Status: DRAFT v0.1 · Bergantung pada `prd.md`.
> Item bertanda **[TBD]** menunggu input dari pemilik usaha.

---

## 1. Prinsip konten

1. **Visual dulu, teks singkat.** Karya ecoprint dinilai dari mata. Teks mendukung foto, bukan sebaliknya.
2. **Satu tujuan per halaman, satu CTA utama:** ke WhatsApp.
3. **Cerita proses adalah pembeda.** Daun alam, teknik, dan keunikan pola tiap karya (tidak ada dua yang sama) adalah nilai jual.
4. **Harga tidak ditampilkan.** Pakai kalimat standar yang sama di semua tempat (lihat bagian 8).
5. **Dua bahasa setara.** Setiap halaman punya padanan ID dan EN.

## 2. Sitemap

```
/                      Beranda
/tentang               Tentang & Proses
/portofolio            Galeri karya (filter kategori sederhana)
/katalog               Katalog produk (per kategori)
/katalog/[slug]        Detail produk (opsional, lihat catatan)
/layanan               Shop · Custom Order · Pre-Order · Pelatihan
/kegiatan              Daftar kegiatan / event
/kegiatan/[slug]       Detail kegiatan (opsional)
/kontak                WhatsApp · Instagram · Maps
```

Versi Inggris memakai prefiks `/en/` dengan struktur yang sama (`/en/about`, `/en/portfolio`, dst.). Slug final dan detail routing ditentukan di `tech-spec.md`.

**Catatan keputusan:**

- **Halaman detail produk dan kegiatan bersifat opsional di fase 1.** Katalog bisa berupa grid dengan lightbox dan CTA WA di tiap kartu. Halaman detail baru perlu kalau tiap produk punya cerita atau foto banyak. Rekomendasi: mulai tanpa halaman detail, tambah kalau perlu.
- **Portofolio dan Katalog dipisah** karena fungsinya beda: portofolio = bukti kualitas dan karya yang pernah dibuat (bisa sudah terjual), katalog = apa yang bisa dipesan sekarang. Kalau isinya nanti ternyata sama, gabungkan dan catat di `decisions.md`.

## 3. Isi tiap halaman

### Beranda

1. Hero: foto terbaik, nama brand, tagline [TBD], CTA "Chat WhatsApp"
2. Pengantar singkat (2-3 kalimat)
3. Sorotan kategori produk (baju, kain, sepatu, gelas/tumbler, aksesoris lain)
4. Proses singkat dalam 3-4 langkah visual
5. Portofolio pilihan (4-6 foto)
6. Ringkasan layanan (4 kartu)
7. Kegiatan terbaru (2-3 item)
8. CTA penutup + kontak

### Tentang & Proses

- Cerita brand [TBD, menunggu dari pemilik]
- Penjelasan ecoprint: apa itu, daun apa saja, kenapa tiap karya unik
- Langkah proses dengan foto
- Nilai brand (ramah lingkungan, buatan tangan), hanya yang benar-benar bisa dipertanggungjawabkan
- Perawatan produk ecoprint (cuci, jemur), berguna dan menjaga kepuasan pembeli

### Portofolio

- Grid foto dengan filter kategori (Pakaian, Kain, Sepatu, Gelas & Tumbler, Lainnya)
- Setiap item: foto, judul singkat, kategori, keterangan bahan/daun (opsional)

### Katalog

- Per kategori, tiap kartu: foto, nama, deskripsi singkat, ketersediaan (Ready / Pre-Order / Custom)
- **Tanpa harga.** CTA "Tanya via WA" dengan pesan terisi nama produk

### Layanan

Empat bagian dengan struktur sama: deskripsi, cocok untuk siapa, cara kerja (3-4 langkah), CTA WA.

- Shop · Custom Order · Pre-Order · Jasa Pelatihan
- Custom Order: sebutkan info yang perlu disiapkan pelanggan (jenis produk, bahan, jumlah, tenggat)
- Pelatihan: format, durasi, peserta [TBD, menunggu detail dari pemilik]

### Kegiatan

- Daftar kronologis (terbaru di atas): foto, judul, tanggal, lokasi, deskripsi singkat
- Cukup 3-5 kegiatan di awal; jangan menunggu lengkap untuk launching

### Kontak

- Tombol WhatsApp, Instagram, tautan Google Maps (embed opsional)
- Lokasi: **nama daerah + tautan Google Maps saja**, tanpa alamat lengkap
- Keterangan jam respons [TBD]

## 4. Model konten (Astro Content Collections)

Konten disimpan terstruktur agar admin bisa menambah item tanpa menyentuh komponen. Skema final ditulis di `tech-spec.md`; di sini hanya field yang dibutuhkan.

### `products` (katalog)

| Field          | Tipe                    | Catatan                                       |
| -------------- | ----------------------- | --------------------------------------------- |
| `slug`         | string                  | unik                                          |
| `category`     | enum                    | pakaian, kain, sepatu, gelas-tumbler, lainnya |
| `title`        | `{ id, en }`            |                                               |
| `description`  | `{ id, en }`            | 1-2 kalimat                                   |
| `availability` | enum                    | ready, pre-order, custom                      |
| `images`       | array gambar            | foto utama di indeks 0                        |
| `materials`    | `{ id, en }` (opsional) | bahan dan daun                                |
| `featured`     | boolean                 | tampil di beranda                             |

### `portfolio`

| Field      | Tipe                    | Catatan            |
| ---------- | ----------------------- | ------------------ |
| `slug`     | string                  |                    |
| `category` | enum                    | sama dengan produk |
| `title`    | `{ id, en }`            |                    |
| `image`    | gambar                  |                    |
| `note`     | `{ id, en }` (opsional) | cerita singkat     |
| `featured` | boolean                 |                    |

### `activities` (kegiatan)

| Field      | Tipe         | Catatan            |
| ---------- | ------------ | ------------------ |
| `slug`     | string       |                    |
| `title`    | `{ id, en }` |                    |
| `date`     | tanggal      |                    |
| `location` | string       | nama tempat / kota |
| `summary`  | `{ id, en }` |                    |
| `images`   | array gambar |                    |

### `services`

Empat entri tetap (shop, custom-order, pre-order, training), masing-masing dengan `title`, `summary`, `steps[]`, `ctaMessage` dalam dua bahasa.

## 5. Inventori aset

Foto sudah tersedia, tapi kelengkapannya belum dicek. Gunakan daftar ini untuk audit:

| Kebutuhan                                                       | Jumlah minimal   | Status   |
| --------------------------------------------------------------- | ---------------- | -------- |
| Foto hero (landscape, resolusi tinggi)                          | 2-3              | [TBD]    |
| Foto per kategori produk (pakaian, kain, sepatu, gelas/tumbler) | 6-8 per kategori | [TBD]    |
| Close-up pola daun / detail tekstur                             | 6-10             | [TBD]    |
| Foto proses (memilih daun, menata, mengukus/ fiksasi)           | 5-8              | [TBD]    |
| Foto kegiatan / event                                           | 3-5 per kegiatan | [TBD]    |
| Logo (SVG) dan variasi terang/gelap                             | 1 set            | Tersedia |
| Foto tempat usaha / pembuat                                     | 2-3              | [TBD]    |

**Panduan foto:**

- Latar bersih, cahaya alami, warna konsisten
- Simpan master resolusi asli; situs menghasilkan ukuran lain otomatis
- Nama file deskriptif: `batik-daun-jati-kemeja-01.jpg`
- Teks alt dibuat untuk tiap foto (ID dan EN)

## 6. Nada dan gaya bahasa

- **Hangat, jujur, sederhana.** Hindari klaim berlebihan ("terbaik", "100% alami") kecuali bisa dibuktikan.
- **Indonesia:** sapaan netral-sopan (kami/Anda atau kami/kamu, **[TBD: pilih satu dan konsisten]**).
- **Inggris:** kalimat pendek dan jelas untuk pembaca non-native. Gunakan "ecoprint" dan "natural leaf printing" sebagai penjelas.
- Istilah tetap: _ecoprint_, _batik daun_ (EN: _leaf-printed textile_), _custom order_, _pre-order_.
- Setiap teks ID ditulis dulu, lalu EN ditulis atau ditinjau manusia.

## 7. Pesan WhatsApp terisi otomatis

Format: `https://wa.me/<nomor>?text=<pesan ter-encode>`

| Konteks   | Pesan ID                                             | Pesan EN                                               |
| --------- | ---------------------------------------------------- | ------------------------------------------------------ |
| Umum      | "Halo, saya ingin bertanya tentang produk ecoprint." | "Hello, I'd like to ask about your ecoprint products." |
| Produk    | "Halo, saya tertarik dengan {nama produk}."          | "Hello, I'm interested in {product name}."             |
| Custom    | "Halo, saya ingin diskusi custom order."             | "Hello, I'd like to discuss a custom order."           |
| Pre-order | "Halo, saya ingin ikut pre-order."                   | "Hello, I'd like to join the pre-order."               |
| Pelatihan | "Halo, saya ingin tanya jadwal pelatihan ecoprint."  | "Hello, I'd like to ask about ecoprint workshops."     |

## 8. Kalimat standar harga

- **ID:** "Harga menyesuaikan bahan, produk, dan jumlah. Hubungi kami via WhatsApp untuk penawaran."
- **EN:** "Prices depend on materials, product, and quantity. Message us on WhatsApp for a quote."

## 9. Daftar konten yang dibutuhkan dari pemilik

- [ ] Tagline dan cerita brand (dikonfirmasi)
- [ ] Penjelasan proses ecoprint dengan bahasa mereka sendiri
- [ ] Detail jasa pelatihan: format, durasi, jumlah peserta
- [ ] Info alur pre-order dan custom order
- [ ] Daftar 3-5 kegiatan terakhir (judul, tanggal, tempat, foto)
- [ ] Jam respons WhatsApp
- [ ] Pilihan gaya sapaan (kami/Anda atau kami/kamu)

## 10. Di luar scope konten (fase 1)

Blog, testimoni berbasis formulir, harga, video panjang, dan halaman detail untuk tiap produk. Dicatat agar tidak masuk diam-diam.
