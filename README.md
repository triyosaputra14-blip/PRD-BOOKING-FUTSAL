⚽

**SISTEM BOOKING FUTSAL**

Product Requirements Document (PRD)

──────────────────────────────────

Laravel 12 • MySQL • Blade + Tailwind CSS

Laravel Breeze • WhatsApp Notification

Versi 1.0 | April 2026

| **Informasi** | **Detail**                         |
| ------------- | ---------------------------------- |
| Nama Proyek   | Sistem Booking Lapangan Futsal     |
| Versi Dokumen | 1.0                                |
| Tanggal       | April 2026                         |
| Status        | Draft                              |
| Backend       | Laravel 12 (PHP 8.3+)              |
| Database      | MySQL 8.0+                         |
| Frontend      | Blade Templating + Tailwind CSS    |
| Auth          | Laravel Breeze                     |
| Notifikasi    | WhatsApp (via Fonnte / WA Gateway) |
| Role Pengguna | Admin & User (Pelanggan)           |

# **1\. Ringkasan Eksekutif**

Dokumen ini adalah Product Requirements Document (PRD) untuk pengembangan Sistem Booking Lapangan Futsal berbasis web. Sistem ini dibuat untuk membantu pengelola lapangan futsal mengelola pemesanan secara digital, menggantikan proses manual yang tidak efisien dan rawan kesalahan.

## **1.1 Latar Belakang & Masalah**

Pengelola lapangan futsal saat ini umumnya masih mencatat pemesanan secara manual (buku, WhatsApp, telepon), yang mengakibatkan:

- Risiko double booking karena tidak ada sistem terpusat
- Pencatatan keuangan yang tidak rapi dan sulit direkap
- Admin kewalahan menjawab pertanyaan ketersediaan lapangan
- Pelanggan tidak bisa melihat jadwal yang kosong secara mandiri

## **1.2 Solusi**

_Aplikasi web booking futsal yang memungkinkan pelanggan melihat ketersediaan_

_jadwal, melakukan pemesanan, dan membayar - sementara admin mengelola semua_

_data dari satu dashboard yang sederhana dan mudah digunakan._

## **1.3 Tujuan Utama**

- Menghilangkan double booking dengan sistem kalender terpusat
- Mempermudah pelanggan memesan lapangan kapan saja tanpa harus menelepon
- Membantu admin memantau pendapatan dan booking dengan mudah
- Notifikasi otomatis via WhatsApp untuk konfirmasi booking

# **2\. Ruang Lingkup Sistem**

## **2.1 Yang Termasuk (In Scope)**

| **Modul**                  | **Deskripsi**                                                     | **Prioritas** |
| -------------------------- | ----------------------------------------------------------------- | ------------- |
| Autentikasi                | Login & register untuk User dan Admin (Laravel Breeze + Tailwind) | Wajib         |
| Manajemen Lapangan         | Admin mengelola data lapangan, jadwal, dan harga per jam          | Wajib         |
| Booking Lapangan           | User memilih lapangan, tanggal, jam, lalu mengajukan booking      | Wajib         |
| Pembayaran Manual          | User upload bukti transfer, admin konfirmasi via dashboard        | Wajib         |
| Notifikasi WhatsApp        | Pesan WA otomatis ke user saat status booking berubah             | Wajib         |
| Dashboard Admin            | Ringkasan booking, pendapatan, dan manajemen data                 | Wajib         |
| Laporan Keuangan Sederhana | Rekap pemasukan harian/bulanan, daftar transaksi                  | Wajib         |
| Riwayat Booking User       | User bisa lihat semua riwayat pemesanannya                        | Wajib         |

## **2.2 Yang Tidak Termasuk (Out of Scope)**

- Aplikasi mobile native (Android/iOS)
- Payment gateway otomatis (Midtrans, Xendit, dll) - dikembangkan di fase berikutnya
- Fitur pencarian/filter lapangan (hanya 1 venue dengan beberapa lapangan)
- Login dengan Google/sosial media
- Sistem loyalty/poin reward pelanggan
- Manajemen multi-cabang
- Akuntansi lanjutan (jurnal, neraca, laporan laba rugi)

## **2.3 Aktor / Pengguna Sistem**

| **Role**         | **Deskripsi**                                 | **Hak Akses Utama**                                                      |
| ---------------- | --------------------------------------------- | ------------------------------------------------------------------------ |
| Admin            | Pengelola lapangan futsal (pemilik atau staf) | Kelola lapangan, konfirmasi booking & pembayaran, lihat laporan keuangan |
| User (Pelanggan) | Pelanggan yang ingin menyewa lapangan         | Daftar, login, lihat jadwal, booking, upload bukti bayar, lihat riwayat  |

# **3\. Teknologi yang Digunakan**

