# System Architecture: HRIS & Career Management System

Dokumen ini menguraikan arsitektur sistem, tumpukan teknologi (tech stack), serta desain fungsional tingkat tinggi dari aplikasi HRIS & Career Management.

---

## 1. High-Level Architecture (Arsitektur Tingkat Tinggi)

Sistem ini menggunakan arsitektur **API-Driven (Decoupled / Headless Architecture)**. Dalam pendekatan ini, sistem dibangun menjadi dua bagian utama yang terpisah (Frontend dan Backend) yang saling berkomunikasi melalui antarmuka REST API.

### Backend (Penyedia API)
Bertugas mengelola *business logic*, transaksi database, keamanan, dan *reporting*.
*   **Bahasa Pemrograman:** PHP 8.1+
*   **Framework Utama:** Laravel 10
*   **Autentikasi API:** Laravel Sanctum (Token-based authentication)
*   **Ekstensi/Paket Utama:**
    *   `phpoffice/phpspreadsheet`: Digunakan untuk ekspor laporan data ke format Excel / CSV (misalnya pada rekap data cuti dan lembur HR).
    *   `barryvdh/laravel-dompdf`: Digunakan untuk *generate* dokumen PDF secara *on-the-fly*.

### Frontend (Client-Side/UI)
Bertanggung jawab atas antarmuka pengguna (User Interface), *routing* sisi klien (SPA), dan manajemen state antarmuka.
*   **Framework Utama:** Vue.js
*   *(Catatan: Frontend mengonsumsi data dalam format JSON dari endpoint Laravel).*

---

## 2. Core Modules (Modul Utama)

Aplikasi ini dibagi menajadi 3 domain besar:

### A. Kepegawaian & Master Data (Core HR)
Fondasi utama dari HRIS yang menangani struktur organisasi.
*   **Entity Management:** Mengelola entitas Divisi, Posisi/Jabatan, dan Karyawan.
*   **Kontrak & Status:** *Tracking* riwayat kontrak kerja dan status aktif/non-aktif karyawan.

### B. HRIS: Manajemen Kedisiplinan & Waktu
Modul operasional harian yang mencakup alur persetujuan berjenjang (*Hierarchical Approval*).
*   **Manajemen Cuti & Izin (Permits):**
    *   Sistem mencatat saldo cuti (kuota tahunan).
    *   *Role* Manager dapat menyetujui izin dari bawahannya (*Subordinates*).
    *   *Role* HR dapat melihat, memvalidasi rekap akhir (*Pending HR*), dan mengekspor datanya.
*   **Manajemen Lembur (Overtime):**
    *   Mengadopsi hierarki approval yang sama dengan Izin.
*   **Otomatisasi Sistem:** Fitur mass-generate saldo cuti tahunan oleh HR.

### C. Career Portal & Web CMS
Sistem rekrutmen dan halaman *company profile* yang dikendalikan oleh admin.
*   **Dynamic Landing Page:** Admin mengendalikan konten beranda (Hero, Core Values, Testimonial, dll) langsung dari CMS.
*   **Sistem Rekrutmen:** Manajemen pembuatan struktur lowongan (*Job Postings*, *Vacancies*) yang dapat diakses oleh publik/pelamar.

---

## 3. Alur Akses & Keamanan (RBAC)

Akses ke sistem dikelola dengan pembatasan hak akses berbasis peran (Role-Based Access Control). Komunikasi dilindungi dengan pengiriman token otorisasi Sanctum pada Header HTTP.

Ringkasan Peran:
1.  **Admin / HRD:** Memiliki akses tak terbatas ke master data, pengelolaan *user*, manajemen CMS perusahaan, dan *export* data analitik.
2.  **Manager (Atasan):** Mengakses data pribadi serta diberikan akses (*endpoint* khusus bawahan) untuk meninjau dan melakukan *Approval* pengajuan izin/lembur tim.
3.  **Karyawan Umum:** Mengakses data profil terbatas, mengajukan cuti/lembur, dan melihat riwayat serta kuota cuti.
4.  **Pelamar (Guest/Applicant):** Memiliki token yang di-generate saat proses melamar kerja untuk mengisi pertanyaan seleksi (*Screening Questions*).

---

## 4. Database & Entity Relationship

Sistem bergantung pada basis data relasional. Laravel Eloquent ORM digunakan secara dominan dengan *eager loading* untuk memetakan relasi kompleks seperti Relasi `Karyawan` dengan `Divisi`, `Posisi`, `Atasan (Managers)`, hingga `Saldo Cuti`.

Untuk detail physical data model serta hubungan entitas antar tabel, harap melihat dokumen diagram berikut:
*   [Database ERD - Versi Lengkap/Full](../assets/erd/erd-full.pdf)
*   [Relasi Entitas Modul Izin (Permits)](../assets/erd/permit_relation.png)
*   [Relasi Entitas Modul Saldo Cuti](../assets/erd/leaveBalance_relation.png)
*   [Relasi Entitas User Master Data](../assets/erd/user_relation.png)

*(File ERD disimpan di dalam folder root `assets/erd/`)*
