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

## Deploy Aplikasi