| **Komponen**        | **Teknologi**            | **Versi**      | **Catatan**                                |
| ------------------- | ------------------------ | -------------- | ------------------------------------------ |
| Backend Framework   | Laravel                  | 12.x           | PHP framework utama (MVC)                  |
| Bahasa Pemrograman  | PHP                      | 8.3+           | Diperlukan untuk Laravel 12                |
| Database            | MySQL                    | 8.0+           | Penyimpanan data utama                     |
| Templating Engine   | Blade                    | Bawaan Laravel | Server-side rendering                      |
| CSS Framework       | Tailwind CSS             | 3.x            | Sudah terintegrasi di Breeze               |
| Autentikasi Starter | Laravel Breeze           | 2.x            | Login, register, reset password + Tailwind |
| JavaScript          | Alpine.js                | 3.x            | Interaktivitas ringan (sudah di Breeze)    |
| Notifikasi          | WA Gateway (Fonnte)      | API            | Kirim pesan WA otomatis                    |
| File Storage        | Laravel Storage (local)  | Bawaan         | Simpan bukti transfer                      |
| Email (opsional)    | Laravel Mail / Mailtrap  | Bawaan         | Hanya untuk reset password                 |
| Queue / Job         | Laravel Queue (database) | Bawaan         | Kirim WA async, tanpa Redis                |
| Scheduler           | Laravel Scheduler        | Bawaan         | Auto-cancel booking kedaluwarsa            |
| Web Server          | Nginx atau Apache        | Latest         | Hosting di shared hosting / VPS            |
| Cache               | File Cache               | Bawaan Laravel | Cache sederhana tanpa Redis                |

**Catatan Arsitektur:** Sistem ini sengaja dirancang sederhana tanpa Redis, payment gateway otomatis, atau microservice. Cukup satu server VPS/shared hosting dengan PHP 8.3 dan MySQL. Queue menggunakan driver database bawaan Laravel.

## **3.1 Arsitektur Aplikasi**

Sistem mengikuti pola MVC standar Laravel:

Browser → Routes (web.php) → Middleware (auth, role) → Controller → Model → MySQL

Controller → Service Class (untuk logika booking & keuangan)

Controller → Blade View → Tailwind CSS → Response ke Browser

# **4\. Fitur & Persyaratan Fungsional**

## **4.1 Modul Autentikasi (Laravel Breeze + Tailwind)**

Autentikasi menggunakan Laravel Breeze yang sudah menyediakan scaffolding lengkap dengan tampilan Tailwind CSS. Tidak perlu membangun dari nol.

### **4.1.1 Halaman Register (User)**

- Field: Nama Lengkap, Nomor WhatsApp, Email, Password, Konfirmasi Password
- Validasi: format email, password minimal 8 karakter, nomor WA unik & valid
- Setelah register, user langsung bisa login (tanpa verifikasi email)
- Role otomatis di-set sebagai 'user' saat registrasi

### **4.1.2 Halaman Login**

- Login menggunakan Email + Password
- Checkbox 'Ingat Saya' untuk perpanjangan sesi
- Pembatasan: maksimal 5x login gagal dalam 1 menit, lalu diblokir 15 menit (bawaan Breeze)
- Redirect setelah login: Admin → Dashboard Admin, User → Dashboard User

### **4.1.3 Lupa & Reset Password**

- Fitur lupa password menggunakan link yang dikirim ke email (bawaan Breeze)
- Token reset kedaluwarsa dalam 60 menit
- Halaman ganti password dari profil (untuk password yang masih diingat)

### **4.1.4 Profil Pengguna**

- Edit: nama, nomor WhatsApp, email, foto profil (JPG/PNG max 2MB)
- Nomor WhatsApp wajib diisi karena digunakan untuk notifikasi
- Admin tidak bisa mengubah role pengguna via UI profil (hanya via seeder/tinker)

## **4.2 Manajemen Lapangan (Admin Only)**

### **4.2.1 Data Lapangan**

- Admin dapat menambahkan lapangan dengan isian:
  - Nama lapangan (misal: Lapangan A, Lapangan B)
  - Tipe: Indoor atau Outdoor
  - Deskripsi singkat
  - Foto lapangan (upload multiple, tampil sebagai galeri)
  - Status: Aktif atau Nonaktif (nonaktif tidak bisa dipesan)
- Admin dapat mengubah atau menonaktifkan lapangan kapan saja

### **4.2.2 Jadwal & Harga**

- Admin mengatur jam operasional: jam buka dan jam tutup (misal 08.00 - 24.00)
- Slot booking per 1 jam (bisa dikonfigurasi)
- Harga dibedakan menjadi dua tipe:
  - Harga Reguler (Senin - Jumat)
  - Harga Weekend (Sabtu - Minggu + Hari Libur Nasional)
- Admin bisa mengatur tanggal libur nasional untuk penyesuaian harga weekend
- Contoh: Lapangan A Reguler Rp 80.000/jam, Weekend Rp 100.000/jam

