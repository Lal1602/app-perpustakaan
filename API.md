# Dokumentasi API - app-perpustakaan

Base URL: `http://127.0.0.1:8000/api` (hasil `php artisan serve`).

Semua request memakai header `Accept: application/json`. Untuk `POST` dan `PUT` dengan body, ditambah `Content-Type: application/json`. Endpoint sengaja dibiarkan publik (tanpa autentikasi) sesuai cakupan Pertemuan 9.

Seluruh contoh response di bawah adalah hasil copy dari output `curl` yang menjalankan request ke server lokal, bukan dikarang manual.

## Daftar Endpoint

| Method | URL | Body (jika ada) | Contoh Response |
|---|---|---|---|
| GET | `/api/books` | - | [`200`](#get-apibooks) |
| GET | `/api/books/{id}` | - | [`200`, `404`](#get-apibooksid) |
| POST | `/api/books` | `{"judul": "Pemrograman Laravel 12", "penulis": "Mu arifin", "penerbit": "PENS Press", "tahun_terbit": 2026, "isbn": "9786020001234", "stok": 5, "category_id": 3}` | [`201`, `422`](#post-apibooks) |
| PUT | `/api/books/{id}` | `{"judul": "Pemrograman Laravel 12 Edisi Revisi", "penulis": "Mu arifin", "penerbit": "PENS Press", "tahun_terbit": 2026, "isbn": "9786020001234", "stok": 8, "category_id": 3}` | [`200`, `422`, `404`](#put-apibooksid) |
| DELETE | `/api/books/{id}` | - | [`200`, `404`](#delete-apibooksid) |
| GET | `/api/members` | - | [`200`](#get-apimembers) |
| GET | `/api/members/{id}` | - | [`200`, `404`](#get-apimembersid) |
| GET | `/api/loans` | - | [`200`](#get-apiloans) |
| POST | `/api/loans` | `{"member_id": 1, "user_id": 3, "tanggal_pinjam": "2026-10-23", "tanggal_kembali": "2026-10-30", "book_ids": [1, 2]}` | [`201`, `422`](#post-apiloans) |
| PUT | `/api/loans/{id}/kembalikan` | - | [`200`, `404`](#put-apiloansidkembalikan) |
| GET | `/api/stats` | - | [`200`](#get-apistats) |

## Rincian Endpoint

<a id="get-apibooks"></a>

### GET `/api/books`

Mendukung filter opsional `?judul=` (pencarian sebagian) dan `?category_id=`, serta pagination 10 data per halaman.

**Sukses (tanpa filter) - `200`**

```json
{
    "data": [
        {
            "id": 1,
            "judul": "Islam Ala Prabowo",
            "penulis": "Jack o Wee",
            "penerbit": "SIDU",
            "tahun_terbit": 2026,
            "isbn": "3123500082",
            "stok": 999,
            "sampul": null,
            "category": {
                "id": 2,
                "nama_kategori": "fiksi"
            },
            "created_at": "2026-09-24T07:34:49.000000Z",
            "updated_at": "2026-09-24T07:34:49.000000Z"
        },
        {
            "id": 2,
            "judul": "Cara Menggunakan AI 2026",
            "penulis": "Jack o Wee",
            "penerbit": "SIDU",
            "tahun_terbit": 2025,
            "isbn": "3123500192",
            "stok": 192,
            "sampul": null,
            "category": {
                "id": 3,
                "nama_kategori": "teknologi"
            },
            "created_at": "2026-09-24T07:35:52.000000Z",
            "updated_at": "2026-09-24T07:35:52.000000Z"
        }
    ],
    "links": {
        "first": "http://127.0.0.1:8000/api/books?page=1",
        "last": "http://127.0.0.1:8000/api/books?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "links": [
            {
                "url": null,
                "label": "&laquo; Previous",
                "page": null,
                "active": false
            },
            {
                "url": "http://127.0.0.1:8000/api/books?page=1",
                "label": "1",
                "page": 1,
                "active": true
            },
            {
                "url": null,
                "label": "Next &raquo;",
                "page": null,
                "active": false
            }
        ],
        "path": "http://127.0.0.1:8000/api/books",
        "per_page": 10,
        "to": 2,
        "total": 2
    }
}
```

**Sukses (`?judul=islam`) - `200`**

```json
{
    "data": [
        {
            "id": 1,
            "judul": "Islam Ala Prabowo",
            "penulis": "Jack o Wee",
            "penerbit": "SIDU",
            "tahun_terbit": 2026,
            "isbn": "3123500082",
            "stok": 999,
            "sampul": null,
            "category": {
                "id": 2,
                "nama_kategori": "fiksi"
            },
            "created_at": "2026-09-24T07:34:49.000000Z",
            "updated_at": "2026-09-24T07:34:49.000000Z"
        }
    ],
    "links": {
        "first": "http://127.0.0.1:8000/api/books?page=1",
        "last": "http://127.0.0.1:8000/api/books?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "links": [
            {
                "url": null,
                "label": "&laquo; Previous",
                "page": null,
                "active": false
            },
            {
                "url": "http://127.0.0.1:8000/api/books?page=1",
                "label": "1",
                "page": 1,
                "active": true
            },
            {
                "url": null,
                "label": "Next &raquo;",
                "page": null,
                "active": false
            }
        ],
        "path": "http://127.0.0.1:8000/api/books",
        "per_page": 10,
        "to": 1,
        "total": 1
    }
}
```

**Sukses (`?category_id=3`) - `200`**

```json
{
    "data": [
        {
            "id": 2,
            "judul": "Cara Menggunakan AI 2026",
            "penulis": "Jack o Wee",
            "penerbit": "SIDU",
            "tahun_terbit": 2025,
            "isbn": "3123500192",
            "stok": 192,
            "sampul": null,
            "category": {
                "id": 3,
                "nama_kategori": "teknologi"
            },
            "created_at": "2026-09-24T07:35:52.000000Z",
            "updated_at": "2026-09-24T07:35:52.000000Z"
        }
    ],
    "links": {
        "first": "http://127.0.0.1:8000/api/books?page=1",
        "last": "http://127.0.0.1:8000/api/books?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "links": [
            {
                "url": null,
                "label": "&laquo; Previous",
                "page": null,
                "active": false
            },
            {
                "url": "http://127.0.0.1:8000/api/books?page=1",
                "label": "1",
                "page": 1,
                "active": true
            },
            {
                "url": null,
                "label": "Next &raquo;",
                "page": null,
                "active": false
            }
        ],
        "path": "http://127.0.0.1:8000/api/books",
        "per_page": 10,
        "to": 1,
        "total": 1
    }
}
```

<a id="get-apibooksid"></a>

### GET `/api/books/{id}`

**Sukses - `200`**

```json
{
    "data": {
        "id": 1,
        "judul": "Islam Ala Prabowo",
        "penulis": "Jack o Wee",
        "penerbit": "SIDU",
        "tahun_terbit": 2026,
        "isbn": "3123500082",
        "stok": 999,
        "sampul": null,
        "category": {
            "id": 2,
            "nama_kategori": "fiksi"
        },
        "created_at": "2026-09-24T07:34:49.000000Z",
        "updated_at": "2026-09-24T07:34:49.000000Z"
    }
}
```

**Gagal: id tidak ada - `404`**

```json
{
    "message": "No query results for model [App\\Models\\Book] 999",
    "exception": "Symfony\\Component\\HttpKernel\\Exception\\NotFoundHttpException"
}
```

> Field debug `file`, `line`, dan `trace` dihilangkan dari contoh karena `APP_DEBUG=true` di lingkungan pengembangan.

<a id="post-apibooks"></a>

### POST `/api/books`

**Body request**

```json
{
    "judul": "Pemrograman Laravel 12",
    "penulis": "Mu arifin",
    "penerbit": "PENS Press",
    "tahun_terbit": 2026,
    "isbn": "9786020001234",
    "stok": 5,
    "category_id": 3
}
```

**Sukses - `201`**

```json
{
    "data": {
        "id": 3,
        "judul": "Pemrograman Laravel 12",
        "penulis": "Mu arifin",
        "penerbit": "PENS Press",
        "tahun_terbit": 2026,
        "isbn": "9786020001234",
        "stok": 5,
        "sampul": null,
        "category": {
            "id": 3,
            "nama_kategori": "teknologi"
        },
        "created_at": "2026-10-23T11:30:11.000000Z",
        "updated_at": "2026-10-23T11:30:11.000000Z"
    }
}
```

**Gagal: body kosong - `422`**

```json
{
    "message": "Judul buku wajib diisi. (and 5 more errors)",
    "errors": {
        "judul": [
            "Judul buku wajib diisi."
        ],
        "penulis": [
            "Nama penulis wajib diisi."
        ],
        "penerbit": [
            "Nama penerbit wajib diisi."
        ],
        "tahun_terbit": [
            "Tahun terbit wajib diisi."
        ],
        "stok": [
            "Stok wajib diisi."
        ],
        "category_id": [
            "Kategori wajib dipilih."
        ]
    }
}
```

<a id="put-apibooksid"></a>

### PUT `/api/books/{id}`

**Body request**

```json
{
    "judul": "Pemrograman Laravel 12 Edisi Revisi",
    "penulis": "Mu arifin",
    "penerbit": "PENS Press",
    "tahun_terbit": 2026,
    "isbn": "9786020001234",
    "stok": 8,
    "category_id": 3
}
```

**Sukses - `200`**

```json
{
    "data": {
        "id": 3,
        "judul": "Pemrograman Laravel 12 Edisi Revisi",
        "penulis": "Mu arifin",
        "penerbit": "PENS Press",
        "tahun_terbit": 2026,
        "isbn": "9786020001234",
        "stok": 8,
        "sampul": null,
        "category": {
            "id": 3,
            "nama_kategori": "teknologi"
        },
        "created_at": "2026-10-23T11:30:11.000000Z",
        "updated_at": "2026-10-23T11:30:13.000000Z"
    }
}
```

**Gagal: body kosong - `422`**

```json
{
    "message": "The judul field is required. (and 5 more errors)",
    "errors": {
        "judul": [
            "The judul field is required."
        ],
        "penulis": [
            "The penulis field is required."
        ],
        "penerbit": [
            "The penerbit field is required."
        ],
        "tahun_terbit": [
            "The tahun terbit field is required."
        ],
        "stok": [
            "The stok field is required."
        ],
        "category_id": [
            "The category id field is required."
        ]
    }
}
```

**Gagal: id tidak ada - `404`**

```json
{
    "message": "No query results for model [App\\Models\\Book] 999",
    "exception": "Symfony\\Component\\HttpKernel\\Exception\\NotFoundHttpException"
}
```

> Field debug `file`, `line`, dan `trace` dihilangkan dari contoh karena `APP_DEBUG=true` di lingkungan pengembangan.

<a id="delete-apibooksid"></a>

### DELETE `/api/books/{id}`

**Sukses - `200`**

```json
{
    "message": "Buku berhasil dihapus."
}
```

**Gagal: id tidak ada - `404`**

```json
{
    "message": "No query results for model [App\\Models\\Book] 999",
    "exception": "Symfony\\Component\\HttpKernel\\Exception\\NotFoundHttpException"
}
```

> Field debug `file`, `line`, dan `trace` dihilangkan dari contoh karena `APP_DEBUG=true` di lingkungan pengembangan.

<a id="get-apimembers"></a>

### GET `/api/members`

**Sukses - `200`**

```json
{
    "data": [
        {
            "id": 1,
            "nama": "Siti Aminah",
            "nim": "31265000286",
            "email": "sitia@example.com",
            "nomor_telepon": "08964112291729",
            "alamat": "Surabaka",
            "status": "aktif",
            "created_at": "2026-09-24T07:39:08.000000Z",
            "updated_at": "2026-09-24T07:39:08.000000Z"
        },
        {
            "id": 2,
            "nama": "Budi Santoso",
            "nim": "31265000287",
            "email": "budis@example.com",
            "nomor_telepon": "0129479801579",
            "alamat": "Sidosermo",
            "status": "aktif",
            "created_at": "2026-09-24T07:40:13.000000Z",
            "updated_at": "2026-09-24T07:40:13.000000Z"
        }
    ],
    "links": {
        "first": "http://127.0.0.1:8000/api/members?page=1",
        "last": "http://127.0.0.1:8000/api/members?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "links": [
            {
                "url": null,
                "label": "&laquo; Previous",
                "page": null,
                "active": false
            },
            {
                "url": "http://127.0.0.1:8000/api/members?page=1",
                "label": "1",
                "page": 1,
                "active": true
            },
            {
                "url": null,
                "label": "Next &raquo;",
                "page": null,
                "active": false
            }
        ],
        "path": "http://127.0.0.1:8000/api/members",
        "per_page": 10,
        "to": 2,
        "total": 2
    }
}
```

<a id="get-apimembersid"></a>

### GET `/api/members/{id}`

**Sukses - `200`**

```json
{
    "data": {
        "id": 1,
        "nama": "Siti Aminah",
        "nim": "31265000286",
        "email": "sitia@example.com",
        "nomor_telepon": "08964112291729",
        "alamat": "Surabaka",
        "status": "aktif",
        "created_at": "2026-09-24T07:39:08.000000Z",
        "updated_at": "2026-09-24T07:39:08.000000Z"
    }
}
```

**Gagal: id tidak ada - `404`**

```json
{
    "message": "No query results for model [App\\Models\\Member] 999",
    "exception": "Symfony\\Component\\HttpKernel\\Exception\\NotFoundHttpException"
}
```

> Field debug `file`, `line`, dan `trace` dihilangkan dari contoh karena `APP_DEBUG=true` di lingkungan pengembangan.

<a id="get-apiloans"></a>

### GET `/api/loans`

**Sukses - `200`**

```json
{
    "data": [
        {
            "id": 1,
            "member": {
                "id": 1,
                "nama": "Siti Aminah"
            },
            "petugas": {
                "id": 1,
                "name": "Admin Perpustakaan"
            },
            "tanggal_pinjam": "2026-10-01",
            "tanggal_kembali": "2026-10-08",
            "tanggal_dikembalikan": "2026-10-01",
            "status": "dikembalikan",
            "books": [
                {
                    "id": 1,
                    "judul": "Islam Ala Prabowo"
                },
                {
                    "id": 2,
                    "judul": "Cara Menggunakan AI 2026"
                }
            ]
        },
        {
            "id": 2,
            "member": {
                "id": 1,
                "nama": "Siti Aminah"
            },
            "petugas": {
                "id": 1,
                "name": "Admin Perpustakaan"
            },
            "tanggal_pinjam": "2026-10-01",
            "tanggal_kembali": "2026-10-08",
            "tanggal_dikembalikan": null,
            "status": "dipinjam",
            "books": [
                {
                    "id": 1,
                    "judul": "Islam Ala Prabowo"
                },
                {
                    "id": 2,
                    "judul": "Cara Menggunakan AI 2026"
                }
            ]
        },
        {
            "id": 3,
            "member": {
                "id": 2,
                "nama": "Budi Santoso"
            },
            "petugas": {
                "id": 2,
                "name": "Budi Santoso (Petugas)"
            },
            "tanggal_pinjam": "2026-10-01",
            "tanggal_kembali": "2026-10-08",
            "tanggal_dikembalikan": null,
            "status": "dipinjam",
            "books": [
                {
                    "id": 1,
                    "judul": "Islam Ala Prabowo"
                }
            ]
        },
        {
            "id": 4,
            "member": {
                "id": 1,
                "nama": "Siti Aminah"
            },
            "petugas": {
                "id": 4,
                "name": "Petugas Satu"
            },
            "tanggal_pinjam": "2026-10-10",
            "tanggal_kembali": "2026-10-17",
            "tanggal_dikembalikan": null,
            "status": "dipinjam",
            "books": [
                {
                    "id": 1,
                    "judul": "Islam Ala Prabowo"
                }
            ]
        }
    ],
    "links": {
        "first": "http://127.0.0.1:8000/api/loans?page=1",
        "last": "http://127.0.0.1:8000/api/loans?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "links": [
            {
                "url": null,
                "label": "&laquo; Previous",
                "page": null,
                "active": false
            },
            {
                "url": "http://127.0.0.1:8000/api/loans?page=1",
                "label": "1",
                "page": 1,
                "active": true
            },
            {
                "url": null,
                "label": "Next &raquo;",
                "page": null,
                "active": false
            }
        ],
        "path": "http://127.0.0.1:8000/api/loans",
        "per_page": 10,
        "to": 4,
        "total": 4
    }
}
```

<a id="post-apiloans"></a>

### POST `/api/loans`

`user_id` (petugas) wajib dikirim di body karena endpoint ini publik dan tidak memakai `auth()`.

**Body request**

```json
{
    "member_id": 1,
    "user_id": 3,
    "tanggal_pinjam": "2026-10-23",
    "tanggal_kembali": "2026-10-30",
    "book_ids": [
        1,
        2
    ]
}
```

**Sukses - `201`**

```json
{
    "data": {
        "id": 5,
        "member": {
            "id": 1,
            "nama": "Siti Aminah"
        },
        "petugas": {
            "id": 3,
            "name": "Admin Perpustakaan"
        },
        "tanggal_pinjam": "2026-10-23",
        "tanggal_kembali": "2026-10-30",
        "tanggal_dikembalikan": null,
        "status": "dipinjam",
        "books": [
            {
                "id": 1,
                "judul": "Islam Ala Prabowo"
            },
            {
                "id": 2,
                "judul": "Cara Menggunakan AI 2026"
            }
        ]
    }
}
```

**Gagal: body kosong - `422`**

```json
{
    "message": "Anggota wajib dipilih. (and 4 more errors)",
    "errors": {
        "member_id": [
            "Anggota wajib dipilih."
        ],
        "user_id": [
            "Petugas (user_id) wajib diisi."
        ],
        "tanggal_pinjam": [
            "Tanggal pinjam wajib diisi."
        ],
        "tanggal_kembali": [
            "Tanggal kembali wajib diisi."
        ],
        "book_ids": [
            "Pilih minimal satu buku."
        ]
    }
}
```

<a id="put-apiloansidkembalikan"></a>

### PUT `/api/loans/{id}/kembalikan`

**Sukses - `200`**

```json
{
    "data": {
        "id": 5,
        "member": {
            "id": 1,
            "nama": "Siti Aminah"
        },
        "petugas": {
            "id": 3,
            "name": "Admin Perpustakaan"
        },
        "tanggal_pinjam": "2026-10-23",
        "tanggal_kembali": "2026-10-30",
        "tanggal_dikembalikan": "2026-10-23",
        "status": "dikembalikan",
        "books": [
            {
                "id": 1,
                "judul": "Islam Ala Prabowo"
            },
            {
                "id": 2,
                "judul": "Cara Menggunakan AI 2026"
            }
        ]
    }
}
```

**Gagal: id tidak ada - `404`**

```json
{
    "message": "No query results for model [App\\Models\\Loan] 999",
    "exception": "Symfony\\Component\\HttpKernel\\Exception\\NotFoundHttpException"
}
```

> Field debug `file`, `line`, dan `trace` dihilangkan dari contoh karena `APP_DEBUG=true` di lingkungan pengembangan.

<a id="get-apistats"></a>

### GET `/api/stats`

**Sukses - `200`**

```json
{
    "total_buku": 2,
    "total_anggota": 2,
    "peminjaman_aktif": 3
}
```
