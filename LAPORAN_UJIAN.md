# Laporan UTS Web Software Tools

**Nama:** Ahmad Hardiansyah
**Nomor Induk Mahasiswa:** 12523016
**Kelas:** A
**Tanggal:** 11 November 2026

## A. Bukti pemeriksaan masalah
| Masalah | Alat yang digunakan | Bukti pemeriksaan | Perkiraan penyebab | Perbaikan | Hasil setelah perbaikan |
|---|---|---|---|---|---|
| Produk | Browser DevTools (Console & Network) | Error HTTP 404 pada console saat memanggil `:5057/api/barang` | Fungsi `loadProducts` di `public/app.js` memanggil endpoint `/api/barang`, padahal `server.mjs` menyediakan `/api/products` | Mengubah URL fetch pada `public/app.js` menjadi `fetch("/api/products")` | Seluruh daftar produk (4 item) berhasil dimuat dan ditampilkan pada antarmuka web |
| Tombol keranjang | Browser DevTools (Elements & Sources) | Tombol "+ Tambah ke Keranjang" tidak merespons klik dan barang tidak masuk ke keranjang | Elemen tombol di HTML memiliki class `add-button`, namun script di `public/app.js` mencari class `.add-to-cart` sehingga event listener klik tidak terpasang | Menyesuaikan selector di `public/app.js` menjadi `document.querySelectorAll(".add-button")` | Tombol berfungsi normal dan produk berhasil masuk ke daftar keranjang belanja saat diklik |
| Total belanja | Node.js Test Runner (`npm run test:once`) & Browser | Dua unit test gagal (`satu jenis barang dengan jumlah dua` dan `dua jenis barang dengan jumlah berbeda`). Di browser, total belanja tidak bertambah jika jumlah barang > 1 | Fungsi `calculateTotal()` di `src/cart.js` hanya menjumlahkan harga barang (`item.price`) tanpa mengalikan kuantitas (`item.quantity`), serta terdapat variabel `var discount` yang tidak digunakan | Mengubah rumus akumulasi di `src/cart.js` menjadi `total + item.price * item.quantity` dan menghapus variabel `var discount` yang tidak terpakai | Seluruh 7 pengujian otomatis lulus (100% pass) dan perhitungan total belanja di browser akurat sesuai jumlah kuantitas |

## B. Hasil pengujian
- Jumlah pengujian sebelum perbaikan: 7 pengujian (5 berhasil, 2 gagal pada uji jumlah kuantitas barang)
- Jumlah pengujian berhasil setelah perbaikan: 7 pengujian (7 berhasil / 100% pass, 0 gagal)
- Catatan perubahan pada fungsi yang diperiksa: Pada fungsi `calculateTotal(cart)` di berkas `src/cart.js`, logika akumulasi `reduce` diperbaiki dari `total + item.price` menjadi `total + item.price * item.quantity` agar memperhitungkan jumlah unit barang yang dibeli.

## C. Kualitas kode
- Temuan sebelum perbaikan: 
  1. Terdapat unused variable `var discount = 0;` di `src/cart.js` yang terdeteksi oleh linter Biome (`noUnusedVariables`).
  2. Format indentasi, pemecahan baris template string, dan arrow function pada `public/app.js` belum seragam dengan konfigurasi `biome.json`.
- Perbaikan yang dilakukan: 
  1. Menghapus variabel `var discount = 0;` dari `src/cart.js`.
  2. Menjalankan formatting otomatis Biome (`biome format --write`) pada `public/app.js` dan `src/cart.js`.
- Hasil pemeriksaan ulang: 
  Pemeriksaan `npm run lint`, `npm run format:check`, dan `npm run quality:check` berhasil lulus secara keseluruhan tanpa error maupun warning (`Checked 2 files. No fixes applied.`).

## D. Git
- Branch (jika membuat): master
- Ringkasan commit:
  - `f502158 chore: initial exam` : Commit awal merekam kondisi awal project sebelum dilakukan perbaikan.
  - `bf7b12e fix: repair shop behavior` : Commit perbaikan yang menyelesaikan endpoint produk, event listener tombol keranjang, logika perhitungan total keranjang, dan pembersihan format kode.
- Hasil pemeriksaan `git status`:
  On branch master, nothing to commit, working tree clean.

## E. Pemakaian kecerdasan buatan (jika digunakan)
- Nama alat: Google Antigravity / Gemini
- Pertanyaan/instruksi yang diberikan: Meminta langkah-langkah panduan pengerjaan UTS, cara menelusuri error 404 pada network fetch, analisis tombol keranjang yang tidak responsif, perbaikan formula reduce pada keranjang belanja, serta menjalankan pengujian Biome dan Node.js test runner.
- Bagian jawaban yang diterapkan:
  1. Penggantian rute API dari `/api/barang` ke `/api/products` di `public/app.js`.
  2. Penyesuaian class selector event listener tombol ke `.add-button` di `public/app.js`.
  3. Perbaikan rumus `calculateTotal` di `src/cart.js` menjadi `item.price * item.quantity` dan penghapusan variabel `var discount`.
  4. Perintah pengujian otomatis (`npm run test:once`) dan pemeriksaan kualitas Biome (`npm run quality:check`).
- Cara memeriksa bahwa saran tersebut benar:
  1. Menguji interaksi langsung pada browser: memverifikasi daftar produk muncul dan barang berhasil ditambahkan saat tombol diklik.
  2. Menjalankan `npm run test:once` dan memverifikasi seluruh 7 pengujian lulus.
  3. Menjalankan `npm run quality:check` untuk memastikan kode memenuhi standar Biome.

## F. Refleksi singkat
- Satu pelajaran paling penting dari ujian ini:
  Pentingnya melakukan debugging berbasis bukti nyata (menggunakan browser DevTools dan automated tests) untuk melacak akar masalah secara presisi alih-alih berasumsi atau menebak, serta menjaga kebersihan kode dan riwayat perubahan yang rapi menggunakan Git.
