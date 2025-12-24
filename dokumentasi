# Sistem Website Penerbitan & Penjualan Buku

Aplikasi ini adalah platform website penerbitan buku yang menyediakan halaman publik untuk informasi penerbit, artikel, dan buku, serta sistem transaksi pembelian buku dan dashboard admin untuk pengelolaan konten, transaksi, dan pengaturan sistem.

---

## 1. Gambaran Umum Sistem

Aplikasi terbagi menjadi beberapa bagian utama:

1. Frontend Publik  
2. Sistem Transaksi Pembelian Buku  
3. Moderasi & Operasional  
4. Dashboard Admin  
5. Manajemen Data & Relasi  

Teknologi utama yang digunakan:
- Laravel
- Livewire & Livewire Volt
- Blade Template
- MySQL / MariaDB

---

## 2. Autentikasi & Hak Akses

### 2.1 Autentikasi
- Menggunakan Livewire Volt
- Fitur yang tersedia:
  - Login
  - Forgot Password
  - Reset Password
  - Email Verification
- Registrasi user publik dinonaktifkan
- Akun admin dibuat melalui dashboard atau database seed

### 2.2 Proteksi Akses
- Semua route dashboard berada dalam middleware `auth`
- Pengunjung publik hanya dapat:
  - Melihat konten
  - Mengirim komentar
  - Melakukan pembelian buku

---

## 3. Alur Publik (Frontend)

### 3.1 Homepage (`/`)
Menampilkan:
- Banner utama
- Sertifikat penerbit
- Buku terbaru (Livewire)
- Artikel terbaru
- CTA kirim naskah

Sumber data:
- Pengaturan
- Sertifikat
- Buku
- Artikel

---

### 3.2 Halaman Informasi
- Tentang
- Tim (data dinamis dari database)
- Kontak (data dari Pengaturan)

---

### 3.3 Penulis & Pengiriman Naskah
- Daftar Penulis: halaman informasi & CTA
- Kirim Naskah:
  - Link Google Form dari Pengaturan
  - Download template dokumen penerbit

---

### 3.4 Progress ISBN
- Embed Google Sheet
- URL sheet disimpan di Pengaturan (`progress-isbn`)

---

### 3.5 Artikel

#### List Artikel (`/artikel`)
- Livewire pagination
- Hanya artikel dengan status `publish`

#### Detail Artikel (`/artikel/{slug}`)
- Menampilkan artikel publish
- View counter otomatis bertambah

---

### 3.6 Toko Buku

#### List Buku (`/toko-buku`)
- Livewire list
- Search dan sorting
- Modal detail dan link marketplace

#### Detail Buku (`/detail-buku/{slug}`)
Menampilkan:
- Informasi buku
- Penulis
- Kategori
- Buku terkait
- Komentar dan reply
- Aksi pembelian buku

---

## 4. Alur Pembelian Buku

### 4.1 Syarat Pembelian via UMRI Press
Tombol **Beli via UMRI Press** muncul jika:
- `allow_umri_press_payment = true`
- Buku bukan status `coming soon`

---

### 4.2 Proses Pembelian
1. User membuka halaman detail buku
2. Klik tombol **Beli via UMRI Press**
3. Modal Livewire ditampilkan
4. User mengisi:
   - Nama
   - Alamat
   - Tipe buku (hardcopy / softcopy)
   - Metode pembayaran
   - Upload bukti pembayaran (opsional)
5. Sistem:
   - Menghitung harga dan diskon
   - Menyimpan data ke tabel `direct_orders`
   - Menghasilkan kode order
   - Membuat link WhatsApp ke admin
6. User diarahkan ke WhatsApp admin

---

### 4.3 Pengelolaan Transaksi oleh Admin
- Admin melihat daftar pesanan
- Admin dapat:
  - Mengubah status pesanan
  - Menambahkan catatan admin
- Sistem berfungsi sebagai pencatatan dan tracking transaksi manual

---

## 5. Komentar Buku & Moderasi

### 5.1 Komentar Publik
- Pengunjung mengirim komentar atau reply
- Data disimpan dengan status `is_approved = false`

### 5.2 Moderasi Admin
- Admin dapat:
  - Menyetujui komentar
  - Menghapus komentar
- Hanya komentar yang disetujui yang tampil di halaman publik

---

## 6. Dashboard Admin

### 6.1 Dashboard Utama
- Ringkasan statistik
- Data terbaru (buku, artikel, transaksi)

---

### 6.2 Manajemen Konten
- Buku
  - CRUD
  - Soft delete
  - Diskon
  - Ebook
  - Link marketplace
  - Toggle hardcopy / softcopy
  - Toggle pembayaran UMRI Press
- Kategori Buku
- Authors (foto & slug)
- Artikel (draft / publish)
- Tim
- Sertifikat
- Paket Penerbit

---

### 6.3 Transaksi & Operasional
- Metode Pembayaran
- Pesanan Langsung
  - Filter data
  - Update status
  - Catatan admin
- Users Admin
  - Tidak dapat menghapus akun sendiri
  - Tidak dapat menghapus email utama

---

### 6.4 Pengaturan Sistem
Digunakan sebagai konfigurasi pusat frontend:
- Logo
- Template dokumen
- File PDF
- Link Google Form
- Progress ISBN
- Konfigurasi tampilan lainnya

---

## 7. Struktur Data & Relasi

### 7.1 Relasi Utama
- Buku
  - belongsTo Kategori
  - many-to-many Authors
  - hasMany Comment (approved only)
- Artikel
  - belongsTo KategoriArtikel
  - belongsTo User
- DirectOrder
  - belongsTo Buku
  - belongsTo PaymentMethod

---

## 8. Alur Sistem End-to-End

Alur utama sistem dari sisi pengguna hingga admin adalah sebagai berikut:

Pengunjung Publik  
↓  
Baca Konten / Pilih Buku  
↓  
Komentar / Pembelian  
↓  
Database (Pending / Draft)  
↓  
Dashboard Admin  
↓  
Moderasi & Update Status  

---

## 9. Catatan Teknis Penting

### 9.1 Route Kategori Buku
Saat ini route `/kategori` mengarah ke method controller yang belum tersedia dan berpotensi menyebabkan error.

Rekomendasi:
Gunakan route `/kategori/{slug}` dan implementasi method `kategoriBuku($slug)` untuk menampilkan buku berdasarkan kategori.

---

## 10. Catatan Akhir
Dokumentasi ini dibuat untuk:
- Handover project
- Onboarding developer baru
- Referensi pengembangan lanjutan

Pastikan dokumentasi ini diperbarui jika terdapat perubahan struktur aplikasi atau alur bisnis.