| **Pengaturan Harga**    | **Contoh Nilai** | **Keterangan**                                            |
| ----------------------- | ---------------- | --------------------------------------------------------- |
| Harga Reguler (Weekday) | Rp 80.000/jam    | Berlaku Senin s.d. Jumat                                  |
| Harga Weekend           | Rp 100.000/jam   | Berlaku Sabtu, Minggu & Hari Libur                        |
| Durasi Minimum Booking  | 1 jam            | User tidak bisa booking kurang dari 1 jam                 |
| Durasi Maksimum Booking | 4 jam            | Batas maksimal per satu transaksi booking                 |
| Batas Waktu Pembayaran  | 2 jam            | Jika tidak bayar dalam 2 jam, booking otomatis dibatalkan |

## **4.3 Modul Booking (User & Admin)**

### **4.3.1 Alur Booking oleh User**

Proses booking dibuat sesederhana mungkin dalam 4 langkah:

| **Langkah** | **Halaman / Aksi**    | **Detail**                                                                                                                                                 |
| ----------- | --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1           | Lihat Daftar Lapangan | User melihat semua lapangan yang aktif beserta info harga dan tipe (indoor/outdoor)                                                                        |
| 2           | Pilih Jadwal          | User memilih tanggal dan slot jam yang tersedia dari tampilan kalender/grid jam. Slot yang sudah dipesan ditampilkan abu-abu dan tidak bisa dipilih.       |
| 3           | Review & Konfirmasi   | Tampilkan ringkasan: nama lapangan, tanggal, jam mulai-selesai, durasi, total harga. User menekan tombol 'Pesan Sekarang'.                                 |
| 4           | Upload Bukti Bayar    | Setelah booking dibuat, user diarahkan ke halaman pembayaran. Ditampilkan nomor rekening dan total yang harus ditransfer. User upload foto bukti transfer. |

### **4.3.2 Status Booking**

| **Status**          | **Warna** | **Arti**                                   | **Aksi Tersedia**                             |
| ------------------- | --------- | ------------------------------------------ | --------------------------------------------- |
| Menunggu Pembayaran | Kuning    | Booking dibuat, belum ada bukti bayar      | User: Upload bukti bayar / Batalkan           |
| Menunggu Konfirmasi | Biru      | Bukti bayar sudah diupload, menunggu admin | Admin: Konfirmasi atau Tolak                  |
| Dikonfirmasi        | Hijau     | Pembayaran valid, booking aktif            | User: Lihat detail. Admin: Batalkan (darurat) |
| Selesai             | Abu-abu   | Waktu bermain sudah lewat                  | User: Beri ulasan (opsional)                  |
| Dibatalkan          | Merah     | Booking dibatalkan oleh user atau admin    | -                                             |

### **4.3.3 Pembatalan Booking**

- User dapat membatalkan booking yang masih berstatus 'Menunggu Pembayaran' kapan saja
- Booking berstatus 'Dikonfirmasi' hanya bisa dibatalkan oleh Admin
- Kebijakan refund untuk pembatalan booking yang sudah dikonfirmasi ditentukan sendiri oleh admin (di luar sistem)

### **4.3.4 Auto-Cancel Otomatis**

- Laravel Scheduler berjalan setiap 5 menit memeriksa booking
- Booking dengan status 'Menunggu Pembayaran' yang sudah melewati batas waktu 2 jam akan otomatis diubah ke status 'Dibatalkan'
- User mendapat notifikasi WhatsApp saat booking otomatis dibatalkan

## **4.4 Modul Pembayaran**

### **4.4.1 Alur Pembayaran (Manual Transfer)**

- Setelah booking dibuat, user diarahkan ke halaman detail booking yang menampilkan:
  - Nomor rekening tujuan transfer (diambil dari pengaturan admin)
  - Nama pemilik rekening
  - Total yang harus dibayar
  - Batas waktu pembayaran (countdown timer 2 jam)
- Tombol 'Upload Bukti Bayar' untuk mengunggah foto struk/screenshot transfer
- File yang diterima: JPG, PNG, PDF - maksimal 5MB
- Setelah upload, status booking berubah menjadi 'Menunggu Konfirmasi'

### **4.4.2 Konfirmasi Pembayaran oleh Admin**

- Admin melihat daftar booking berstatus 'Menunggu Konfirmasi' di dashboard
- Admin membuka detail booking dan melihat foto bukti bayar
- Admin memilih aksi:
  - Konfirmasi: status booking jadi 'Dikonfirmasi', notif WA dikirim ke user
  - Tolak: admin mengisi alasan, status kembali ke 'Menunggu Pembayaran', notif WA dikirim

### **4.4.3 Data Rekening Bank (Pengaturan Admin)**

- Admin mengisi informasi rekening penerima di halaman Pengaturan:
  - Nama bank, nomor rekening, nama pemilik rekening
  - Bisa mengatur lebih dari satu rekening (BCA, BRI, Mandiri, dll)
