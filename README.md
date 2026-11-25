# Sistem Kasir Toko (Point of Sale)

Aplikasi kasir berbasis web dengan **PHP native** dan **MySQL/MariaDB** yang diakses lewat **PDO**. Proyek Pemrograman Web, Studi Kasus 2.

## Daftar isi
- [Fitur](#fitur)
- [Kebutuhan](#kebutuhan)
- [Cara menjalankan](#cara-menjalankan)
- [Struktur folder](#struktur-folder)
- [Berkontribusi](#berkontribusi)
- [Catatan](#catatan)

## Fitur
- **Login admin**: `password_hash` / `password_verify`, sesi aman, logout otomatis setelah 30 menit tanpa aktivitas.
- **Kasir**: pilih produk, atur jumlah, bayar, lalu cetak struk. Pesanan dapat dihapus per item atau dikosongkan, dan semuanya bisa **diurungkan**. Keranjang tersimpan di browser sehingga tidak hilang saat halaman dimuat ulang.
- **Transaction**: header, detail, dan pengurangan stok disimpan dalam satu transaction. Jika stok kurang, seluruh proses di-rollback.
- **CRUD produk** dengan pencarian dan pagination.
- **Riwayat transaksi** (filter tanggal, pagination) dan struk siap cetak.
- **Laporan harian** dengan `GROUP BY`: jumlah transaksi, barang terjual, pendapatan, produk terlaris, dan grafik.
- **Mode terang/gelap**: tombol bulan/matahari di sidebar (di HP ada di bilah atas). Pilihan tersimpan di browser dan mengikuti pengaturan sistem jika belum dipilih.

## Kebutuhan
- PHP 8.1 atau lebih baru dengan ekstensi `pdo_mysql` dan `mbstring` (sudah aktif di XAMPP).
- MySQL 8.0.16+ atau MariaDB 10.2+.

## Cara menjalankan

### Dengan XAMPP
1. Salin folder proyek ke `C:\xampp\htdocs\pos-kasir`.
2. Jalankan **Apache** dan **MySQL** di XAMPP Control Panel.
3. Buka `http://localhost/phpmyadmin`, pilih **Import**, lalu unggah `database/kasir_db.sql`.
4. Jika perlu, sesuaikan pengguna, dan kata sandi database di `config/database.php`.
5. Buka `http://localhost/pos-kasir/` dan masuk dengan akun contoh di bawah.

### Tanpa XAMPP
```bash
mysql -u root -p < database/kasir_db.sql
php -S localhost:8000
```
Jalankan `php -S` dari dalam folder proyek, lalu buka `http://localhost:8000`.

### Akun contoh
| Pengguna | Kata sandi |
|---|---|
| `admin` | `admin123` |

Ganti kata sandi ini sebelum dipakai sungguhan. Buat hash baru dengan `php -r "echo password_hash('PasswordBaru', PASSWORD_DEFAULT);"`, lalu jalankan `UPDATE admin SET password_hash = '...'`.

### Pintasan di halaman kasir
Tekan `/` untuk mencari produk. Ketik kode produk lalu `Enter` untuk langsung menambahkannya (cocok untuk pemindai barcode).

## Struktur folder
```
pos-kasir/
├── config/        database.php (koneksi PDO), app.php (nama toko, konstanta)
├── includes/      bootstrap, helpers (XSS, CSRF, pagination), auth, layout, transaksi_service
├── produk/        index (cari + pagination), form (tambah/ubah), hapus
├── transaksi/     baru (kasir), simpan, struk, riwayat
├── laporan/       harian
├── assets/        css/app.css, js/app.js, js/kasir.js
├── database/      kasir_db.sql (struktur + data contoh)
├── docs/          ERD.md
└── index.php  login.php  logout.php
```

## Berkontribusi

Panduan ini untuk anggota kelompok atau siapa pun yang ingin ikut mengubah proyek lewat GitHub. Pilih jalur sesuai situasi Anda:

| Situasi | Jalur |
|---|---|
| Anda diundang sebagai collaborator | Bagian **A** |
| Anda belum diundang | Bagian **B** (fork) lalu **C** (menjaga fork tetap terbaru) |


Agar `main` tetap aman, minta semua orang bekerja di branch masing-masing dan mengirim *pull request*.

### A. Kontributor yang sudah diundang
1. **Salin repo ke komputer (sekali saja)**
   ```bash
   git clone https://github.com/Mr-DSP/pos-kasir.git
   cd pos-kasir
   ```
2. **Siapkan lokal**: ikuti bagian [Cara menjalankan](#cara-menjalankan) (impor database dan sesuaikan `config/database.php` untuk komputer Anda).
3. **Ambil perubahan terbaru, lalu buat branch** (satu branch untuk satu tujuan)
   ```bash
   git checkout main
   git pull origin main
   git checkout -b fitur/nama-singkat-fitur
   ```
4. **Ubah kode dan uji** sampai semua halaman terbuka tanpa error.
5. **Commit**
   ```bash
   git status
   git add .
   git commit -m "tambah: filter produk berdasarkan stok"
   ```
6. **Kirim branch ke GitHub**
   ```bash
   git push -u origin fitur/nama-singkat-fitur
   ```
   Jika diminta password, gunakan *Personal Access Token* (GitHub → Settings → Developer settings), atau pakai GitHub Desktop.
7. **Buka Pull Request**: klik **Compare & pull request** di halaman repo, tulis ringkasan, lalu **Create pull request**.
8. **Merge dan perbarui lokal**: setelah pemilik repo menekan **Merge pull request**, jalankan
   ```bash
   git checkout main
   git pull origin main
   ```

### B. Belum diundang: gunakan fork
1. Klik **Fork** di pojok kanan atas halaman repo untuk membuat salinan di akun Anda.
2. Salin fork Anda ke komputer:
   ```bash
   git clone https://github.com/USERNAME-ANDA/pos-kasir.git
   cd pos-kasir
   ```
3. Lanjutkan dengan langkah 3 sampai 6 pada bagian B (buat branch, ubah, commit, push ke fork Anda).
4. Di GitHub, klik **Contribute → Open pull request** untuk mengirim perubahan ke repo asli.

### C. Menjaga fork tetap up to date
Repo asli terus berubah karena kontributor lain. Fork Anda tidak ikut berubah secara otomatis, jadi sinkronkan sebelum mulai bekerja dan sebelum membuka pull request.

**Cara 1: lewat website GitHub (paling mudah)**
1. Buka fork Anda di GitHub (`github.com/USERNAME-ANDA/pos-kasir`).
2. Klik tombol **Sync fork**, lalu **Update branch**.
3. Di komputer, tarik hasilnya:
   ```bash
   git checkout main
   git pull origin main
   ```

**Cara 2: lewat terminal**

Daftarkan repo asli sebagai `upstream` (cukup sekali):
```bash
git remote add upstream https://github.com/Mr-DSP/pos-kasir.git
git remote -v
```
Hasil `git remote -v` harus menampilkan dua pasang alamat: `origin` (fork Anda) dan `upstream` (repo asli).

Lalu setiap kali ingin sinkron:
```bash
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```
Perintah `git pull upstream main` melakukan hal yang sama dalam satu langkah (fetch lalu merge).

**Memperbarui branch fitur yang sedang dikerjakan**
```bash
git checkout fitur/nama-singkat-fitur
git merge main
```
Jika muncul konflik, selesaikan dengan langkah di bagian E.

**Tips**
- Jangan mengubah kode langsung di `main` milik fork. Anggap `main` sebagai cermin repo asli, dan kerjakan semuanya di branch.
- Jika `git merge upstream/main` menolak karena Anda sempat mengubah `main`, simpan perubahan itu ke branch baru (`git checkout -b cadangan`), lalu kembalikan `main` dengan `git checkout main` dan `git reset --hard upstream/main`. Perintah `reset --hard` menghapus perubahan lokal di `main`, jadi pastikan sudah dicadangkan.

### D. Mengatasi konflik
Jika saat `git pull` atau `git merge` muncul pesan *conflict*:
1. Buka file yang ditandai. Cari tanda `<<<<<<<`, `=======`, dan `>>>>>>>`, pilih atau gabungkan isi yang benar, lalu hapus ketiga tanda itu.
2. Jalankan `git add nama-file`, lalu `git commit`.
3. Jalankan `git push` seperti biasa.

### Ringkasan perintah harian
| Tujuan | Perintah |
|---|---|
| Ambil perubahan terbaru (repo sendiri) | `git pull origin main` |
| Ambil perubahan dari repo asli (fork) | `git fetch upstream` lalu `git merge upstream/main` |
| Buat branch baru | `git checkout -b fitur/nama-singkat-fitur` |
| Lihat perubahan | `git status` |
| Simpan perubahan | `git add .` lalu `git commit -m "pesan"` |
| Kirim ke GitHub | `git push -u origin nama-branch` |

### Aturan bersama
- Tulis pesan commit singkat dan jelas dengan awalan seperti `tambah:`, `perbaiki:`, `ubah:`, atau `dokumentasi:`.
- Jangan commit password database, file `.env`, atau data pribadi. `.gitignore` sudah menyaring sebagian, tetapi tetap periksa dengan `git status` sebelum commit.
- Jangan langsung push ke `main`. Gunakan branch dan pull request.
- Jalankan `git pull origin main` (atau sinkronkan fork) sebelum mulai bekerja.

## Catatan
- Folder `config/`, `includes/`, dan `database/` dilindungi `.htaccess` (`Require all denied`) agar, tidak bisa dibuka langsung dari browser di Apache.
- Diagram relasi database ada di `docs/ERD.md`.
