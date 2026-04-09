# API Documentation (HRIS & Career Management System)

Dokumen ini merangkum daftar *endpoint* API utama yang digunakan dalam sistem, dibagi berdasarkan modul fungsionalitasnya. Karena sistem cukup besar, daftar ini difokuskan pada fungsionalitas inti (Auth, HRIS, & CMS).

---

## 1. Authentication & Users
Endpoint terkait autentikasi, manajemen sesi pengguna (karyawan, admin, pelamar), dan profil pengguna.

| Endpoint | Method | Auth | Keterangan |
|----------|--------|------|------------|
| `/api/admin/login` | `POST` | Public | Login untuk Admin. |
| `/api/login-token` | `POST` | Public | Login untuk Pelamar (Akses via token). |
| `/api/user` | `GET` | Sanctum | Mendapatkan profil detail *logged-in user* (beserta relasi ke divisi, posisi, atasan, dan info cuti). |
| `/api/admin/users` | `POST` | Sanctum | Membuat akun user/karyawan baru (Untuk Admin). |
| `/api/admin/karyawan/{id}/reset-password` | `POST` | Sanctum | Reset password akun karyawan tertentu. |
| `/api/karyawan/profile-update/{id}` | `POST` | Sanctum | Update profil karyawan. |

---

## 2. HRIS - Manajemen Izin (Permits) & Cuti
Endpoint pencatatan, pengajuan, dan persetujuan (approval) terkait absensi/izin dan cuti karyawan.

| Endpoint | Method | Keterangan |
|----------|--------|------------|
| `/api/permits` | `GET`/`POST` | Mengambil daftar izin atau membuat pengajuan izin baru. |
| `/api/permits/manager/subordinates` | `GET` | Daftar izin bawahan (Untuk view Manager/Atasan). |
| `/api/permits/hr/pending` | `GET` | Daftar izin dengan status tertunda / perlu diproses (Untuk view HRD). |
| `/api/permits/{id}/approve` | `POST` | Mengubah status/menyepakati persetujuan izin (Manager/HRD). |
| `/api/saldo-cuti` | `GET`/`POST` | Manajemen saldo cuti karyawan. |
| `/api/karyawan/info-cuti` | `GET` | Melihat informasi dan sisa *balance* cuti milik pengguna yang sedang login. |
| `/api/saldo-cuti/generate-massal` | `POST` | *Trigger* otomatis via HR untuk men-generate saldo cuti tahunan massal. |

---

## 3. HRIS - Manajemen Lembur (Overtime)
| Endpoint | Method | Keterangan |
|----------|--------|------------|
| `/api/overtime` | `GET`/`POST` | Mendapatkan *list* dari lembur saya, atau membuat pengajuan lembur baru. |
| `/api/overtime/subordinates` | `GET` | Daftar pengajuan lembur tim/bawahan (Manager View). |
| `/api/overtime/hr-pending` | `GET` | Daftar lembur yang membutuhkan validasi tahap HR. |
| `/api/overtime/{id}/approve` | `POST` | Melakukan persetujuan (Approve/Reject) pengajuan lembur. |

---

## 4. Master Data & Kepegawaian (Core HR)
Data referensi utama perusahaan seperti manajemen divisi, jabatan, kontrak, dan hari libur.

| Endpoint | Method | Keterangan |
|----------|--------|------------|
| `/api/positions` | `CRUD` | Manajemen master data Posisi / Jabatan karyawan. |
| `/api/division` | `CRUD` | Manajemen departemen / divisi perusahaan. |
| `/api/master-libur` | `CRUD*` | Pengaturan master hari libur nasional / perusahaan. |
| `/api/karyawan/{id}/history-kontrak`| `GET`/`POST` | Mengakses riwayat perubahan status kontrak dari seorang karyawan. |
| `/api/admin/karyawan/{id}/toggle-status`| `PATCH` | Menonaktifkan atau mengaktifkan kembali (*Active/Inactive*) karyawan. |

---

## 5. CMS & Career Portal (Rekrutmen)
Endpoint untuk pengelolaan *Content Management System* halaman depan (Karir) dan lowongan pekerjaan.

| Endpoint | Method | Keterangan |
|----------|--------|------------|
| `/api/job-postings` | `POST` | Mengunggah / Posting lowongan pekerjaan baru. |
| `/api/job-postings/{id}/status` | `PUT` | Memperbarui status lowongan (misal: *Draft*, *Active*, *Closed*). |
| `/api/page-sections` | `CRUD` | Mengelola modul dan urutan section pada *landing page*. |
| `/api/page-content/{key}` | `GET`/`PUT` | Dinamis CMS untuk me-load konten teks HTML berdasarkan parameter kunci (Hero, Testimonial, dl). |
| `/api/vacancy` | `CRUD` | Master data klasifikasi pekerjaan. |

---

> **Catatan:**
> - Seluruh endpoint (selain di kategori *Public*) mewajibkan penyertaan header `Authorization: Bearer <token>`.
> - Label atribut `CRUD` menandakan rute implementasi dari **Laravel API Resources**, yang secara bawaan mendukung fungsi (*Index, Store, Show, Update, Destroy*).