- Info ini ditampilkan otomatis di halaman pembayaran user

## **4.5 Notifikasi WhatsApp**

**📱 Notifikasi WhatsApp**

Sistem menggunakan WhatsApp Gateway (Fonnte atau provider serupa) untuk mengirim pesan otomatis ke nomor WA pelanggan. Pengiriman dilakukan secara async melalui Laravel Queue dengan driver database, tanpa memerlukan Redis.

| **Event Trigger**           | **Penerima** | **Contoh Isi Pesan WA**                                                                                                                                                                                                         |
| --------------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Booking berhasil dibuat     | User         | Halo \[Nama\], booking lapangan \[Nama Lapangan\] pada \[Tanggal\] pukul \[Jam\] berhasil dibuat. Segera lakukan pembayaran sebesar Rp \[Total\] ke rekening \[Info Rekening\] sebelum \[Batas Waktu\]. Kode Booking: \[Kode\]. |
| Bukti bayar diterima        | User         | Halo \[Nama\], bukti pembayaran Anda untuk booking \[Kode\] sudah kami terima dan sedang dalam proses verifikasi. Mohon tunggu konfirmasi dari kami.                                                                            |
| Booking dikonfirmasi        | User         | Selamat! Booking \[Kode\] Anda telah DIKONFIRMASI. Lapangan \[Nama\], \[Tanggal\] pukul \[Jam\]. Sampai jumpa di lapangan!                                                                                                      |
| Pembayaran ditolak          | User         | Maaf, bukti pembayaran untuk booking \[Kode\] tidak dapat kami verifikasi. Alasan: \[Alasan\]. Silakan upload ulang bukti bayar yang valid sebelum \[Batas Waktu\].                                                             |
| Booking otomatis dibatalkan | User         | Booking \[Kode\] telah otomatis dibatalkan karena melebihi batas waktu pembayaran. Silakan buat booking baru jika masih ingin menyewa lapangan.                                                                                 |
| Booking dibatalkan admin    | User         | Booking \[Kode\] telah dibatalkan oleh admin. Alasan: \[Alasan\]. Mohon maaf atas ketidaknyamanannya.                                                                                                                           |

### **4.5.1 Setup Integrasi WhatsApp Gateway**

- Gunakan provider: Fonnte (fonnte.com) atau WA Gateway lain yang menyediakan REST API
- Konfigurasi di file .env: WA_API_URL, WA_API_TOKEN, WA_SENDER_NUMBER
- Pesan dikirim via HTTP POST ke endpoint API provider
- Jika pengiriman gagal, sistem akan mencoba ulang (retry) maksimal 3 kali
- Log pengiriman WA disimpan di tabel 'whatsapp_logs' untuk keperluan troubleshooting

## **4.6 Dashboard & Fitur Admin**

### **4.6.1 Halaman Dashboard Utama Admin**

- Kartu ringkasan (summary cards) yang menampilkan:
  - Total booking hari ini
  - Pendapatan hari ini (dari booking yang sudah dikonfirmasi)
  - Jumlah booking menunggu konfirmasi (perlu tindakan)
  - Total booking bulan ini
- Daftar 10 booking terbaru dengan status dan aksi cepat
- Kalender overview ketersediaan lapangan (opsional, tampilan sederhana)

### **4.6.2 Manajemen Booking (Admin)**

- Tabel semua booking dengan filter: status, lapangan, tanggal
- Klik booking untuk melihat detail lengkap: data user, foto bukti bayar, riwayat status
- Aksi dari halaman detail: Konfirmasi Pembayaran, Tolak Pembayaran, Batalkan Booking
- Tombol cepat 'Konfirmasi' dan 'Tolak' langsung dari tabel list

### **4.6.3 Manajemen User (Admin)**

- Tabel daftar user terdaftar: nama, email, nomor WA, tanggal daftar
- Admin dapat menonaktifkan user (user tidak bisa login jika dinonaktifkan)
- Admin tidak bisa mengubah password user, hanya menonaktifkan akun

## **4.7 Modul Keuangan Sederhana**

Modul keuangan dibuat sesederhana mungkin: hanya mencatat pemasukan dari booking yang sudah dikonfirmasi. Tidak ada jurnal akuntansi, tidak ada pengeluaran, tidak ada laporan neraca.

### **4.7.1 Pencatatan Pemasukan**

- Setiap booking yang berstatus 'Dikonfirmasi' secara otomatis tercatat sebagai pemasukan
- Data yang dicatat: tanggal transaksi, nama user, lapangan, durasi, total pembayaran
- Jika booking dikonfirmasi dibatalkan, catatan keuangan tidak otomatis terhapus (admin mencatat manual)

### **4.7.2 Halaman Laporan Keuangan**

