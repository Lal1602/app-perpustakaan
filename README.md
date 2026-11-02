# Aplikasi Perpustakaan (app-perpustakaan)

## Tujuan Aplikasi
Aplikasi ini dibuat sebagai sistem manajemen perpustakaan sederhana untuk mempelajari dasar-dasar framework Laravel. Tujuannya adalah mempermudah pengelolaan data yang berkaitan dengan perpustakaan, seperti pendataan buku, anggota, dan transaksi peminjaman.

## Cara Menjalankan Project Secara Lokal

Ikuti langkah-langkah berikut untuk menjalankan aplikasi ini di komputer Anda:

1. Clone repository ini (`git clone https://github.com/Lal1602/app-perpustakaan.git`) atau pastikan Anda berada di direktori project `app-perpustakaan`.
2. Buka terminal dan jalankan `composer install` untuk menginstal dependensi PHP.
3. Salin file `.env.example` menjadi `.env` (misalnya melalui perintah `cp .env.example .env`).
4. Generate application key dengan menjalankan `php artisan key:generate`.
5. Sesuaikan konfigurasi database pada file `.env` jika diperlukan. Pastikan variabel `INTERNAL_API_URL` ada di `.env` (nilai bawaan `http://127.0.0.1:8011`), karena Dashboard dan Laporan Peminjaman memanggil API milik aplikasi ini sendiri lewat alamat tersebut.
6. Jalankan `php artisan migrate:fresh --seed` untuk membuat seluruh tabel sekaligus mengisi data dummy (user, kategori, buku, anggota, dan peminjaman).
7. Jalankan **dua** server pengembangan di dua terminal berbeda, keduanya harus menyala bersamaan:
   - Terminal 1 (untuk browser): `php artisan serve --port=8000`
   - Terminal 2 (khusus panggilan API internal): `php artisan serve --port=8011`

   `php artisan serve` hanya memproses satu request per waktu. Jika Dashboard memanggil `/api/stats` ke port yang sama dengan server yang sedang memprosesnya, kedua request akan saling menunggu (deadlock). Server kedua di port `8011` mencegah hal itu. Jika server kedua tidak menyala, Dashboard dan Laporan Peminjaman tetap tampil, tetapi statistik bernilai 0 dan tabel laporan kosong.
8. Buka browser dan akses aplikasi melalui `http://127.0.0.1:8000`, lalu login dengan akun hasil seeder, misalnya `admin@pens.ac.id` dengan password `password` (admin) atau `petugas1@pens.ac.id` dengan password `password` (petugas).

---

## Pemahaman MVC (Model, View, Controller)
Menurut pemahaman saya, **Model** adalah bagian yang menangani logika data dan berinteraksi langsung dengan database (seperti menyimpan atau mengambil data). **View** adalah bagian antarmuka (UI) yang bertanggung jawab menampilkan data tersebut kepada pengguna dalam bentuk halaman web. Sementara itu, **Controller** berfungsi sebagai jembatan yang menerima permintaan (request) dari pengguna, mengolahnya melalui Model, lalu menentukan View mana yang akan ditampilkan sebagai respon.
