# UMRI PRESS – System Documentation

Dokumentasi ini menjelaskan arsitektur sistem, struktur modul, alur bisnis, dan mekanisme royalti pada aplikasi **UMRI PRESS**.  
Dokumen ini ditujukan sebagai **referensi teknis utama**, termasuk untuk onboarding developer dan handover.

---

## 1. Stack Teknologi & Struktur Dasar

### Stack Utama
- **Framework**: Laravel 11
- **Frontend Interaktif**: Livewire (Volt)
- **Asset Bundler**: Vite
- **CSS Framework**: Tailwind CSS

### Entry Point
- `public/index.php`  
  Seluruh request HTTP masuk melalui file ini.

### File Routing Utama
- `routes/web.php` → Website publik & dashboard admin umum
- `routes/auth.php` → Autentikasi
- `routes/dashboard-surat.php` → Modul Dashboard Surat
- `routes/author.php` → Dashboard Author (Penulis)

---

## 2. Alur Request Global

### Alur Umum
```text
Request masuk
→ index.php
→ Laravel bootstrapping
→ Routing (routes/*)
→ Middleware (auth / role / permission)
→ Controller atau Livewire Component
→ Model (Eloquent)
→ View (Blade / Livewire)
Diagram Alur Request
mermaid
Salin kode
flowchart LR
A[Client Request] --> B[index.php]
B --> C[Laravel Bootstrap]
C --> D[Routing]
D --> E[Middleware]
E --> F[Controller / Livewire]
F --> G[Eloquent Model]
G --> H[View]
3. Modul Publik (Website UMRI PRESS)
Routing
File: routes/web.php

Controller utama: HomeController

Endpoint Publik
/

/tentang-kami

/team

/harga

/toko-buku

/artikel

dan halaman publik lainnya

Fitur
Home

Artikel terbaru

Pengaturan

Sertifikat

Detail Buku

Data buku

Relasi penulis

Komentar pembaca

Komentar Buku

Disimpan dengan status pending approval

Struktur File
Controller:
app/Http/Controllers/HomeController.php

Views:
resources/views/home/*

Livewire Components:
app/Livewire/Home/*

4. Modul Dashboard Admin (Umum)
Routing
File: routes/web.php

Middleware: auth

Endpoint
/dashboard → Halaman ringkasan

Fitur
Manajemen data:

Buku

Artikel

Author

Tim

Harga paket

Pembayaran

Transaksi

User

Pengaturan

Controller hanya bertugas menampilkan halaman.
Seluruh operasi CRUD dilakukan menggunakan Livewire.

Struktur File
Controller:
DashboardController.php

Livewire CRUD:
app/Livewire/Dashboard/*

Views:

resources/views/dashboard/*

resources/views/livewire/dashboard/*

Kustomisasi Royalti
Input royalti per penulis per buku:

Tambah.php

EditBuku.php

View buku:

resources/views/livewire/dashboard/buku/*

Tabel buku menampilkan royalti:

semua-buku.blade.php

5. Modul Dashboard Surat
Routing
File: routes/dashboard-surat.php

Prefix: /dashboard-surat

Middleware
EnsureSuratAccess

EnsureSuratPermission

IsAdmin

SetSuratIntendedRedirect

Fitur
Surat masuk & keluar

Disposisi

Template surat

Notifikasi

Audit log

Pengaturan

Struktur File
Controller:
app/Http/Controllers/Surat/*

Views:
resources/views/dashboard-surat/*

6. Modul Dashboard Author (Royalti Penulis)
Routing
File: routes/author.php

Prefix: /dashboard-author

Middleware: auth, role.author

Endpoint
/dashboard-author

/dashboard-author/sales

/dashboard-author/payouts

/dashboard-author/settings

Controller
AuthorDashboardController.php

AuthorSalesController.php

AuthorPayoutController.php

AuthorSettingsController.php

Views
resources/views/author/*

Layout: author.blade.php

7. Modul Royalti (Perhitungan Otomatis)
Trigger
Saat status order berubah menjadi completed

Mekanisme
Observer
DirectOrderObserver.php

Action
CalculateRoyaltyAction.php

Fallback Manual

DashboardController@updatePesanan

Livewire Transaksi / Pesanan Langsung

Output
Tabel: royalty_transactions

Default nilai:

type = credit

status = pending

8. Modul Admin Royalti & Payout
Admin Royalti
Route: /dashboard/royalty

Controller: RoyaltyTransactionController.php

View: index.blade.php

Status:

pending

approved

paid

Admin Payout
Route: /dashboard/payouts

Controller: PayoutRequestController.php

View: index.blade.php

Status:

pending

approved

paid

9. Database & Relasi Inti
Tabel Utama
users (user / editor / admin / author)

authors (relasi ke users)

buku

author_buku (pivot + royalty_percentage)

direct_orders

royalty_transactions

payout_requests

Relasi
User 1–1 Author

Buku N–M Author

DirectOrder → Buku

RoyaltyTransaction → Author & DirectOrder

PayoutRequest → Author

10. Alur Royalti (Flow Bisnis)
mermaid
Salin kode
flowchart TD
A[Admin set royalti buku] --> B[Order completed]
B --> C[Buat royalty_transactions]
C --> D[Admin approve royalti]
D --> E[Saldo author bertambah]
E --> F[Author request payout]
F --> G[Admin approve payout]
11. Login & Autentikasi
Login umum: /login

Login author: /login-author

Livewire Volt Auth:

resources/views/livewire/pages/auth/*

12. Catatan Teknis & Handover
Semua CRUD dashboard menggunakan Livewire

Sistem royalti berbasis event (observer)

Hak akses dikontrol via middleware role & permission

Struktur modular memudahkan scaling fitur

13. Rekomendasi Pengembangan
Tambahkan unit test untuk:

Royalti calculation

Payout approval

Tambahkan logging untuk audit payout

Optimasi query relasi buku–author

