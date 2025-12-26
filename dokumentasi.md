# Alur Proyek UMRI PRESS

**Path Proyek:**  
`D:\UMRI PRESS\KP\httpdocs`

Dokumen ini menjelaskan alur proyek secara detail dan terstruktur.

---

## 1. Stack & Struktur

### Teknologi
- **Framework:** Laravel 11
- **Frontend:** Livewire (Volt), Vite, Tailwind CSS

### Entry Point
- `index.php`

### Routing Utama
- `web.php`
- `auth.php`
- `dashboard-surat.php`
- `author.php`

### Alur Request Global
1. Request masuk → `index.php`
2. Laravel bootstrap → routing (`routes/*`)
3. Middleware cek auth / role / permission
4. Controller / Livewire menangani
5. Model (Eloquent)
6. View (Blade / Livewire)

---

## 2. Modul Publik (Website UMRI PRESS)

### Routing
- File: `web.php`
- Controller: `HomeController`

### Endpoint
- `/` → `HomeController@index`
- `/tentang-kami`
- `/team`
- `/harga`
- `/toko-buku`
- `/artikel`
- dll

### Fitur
- Home: artikel terbaru, pengaturan, sertifikat
- Detail buku: Buku + Authors + Comment
- Komentar buku: POST ke Comment (status **pending approval**)

### Struktur File
- Controller: `HomeController.php`
- Views: `resources/views/home/*`
- Livewire Components: `app/Livewire/Home/*`

---

## 3. Modul Dashboard Admin (Umum)

### Routing
- File: `web.php`
- Middleware: `auth`
- Controller: `DashboardController`

### Endpoint
- `/dashboard` → ringkasan

### Manajemen Data
- Buku
- Artikel
- Authors
- Tim
- Harga paket
- Pembayaran
- Transaksi
- Users
- Pengaturan

### Arsitektur
- Controller hanya menyajikan halaman
- CRUD dilakukan via Livewire

### Struktur File
- Livewire CRUD: `app/Livewire/Dashboard/*`
- Views:
  - `resources/views/dashboard/*`
  - `resources/views/livewire/dashboard/*`

### Perubahan Khusus (Royalti Buku)
- Input royalti per penulis per buku:
  - `Tambah.php`
  - `EditBuku.php`
- Lokasi:
  - `resources/views/livewire/dashboard/buku/*`
- Tabel buku menampilkan royalti:
  - `semua-buku.blade.php`

---

## 4. Modul Dashboard Surat

### Routing
- File: `dashboard-surat.php`
- Prefix: `dashboard-surat`

### Middleware
- `EnsureSuratAccess`
- `EnsureSuratPermission`
- `IsAdmin`
- `SetSuratIntendedRedirect`

### Controller
- `app/Http/Controllers/Surat/*`

### Fitur
- Surat masuk / keluar
- Disposisi
- Template
- Notifikasi
- Audit log
- Pengaturan

### Views
- `resources/views/dashboard-surat/*`

---

## 5. Modul Dashboard Author (Royalti Penulis)

### Routing
- File: `author.php`
- Prefix: `dashboard-author`
- Middleware: `auth`, `role.author`

### Endpoint
- `/dashboard-author` → ringkasan
- `/dashboard-author/sales` → penjualan
- `/dashboard-author/payouts` → pencairan
- `/dashboard-author/settings` → rekening

### Controller
- `AuthorDashboardController.php`
- `AuthorSalesController`
- `AuthorPayoutController`
- `AuthorSettingsController`

### Views & Layout
- Views: `resources/views/author/*`
- Layout: `author.blade.php`

---

## 6. Modul Royalti (Perhitungan)

### Trigger
- Saat status order berubah menjadi **completed**

### Mekanisme
- Observer: `DirectOrderObserver.php`
- Action: `CalculateRoyaltyAction.php`

### Fallback (Manual)
- `DashboardController@updatePesanan`
- Livewire Dashboard:
  - Transaksi
  - PesananLangsung

### Penyimpanan Data
- Tabel: `royalty_transactions`
- `type`: `credit`
- `status`: `pending` (default awal)

---

## 7. Modul Admin Royalti & Payout

### Admin Royalti
- Route: `/dashboard/royalty`
- Controller: `RoyaltyTransactionController.php`
- View: `index.blade.php`
- Status:
  - `pending`
  - `approved`
  - `paid`

### Admin Payout
- Route: `/dashboard/payouts`
- Controller: `PayoutRequestController.php`
- View: `index.blade.php`
- Status:
  - `pending`
  - `approved`
  - `paid`

---

## 8. Database & Relasi Inti

### Tabel Utama
- `users` (role: user / editor / admin / author)
- `authors` (relasi ke users via `user_id`)
- `buku`
- `author_buku` (pivot + royalty)
- `direct_orders`
- `royalty_transactions`
- `payout_requests`

### Relasi
- User **1–1** Authors
- Buku **many–many** Authors (via `author_buku` + `royalty_percentage`)
- DirectOrder → Buku
- RoyaltyTransaction → Authors + DirectOrder
- PayoutRequest → Authors

---

## 9. Alur Royalti (Flow)

1. Admin set persentase royalti di buku
2. Order selesai (`completed`)
3. Sistem membuat data di `royalty_transactions` (credit, pending)
4. Admin approve di `/dashboard/royalty`
5. Saldo penulis bertambah (approved / paid)
6. Penulis request payout di `/dashboard-author/payouts`
7. Admin approve payout di `/dashboard/payouts`

---

## 10. Login & Authentication

- Login umum: `/login`
- Login author khusus: `/login-author`
- Auth (Volt Livewire):
  - `resources/views/livewire/pages/auth/*`

---
