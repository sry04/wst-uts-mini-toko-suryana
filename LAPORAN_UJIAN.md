# LAPORAN UJIAN TENGAH SEMESTER
## WEB SOFTWARE TOOLS — MINI TOKO ONLINE

**Nama:** Suryana  
**NPM:** 12523014  
**Kelas:** Teknik Informatika – B  
**Tanggal:** 13 November 2026  
**Repository:** https://github.com/sry04/wst-uts-mini-toko-suryana.git  
**Branch:** `master`

## 1. Tujuan dan ruang lingkup

Pada UTS Web Software Tools ini, saya memeriksa aplikasi Mini Toko Online yang terdiri dari frontend HTML/CSS/JavaScript, server Node.js, dan modul keranjang belanja. Tujuannya bukan menambah fitur, melainkan menemukan penyebab tiga masalah yang sudah ditentukan: produk tidak tampil, tombol tambah keranjang tidak bekerja, dan total belanja salah ketika kuantitas barang lebih dari satu. Setelah menemukan penyebabnya, saya memperbaiki bagian kode yang terkait, menjalankan pengujian, memeriksa kualitas kode, serta mencatat perubahan menggunakan Git.

Saya mempertahankan file `tests/cart.test.mjs` tanpa perubahan agar hasil pengujian tetap mengacu pada ketentuan soal.

## 2. Pemeriksaan awal

Saya mulai dari struktur project dan membaca `package.json` untuk mengetahui perintah yang tersedia. Selanjutnya saya membandingkan `public/app.js` dengan route pada `server.mjs`, memeriksa elemen tombol yang dibentuk oleh JavaScript, dan membaca fungsi `calculateTotal()` di `src/cart.js`. Untuk memperoleh pembanding yang objektif, saya juga menjalankan pengujian awal:

```bash
npm run test:once
```

Hasil awal menunjukkan **7 pengujian, 5 berhasil, dan 2 gagal**. Dua kegagalan tersebut terkait perhitungan total saat jumlah barang lebih dari satu. Masalah pemuatan produk dan tombol keranjang saya telusuri dengan mencocokkan kode frontend, endpoint backend, dan selector elemen; keduanya tidak secara langsung diuji oleh tujuh unit test keranjang.

Alat pemeriksaan browser yang relevan adalah tab **Network** untuk mengecek permintaan API, **Console** untuk melihat error JavaScript, serta **Elements** untuk melihat class pada tombol. Temuan di bawah merujuk pada kode dan hasil pengujian yang dapat ditelusuri; saya tidak menambahkan klaim tentang screenshot DevTools yang tidak dilampirkan.

## 3. Analisis dan perbaikan tiga masalah

### 3.1 Daftar produk tidak tampil

**Gejala:** daftar produk tidak berhasil dimuat pada halaman.

**Bukti dan analisis:** di `public/app.js`, permintaan data awal diarahkan ke `/api/barang`, sedangkan `server.mjs` menyediakan endpoint `/api/products`. Karena alamat yang diminta berbeda dari route yang tersedia, aplikasi tidak memperoleh daftar produk melalui endpoint yang benar.

**Perbaikan:** saya menyamakan URL permintaan frontend dengan endpoint backend.

```js
// Sebelum
const response = await fetch("/api/barang");

// Sesudah
const response = await fetch("/api/products");
```

**Verifikasi:** pemeriksaan HTTP ke `GET /api/products` menghasilkan **200 OK** dengan empat data produk: Mouse Nirkabel, Papan Ketik, Lampu Meja, dan Dudukan Laptop. Artinya endpoint yang digunakan sekarang sesuai dengan layanan server. Ketika membuka aplikasi di browser, tab Network dapat digunakan untuk memastikan request yang sama menerima respons berhasil.

### 3.2 Tombol tambah ke keranjang tidak bereaksi

**Gejala:** produk memiliki tombol tambah ke keranjang, tetapi aksi klik tidak tersambung dengan benar.

