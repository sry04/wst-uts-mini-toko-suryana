# LAPORAN UJIAN TENGAH SEMESTER — WEB SOFTWARE TOOLS

**Nama:** Suryana  
**Nomor Induk Mahasiswa (NPM):** 12523014  
**Kelas:** Teknik Informatika – B  
**Tanggal:** 13 November 2026  
**Studi kasus:** Periksa, Perbaiki, Uji, dan Rapikan Mini Toko Online

## A. Bukti pemeriksaan masalah

Saya menggunakan project yang diberikan pada ujian. Pemeriksaan dilakukan dengan membaca file aplikasi, membandingkan permintaan frontend dengan route server, menjalankan unit test melalui Node.js, serta memeriksa respons server lokal. Pemeriksaan DevTools browser (Console, Network, Elements) tetap perlu diverifikasi secara langsung saat project dibuka di komputer mahasiswa; saya tidak mencantumkan screenshot DevTools yang belum dibuat.

| Masalah | Alat yang digunakan | Bukti pemeriksaan | Perkiraan penyebab | Perbaikan | Hasil setelah perbaikan |
|---|---|---|---|---|---|
| Daftar produk | Pemeriksaan source `public/app.js` dan `server.mjs`, pemeriksaan HTTP lokal | Pada kode awal, frontend meminta `/api/barang`, sementara server hanya melayani `/api/products`. Route API yang tidak dikenal mengembalikan 404. | Alamat API frontend tidak sesuai dengan endpoint server. | Mengganti `fetch('/api/barang')` menjadi `fetch('/api/products')`. | Pengujian HTTP `GET /api/products` menghasilkan status 200 dan data berisi empat produk. |
| Tombol keranjang | Pemeriksaan `public/app.js` pada HTML tombol dan selector | Tombol di-render dengan class `add-button`, tetapi kode awal mencari `.add-to-cart`. | Event listener tidak dipasangkan pada tombol yang tampil. | Mengubah selector JavaScript menjadi `.add-button`. | Selector sekarang sesuai dengan class tombol pada HTML; alur klik menambah produk memakai `addProduct` dan memperbarui tampilan keranjang. Perlu verifikasi klik langsung di browser mahasiswa. |
| Total belanja | Pemeriksaan `src/cart.js` dan tes `tests/cart.test.mjs` | Kode awal menjumlahkan `item.price` saja tanpa mengalikan `item.quantity`, sehingga tes untuk jumlah barang lebih dari satu gagal. | Rumus total belum memperhitungkan kuantitas. | Mengganti perhitungan menjadi `item.price * item.quantity` dan menghapus variabel tak terpakai. | Semua unit test yang tersedia berhasil, termasuk total dua mouse Rp150.000 dan dua mouse + tiga keyboard Rp600.000. |

## B. Hasil pengujian

Perintah pengujian: `npm run test:once` (memanggil `node --test tests/*.test.mjs`).

- **Sebelum perbaikan:** 7 pengujian; **5 berhasil, 2 gagal**.
- **Setelah perbaikan:** 7 pengujian; **7 berhasil, 0 gagal**.
- Berkas di folder `tests/` **tidak diubah**.
- Pemeriksaan server lokal juga menunjukkan status HTTP 200 untuk `/`, `/api/products`, `/app.js`, dan `/src/cart.js`.
- Keempat data produk tersedia pada API: Mouse Nirkabel, Papan Ketik, Lampu Meja, dan Dudukan Laptop.

Cuplikan ringkasan hasil uji setelah perbaikan:

```text
# tests 7
# pass 7
# fail 0
# cancelled 0
# skipped 0
```

## C. Kualitas kode

- **Temuan sebelum perbaikan:** variabel `discount` pada fungsi `calculateTotal` tidak digunakan, serta terjadi kesalahan logika perhitungan total.
- **Perbaikan yang dilakukan:** menghapus variabel tersebut dan memperbaiki rumus total. Perubahan pada frontend terbatas pada path API dan selector tombol, sesuai ruang lingkup soal.
- **Pemeriksaan sintaks/perilaku:** pengujian otomatis Node.js berhasil 7/7 dan server lokal dapat memberikan respons HTTP 200.
- **Pemeriksaan Biome:** `npm install` telah dicoba, namun pemasangan dependency tidak selesai dalam sesi pemeriksaan ini. Oleh karena itu hasil `npm run lint`, `npm run format:check`, dan `npm run quality:check` **belum dapat dinyatakan lulus**. Ketiga perintah tersebut harus dijalankan kembali setelah `npm install` berhasil di komputer mahasiswa; jika muncul temuan, perbaiki seperlunya dan catat hasil aktual.

## D. Git

- **Branch:** `master` (sesuai repository lokal dalam paket).
- **Commit awal:** `chore: initial exam project` — kondisi project sebelum perbaikan.
- **Commit perbaikan fungsi:** `fix: load products and handle add-to-cart actions` — menyesuaikan endpoint, selector, dan perhitungan total.
- **Commit dokumentasi:** `docs: complete UTS examination report` — melengkapi laporan sesuai hasil pemeriksaan.
- **Status akhir:** diperiksa menggunakan `git status` setelah commit dokumentasi.
- Riwayat lengkap dapat dibuktikan dengan menjalankan `git log --oneline` dari folder project, termasuk folder `.git` pada ZIP.

## E. Pemakaian kecerdasan buatan (AI)

- **Nama alat:** ChatGPT (OpenAI).
- **Pertanyaan/instruksi yang diberikan:** membantu menyelesaikan UTS Web Software Tools dengan memperbaiki project ZIP berdasarkan PDF instruksi, tanpa mengubah folder pengujian, melakukan pengujian, dan membuat laporan serta paket pengumpulan.
- **Bagian saran yang diterapkan:** penyelarasan path API, perbaikan selector tombol, dan rumus total harga dikalikan jumlah. AI juga membantu menyusun dokumentasi langkah pemeriksaan.
- **Cara memeriksa kebenaran saran:** membandingkan perubahan kode dengan kondisi awal, menjalankan tujuh pengujian otomatis tanpa mengubah tes, dan memeriksa respons server lokal. Pengujian tampilan/interaksi di browser dan pemeriksaan Biome masih harus dikonfirmasi langsung oleh mahasiswa.

## F. Refleksi singkat

Pelajaran yang saya dapat adalah saat menemukan bug, kita sebaiknya tidak langsung menebak letaknya. Saya perlu membandingkan kode frontend dan backend, membaca hasil pengujian, lalu memperbaiki bagian yang menjadi penyebab. Setelah perubahan, pengujian ulang penting supaya hasil perbaikan dapat dibuktikan. Git juga membantu melihat perbedaan antara kode awal dan kode setelah diperbaiki.

## G. Pemeriksaan sebelum pengumpulan

- [x] Source code dan konfigurasi project tersedia.
- [x] Endpoint produk dapat merespons HTTP 200.
- [x] Selector tombol sudah diperbaiki.
- [x] Rumus total sudah memperhitungkan kuantitas.
- [x] Ketujuh pengujian otomatis berhasil.
- [x] Folder `tests/` tidak diubah.
- [x] Commit awal dan commit perbaikan tersedia.
- [x] Laporan pemeriksaan terisi.
- [ ] Uji klik tombol dan tampilan langsung pada browser komputer mahasiswa.
- [ ] Jalankan Biome lint, format check, dan quality check setelah dependency berhasil terpasang.
- [ ] Lakukan deployment ke layanan publik dan cantumkan URL apabila diminta pada kolom CBT.
- [ ] Pastikan tautan berbagi ZIP dan/atau GitHub dapat diakses pemeriksa.
