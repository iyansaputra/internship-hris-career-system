# Sinko HRIS Postman Collections

Direktori ini dikhususkan untuk menyimpan file ekspor (JSON) dari [Postman](https://www.postman.com/) untuk keperluan dokumentasi, *testing*, dan kemudahan *developer* lain saat mencoba API project ini.

## Struktur Direktori Ini
- `collections/`: Letakkan file ekspor Postman Collection JSON (v2.1) di dalam folder ini.
- `environments/`: Letakkan file Postman Environment JSON (Jika Anda mensetup *base url* atau variabel token lain).

## Panduan Folder di dalam Postman Collection Anda (Rekomendasi)
Saat membuat koleksi API di aplikasi Postman Anda nantinya, sangat direkomendasikan untuk menyortirnya ke dalam folder berikut agar tidak berantakan:

1. **`01 - Authentication`**
   - Berisi Endpoint `Login Admin`, `Login Pelamar`, `Get Data Profile User`, dll.
2. **`02 - HRIS Core (Permits & Overtime)`**
   - Berisi Endpoint CRUD Cuti/Izin, Lembur, dan eksekusi persetujuan (*Approve*) Atasan dan Ekspor. 
3. **`03 - Master Data`**
   - Endpoint Master libur, Posisi, dan User.
4. **`04 - CMS Web Career`**
   - Berisi Endpoint Homepage (*Page content*), Dinamis Hero, dan Pembuatan Job Postings.

### Cara Pakai
Cukup *Import* JSON dari folder `collections/` langsung ke Postman Anda. Apabila ada Environment, *Import* juga file JSON dari `environments/`.
