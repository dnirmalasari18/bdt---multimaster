# ETS Basis Data Terdistribusi
05111640000115 Dewi Ayu Nirmalasari

## Daftar Isi
- [1. Desain dan Implementasi Infrastruktur](https://github.com/dnirmalasari18/bdt-multimaster#1-desain-dan-implementasi-infrastruktur)
- [2. Penggunaan Basis Data Terdistribusi dalam Aplikasi](https://github.com/dnirmalasari18/bdt-multimaster#2-penggunaan-basis-data-terdistribusi-dalam-aplikasi)
- [3. Simulasi Fail Over]
## 1. Desain dan Implementasi Infrasturktur
### A. Desain Infrastrukur Basis Data Terdistribusi
 ![infrastruktur](infrastruktur.PNG)

  ### Server
  Server yang digunakan sebanyak 5 buah, dengan perincian sebagai berikut:
#### a. Server Database
Sebanyak 3 buah. Ketiganya memiliki spesifikasi yang sama, yaitu:
- Menggunakan ```MySQL server```
- OS : ```bento/ubuntu-16.04```
- RAM: 512 MB

#### b. Load Balancer
Sebanyak 1 buah, dengan spesifikasi:
- ```Proxy MySQL```
- OS : ```bento/ubuntu-16.04```
- RAM: 512 MB

#### c. Web Server Apache
Sebanyak 1 buah.

### B. Implementasi Infrastruktur Basis Data Terdistribusi
#### a. Proses Instalasi
Aplikasi yang perlu diinstall sebelumnya:
- Vagrant(versi 2.2.5)
- Virtual Box(versi 6.0.12)

#### b. Tahapan konfigurasi
Langkah-langkah yang dilakukan dalam tahapan konfigurasi dijelaskan sebagai berikut.
- Membuat ```VagrantFile```<br>
  Vagrantfile dibuat dengan mengetikkan ```vagrant init``` pada command line. Setelahnya, vagrantfile akan terbuat pada direktori dimana command tersebut dijalankan.
- Mengubah Vagrantfile sesuai dengan rancangan Infrastruktur yang ada pada gambar[!infrastruktur] di atas.
## 2. Penggunaan Basis Data Terdistribusi dalam Aplikasi
### LPencerdas
LPencerdas merupakan aplikasi berbasis website milik Laboratorium Pemrograman 1 Informatika ITS yang dibuat menggunakan framework Laravel. LPencerdas sendiri dibangun dengan tujuan agar nantinya wadah internal Informatika ITS membagikan materi dalam bentuk teks, gambar, atau video seputar pelajaran Informatika yang disesuaikan dengan kurikulum Departemen Informatika ITS.
![lpencerdas_1](screenshoot/lpencerdas_1.png)

### Implementasi Basis Data Terdistribusi dalam LPencerdas
Berikut langkah-langkah mengimplementasikan basis data terdistribusi dalam LPencerdas.
1. Instalasi LPencerdas
Instalasi LPencerdas diawali dengan meng-_clone_ repo LPencerdas dari akun github lpif. Langkah-langkah yang dilakukan antara lain dengan memasukkan inputan di bawah ini ke dalam _command line_
```
git clone https://github.com/lpif/lpencerdas.git
composer install
composer dump-autoload
cp .env.example .env
php artisan key:generate
```

2. Mengubah file .env
3. Menjalankan laravel migration dan seeding
4. Menjalankan aplikasi
Jalankan ```php artisan serve``` pada aplikasi


## 3. Simulasi Fail Over
