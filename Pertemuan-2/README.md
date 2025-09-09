# Pertemuan 2 LBE NCC 2025

## Daftar Isi

- [Network Service Azure](#network-service-azure)
- [Membangun Infrastruktur Azure Sederhana](#membangun-infrastruktur-azure-sederhana)
  - [Firewall](#firewall)
  - [Konfigurasi Jaringan](#konfigurasi-jaringan)
  - [Membuat database berbasis cloud](#membuat-database-berbasis-cloud)
- [Deploy Aplikasi](#deploy-aplikasi)
  - [Konfigurasi Skema dan Tabel Database](#konfigurasi-skema-dan-tabel-database)
  - [Port Forwarding pada NGINX](#port-forwarding-pada-nginx)
  - [Menjalankan Aplikasi sebagai Layanan dengan Systemd](#menjalankan-aplikasi-sebagai-layanan-dengan-systemd)

## Network Service Azure

### Fondasi Jaringan

#### Azure Virtual Network (VNet)

Azure Virtual Network (VNet) adalah jaringan privat di Azure yang memungkinkan komunikasi antar resource, antar VNet, akses ke internet, serta koneksi ke jaringan on-premises melalui VPN Gateway atau ExpressRoute. VNet juga mendukung enkripsi lalu lintas antar resource untuk meningkatkan keamanan.

#### Azure Private Link

Azure Private Link memungkinkan akses aman ke layanan Azure PaaS atau layanan mitra melalui private endpoint di Virtual Network, dengan lalu lintas tetap di jaringan backbone Microsoft tanpa harus terbuka ke internet publik.

#### Azure DNS

Azure DNS adalah layanan hosting dan resolusi DNS berbasis infrastruktur Microsoft Azure. Layanannya mencakup Azure Public DNS untuk mengelola domain publik, Azure Private DNS untuk resolusi nama dalam virtual network tanpa DNS khusus, dan Azure DNS Private Resolver untuk memungkinkan query dua arah antara private zone Azure dan lingkungan on-premises. Dengan Azure DNS, pengguna dapat mengelola domain publik, resolusi DNS internal, serta integrasi nama antara Azure dan jaringan lokal.

#### Azure Bastion

Azure Bastion adalah layanan PaaS untuk koneksi aman ke Virtual Machine via browser Azure Portal atau SSH/RDP lokal, tanpa memerlukan Public IP maupun software tambahan, dengan konektivitas TLS dan opsi SKU/tiers sesuai kebutuhan.

#### Azure Route Server

Azure Route Server menyederhanakan routing dinamis antara Network Virtual Appliance (NVA) dan Virtual Network dengan bertukar informasi rute melalui protokol BGP secara langsung, sehingga tidak perlu konfigurasi atau pemeliharaan manual pada route table.

#### NAT Gateway

Azure NAT Gateway menyederhanakan koneksi keluar-only ke internet dari Virtual Network dengan menggunakan IP publik statis yang ditentukan, tanpa perlu load balancer atau Public IP langsung pada Virtual Machine.

#### Traffic Manager

### Load balancing and content delivery

#### Load Balancer

#### Application Gateway

#### Azure Front Door

### Hybrid connectivity

#### VPN Gateway

#### ExpressRoute

#### Virtual WAN

### Network Security

#### Firewall Manager

#### Azure Firewall

#### Web Application Firewall

#### DDoS Protection

### Network Management and monitoring

#### Azure Network Watcher

#### Azure Virtual Network Manager

#### Container network observability

## Membangun Infrastruktur Azure Sederhana

Buat Virtual Machine dengan langkah-langkah yang sama seperti pada modul sebelumnya. Tambahkan konfigurasi pada menu `Advanced` di bagian `Custom data and cloud-init` dengan skrip berikut:

```
#!/bin/bash
apt-get update -y
apt-get install -y nginx postgresql-client-16 nodejs npm

systemctl enable nginx
systemctl start nginx
```

### Firewall
Saat membuat VM, secara default hanya port 22 (SSH) yang diizinkan untuk koneksi masuk. Agar aplikasi web dapat diakses publik, kita perlu membuka port 80 (HTTP) dan 443 (HTTPS). Ikuti langkah-langkah berikut:
1. Masuk ke dashboard virtual machine yang sudah dibuat
2. Pilih menu `Networking` -> `Network Settings`
3. Klik `Create Port Rule` -> `Inbound Port Rule`

<img width="1594" height="390" alt="image" src="https://github.com/user-attachments/assets/e61f29ed-759f-4f23-9aa8-48a3226934d2" />

4. Ubah opsi `Service` menjadi HTTP, lalu isi kolom `Name` dengan http.

<img width="571" height="756" alt="image" src="https://github.com/user-attachments/assets/b88ccd92-8f79-4711-bba2-05c9118e52b1" />

5. Klik `Add` untuk menyimpan aturan firewall.

### Konfigurasi Jaringan

Kita perlu membuat sebuah subnet agar Virtual Machine (VM) dan database cloud berada dalam jaringan yang sama, sehingga keduanya bisa saling terhubung dengan aman dan efisien. Dengan subnet ini, komunikasi antara VM dan database lebih cepat, terlindungi dari akses luar, dan memudahkan pengelolaan koneksi jaringan tanpa mengganggu layanan lain di Azure. Berikut langkah-langkahnya:

1. Klik `Virtual Networks`, lalu pilih virtual network yang digunakan oleh vm utama.
2. Pilih menu `Subnets`, lalu tekan `+ Subnet`
3. Biarkan semua konfigurasi pada pengaturan Default, lalu pada dropdown `Subnet Delegation` dan pilih `Microsoft.DBforPostgreSQL/flexibleServers`
   <img width="818" height="85" alt="Screenshot 2025-09-08 201249" src="https://github.com/user-attachments/assets/cba4f8b6-0dcd-4e68-84f3-46b83f814295" />

4. Tekan tombol save

### Membuat database berbasis cloud

Berikut adalah langkah langkah untuk membuat database berbasis cloud terutama untuk postgres:

1. Klik `Azure Database for PostgreSQL flexible servers`, lalu pilih `Create`
2. Isi nama server, region (posisi data center VM, pastikan sama dengan region VM), versi PostgreSQL, dan workload type (pilih Development untuk keperluan belajar).</br>

<img width="1079" height="700" alt="Screenshot 2025-09-08 191527" src="https://github.com/user-attachments/assets/f542ac28-ea7c-40dd-b39a-b354e8e42cdc" />

3. Selanjutnya untuk autentikasi dapat menggunakan PostgreSQL authentication only (menggunakan username dan password PostgreSQL), Microsoft Entra authentication only (menggunakan identitas Microsoft Entra (Azure AD)) ataupun keduanya, dalam contoh ini akan menggunakan `PostgreSQL authentication only`.

<img width="716" height="401" alt="Screenshot 2025-09-08 191547" src="https://github.com/user-attachments/assets/c1ccb85c-1129-4eb5-beae-97a7a7260fbc" />

4. Pilih `Next : Networking >`
5. Pilih `Private access (VNet Integration)` sebagai metode koneksi, lalu tentukan `Subnet` yang telah dibuat pada `Virtual Network`.

<img width="709" height="477" alt="Screenshot 2025-09-08 203833" src="https://github.com/user-attachments/assets/6c7581cb-ac40-4807-9fa9-e78739d112d2" />

6. Pilih `Review + Create` dan tekan tombol `Create`

## Deploy Aplikasi
Setelah berhasil membangun infrastruktur di Azure, kita akan mencoba melakukan deploy aplikasi. Aplikasi yang akan dideploy merupakan aplikasi CRUD sederhana dan dapat diclone dari `https://github.com/arizki787/API-Project`.

### Konfigurasi Skema dan Tabel Database

Pada tahap ini kita akan menyiapkan skema dan tabel-tabel yang diperlukan untuk aplikasi. Ikuti langkah-langkah berikut:

1. Hubungkan VM utama ke database cloud untuk mengeksekusi perintah SQL dengan menggunakan perintah berikut:

```
psql -h <server-name>.postgres.database.azure.com -p 5432 -U <username> <database-name>
```
<img width="771" height="227" alt="Screenshot 2025-09-08 214856" src="https://github.com/user-attachments/assets/47bb1efd-2c7d-4b14-bff9-dd1072e9e49b" />


2. Setelah berhasil terhubung, jalankan perintah SQL berikut untuk membuat database dan tabel:

```
CREATE DATABASE school;

\c school;

CREATE TABLE student (
    sid VARCHAR(20),
    sname VARCHAR(100),
    uktstatus VARCHAR(15),
    alamat VARCHAR(100),
    email VARCHAR(100)
);
```

### Port Forwarding pada NGINX
Sebenarnya, aplikasi di atas sudah bisa dijalankan dengan command `npm start`, maka server akan mendengar pada port 3000. Namun, pada praktiknya, tidak lazim kita membuka port secara custom. Biasanya port yang dibuka untuk akses api ada pada port 80 (http) atau 443 (https). Hal tersebut sesuai dengan aturan security group pada instance webServer yang hanya menerima koneksi ke port 22 (ssh) dan 80 (http). Oleh karena itu, kita perlu melakukan port forwarding dari port 80 ke port 3000 di mana aplikasi kita berjalan.
1. Buat salinan konfigurasi server NGINX dengan command
```
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/backup
```
2. Modifikasi file `/etc/nginx/sites-available/default` dengan menggunakan nano menjadi sebagai berikut:
```
server {
    listen 80;

    location / {
        proxy_set_header   X-Forwarded-For $remote_addr;
        proxy_set_header   Host $http_host;
        proxy_pass         "http://127.0.0.1:3000";
    }
}
```
3. Aktifkan konfigurasi dengan membuat symbolic link di folder `/etc/nginx/sites-enabled` dengan command berikut.
```sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/```
4. Restart Nginx dan jalankan aplikasi
```
sudo systemctl restart nginx
node ./index.js
```

### Menjalankan Aplikasi sebagai Layanan dengan Systemd
Sebelumnya, kita perlu secara manual menjalankan aplikasi dengan command `node index.js` atau `npm start`. Namun, kita tidak mungkin dapat menjalankan server setiap saat. Oleh karena itu, kita perlu menjalankan aplikasi di latar belakang sebagai suatu layanan. Dengan demikian, aplikasi pada server dapat tetap berjalan dan dapat diakses kapanpun. Hal ini dapat dilakukan dengan mudah di linux dengan menggunakan `systemd`.
1. Buat systemd file dengan `sudo nano /lib/systemd/system/server.service`, lalu isi sebagai berikut. Jangan lupa untuk mengubah `username` dan `path-to-project` sesuai dengan server masing-masing.
```R
[Unit]
Description=Server service
After=network.target

[Service]
Type=simple
User=<username>
ExecStart=/usr/bin/node <path-to-project>
Restart=on-failure
WorkingDirectory=<path-to-project>

[Install]
WantedBy=multi-user.target
```
2. Restart daemon dan jalankan service server. Periksa juga apakah layanan server telah berjalan.
```
sudo systemctl daemon-reload
sudo systemctl start server
sudo systemctl status server
```
3. Akses kembali public ip. Kalian akan mendapati bahwa server berjalan walaupun kalian tidak mengeksekusi npm start pada root project. Dengan demikian, project kalian sudah berjalan pada background.  
