# Skenario Demo - app-perpustakaan

Skenario ini mengikuti urutan demo UAS (Pertemuan 11). Setiap langkah diamati langsung di browser dan Postman.

## Persiapan Sebelum Demo

- [ ] Jalankan `php artisan migrate:fresh --seed` supaya data konsisten.
- [ ] Nyalakan **dua** server di dua terminal: `php artisan serve --port=8000` dan `php artisan serve --port=8011`. Jika server kedua mati, Dashboard dan Laporan Peminjaman tampil kosong.
- [ ] Buka browser di `http://127.0.0.1:8000` dan Postman dengan base URL `http://127.0.0.1:8000/api`.
- [ ] Siapkan kredensial:

| Role | Email | Password |
|---|---|---|
| Admin | `admin@pens.ac.id` | `password` |
| Petugas | `petugas1@pens.ac.id` | `password` |
| Petugas | `petugas2@pens.ac.id` | `password` |

## Langkah Demo

### 1. Login sebagai petugas/admin
Buka `/login`, masuk dengan akun admin. Tunjukkan redirect ke Dashboard setelah login berhasil, serta nama dan role user (`Bambang Sutrisno (Admin)`) tampil di navbar.

### 2. Tampilkan dashboard statistik
Tunjukkan 3 kartu statistik (Total Buku, Total Anggota, Peminjaman Aktif) di halaman Dashboard. Jelaskan singkat bahwa angka ini diambil lewat `Http::get()` ke `GET /api/stats`, bukan query langsung ke database.

### 3. Tambah kategori baru
Buka `/categories`, tambah 1 kategori baru lewat form, lalu tunjukkan data tersimpan dan muncul di index dengan flash message sukses.

### 4. Tambah buku dengan kategori
Buka `/books`, tambah 1 buku baru dengan memilih kategori dari dropdown (termasuk kategori yang baru dibuat di langkah 3). Tunjukkan nama kategori tampil benar, bukan `category_id` mentah.

### 5. Tambah anggota baru
Buka `/members`, tambah 1 anggota baru. Tunjukkan data tersimpan, lalu uji validasi dengan `nim` atau `email` yang sama untuk membuktikan keunikan data berfungsi.

### 6. Buat transaksi peminjaman
Buka `/loans/create`, buat 1 transaksi peminjaman dengan memilih anggota dan buku (termasuk anggota dan buku yang baru ditambahkan). Tunjukkan petugas terisi otomatis dari sesi login, bukan input manual.

### 7. Lihat daftar peminjaman aktif
Buka `/loans`, tunjukkan transaksi baru tampil dengan status `dipinjam`, lengkap dengan nama anggota, petugas, dan daftar buku.

### 8. Proses pengembalian buku
Di halaman `/loans`, klik tombol **Kembalikan** pada transaksi yang dibuat di langkah 6. Tunjukkan status berubah menjadi `dikembalikan`. Alternatif lewat Postman: `PUT /api/loans/{id}/kembalikan`.

### 9. Tampilkan halaman laporan
Buka `/loans/report`, tunjukkan seluruh transaksi (termasuk yang baru diproses) tampil lengkap. Jelaskan singkat bahwa data diambil dari `GET /api/loans`, bukan query Eloquent langsung dari Controller web.

### 10. Demo API di Postman
Jalankan langsung di Postman:
- `GET /api/books` - tunjukkan response JSON terstruktur (format `BookResource`), termasuk pagination.
- `GET /api/stats` - tunjukkan response `total_buku`, `total_anggota`, dan `peminjaman_aktif` sesuai data terkini.

### 11. Tanya jawab konseptual
Dosen memilih beberapa pertanyaan sesuai sisa waktu.
