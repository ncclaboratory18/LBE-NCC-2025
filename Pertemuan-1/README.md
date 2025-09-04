# Pertemuan 1 LBE NCC 2024
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
