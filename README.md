# TP9DPBO2025C1

Saya A Bintang iftitah FJ dengan NIM 2305995 Mengerjakan tugas praktikum 9 dengan sesuai spesfikasi yang telah ditentukan. Untuk keberkahannya maka saya tidak melakukan kecurangan apapun Aamminn.

# Penjelasan Sistem CRUD Mahasiswa dengan Arsitektur MVP

## Struktur Kelas dan File

### **Model (Directory: /model)**

- **DB.class.php**  
  Kelas dasar untuk koneksi database.  
  Menangani operasi dasar seperti `open()`, `execute()`, `getResult()`, dan `close()`.

- **Mahasiswa.class.php**  
  Kelas entitas untuk merepresentasikan data mahasiswa.  
  Memiliki atribut seperti `id`, `nim`, `nama`, `tempat`, `tl`, `gender`, `email`, dan `telp`.  
  Menyediakan getter dan setter untuk setiap atribut.

- **TabelMahasiswa.class.php**  
  Kelas untuk operasi CRUD pada tabel mahasiswa.  
  Meng-extend kelas DB dan menyediakan method seperti `getMahasiswa()`, `getMahasiswaById()`, `addMahasiswa()`, `updateMahasiswa()`, dan `deleteMahasiswa()`.

- **Template.class.php**  
  Kelas untuk mekanisme templating.  
  Digunakan untuk merender template HTML dan mengganti placeholder dengan data dinamis.

### **Presenter (Directory: /presenter)**

- **KontrakPresenter.php**  
  Interface yang mendefinisikan kontrak untuk presenter.  
  Menetapkan method apa saja yang harus diimplementasi oleh presenter.

- **ProsesMahasiswa.php**  
  Kelas yang mengimplementasi KontrakPresenter.  
  Berfungsi sebagai perantara antara model dan view.  
  Menyediakan method untuk memproses data dari model ke view.

### **View (Directory: /view)**

- **KontrakView.php**  
  Interface yang mendefinisikan kontrak untuk view.  
  Menetapkan method apa saja yang harus diimplementasi oleh view.

- **TampilMahasiswa.php**  
  Kelas utama untuk tampilan yang mengimplementasi KontrakView.  
  Menangani rendering berbagai tampilan seperti daftar, detail, form tambah, dan form edit.

#### **crud/ (Subdirectory untuk komponen view CRUD)**

- **TabelMahasiswaView.php**  
  Kelas untuk menampilkan tabel data mahasiswa.  
  Menyediakan method `getTabel()` dan `getFullTable()`.

- **FormTambahMahasiswa.php**  
  Kelas untuk menampilkan form penambahan mahasiswa.  
  Menyediakan method `getForm()`.

- **FormEditMahasiswa.php**  
  Kelas untuk menampilkan form pengeditan mahasiswa.  
  Menyediakan method `getForm()` yang menerima data mahasiswa.

- **DetailMahasiswa.php**  
  Kelas untuk menampilkan detail mahasiswa.  
  Menyediakan method `getDetail()` yang menerima data mahasiswa.

- **Notification.php**  
  Kelas untuk menampilkan notifikasi (sukses/error).  
  Menyediakan method `getNotification()`.

### **File Lainnya**

- **index.php**  
  File entry point aplikasi.  
  Menangani routing dan pemilihan aksi berdasarkan parameter URL.

- **skin.html**  
  Template HTML utama untuk seluruh aplikasi.  
  Berisi placeholder yang akan diganti dengan konten dinamis.

- **mvp_php.sql**  
  File SQL untuk struktur database dan data contoh.

## Alur Kerja Aplikasi (Flow)

### 1. Alur Umum

- **Inisialisasi**: User mengakses `index.php`.
- **Routing**: `index.php` menentukan tindakan berdasarkan parameter URL.
- **Rendering**: `TampilMahasiswa` memanggil method yang sesuai.
- **Pemrosesan Data**: `ProsesMahasiswa` memproses data dari database.
- **Tampilan**: Template dirender dengan data yang telah diproses.
- 