**Bukti dan analisis:** tombol yang dibuat pada `renderProducts()` menggunakan class `add-button`, tetapi kode awal memasang event listener ke selector `.add-to-cart`. Karena kedua nama class tersebut tidak sama, event listener tidak menemukan tombol yang seharusnya diproses.

**Perbaikan:** saya menyesuaikan selector dengan class tombol yang memang dibuat oleh aplikasi.

```js
// Sebelum
document.querySelectorAll(".add-to-cart").forEach((button) => {

// Sesudah
document.querySelectorAll(".add-button").forEach((button) => {
```

**Verifikasi:** selector sekarang merujuk pada elemen tombol yang benar. Ketika tombol diklik, program mencari produk berdasarkan `data-id`, memanggil `addProduct(cart, product)`, lalu menjalankan `renderCart()` untuk memperbarui keranjang. Fungsi `addProduct()` juga telah teruji pada skenario menambahkan produk baru dan menambahkan produk yang sama. Pemeriksaan interaksi visual di browser tetap dapat dilakukan sebagai uji end-to-end tambahan.

### 3.3 Total belanja tidak sesuai jumlah barang

**Gejala:** total masih salah untuk barang yang kuantitasnya lebih dari satu.

**Bukti dan analisis:** fungsi `calculateTotal()` awalnya hanya menambahkan `item.price`, padahal satu baris keranjang memiliki `quantity`. Akibatnya, dua barang dengan harga satuan Rp75.000 dapat dihitung sebagai Rp75.000, bukan Rp150.000. Pada kode awal juga terdapat variabel `discount` yang tidak dipakai.

**Perbaikan:** saya mengalikan harga satuan dengan jumlah barang pada setiap baris, lalu menjumlahkan seluruh subtotal. Variabel yang tidak digunakan juga saya hapus.

```js
// Sebelum: harga tidak dikalikan kuantitas
return cart.reduce((total, item) => total + item.price, 0);

// Sesudah
return cart.reduce((total, item) => total + item.price * item.quantity, 0);
```

**Verifikasi:** dua Mouse Nirkabel menghasilkan Rp150.000; dua Mouse Nirkabel dan tiga Papan Ketik menghasilkan Rp600.000. Keduanya sesuai dengan unit test yang disediakan.

### 3.4 Ringkasan perbaikan

| Masalah | Penyebab utama | File yang diperbaiki | Hasil |
|---|---|---|---|
| Produk tidak tampil | URL API frontend dan backend tidak sama | `public/app.js` | Endpoint `/api/products` mengembalikan data |
| Tombol keranjang | Selector event listener berbeda dari class tombol | `public/app.js` | Event listener mengarah ke tombol yang benar |
| Total belanja | Harga tidak dikalikan kuantitas | `src/cart.js` | Seluruh tes perhitungan total lulus |

## 4. Pengujian otomatis

Pengujian dijalankan dengan perintah berikut:

```bash
npm run test:once
```

| Kondisi | Total tes | Berhasil | Gagal |
|---|---:|---:|---:|
| Sebelum perbaikan | 7 | 5 | 2 |
| Sesudah perbaikan | 7 | 7 | 0 |

Ringkasan output pengujian sesudah perbaikan:

```text
# tests 7
# pass 7
# fail 0
# cancelled 0
# skipped 0
```

Tujuh skenario mencakup keranjang kosong, satu barang, dua jenis barang, kuantitas lebih dari satu, dua jenis barang dengan kuantitas berbeda, menambahkan produk baru, dan menambah kuantitas produk yang sama. **Folder `tests/` tidak saya ubah.** Unit test ini membuktikan logika keranjang; keberhasilan tampilan dan klik di browser perlu dipastikan melalui pemeriksaan aplikasi yang berjalan.

## 5. Pemeriksaan kualitas kode dengan Biome

Untuk pemeriksaan kualitas kode, perintah yang digunakan adalah:

