# Mini Toko Online — UTS Web Software Tools

Aplikasi web sederhana untuk menampilkan daftar produk, menambahkan produk ke keranjang, menghitung kuantitas dan total belanja, serta mengosongkan keranjang. Project ini digunakan untuk praktik **debugging, automated testing, pemeriksaan kualitas kode, dan version control** pada Ujian Tengah Semester Web Software Tools.

**Mahasiswa:** Suryana (12523014)  
**Kelas:** Teknik Informatika – B  
**Repository:** https://github.com/sry04/wst-uts-mini-toko-suryana.git  
**Branch:** `master`  
**Laporan pemeriksaan:** [`LAPORAN_UJIAN.md`](./LAPORAN_UJIAN.md)

## Teknologi

- HTML, CSS, dan JavaScript (ES Modules) untuk tampilan dan interaksi.
- Node.js untuk server HTTP lokal dan pengujian otomatis.
- Node.js built-in test runner untuk unit test.
- Biome untuk pemeriksaan lint dan format.
- Git untuk mencatat perubahan kode.

## Struktur project

```text
wst-uts-mini-toko/
├── public/
│   ├── index.html       # Halaman toko
│   ├── style.css        # Tampilan aplikasi
│   └── app.js           # Fetch produk dan interaksi keranjang
├── src/
│   └── cart.js          # addProduct() dan calculateTotal()
├── tests/
│   └── cart.test.mjs    # Tujuh unit test (tidak diubah)
├── server.mjs           # Server lokal dan endpoint API
├── package.json        # Script npm
├── biome.json          # Konfigurasi Biome
├── README.md            # Petunjuk project
└── LAPORAN_UJIAN.md     # Analisis bug dan laporan pengujian
```

## Cara menjalankan

**Prasyarat:** Node.js versi 20.19.0 atau lebih baru dan npm.

Buka terminal di folder project yang berisi `package.json`, lalu jalankan:

```bash
npm install
npm run dev
```

Akses aplikasi melalui **http://localhost:5057**.

Saat aplikasi dibuka, frontend meminta data dari endpoint `GET /api/products`. Server mengembalikan empat produk, lalu `public/app.js` menampilkannya pada halaman.

## Cara menggunakan aplikasi

1. Buka `http://localhost:5057` dan periksa apakah empat produk tampil.
2. Klik **+ Tambah ke Keranjang** pada produk yang dipilih.
3. Klik kembali produk yang sama untuk menambah kuantitasnya.
4. Periksa bahwa subtotal tiap produk dan total belanja sesuai `harga × quantity`.
5. Gunakan tombol **Kosongkan Keranjang** untuk menghapus daftar belanja.

Contoh perhitungan: dua Mouse Nirkabel (2 × Rp75.000) dan tiga Papan Ketik (3 × Rp150.000) menghasilkan total **Rp600.000**.

## Alur debugging dan perbaikan

Tiga masalah yang ditelusuri dalam project adalah:

| Masalah | Akar masalah | Solusi |
|---|---|---|
| Produk tidak tampil | Frontend meminta `/api/barang`, tetapi server memakai `/api/products` | Mengarahkan `fetch()` ke `/api/products` |
| Tombol keranjang tidak bereaksi | Class tombol `.add-button` berbeda dari selector listener `.add-to-cart` | Menggunakan `.add-button` pada listener |
| Total belanja keliru | `calculateTotal()` menjumlahkan harga satuan tanpa kuantitas | Menghitung `item.price * item.quantity` |

Urutan pemeriksaan yang digunakan adalah **amati gejala → baca kode dan hasil tes → identifikasi akar masalah → perbaiki → uji ulang**. Penjelasan bukti dan perubahan sebelum/sesudah tersedia di [`LAPORAN_UJIAN.md`](./LAPORAN_UJIAN.md).

## Pengujian otomatis

Jalankan:

```bash
npm run test:once
```

Hasil unit test yang tercatat:

| Tahap | Berhasil | Gagal |
|---|---:|---:|
| Sebelum perbaikan | 5 | 2 |
| Setelah perbaikan | 7 | 0 |

File `tests/cart.test.mjs` tidak diubah. Perlu diperhatikan bahwa tujuh tes tersebut menguji modul keranjang; untuk memastikan pemuatan produk dan interaksi tombol secara langsung, buka aplikasi di browser dan gunakan DevTools (Console, Network, Elements).

## Pemeriksaan kualitas kode

```bash
npm run lint
npm run format:check
npm run quality:check
```

Pemeriksaan Biome telah dicek secara manual di lingkungan lokal mahasiswa. Jika muncul warning/error pada pemeriksaan ulang, gunakan keluaran terminal sebagai dasar perbaikan, bukan mengubah folder pengujian.

## Git dan repository

Repository: **https://github.com/sry04/wst-uts-mini-toko-suryana.git**  
Branch kerja: **`master`**

Untuk melihat perubahan dan riwayat lokal:

```bash
git status
git branch --show-current
git log --oneline
```

Contoh urutan commit yang tercatat pada project lokal ketika laporan awal disusun:

```text
94cb2d6 docs: complete UTS examination report
b71dcc1 fix: load products and handle add-to-cart actions
f2d1875 chore: initial exam project
```

Setelah menyalin versi terbaru README dan laporan ke project, simpan revisi dokumentasi pada Git sesuai kebutuhan. URL repository di atas dicantumkan untuk referensi; tidak ada perubahan repository yang dilakukan melalui penyusunan README ini.

## Dokumentasi dan ketentuan ujian

- Perbaikan dibatasi pada tiga bug sesuai soal; tidak menambah fitur baru.
- Folder `tests/` dipertahankan agar hasil pengujian bisa diverifikasi.
- Penggunaan AI sebagai alat bantu dicatat secara terbuka di bagian 7 laporan.
- Bukti pemeriksaan, perubahan kode, hasil unit test, pemeriksaan kualitas, dan Git dijelaskan di [`LAPORAN_UJIAN.md`](./LAPORAN_UJIAN.md).

**Catatan:** Server pada `localhost:5057` hanya dapat diakses dari komputer yang menjalankannya. Alamat repository GitHub bukan otomatis alamat deployment aplikasi web.