| **Laporan**        | **Isi**                                                                     | **Filter Tersedia**      |
| ------------------ | --------------------------------------------------------------------------- | ------------------------ |
| Rekap Harian       | Daftar semua booking dikonfirmasi dalam satu hari, total pemasukan hari itu | Pilih tanggal            |
| Rekap Bulanan      | Daftar booking per bulan, total pemasukan per bulan                         | Pilih bulan & tahun      |
| Rekap Per Lapangan | Pemasukan berdasarkan masing-masing lapangan                                | Pilih lapangan & periode |

### **4.7.3 Fitur Export**

- Laporan bisa diexport ke format CSV/Excel sederhana
- Kolom export: No, Kode Booking, Tanggal, User, Lapangan, Jam, Durasi, Total
- Tidak ada laporan PDF untuk fase pertama (cukup Excel/CSV)

## **4.8 Halaman Riwayat Booking (User)**

- User dapat melihat semua histori booking miliknya
- Filter berdasarkan status: Semua, Aktif, Selesai, Dibatalkan
- Klik booking untuk lihat detail: lapangan, jadwal, bukti bayar yang diupload, status terkini
- Tombol 'Pesan Lagi' untuk langsung booking lapangan yang sama di waktu berbeda

# **5\. Desain Database**

## **5.1 Daftar Tabel**

| **Tabel**     | **Deskripsi**                                            |
| ------------- | -------------------------------------------------------- |
| users         | Data akun pengguna (user & admin)                        |
| fields        | Data lapangan futsal                                     |
| field_prices  | Harga lapangan (reguler & weekend)                       |
| field_photos  | Foto-foto lapangan                                       |
| bookings      | Data pemesanan lapangan                                  |
| payments      | Data upload bukti pembayaran                             |
| whatsapp_logs | Log pengiriman notifikasi WhatsApp                       |
| settings      | Konfigurasi sistem (info rekening, jam operasional, dll) |
| jobs          | Queue jobs Laravel (untuk kirim WA async)                |
| failed_jobs   | Queue jobs yang gagal                                    |

## **5.2 Detail Tabel Utama**

### **Tabel: users**

| **Kolom**  | **Tipe**             | **Constraint**     | **Keterangan**            |
| ---------- | -------------------- | ------------------ | ------------------------- |
| id         | BIGINT UNSIGNED      | PK, AUTO_INCREMENT | Primary key               |
| name       | VARCHAR(100)         | NOT NULL           | Nama lengkap              |
| email      | VARCHAR(150)         | UNIQUE, NOT NULL   | Untuk login               |
| phone      | VARCHAR(20)          | NOT NULL           | Nomor WA (format: 628xxx) |
| password   | VARCHAR(255)         | NOT NULL           | Hash bcrypt               |
| avatar     | VARCHAR(255)         | NULLABLE           | Path foto profil          |
| role       | ENUM('admin','user') | DEFAULT 'user'     | Hanya 2 role              |
| is_active  | TINYINT(1)           | DEFAULT 1          | 0 = nonaktif              |
| created_at | TIMESTAMP            | NOT NULL           | -                         |
| updated_at | TIMESTAMP            | NOT NULL           | -                         |

### **Tabel: fields (Lapangan)**

| **Kolom**               | **Tipe**                  | **Constraint**     | **Keterangan**                |
| ----------------------- | ------------------------- | ------------------ | ----------------------------- |
| id                      | BIGINT UNSIGNED           | PK, AUTO_INCREMENT | Primary key                   |
| name                    | VARCHAR(100)              | NOT NULL           | Misal: Lapangan A             |
| type                    | ENUM('indoor','outdoor')  | NOT NULL           | Tipe lapangan                 |
| description             | TEXT                      | NULLABLE           | Deskripsi singkat             |
| open_time               | TIME                      | NOT NULL           | Jam buka (misal 08:00)        |
| close_time              | TIME                      | NOT NULL           | Jam tutup (misal 24:00)       |
| status                  | ENUM('active','inactive') | DEFAULT 'active'   | Nonaktif = tidak bisa dipesan |
| created_at / updated_at | TIMESTAMP                 | NOT NULL           | -                             |

### **Tabel: field_prices (Harga)**

| **Kolom**               | **Tipe**                  | **Constraint**     | **Keterangan**             |
| ----------------------- | ------------------------- | ------------------ | -------------------------- |
| id                      | BIGINT UNSIGNED           | PK, AUTO_INCREMENT | Primary key                |
| field_id                | BIGINT UNSIGNED           | FK fields.id       | Lapangan terkait           |
| day_type                | ENUM('weekday','weekend') | NOT NULL           | Tipe hari                  |
| price_per_hour          | DECIMAL(10,2)             | NOT NULL           | Harga per jam dalam Rupiah |
| created_at / updated_at | TIMESTAMP                 | NOT NULL           | -                          |

### **Tabel: bookings**

