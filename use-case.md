# Use Case Sistem UMRI PRESS

Dokumen ini merangkum seluruh use case pada sistem UMRI PRESS berdasarkan peran pengguna dan modul aplikasi.

---

## 1. Publik (Website UMRI PRESS)

### Akses Konten Publik
- Melihat halaman beranda dan konten terbaru:
  - Artikel
  - Sertifikat
- Melihat daftar buku
- Melihat detail buku
- Melihat profil penulis

### Interaksi Pengguna
- Mengirim komentar pada buku  
  - Status awal: **pending approval**

### Informasi Layanan
- Mengakses informasi layanan penerbitan
- Melihat harga paket layanan
- Melihat informasi kontak

---

## 2. Dashboard Admin (Umum)

### Manajemen Buku
- CRUD buku
- Upload cover buku
- Upload file ebook
- Menentukan penulis buku
- Mengatur persentase royalti per penulis

### Manajemen Artikel
- CRUD artikel
- CRUD kategori artikel

### Manajemen Author
- CRUD profil penulis (Authors)

### Manajemen Tim & Sertifikat
- CRUD data tim
- CRUD sertifikat

### Manajemen Layanan & Pembayaran
- Kelola paket harga layanan
- Kelola metode pembayaran

### Manajemen User & Hak Akses
- Kelola user
- Kelola role:
  - user
  - editor
  - admin
  - author

### Moderasi Konten
- Moderasi komentar buku (approve / reject)

---

## 3. Transaksi (Pesanan Langsung)

### Manajemen Pesanan
- Melihat daftar pesanan
- Melihat detail pesanan
- Filter pesanan berdasarkan status

### Update Status
- Mengubah status pesanan:
  - `pending`
  - `completed`

---

## 4. Royalti Penulis (Admin)

### Manajemen Royalti
- Melihat daftar transaksi royalti
- Filter transaksi berdasarkan:
  - status
  - tipe transaksi
  - penulis (author)

### Update Status Royalti
- Mengubah status royalti:
  - `pending`
  - `approved`
  - `paid`

---

## 5. Pencairan Penulis (Admin)

### Manajemen Payout
- Melihat daftar permintaan pencairan dana
- Filter pencairan berdasarkan:
  - penulis
  - status

### Update Status Payout
- Mengubah status pencairan:
  - `pending`
  - `approved`
  - `paid`

---

## 6. Dashboard Author

### Informasi Keuangan
- Melihat ringkasan:
  - total pendapatan
  - saldo tersedia
  - total dana dicairkan
- Melihat tren pendapatan bulanan

### Performa Buku
- Melihat daftar buku milik penulis
- Melihat performa buku:
  - jumlah terjual
  - omzet

### Royalti
- Melihat transaksi royalti terbaru

### Payout
- Mengajukan pencairan dana
- Melihat status pencairan

### Pengaturan Akun
- Update informasi rekening bank

---

## 7. Surat (Dashboard Surat)

### Surat Masuk
- Melihat daftar surat masuk
- Melihat detail surat masuk
- Membuat surat masuk
- Update surat masuk
- Menghapus surat masuk

### Surat Keluar
- Melihat daftar surat keluar
- Melihat detail surat keluar
- Membuat surat keluar
- Update surat keluar
- Menghapus surat keluar

### Export Data
- Export surat masuk
- Export surat keluar
- Format:
  - CSV
  - PDF

### Disposisi
- Membuat disposisi surat
- Update status disposisi

### Template Surat
- CRUD template surat

### Notifikasi
- Melihat notifikasi
- Menandai notifikasi sebagai terbaca

### Audit & Pengaturan
- Melihat audit log aktivitas
- Mengelola pengaturan surat:
  - unit
  - jenis surat

---