### 2. Alur Operasi CRUD

#### Tampilkan Data (Read - List)


1. `index.php` memanggil `$tampil->tampil()`.
2. `TampilMahasiswa.tampil()` memanggil `$this->prosesmahasiswa->prosesDataMahasiswa()`.
3. `ProsesMahasiswa` membuka koneksi dan menjalankan query melalui `TabelMahasiswa`.
4. Data diproses dan disimpan dalam array `$mahasiswaData`.
5. `TabelMahasiswaView.getFullTable()` digunakan untuk merender tabel.
6. Hasilnya ditampilkan menggunakan Template.

#### Tambah Data (Create)

1. User mengklik "Tambah Data" dan diarahkan ke `index.php?tambah=1`.
2. `index.php` memanggil `$tampil->tampilTambah()`.
3. `FormTambahMahasiswa.getForm()` digunakan untuk menampilkan form.
4. User mengisi form dan mengirimkannya ke `index.php?tambah_submit=1`.
5. `index.php` mengekstrak data form dan memanggil `ProsesMahasiswa.addDataMahasiswa()`.
6. `TabelMahasiswa.addMahasiswa()` dijalankan untuk menyimpan data ke database.
7. User diredirect ke homepage dengan notifikasi sukses/error.

#### Lihat Detail (Read - Detail)

1. User mengklik tombol "Detail" dan diarahkan ke `index.php?detail=ID`.
2. `index.php` memanggil `$tampil->tampilDetail(ID)`.
3. `TampilMahasiswa` memanggil `$this->prosesmahasiswa->prosesMahasiswaById(ID)`.
4. Data mahasiswa diambil dari database dan disimpan dalam variabel `$mahasiswaData`.
5. `DetailMahasiswa.getDetail()` digunakan untuk merender halaman detail.
6. Hasilnya ditampilkan menggunakan Template.

#### Edit Data (Update)

1. User mengklik tombol "Edit" dan diarahkan ke `index.php?edit=ID`.
2. `index.php` memanggil `$tampil->tampilEdit(ID)`.
3. `TampilMahasiswa` memanggil `$this->prosesmahasiswa->prosesMahasiswaById(ID)`.
4. `FormEditMahasiswa.getForm()` digunakan untuk menampilkan form dengan data yang sudah ada.
5. User mengubah data dan mengirimkannya ke `index.php?edit_submit=ID`.
6. `index.php` mengekstrak data form dan memanggil `ProsesMahasiswa.updateDataMahasiswa()`.
7. `TabelMahasiswa.updateMahasiswa()` dijalankan untuk memperbarui data di database.
8. User diredirect ke homepage dengan notifikasi sukses/error.

#### Hapus Data (Delete)

1. User mengklik tombol "Hapus" dan diminta konfirmasi.
2. Jika dikonfirmasi, user diarahkan ke `index.php?hapus=ID`.
3. `index.php` memanggil `ProsesMahasiswa.deleteDataMahasiswa(ID)`.
4. `TabelMahasiswa.deleteMahasiswa()` dijalankan untuk menghapus data dari database.
5. User diredirect ke homepage dengan notifikasi sukses/error.

## Mekanisme Templating

Sistem menggunakan mekanisme templating sederhana dengan template `skin.html` yang berisi placeholder seperti:

- `PAGE_TITLE`: Judul halaman
- `FORM_CONTENT`: Konten utama halaman
- `DATA_TABEL`: Data dalam bentuk tabel (jika diperlukan)
- `TAMBAH_BUTTON`: Tombol untuk menambah data baru

Kelas `Template` mengganti placeholder ini dengan konten dinamis menggunakan method `replace()`, dan akhirnya merender halaman dengan method `write()`.

## Kesimpulan

Aplikasi ini mengimplementasikan pola arsitektur **MVP (Model-View-Presenter)** untuk memisahkan logika bisnis, presentasi data, dan antarmuka pengguna. Aliran data dan kontrol terstruktur dengan baik sehingga memudahkan pemeliharaan dan pengembangan lebih lanjut.