| **Kolom**               | **Tipe**        | **Constraint**     | **Keterangan**                                                         |
| ----------------------- | --------------- | ------------------ | ---------------------------------------------------------------------- |
| id                      | BIGINT UNSIGNED | PK, AUTO_INCREMENT | Primary key                                                            |
| booking_code            | VARCHAR(20)     | UNIQUE, NOT NULL   | Format: BK-YYYYMMDD-XXXX                                               |
| user_id                 | BIGINT UNSIGNED | FK users.id        | User yang memesan                                                      |
| field_id                | BIGINT UNSIGNED | FK fields.id       | Lapangan yang dipesan                                                  |
| booking_date            | DATE            | NOT NULL           | Tanggal bermain                                                        |
| start_time              | TIME            | NOT NULL           | Jam mulai                                                              |
| end_time                | TIME            | NOT NULL           | Jam selesai                                                            |
| duration_hours          | INT             | NOT NULL           | Durasi (jam)                                                           |
| price_per_hour          | DECIMAL(10,2)   | NOT NULL           | Harga/jam saat booking dibuat (snapshot)                               |
| total_price             | DECIMAL(10,2)   | NOT NULL           | Total = durasi × harga/jam                                             |
| status                  | ENUM(...)       | NOT NULL           | pending_payment, pending_confirmation, confirmed, completed, cancelled |
| payment_deadline        | TIMESTAMP       | NOT NULL           | Batas waktu bayar (created_at + 2 jam)                                 |
| notes                   | TEXT            | NULLABLE           | Catatan dari user                                                      |
| cancelled_at            | TIMESTAMP       | NULLABLE           | Waktu pembatalan                                                       |
| cancel_reason           | TEXT            | NULLABLE           | Alasan pembatalan                                                      |
| created_at / updated_at | TIMESTAMP       | NOT NULL           | -                                                                      |

### **Tabel: payments**

| **Kolom**               | **Tipe**        | **Constraint**        | **Keterangan**                  |
| ----------------------- | --------------- | --------------------- | ------------------------------- |
| id                      | BIGINT UNSIGNED | PK, AUTO_INCREMENT    | Primary key                     |
| booking_id              | BIGINT UNSIGNED | FK bookings.id        | Booking terkait                 |
| proof_image             | VARCHAR(255)    | NOT NULL              | Path file bukti transfer        |
| status                  | ENUM(...)       | DEFAULT 'pending'     | pending, approved, rejected     |
| confirmed_by            | BIGINT UNSIGNED | NULLABLE, FK users.id | Admin yang memproses            |
| confirmed_at            | TIMESTAMP       | NULLABLE              | Waktu konfirmasi/tolak          |
| rejection_reason        | TEXT            | NULLABLE              | Alasan penolakan (jika ditolak) |
| created_at / updated_at | TIMESTAMP       | NOT NULL              | -                               |

### **Tabel: settings**

| **key (contoh)**       | **Contoh Value**                | **Keterangan**                               |
| ---------------------- | ------------------------------- | -------------------------------------------- |
| app_name               | Futsal Arena                    | Nama venue ditampilkan di halaman            |
| bank_accounts          | \[{bank:BCA,no:123,name:John}\] | JSON array info rekening (bisa lebih dari 1) |
| booking_duration_limit | 2                               | Batas waktu bayar dalam jam                  |
| max_booking_hours      | 4                               | Maksimal durasi booking per transaksi        |
| wa_api_token           | xxxxx                           | Token API WA Gateway                         |
| wa_sender_number       | 6281234567890                   | Nomor WA pengirim notifikasi                 |

# **6\. Panduan Desain UI (Tailwind CSS + Blade)**

## **6.1 Identitas Visual**

| **Elemen**         | **Nilai**               | **Tailwind Class**           |
| ------------------ | ----------------------- | ---------------------------- |
| Warna Primer       | #1D4ED8 - Biru          | bg-blue-700, text-blue-700   |
| Warna Sukses       | #15803D - Hijau         | bg-green-700, text-green-700 |
| Warna Warning      | #B45309 - Coklat kuning | bg-amber-700, text-amber-700 |
| Warna Danger       | #B91C1C - Merah         | bg-red-700, text-red-700     |
| Background Halaman | #F8FAFC                 | bg-slate-50                  |
| Font Utama         | Inter (Google Fonts)    | font-sans                    |
| Border Radius Card | 8px                     | rounded-lg                   |
| Shadow Card        | Sedang                  | shadow-md                    |

## **6.2 Komponen UI yang Digunakan (Blade Components)**

- x-app-layout - Layout utama dengan navbar dan sidebar
- x-card - Kartu dengan padding dan shadow standar
- x-button - Tombol dengan varian: primary, success, danger, secondary
- x-badge - Badge status booking dengan warna sesuai status
- x-modal - Modal konfirmasi untuk aksi penting (batalkan, tolak, dll)
- x-alert - Alert sukses/error/warning setelah form submit
- x-table - Tabel data responsif dengan pagination

