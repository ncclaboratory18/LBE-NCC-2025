# Pertemuan 2 LBE NCC 2025

## Daftar Isi

- [Network Service Azure](#network-service-azure)
- [Membangun Infrastruktur Azure Sederhana](#membangun-infrastruktur-azure-sederhana)
  - [Konfigurasi Jaringan](#konfigurasi-jaringan)
- [Deploy Aplikasi](#deploy-aplikasi)

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
apt-get install -y nginx postgresql-client-16

systemctl enable nginx
systemctl start nginx
```

### Konfigurasi Jaringan

Kita perlu membuat sebuah subnet agar Virtual Machine (VM) dan database cloud berada dalam jaringan yang sama, sehingga keduanya bisa saling terhubung dengan aman dan efisien. Dengan subnet ini, komunikasi antara VM dan database lebih cepat, terlindungi dari akses luar, dan memudahkan pengelolaan koneksi jaringan tanpa mengganggu layanan lain di Azure. Berikut langkah-langkahnya:

1. Klik `Virtual Networks`, lalu pilih virtual network yang digunakan oleh vm utama.
2. Pilih menu `Subnets`, lalu tekan `+ Subnet`
3. Biarkan semua konfigurasi pada pengaturan Default, lalu pada dropdown `Subnet Delegation` dan pilih `Microsoft.DBforPostgreSQL/flexibleServers`
4. Tekan tombol save

### Membuat database berbasis cloud

Berikut adalah langkah langkah untuk membuat database berbasis cloud terutama untuk postgres:

1. Klik `Azure Database for PostgreSQL flexible servers`, lalu pilih `Create`
2. Isi nama server, region (posisi data center VM, pastikan sama dengan region VM), versi PostgreSQL, dan workload type (pilih Development untuk keperluan belajar).</br>

<img width="1079" height="700" alt="Screenshot 2025-09-08 191527" src="https://github.com/user-attachments/assets/f542ac28-ea7c-40dd-b39a-b354e8e42cdc" />

3. Selanjutnya untuk autentikasi dapat menggunakan PostgreSQL authentication only (menggunakan username dan password PostgreSQL), Microsoft Entra authentication only (menggunakan identitas Microsoft Entra (Azure AD)) ataupun keduanya, dalam contoh ini akan menggunakan `PostgreSQL authentication only`.

<img width="716" height="401" alt="Screenshot 2025-09-08 191547" src="https://github.com/user-attachments/assets/c1ccb85c-1129-4eb5-beae-97a7a7260fbc" />

4. Pilih `Next : Networking >`

## Deploy Aplikasi
