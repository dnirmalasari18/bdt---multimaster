# ETS Basis Data Terdistribusi
05111640000115 Dewi Ayu Nirmalasari

## Daftar Isi
- [1. Desain dan Implementasi Infrastruktur](https://github.com/dnirmalasari18/bdt-multimaster#1-desain-dan-implementasi-infrastruktur)
- [2. Penggunaan Basis Data Terdistribusi dalam Aplikasi](https://github.com/dnirmalasari18/bdt-multimaster#2-penggunaan-basis-data-terdistribusi-dalam-aplikasi)
- [3. Simulasi Fail Over](https://github.com/dnirmalasari18/bdt-multimaster#3-simulasi-fail-over)
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
- Mengubah Vagrantfile sesuai dengan rancangan Infrastruktur yang sudah dibuat.
  ```ruby
  # -*- mode: ruby -*-
  # vi: set ft=ruby :

  # All Vagrant configuration is done below. The "2" in Vagrant.configure
  # configures the configuration version (we support older styles for
  # backwards compatibility). Please don't change it unless you know what
  # you're doing.

  Vagrant.configure("2") do |config|
    
    # MySQL Cluster dengan 3 node
    (1..3).each do |i|
      config.vm.define "db#{i}" do |node|
        node.vm.hostname = "db#{i}"
        node.vm.box = "bento/ubuntu-16.04"
        node.vm.network "private_network", ip: "192.168.16.11#{i+5}"

        # Opsional. Edit sesuai dengan nama network adapter di komputer
        #node.vm.network "public_network", bridge: "Qualcomm Atheros QCA9377 Wireless Network Adapter"
        
        node.vm.provider "virtualbox" do |vb|
          vb.name = "db#{i}"
          vb.gui = false
          vb.memory = "512"
        end
    
        node.vm.provision "shell", path: "deployMySQL#{i}.sh", privileged: false
      end
    end

    config.vm.define "proxy" do |proxy|
      proxy.vm.hostname = "proxy"
      proxy.vm.box = "bento/ubuntu-16.04"
      proxy.vm.network "private_network", ip: "192.168.16.115"
      #proxy.vm.network "public_network",  bridge: "Qualcomm Atheros QCA9377 Wireless Network Adapter"
      
      proxy.vm.provider "virtualbox" do |vb|
        vb.name = "proxy"
        vb.gui = false
        vb.memory = "512"
      end

      proxy.vm.provision "shell", path: "deployProxySQL.sh", privileged: false
    end

  end
  ```
- Membuat Script Provisioning
  - Provisioning untuk Proxy SQL
  - Provisioning untuk DB1
  - Provisioning untuk DB2
  - Provisioning untuk DB3

- Membuat File Konfigurasi SQL
  - File Konfigurasi DB1
  - File Konfigurasi DB2
  - File Konfigurasi DB3

- Membuat File Script SQL Pendukung
  - File ```addition_to_sys.sql```
  - File ```cluster_bootstrap.sql```
  - File ```cluster_member.sql```
  - File ```create_proxysql_user.sql```
  - File ```proxysql.sql```

- Menjalankan Vagrant
  - Melakukan ```Vagrant Up```
  - Mengecek apakah vagrant sudah berjalan dengan baik

- Melakukan konfigurasi tambahan untuk proxysql.sql
  - Masuk ke Virtual Machine ProxySql
  - Masukkan 

## 2. Penggunaan Basis Data Terdistribusi dalam Aplikasi
### LPencerdas
LPencerdas merupakan aplikasi berbasis website milik Laboratorium Pemrograman 1 Informatika ITS yang dibuat menggunakan framework Laravel. LPencerdas sendiri dibangun dengan tujuan agar nantinya wadah internal Informatika ITS membagikan materi dalam bentuk teks, gambar, atau video seputar pelajaran Informatika yang disesuaikan dengan kurikulum Departemen Informatika ITS.
![lpencerdas_1](screenshoot/lpencerdas_1.png)

### Implementasi Basis Data Terdistribusi dalam LPencerdas
Berikut langkah-langkah mengimplementasikan basis data terdistribusi dalam LPencerdas.
1. Instalasi LPencerdas
Instalasi LPencerdas diawali dengan meng-_clone_ repo LPencerdas dari akun github lpif. Langkah-langkah yang dilakukan antara lain dengan memasukkan inputan di bawah ini ke dalam _command line_.
```
git clone https://github.com/lpif/lpencerdas.git
composer install
composer dump-autoload
cp .env.example .env
```

2. Mengubah file .env
Ubah konfigurasi awal yang sudah ada pada file ```.env``` menjadi:
```s
DB_CONNECTION=mysql
DB_HOST=192.168.16.115
DB_PORT=6033
DB_DATABASE=lpencerdas
DB_USERNAME=user
DB_PASSWORD=password
```
- `DB_HOST` disesuaikan dengan IP dari ProxySQL
- `DB_PORT` disesuaikan dengan port dari ProxySQL
- `DB_DATABASE` disesuaikan dengan database yang dibutuhkan oleh aplikasi
- `DB_USERNAME` dan `DB_PASSWORD` disesuaikan dengan user dan password yang dibuat pada file [`create_proxysql_user.sql`](https://github.com/dnirmalasari18/bdt-multimaster/blob/master/config/create_proxysql_user.sql)

3. Melakukan _generating key_
_Generating key_ dilakukan dengan menjalankan `php artisan key:generate` pada _command line_. 

4. Menjalankan _Laravel Migration_ dan _Seeding_
_Laravel Migration_ dan _Seeding_ merupakan fitur dari laravel yang mampu membantu _developer_ untuk membangun _database_ yang mencakup command `_CREATE_`, `_DROP_`, serta `_INSERT_`. Langkah-langkah yang dilakukan antara lain dengan memasukkan inputan di bawah ini ke dalam _command line_.
```
php artisan migrate
php artisan db:seed
```

5. Menjalankan aplikasi
Jalankan `php artisan serve` pada _command line_ untuk menjalankan aplikasi


## 3. Simulasi Fail Over
1. Mematikan Salah Satu Server Basis Data
2. Melakukan Pengubahan Data pada Aplikasi
3. Mengaktifkan Kembali Server yang Dimatikan dan Verifikasi Hasil Replikasi