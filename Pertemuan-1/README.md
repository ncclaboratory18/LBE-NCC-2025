# Pertemuan 1 LBE NCC 2024

## Daftar Isi
+ [Dasar-dasar Cloud Computing](#dasar-dasar-cloud-computing)
  + [Pengertian Cloud](#pengertian-cloud)
  + [Perbandingan Dulu vs Sekarang](#perbandingan-dulu-vs-sekarang)
  + [Model Layanan](#model-layanan)
  + [Model Penyebaran](#model-penyebaran)
  + [Keuntungan dan Tantangan](#keuntungan-dan-tantangan)
+ [Pengenalan Microsoft Azure](#pengenalan-microsoft-azure)
  + [Edge Locations dan Infrastruktur Azure](#edge-locations-dan-infrastruktur-azure)
    + [Region (Lokasi Fisik Data Center)](#region-lokasi-fisik-data-center)
    + [Availability Zone (AZ)](#availability-zone-az)
    + [Edge Location](#edge-location)

## Dasar-dasar Cloud Computing
### Pengertian Cloud
Cloud merupakan simbol atau perumpamaan dari internet, sedangkan computing berarti pemrosesan menggunakan komputer. Jadi, cloud computing adalah penggunaan sumber daya komputasi (seperti server, storage, network, dsb.) melalui internet yang dilakukan secara on-demand (sesuai kebutuhan) dan tagihannya dibayarkan dengan harga sesuai pemakaian (pay-as-you-go).

Penerapan yang sepenuhnya menggunakan teknologi cloud. Terdapat 2 opsi dalam membangun aplikasi cloud-based:
  - Low-level infrastructure: Kita masih memerlukan staf IT untuk mengelola server.
  - Higher-level service: Dengan layanan serverless yang mampu mengurangi kebutuhan pengelolaan, arsitektur, dan scaling (penyesuaian kapasitas) pada infrastruktur. **serverless** disini memiliki arti bahwa server masih digunakan tetapi sepenuhnya dikelola oleh penyedia cloud, sehingga client hanya fokus pada penulisan dan menjalankan kode, tanpa perlu khawatir tentang infrastruktur di belakangnya.

### Perbandingan Dulu vs Sekarang
| **Aspek** | **Dulu (Sebelum Cloud Computing)** | **Sekarang (Dengan Cloud Computing)** |
|-----------|-------------------------------------|---------------------------------------|
| **Infrastruktur** | Harus membangun data center sendiri | Tidak perlu data center fisik, cukup sewa layanan cloud |
| **Biaya Awal** | Tinggi: pembelian server, rak komputer, sistem penyimpanan, instalasi jaringan | Rendah: bayar sesuai pemakaian (*pay as you go*) |
| **Operasional** | Harus membayar listrik, pendingin, kebersihan, keamanan, dan sewa gedung | Semua dikelola oleh penyedia cloud (Microsoft Azure, AWS, dsb.) |
| **Skalabilitas** | Sulit menambah kapasitas, harus beli perangkat keras baru | Skalabilitas mudah dan cepat, tinggal tambah resource di portal cloud |
| **Waktu Implementasi** | Lama, perlu perencanaan, instalasi, dan konfigurasi manual | Cepat, bisa deploy aplikasi dalam hitungan menit |
| **Fleksibilitas** | Terbatas pada kapasitas fisik yang dimiliki | Sangat fleksibel, bisa menyesuaikan kebutuhan kapan saja |
| **Keamanan** | Ditanggung penuh oleh perusahaan (biaya tambahan) | Keamanan tingkat enterprise disediakan penyedia cloud |

### Model Layanan
Secara umum model layanan pada sistem cloud terbagi menjadi:
![shared-responsibility](https://github.com/user-attachments/assets/787360d5-ff57-41ba-a550-d571d611242e)
- On-Premise : Terkadang disebut sebagai private cloud, user bertanggung jawab untuk memanajemen seluruh resource yang ada, seperti virtualisasi server, perawatan hardware, cabling, penyusunan rak, update software, dan data center sendiri.
- IaaS : User tidak perlu memanajemen infrastruktur (termasuk hardware), karena telah disediakan cloud provider. Contohnya adalah ketika menyewa virtual machine pada cloud provider. Kita tidak perlu membeli hardware sendiri secara fisik, tetapi kita dapat memilih OS yang ingin digunakan maupun aplikasi yang ingin diinstall pada VM tersebut.
- PaaS (Platform as a Service): Penyedia layanan mengelola infrastruktur, sistem operasi, dan middleware. user hanya perlu mengelola aplikasi dan data. 
- SaaS: Penyedia layanan mengelola semuanya, mulai dari infrastruktur hingga aplikasi. User hanya perlu menggunakan aplikasi dan mengelola datanya.

### Model Penyebaran
Secara umum model penyebaran pada sistem cloud terbagi menjadi:
- Cloud Hybrid : Jenis komputasi cloud yang menggabungkan infrastruktur lokal atau cloud private dengan cloud public. Cloud hybrid memungkinkan data dan aplikasi berpindah di antara dua lingkungan.
- Cloud Public : Sumber daya cloud (seperti server dan penyimpanan) dimiliki dan dioperasikan oleh penyedia cloud pihak ketiga dan dikirim melalui internet. Dengan cloud publid, semua perangkat keras, perangkat lunak, dan infrastruktur pendukung lainnya dimiliki dan dikelola oleh penyedia cloud.
- Cloud Private : Sumber daya komputasi cloud yang digunakan eksklusif oleh satu organisasi, baik di pusat data internal maupun dihosting pihak ketiga, dengan layanan dan infrastruktur tetap berada di jaringan privat.

### Keuntungan dan Tantangan
Keuntungan Cloud Computing:
- Efisiensi Biaya
- Skalabilitas
- Fleksibilitas & Aksesibilitas
- Kecepatan Implementasi
- Keamanan Tingkat Lanjut
- Inovasi Cepat

Tantangan Cloud Computing:
- Ketergantungan pada Internet
- Kontrol Terbatas
- Keamanan & Privasi Data
- Biaya Tidak Terduga
- Kebutuhan Regulasi
- Vendor Lock In

## Pengenalan Microsoft Azure
Microsoft Azure adalah platform komputasi cloud. Azure menawarkan berbagai layanan cloud, termasuk komputasi, analitik, penyimpanan, jaringan, dan AI. Pelanggan dapat memilih dari berbagai layanan ini untuk mengembangkan dan menskalakan aplikasi baru, atau menjalankan aplikasi yang ada, di cloud. Azure membantu bisnis mengelola dan menyebarkan aplikasi secara global dengan mudah dan fleksibel.

### Edge Locations dan Infrastruktur Azure
#### Region (Lokasi Fisik Data Center)
Sebuah region Azure terdiri dari satu atau lebih pusat data yang terhubung melalui jaringan berkapasitas tinggi, tahan terhadap gangguan, dan berlatensi rendah. Pusat data Azure umumnya berlokasi di dalam area metropolitan besar.
Faktor Utama Memilih Region:
- Latensi: Pilih region yang secara geografis dekat dengan pengguna untuk mengurangi latensi. Sebagai contoh, jika pengguna berada di Amerika Serikat, sebaiknya pilih region di Amerika Serikat atau Kanada.
- Availability Zones: Pilih region yang mendukung availability zone untuk menyediakan redundansi dan isolasi gangguan. Pastikan Anda menyebarkan sumber daya di beberapa availability zone dalam region tersebut.
- Residensi Data: Pastikan region yang dipilih berada dalam batasan residensi data yang disyaratkan oleh organisasi Anda.

#### Availability Zone (AZ)
Availability zone adalah sekumpulan pusat data independen yang memiliki daya, pendingin, dan koneksi jaringan yang terisolasi. Availability zone secara fisik terletak cukup berdekatan untuk menyediakan jaringan dengan latensi rendah, tetapi juga cukup berjauhan untuk memberikan isolasi gangguan dari hal-hal seperti badai atau pemadaman listrik lokal.
<img width="907" height="540" alt="cross-region-replication" src="https://github.com/user-attachments/assets/3bd17a98-6796-4e2b-a6ac-b03623ac3e76" />

#### Edge Location
Edge Location adalah lokasi jaringan terdistribusi yang berfungsi sebagai titik akhir untuk mempercepat pengiriman konten kepada pengguna. Edge location biasanya digunakan oleh layanan Content Delivery Network (CDN) untuk menyimpan salinan sementara (cache) dari data atau aplikasi. Secara fisik, edge location ditempatkan dekat dengan wilayah pengguna akhir sehingga mampu mengurangi latensi akses, meningkatkan kecepatan respons, dan mengurangi beban pada pusat data utama (region).
<img width="1200" height="675" alt="microsoft-global-wan" src="https://github.com/user-attachments/assets/63cf498a-acd9-40ac-ad49-6adfdc391ebd" />
