# Aplikasi Perpustakaan (app-perpustakaan)

## Tujuan Aplikasi
Aplikasi ini dibuat sebagai sistem manajemen perpustakaan sederhana untuk mempelajari dasar-dasar framework Laravel. Tujuannya adalah mempermudah pengelolaan data yang berkaitan dengan perpustakaan, seperti pendataan buku, anggota, dan transaksi peminjaman.

## Cara Menjalankan Project Secara Lokal

Ikuti langkah-langkah berikut untuk menjalankan aplikasi ini di komputer Anda:

1. Clone repository ini atau pastikan Anda berada di direktori project `app-perpustakaan`.
2. Buka terminal dan jalankan `composer install` untuk menginstal dependensi PHP.
3. Salin file `.env.example` menjadi `.env` (misalnya melalui perintah `cp .env.example .env`).
4. Generate application key dengan menjalankan `php artisan key:generate`.
5. Sesuaikan konfigurasi database pada file `.env` jika diperlukan, lalu jalankan `php artisan migrate`.
6. Jalankan server pengembangan lokal dengan perintah `php artisan serve`.
7. Buka browser dan akses aplikasi melalui `http://localhost:8000`.

---

## Pemahaman MVC (Model, View, Controller)
Menurut pemahaman saya, **Model** adalah bagian yang menangani logika data dan berinteraksi langsung dengan database (seperti menyimpan atau mengambil data). **View** adalah bagian antarmuka (UI) yang bertanggung jawab menampilkan data tersebut kepada pengguna dalam bentuk halaman web. Sementara itu, **Controller** berfungsi sebagai jembatan yang menerima permintaan (request) dari pengguna, mengolahnya melalui Model, lalu menentukan View mana yang akan ditampilkan sebagai respon.