```bash
npm install
npm run lint
npm run format:check
npm run quality:check
```

`lint` digunakan untuk melihat potensi masalah pada penulisan kode, `format:check` untuk memeriksa konsistensi format, dan `quality:check` untuk pemeriksaan gabungan. Temuan yang relevan saat meninjau kode awal adalah variabel `discount` yang tidak digunakan di modul keranjang. Variabel tersebut dihapus bersamaan dengan pembetulan rumus total.

**Status pemeriksaan:** saya telah melakukan pengecekan Biome secara manual pada komputer lokal. Log output Biome tidak disertakan dalam dokumen ini, sehingga saya tidak menuliskan jumlah warning/error atau mengutip keluaran terminal yang tidak terdokumentasi. Perintah di atas dapat dijalankan ulang oleh dosen atau pemeriksa untuk mengonfirmasi kondisi terbaru pada branch `master`.

## 6. Dokumentasi Git

Penggunaan Git bertujuan agar kondisi awal dan perubahan setelah debugging dapat dibandingkan. Branch yang digunakan adalah `master`, dengan alamat repository:

**https://github.com/sry04/wst-uts-mini-toko-suryana.git**

Riwayat commit yang terdapat dalam project lokal saat dokumentasi awal dibuat:

```text
94cb2d6 docs: complete UTS examination report
b71dcc1 fix: load products and handle add-to-cart actions
f2d1875 chore: initial exam project
```

Urutan tersebut memperlihatkan baseline aplikasi, perbaikan fungsional, dan penyusunan laporan. Saya juga telah memeriksa repository GitHub secara manual dari komputer lokal. Tautan di atas dicantumkan sebagai identitas repository; laporan ini tidak mengklaim bahwa ada push baru yang dilakukan selama penyuntingan dokumen.

Untuk mengecek ulang riwayat di project lokal:

```bash
git status
git branch --show-current
git log --oneline
```

Apabila laporan dan README diperbarui lagi, perubahan dokumen tersebut dapat direkam dalam commit dokumentasi berikutnya.

## 7. Penggunaan AI sebagai alat bantu

Sesuai ketentuan ujian, saya mencatat penggunaan kecerdasan buatan secara terbuka:

- **Alat:** ChatGPT (OpenAI).
- **Instruksi yang diberikan:** membantu meninjau alur aplikasi Mini Toko Online, mengidentifikasi dan memperbaiki tiga bug sesuai soal UTS, menjaga folder `tests/`, memeriksa hasil pengujian, dan menyusun dokumentasi teknis.
- **Saran yang diterapkan:** penyamaan endpoint API, penyesuaian selector tombol, dan perbaikan rumus total harga dikalikan kuantitas.
- **Validasi saran:** membandingkan kode sebelum dan sesudah, mencocokkan endpoint dengan route server, menjalankan `npm run test:once`, serta melakukan pemeriksaan lokal terhadap kualitas kode dan Git.

AI berfungsi sebagai bantuan analisis dan dokumentasi. Kebenaran solusi tetap dinilai berdasarkan source code, hasil tes, dan pemeriksaan aplikasi, bukan hanya berdasarkan jawaban AI.

## 8. Kesimpulan

Dari pengerjaan ini, saya memahami bahwa debugging perlu dilakukan secara runtut. Pertama, gejala harus dicocokkan dengan bukti di kode atau hasil pengujian. Kedua, perubahan dibuat pada akar masalah tanpa menambah fitur yang tidak diminta. Ketiga, hasilnya diuji lagi agar terlihat perbedaan antara kondisi awal dan sesudah perbaikan.

Ketiga masalah utama telah ditangani pada bagian aplikasi yang relevan, dan hasil unit test meningkat dari **5/7 menjadi 7/7**. Catatan Git memudahkan penelusuran perubahan, sedangkan pemeriksaan Biome dan DevTools membantu mengecek aspek yang tidak seluruhnya tercakup dalam unit test.
