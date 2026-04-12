# 🏢 HRIS & Career Management System
### PT. Sinko Prima Alloy

> Fullstack HRIS & career management system dengan dynamic CMS, admin dashboard, dan employee leave management, dibangun menggunakan **Laravel 10** (Backend) dan **Vue.js** (Frontend).

---

## 📋 Daftar Isi

- [Tentang Proyek](#-tentang-proyek)
- [Fitur Utama](#-fitur-utama)
- [Tech Stack](#-tech-stack)
- [Flowchart Sistem](#-flowchart-sistem)
- [Peran Pengguna (RBAC)](#-peran-pengguna-rbac)
- [Struktur Dokumentasi](#-struktur-dokumentasi)
- [Cara Menjalankan Lokal](#-cara-menjalankan-lokal)

---

## 📖 Tentang Proyek

Proyek ini dibangun untuk mendigitalisasi dan menyederhanakan manajemen kepegawaian internal sekaligus proses rekrutmen eksternal **PT. Sinko Prima Alloy**. Sistem ini menggunakan arsitektur **API-Driven (Decoupled)** di mana Backend (Laravel) dan Frontend (Vue.js) berjalan terpisah dan saling berkomunikasi via REST API.

Tujuan utama proyek ini:
- ✅ **100% Paperless Administration** — Menghapus proses persetujuan fisik via kertas
- 📊 **Transparansi Real-time** — Seluruh pihak (karyawan, atasan, direksi) mengakses data dari satu sumber
- 🌐 **Peningkatan Employer Branding** — Melalui Portal Karir dinamis yang menarik pelamar berkualitas

---

## ✨ Fitur Utama

### 👤 Employee Self-Service (Karyawan Mandiri)
- Memonitor saldo cuti tahunan & cuti pengganti secara *real-time*
- Mengajukan Izin (Terlambat, Sakit, dll) dan Cuti secara online
- Mencatat dan melihat riwayat pengajuan Lembur (*Overtime*)
- Update profil karyawan mandiri

### ✅ Hierarchical Approval (Persetujuan Berjenjang)
- Pengajuan karyawan direview terlebih dahulu oleh **Atasan/Manager**
- Setelah disetujui Atasan, **HRD** melakukan validasi akhir
- Notifikasi otomatis setiap perubahan status pengajuan

### 🌐 Portal Karir & CMS Dinamis
- Admin HR dapat mengubah konten halaman karir (Hero, Core Values, Misi) **tanpa perlu programmer**
- Manajemen posting dan penutupan lowongan pekerjaan
- Sistem seleksi awal untuk pelamar via token autentikasi

### 📊 Reporting & Ekspor Data
- Generate saldo cuti massal tahunan untuk seluruh karyawan
- Ekspor rekap lembur dan izin bulanan ke format **Excel / CSV / PDF**

---

## 🛠 Tech Stack

| Layer | Teknologi |
|---|---|
| **Backend** | PHP 8.1+ / Laravel 10 |
| **Frontend** | Vue.js (SPA) |
| **Autentikasi** | Laravel Sanctum (Token-based) |
| **Database** | MySQL (Eloquent ORM) |
| **Ekspor Excel/CSV** | `phpoffice/phpspreadsheet` |
| **Ekspor PDF** | `barryvdh/laravel-dompdf` |

---

## 🗺 Flowchart Sistem

Gambaran alur kerja sistem secara menyeluruh — dari rekrutmen publik, pengelolaan kepegawaian, workflow persetujuan, hingga pelaporan HR.

![Flowchart Alur Sistem HRIS](./assets/flowchart/HRIS.png)

---

## 👥 Peran Pengguna (RBAC)

Sistem menggunakan **Role-Based Access Control (RBAC)** yang diproteksi oleh token Sanctum.

| Role | Hak Akses |
|---|---|
| **Admin / HRD** | Akses penuh: master data, kelola user, CMS, ekspor laporan |
| **Manager (Atasan)** | Data pribadi + review & approve pengajuan bawahannya |
| **Karyawan** | Data profil sendiri, ajukan cuti/lembur, lihat riwayat & saldo |
| **Pelamar (Guest)** | Akses via token untuk mengisi form seleksi lowongan |

---

## 📁 Struktur Dokumentasi

```
internship-hris-career-system/
├── docs/
│   ├── overview.md        # Gambaran umum, fitur, dan tujuan proyek
│   ├── architecture.md    # Tech stack, modul, dan desain arsitektur sistem
│   └── api.md             # Daftar endpoint API utama per modul
├── assets/
│   ├── flowchart/
│   │   └── HRIS.png       # Diagram alur sistem
│   ├── erd/
│   │   ├── erd-full.pdf          # ERD lengkap seluruh sistem
│   │   ├── permit_relation.png   # Relasi entitas modul Izin
│   │   ├── leaveBalance_relation.png  # Relasi entitas Saldo Cuti
│   │   └── user_relation.png    # Relasi entitas User & Master Data
│   └── ui/                # Aset tampilan / screenshot UI
└── postman/
    ├── collections/        # File JSON Postman Collection per modul
    └── environments/       # File JSON Postman Environment (local/production)
```

---

## 🚀 Cara Menjalankan Lokal

### Prasyarat
- PHP >= 8.1 & Composer
- Node.js & NPM
- MySQL

### Langkah Instalasi

```bash
# 1. Clone repository aplikasi utama
git clone <url-repo-aplikasi>
cd website_career

# 2. Install dependensi PHP
composer install

# 3. Install dependensi JavaScript
npm install

# 4. Salin dan konfigurasi environment
cp .env.example .env
php artisan key:generate

# 5. Konfigurasi database di file .env, lalu jalankan migrasi & seeder
php artisan migrate --seed

# 6. Jalankan server Backend & Frontend secara bersamaan
php artisan serve
npm run dev
```

> **Catatan:** Untuk pengujian API, import file dari folder `postman/collections/` dan `postman/environments/` ke aplikasi Postman Anda.

---

*Dikembangkan sebagai proyek magang — PT. Sinko Prima Alloy*