## **6.3 Peta Halaman (Sitemap)**

### **Halaman Publik (Tanpa Login)**

- / - Beranda: daftar lapangan, info harga, cara pesan
- /lapangan/{id} - Detail lapangan: foto, info, kalender ketersediaan
- /login - Halaman login (Breeze)
- /register - Halaman registrasi (Breeze)
- /forgot-password - Lupa password (Breeze)

### **Halaman User (Setelah Login sebagai User)**

- /dashboard - Dashboard user: booking aktif, ringkasan
- /booking/create/{field_id} - Proses booking (pilih jadwal & konfirmasi)
- /booking/{id}/payment - Halaman upload bukti bayar
- /booking/{id} - Detail booking
- /bookings - Riwayat semua booking
- /profile - Edit profil

### **Halaman Admin (Setelah Login sebagai Admin)**

- /admin/dashboard - Dashboard admin: KPI & booking terbaru
- /admin/bookings - Daftar semua booking + filter
- /admin/bookings/{id} - Detail booking + konfirmasi/tolak pembayaran
- /admin/fields - Daftar lapangan
- /admin/fields/create - Tambah lapangan baru
- /admin/fields/{id}/edit - Edit lapangan
- /admin/users - Daftar pengguna
- /admin/finance - Laporan keuangan sederhana
- /admin/settings - Pengaturan sistem (rekening, konfigurasi WA)

# **7\. Persyaratan Non-Fungsional**

| **Aspek**       | **Persyaratan**                        | **Implementasi**                                            |
| --------------- | -------------------------------------- | ----------------------------------------------------------- |
| Performa        | Halaman load < 3 detik pada koneksi 4G | Eager loading Eloquent, pagination, compress foto lapangan  |
| Keamanan        | Proteksi dari CSRF, XSS, SQL Injection | Bawaan Laravel: CSRF token, Blade auto-escape, Eloquent ORM |
| Keamanan Login  | Lockout setelah 5x gagal               | Bawaan Laravel Breeze (RateLimiter)                         |
| File Upload     | Validasi MIME type & ukuran max 5MB    | Laravel validation: mimes:jpg,png,pdf\|max:5120             |
| Responsif       | Tampil baik di mobile & desktop        | Tailwind CSS responsive classes (sm:, md:, lg:)             |
| Skalabilitas    | Mendukung hingga 50 concurrent users   | Cukup dengan VPS 2 core / 2GB RAM                           |
| Maintainability | Kode bersih, pakai PSR-12              | Laravel conventions, Service class, Form Request            |
| Backup          | Backup database harian                 | Cronjob mysqldump atau plugin backup hosting                |

# **8\. User Stories**

## **8.1 User (Pelanggan)**

| **ID** | **User Story**                                                                             | **Acceptance Criteria**                                                                   |
| ------ | ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------- |
| US-01  | Sebagai user baru, saya ingin mendaftar akun agar bisa melakukan booking.                  | Form register berhasil, akun tersimpan dengan role user, bisa langsung login.             |
| US-02  | Sebagai user, saya ingin melihat lapangan yang tersedia beserta harganya.                  | Halaman listing lapangan tampil dengan foto, nama, tipe, harga reguler & weekend.         |
| US-03  | Sebagai user, saya ingin melihat slot jam yang sudah dipesan agar tidak salah pilih waktu. | Kalender/grid jam menampilkan slot terpesan (abu-abu) dan tersedia (hijau) secara akurat. |
| US-04  | Sebagai user, saya ingin membuat booking lapangan di tanggal dan jam yang saya pilih.      | Booking tersimpan, kode booking digenerate, notif WA terkirim dengan info pembayaran.     |
| US-05  | Sebagai user, saya ingin mengupload bukti transfer agar admin bisa mengkonfirmasi.         | File terupload, status berubah ke 'Menunggu Konfirmasi', admin mendapat notifikasi.       |
| US-06  | Sebagai user, saya ingin melihat riwayat booking saya.                                     | Halaman riwayat menampilkan semua booking dengan status terkini dan detail.               |
| US-07  | Sebagai user, saya ingin membatalkan booking yang belum saya bayar.                        | Booking dengan status 'Menunggu Pembayaran' berhasil dibatalkan, slot kembali tersedia.   |

## **8.2 Admin**

