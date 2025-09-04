# Pertemuan 1 LBE NCC 2025

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
  + [Fitur Utama Microsoft Azure](#fitur-utama-microsoft-azure)
    + [Portal Azure](#portal-azure)
    + [Azure CLI](#azure-cli)
    + [Azure Resource Manager (ARM)](#azure-resource-manager-arm)
    + [Azure Marketplace](#azure-marketplace)
+ [Layanan Utama Microsoft Azure](#layanan-utama-microsoft-azure)
    + [App Hosting and Compute](#app-hosting-and-compute)
    + [Azure AI services](#azure-ai-services)
    + [Data](#data)
    + [Storage](#storage)
    + [Messaging](#messaging)
    + [Identity and Security](#identity-and-security)
    + [Management](#management)

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
- Availability Zones: Pilih region yang mendukung availability zone untuk menyediakan redundansi dan isolasi gangguan. Pastikan pengguna menyebarkan sumber daya di beberapa availability zone dalam region tersebut.
- Residensi Data: Pastikan region yang dipilih berada dalam batasan residensi data yang disyaratkan oleh organisasi pengguna.

#### Availability Zone (AZ)
Availability zone adalah sekumpulan pusat data independen yang memiliki daya, pendingin, dan koneksi jaringan yang terisolasi. Availability zone secara fisik terletak cukup berdekatan untuk menyediakan jaringan dengan latensi rendah, tetapi juga cukup berjauhan untuk memberikan isolasi gangguan dari hal-hal seperti badai atau pemadaman listrik lokal.
<img width="907" height="540" alt="cross-region-replication" src="https://github.com/user-attachments/assets/3bd17a98-6796-4e2b-a6ac-b03623ac3e76" />

#### Edge Location
Edge Location adalah lokasi jaringan terdistribusi yang berfungsi sebagai titik akhir untuk mempercepat pengiriman konten kepada pengguna. Edge location biasanya digunakan oleh layanan Content Delivery Network (CDN) untuk menyimpan salinan sementara (cache) dari data atau aplikasi. Secara fisik, edge location ditempatkan dekat dengan wilayah pengguna akhir sehingga mampu mengurangi latensi akses, meningkatkan kecepatan respons, dan mengurangi beban pada pusat data utama (region).
<img width="1200" height="675" alt="microsoft-global-wan" src="https://github.com/user-attachments/assets/63cf498a-acd9-40ac-ad49-6adfdc391ebd" />

### Fitur Utama Microsoft Azure
#### Portal Azure
Portal Azure adalah konsol berbasis web untuk membuat, mengelola, dan memantau semua sumber daya Azure melalui antarmuka grafis. Melalui portal ini, pengguna dapat mengatur database, menambah daya komputasi, memantau biaya, hingga membangun aplikasi dari sederhana hingga kompleks.

#### Azure CLI
Azure Command-Line Interface (CLI) adalah alat lintas platform berbasis terminal untuk mengelola sumber daya Azure. CLI dapat digunakan secara interaktif melalui shell (cmd, Bash, atau lainnya) maupun otomatis dengan menyusun perintah ke dalam skrip untuk menjalankan tugas berulang.

#### Azure Resource Manager (ARM)
Azure Resource Manager (ARM) adalah layanan deployment dan manajemen di Azure yang memungkinkan pengguna membuat, memperbarui, dan menghapus resource. ARM juga menyediakan fitur seperti access control, locks, dan tags untuk mengamankan serta mengorganisasi resource setelah deployment.

#### Azure Marketplace
Azure Marketplace adalah toko online yang menyediakan solusi berbasis Azure untuk profesional TI dan pengembang. Melalui portal Azure atau situs web, pengguna dapat menemukan produk dan layanan, seperti konsultasi serta layanan terkelola yang dikategorikan berdasarkan area seperti analitik, jaringan, keamanan, dan database, sehingga memudahkan pencarian solusi sesuai kebutuhan organisasi.

## Layanan Utama Microsoft Azure
### App Hosting and Compute
| **Layanan** | **Deskripsi** |
|-------------|---------------|
| Azure App Service | Host aplikasi web dan API .NET, Java, Node.js, dan Python menggunakan layanan Azure yang sepenuhnya dikelola. Pengguna hanya perlu men-deploy kode ke Azure, sementara Azure mengurus semua manajemen infrastruktur seperti high availability, load balancing, dan autoscaling. |
| Azure Static Web Apps | Host aplikasi web statis yang dibangun menggunakan framework seperti Gatsby, Hugo, atau VuePress, atau aplikasi web modern menggunakan Angular, React, Svelte, atau Vue. Aplikasi web statis ini secara otomatis membangun dan men-deploy berdasarkan perubahan kode, serta memiliki integrasi API dengan Azure Functions. |
| Azure Container Apps | Azure Container Apps memungkinkan pengguna menjalankan aplikasi berbasis container tanpa perlu mengkhawatirkan orkestrasi atau infrastruktur, melalui platform serverless. |
| Azure Container Instances | Jalankan Docker container sesuai permintaan dalam lingkungan Azure yang dikelola dan serverless. Solusi ini cocok untuk skenario yang dapat berjalan di container terisolasi tanpa memerlukan orkestrasi. |
| Azure Kubernetes Services | Cepat deploy cluster Kubernetes siap produksi ke cloud dan serahkan beban operasional ke Azure. Azure menangani tugas penting seperti monitoring kesehatan dan pemeliharaan, sementara pengguna hanya perlu mengelola agent nodes. |
| Azure Virtual Machines | Hosting aplikasi menggunakan virtual machine di Azure ketika membutuhkan kontrol lebih besar atas lingkungan komputasi. Azure VMs menyediakan lingkungan komputasi yang fleksibel dan skalabel untuk Linux maupun Windows. |
| Azure Functions | Platform komputasi serverless untuk membuat potongan kode kecil yang dapat dipicu oleh berbagai jenis event. Contoh penggunaan umum: membangun API serverless atau mengorkestrasi arsitektur berbasis event. |
| Azure Spring Apps | Hosting aplikasi Spring Boot microservice di Azure tanpa perlu perubahan kode. Menyediakan fitur seperti monitoring, manajemen konfigurasi, service discovery, integrasi CI/CD, dan lainnya. |

### Azure AI services
| **Layanan** | **Deskripsi** |
|-------------|---------------|
| Azure OpenAI | Gunakan model bahasa canggih seperti GPT-3, Codex, dan Embeddings untuk generasi konten, ringkasan, pencarian semantik, dan terjemahan bahasa alami ke kode. |
| Azure AI Speech | Mengubah ucapan menjadi teks yang dapat dibaca dan dicari, atau mengubah teks menjadi suara yang terdengar alami untuk antarmuka yang lebih interaktif. |
| Azure AI Language | Memanfaatkan Natural Language Processing (NLP) untuk mengidentifikasi frasa penting dan melakukan analisis sentimen dari teks. |
| Azure AI Translator | Menerjemahkan lebih dari 100 bahasa dan dialek. |
| Azure AI Vision | Menganalisis konten dalam gambar dan video, seperti objek, wajah, atau teks. |
| Azure AI Search | Layanan pencarian informasi skala besar untuk aplikasi pencarian tradisional maupun percakapan, dengan opsi keamanan, AI enrichment, dan vektorisasi. |
| Azure AI Document Intelligence | Layanan ekstraksi dokumen yang memahami form dan struktur dokumen, memungkinkan ekstraksi teks dan struktur secara cepat. |

### Data
| **Layanan** | **Deskripsi** |
|-------------|---------------|
| Azure SQL | Keluarga produk engine database SQL Server yang tersedia di cloud. |
| Azure SQL Database | Versi SQL Server berbasis cloud yang sepenuhnya dikelola oleh Azure. |
| Azure Cosmos DB | Layanan database PostgreSQL berbasis cloud yang sepenuhnya dikelola, menggunakan PostgreSQL Community Edition. |
| Azure Database for PostgreSQL | Layanan database PostgreSQL berbasis cloud yang sepenuhnya dikelola, menggunakan PostgreSQL Community Edition. |
| Azure Database for MySQL | Layanan database MySQL berbasis cloud yang sepenuhnya dikelola, menggunakan MySQL Community Edition. |
| Azure Database for MariaDB | Layanan database MariaDB berbasis cloud yang sepenuhnya dikelola, menggunakan MariaDB Community Edition. |
| Azure Cache for Redis | Cache data dan messaging broker yang aman, menyediakan akses data berlatensi rendah dan throughput tinggi untuk aplikasi. |

### Storage
| **Layanan** | **Deskripsi** |
|-------------|---------------|
| Azure Blob Storage | Memungkinkan aplikasi pengguna menyimpan dan mengambil file di cloud. Layanan ini sangat skalabel untuk menyimpan data dalam jumlah besar, dan data disimpan secara redundan untuk memastikan high availability. |
| Azure Data Lake Storage | Mendukung analitik big data dengan menyediakan penyimpanan yang skalabel dan hemat biaya untuk data terstruktur, semi-terstruktur, maupun tidak terstruktur. |

### Messaging
| **Layanan** | **Deskripsi** |
|-------------|---------------|
| Azure Service Bus | Layanan message broker enterprise yang sepenuhnya dikelola, mendukung integrasi point-to-point dan publish-subscribe. Cocok untuk membangun aplikasi yang terdekomposisi, queue-based load leveling, atau memfasilitasi komunikasi antar microservices. |
| Azure Event Hubs | Layanan yang sepenuhnya dikelola untuk menerima dan memproses aliran data besar dari website, aplikasi, atau perangkat. |
| Azure Queue Storage | Layanan queue sederhana dan andal yang dapat menangani beban kerja besar untuk antrean pesan. |

### Identity and Security
| **Layanan** | **Deskripsi** |
|-------------|---------------|
| Microsoft Entra ID | Mengelola identitas pengguna dan mengontrol akses ke aplikasi, data, dan sumber daya pengguna. |
| Azure Key Vault | Menyimpan dan mengakses rahasia aplikasi seperti connection string dan API key dalam vault terenkripsi dengan akses terbatas, sehingga rahasia dan aplikasi pengguna tetap aman. |
| App Configuration | Layanan cepat dan skalabel untuk mengelola pengaturan aplikasi dan feature flags secara terpusat. |

### Management
| **Layanan** | **Deskripsi** |
|-------------|---------------|
| Azure Monitor | Solusi monitoring komprehensif untuk mengumpulkan, menganalisis, dan merespons data pemantauan dari lingkungan cloud maupun on-premises pengguna. |
| Application Insights | itur dari Azure Monitor yang menyediakan Application Performance Management (APM) untuk meningkatkan performa, keandalan, dan kualitas aplikasi web yang sedang berjalan. |
