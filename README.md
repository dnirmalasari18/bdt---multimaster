# ETS Basis Data Terdistribusi
05111640000115 Dewi Ayu Nirmalasari

## 1. Desain dan Implementasi Infrasturktur
### Gambar Infrastruktur
![infrastruktur](infrastruktur.png)

### Server
Server yang digunakan sebanyak 5 buah, dengan perincian sebagai berikut:
#### a. Server Database
Sebanyak 3 buah. Ketiganya memiliki spesifikasi yang sama, yaitu:
- Menggunakan MySQL server
- OS : bento/ubuntu-16.04
- RAM: 512 MB

#### b. Load Balancer
Sebanyak 1 buah, dengan spesifikasi:
- Proxy MySQL
- OS : bento/ubuntu-16.04
- RAM: 512 MB

#### c. Web Server Apache
Sebanyak 1 buah.