| **ID** | **User Story**                                                                | **Acceptance Criteria**                                                                                                         |
| ------ | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| US-10  | Sebagai admin, saya ingin menambah lapangan baru beserta harga dan jadwalnya. | Lapangan tersimpan, foto terupload, langsung muncul di halaman listing (jika aktif).                                            |
| US-11  | Sebagai admin, saya ingin melihat daftar booking yang perlu dikonfirmasi.     | Dashboard menampilkan kartu jumlah 'Menunggu Konfirmasi' dan daftar booking tersebut.                                           |
| US-12  | Sebagai admin, saya ingin mengkonfirmasi atau menolak pembayaran user.        | Admin klik konfirmasi → status jadi 'Dikonfirmasi' → WA terkirim ke user. Tolak → status kembali dan WA terkirim dengan alasan. |
| US-13  | Sebagai admin, saya ingin melihat rekap pemasukan hari ini dan bulan ini.     | Halaman keuangan menampilkan total pemasukan harian/bulanan dan daftar transaksi.                                               |
| US-14  | Sebagai admin, saya ingin mengekspor data keuangan ke Excel.                  | File CSV/Excel berhasil diunduh dengan data sesuai filter yang dipilih.                                                         |
| US-15  | Sebagai admin, saya ingin mengatur info rekening bank di sistem.              | Admin bisa tambah/edit rekening di halaman Pengaturan, langsung tampil di halaman bayar user.                                   |

# **9\. Rencana Pengembangan**

| **Sprint** | **Durasi** | **Fokus**           | **Deliverable**                                                                                                           |
| ---------- | ---------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Sprint 1   | 1,5 Minggu | Setup & Auth        | Instalasi Laravel 12 + Breeze (Tailwind), konfigurasi MySQL, halaman register/login/profil, middleware role admin vs user |
| Sprint 2   | 1,5 Minggu | Manajemen Lapangan  | CRUD lapangan (nama, tipe, foto, harga reguler & weekend, jadwal), halaman listing lapangan publik                        |
| Sprint 3   | 2 Minggu   | Sistem Booking      | Kalender ketersediaan, proses booking 4 langkah, manajemen status booking, auto-cancel dengan scheduler                   |
| Sprint 4   | 1,5 Minggu | Pembayaran & WA     | Upload bukti bayar, dashboard konfirmasi admin, integrasi WhatsApp Gateway (Fonnte), queue jobs via database              |
| Sprint 5   | 1 Minggu   | Keuangan & Laporan  | Halaman laporan keuangan harian/bulanan, export CSV/Excel, pengaturan rekening bank                                       |
| Sprint 6   | 1,5 Minggu | Testing & Finishing | Bug fixing, optimasi query, UI polish, testing end-to-end, seeder data dummy, dokumentasi deployment                      |

**Total Estimasi:** 9 Minggu (~2,5 Bulan) | Target Go-Live: ~3 bulan dari kick-off (termasuk buffer)

# **10\. Risiko & Mitigasi**

| **Risiko**                                                                | **Dampak** | **Mitigasi**                                                                                    |
| ------------------------------------------------------------------------- | ---------- | ----------------------------------------------------------------------------------------------- |
| Double booking (race condition saat dua user memilih slot sama bersamaan) | Tinggi     | Gunakan database transaction + SELECT FOR UPDATE saat proses booking, validasi ulang di server  |
| WA Gateway bermasalah / nomor diblokir                                    | Sedang     | Log semua notifikasi, admin bisa kirim WA manual jika diperlukan, tampilkan status di dashboard |
| File bukti bayar yang diupload rusak/tidak terbaca                        | Sedang     | Validasi format & ukuran file, preview sebelum upload, admin bisa minta upload ulang            |
| Queue jobs gagal (WA tidak terkirim)                                      | Sedang     | Implementasi failed_jobs table, admin bisa trigger ulang dari dashboard (manual retry)          |
| Lupa konfigurasi scheduler (auto-cancel tidak jalan)                      | Sedang     | Dokumentasi jelas cara setup cron di server, test sebelum go-live                               |

# **11\. Lampiran**

## **11.1 Referensi**

- Laravel 12 Official Docs: <https://laravel.com/docs/12.x>
- Laravel Breeze: <https://laravel.com/docs/12.x/starter-kits#breeze-and-blade>
- Tailwind CSS: <https://tailwindcss.com/docs>
- Fonnte WA Gateway: <https://fonnte.com/docs>
- Alpine.js: <https://alpinejs.dev>

## **11.2 Konfigurasi .env Penting**

DB_CONNECTION=mysql

DB_DATABASE=futsal_booking

QUEUE_CONNECTION=database # tanpa Redis

CACHE_DRIVER=file # tanpa Redis

WA_API_URL=<https://api.fonnte.com/send>

WA_API_TOKEN=your_token_here

WA_SENDER_NUMBER=628xxxxxxxxxx

## **11.3 Perintah Setup Awal**

composer create-project laravel/laravel futsal-booking

composer require laravel/breeze --dev

php artisan breeze:install blade # pilih Blade + Tailwind

npm install && npm run build

php artisan migrate --seed

php artisan queue:work # jalankan queue (WA notif)

\# Tambahkan ke crontab untuk scheduler:

\* \* \* \* \* php /path/to/artisan schedule:run

_- Akhir Dokumen PRD v1.0 - Sistem Booking Futsal -_
