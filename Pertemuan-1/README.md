# Pertemuan 1 LBE NCC 2025

## Daftar Isi

- [Dasar-dasar Cloud Computing](#dasar-dasar-cloud-computing)
  - [Pengertian Cloud](#pengertian-cloud)
  - [Perbandingan Dulu vs Sekarang](#perbandingan-dulu-vs-sekarang)
  - [Model Layanan](#model-layanan)
  - [Model Penyebaran](#model-penyebaran)
  - [Keuntungan dan Tantangan](#keuntungan-dan-tantangan)
- [Pengenalan Microsoft Azure](#pengenalan-microsoft-azure)
  - [Edge Locations dan Infrastruktur Azure](#edge-locations-dan-infrastruktur-azure)
  - [Fitur Utama Microsoft Azure](#fitur-utama-microsoft-azure)
- [Layanan Utama Microsoft Azure](#layanan-utama-microsoft-azure)
  - [App Hosting and Compute](#app-hosting-and-compute)
  - [Azure AI services](#azure-ai-services)
  - [Data](#data)
  - [Storage](#storage)
  - [Messaging](#messaging)
  - [Identity and Security](#identity-and-security)
  - [Management](#management)
- [Hosting Aplikasi di Azure](#hosting-aplikasi-di-azure)
  - [Opsi Hosting di Azure](#opsi-hosting-di-azure)
  - [Model Hosting di Azure](#model-hosting-di-azure)  
- [Membuat Virtual Machine di Azure](#membuat-virtual-machine-di-azure)
  - [Persiapan](#persiapan)
  - [Langkah-langkah Membuat VM](#langkah-langkah-membuat-vm)
  - [Mengakses Virtual Machine](#mengakses-virtual-machine)

## Dasar-dasar Cloud Computing

### Pengertian Cloud

Cloud merupakan simbol atau perumpamaan dari internet, sedangkan computing berarti pemrosesan menggunakan komputer. Jadi, cloud computing adalah penggunaan sumber daya komputasi (seperti server, storage, network, dsb.) melalui internet yang dilakukan secara on-demand (sesuai kebutuhan) dan tagihannya dibayarkan dengan harga sesuai pemakaian (pay-as-you-go).

Penerapan yang sepenuhnya menggunakan teknologi cloud. Terdapat 2 opsi dalam membangun aplikasi cloud-based:

- Low-level infrastructure: Kita masih memerlukan staf IT untuk mengelola server.
- Higher-level service: Dengan layanan serverless yang mampu mengurangi kebutuhan pengelolaan, arsitektur, dan scaling (penyesuaian kapasitas) pada infrastruktur. **serverless** disini memiliki arti bahwa server masih digunakan tetapi sepenuhnya dikelola oleh penyedia cloud, sehingga client hanya fokus pada penulisan dan menjalankan kode, tanpa perlu khawatir tentang infrastruktur di belakangnya.

### Perbandingan Dulu vs Sekarang

| **Aspek**              | **Dulu (Sebelum Cloud Computing)**                                             | **Sekarang (Dengan Cloud Computing)**                                 |
| ---------------------- | ------------------------------------------------------------------------------ | --------------------------------------------------------------------- |
| **Infrastruktur**      | Harus membangun data center sendiri                                            | Tidak perlu data center fisik, cukup sewa layanan cloud               |
| **Biaya Awal**         | Tinggi: pembelian server, rak komputer, sistem penyimpanan, instalasi jaringan | Rendah: bayar sesuai pemakaian (_pay as you go_)                      |
| **Operasional**        | Harus membayar listrik, pendingin, kebersihan, keamanan, dan sewa gedung       | Semua dikelola oleh penyedia cloud (Microsoft Azure, AWS, dsb.)       |
| **Skalabilitas**       | Sulit menambah kapasitas, harus beli perangkat keras baru                      | Skalabilitas mudah dan cepat, tinggal tambah resource di portal cloud |
| **Waktu Implementasi** | Lama, perlu perencanaan, instalasi, dan konfigurasi manual                     | Cepat, bisa deploy aplikasi dalam hitungan menit                      |
| **Fleksibilitas**      | Terbatas pada kapasitas fisik yang dimiliki                                    | Sangat fleksibel, bisa menyesuaikan kebutuhan kapan saja              |
| **Keamanan**           | Ditanggung penuh oleh perusahaan (biaya tambahan)                              | Keamanan tingkat enterprise disediakan penyedia cloud                 |

### Model Layanan

Secara umum model layanan pada sistem cloud terbagi menjadi:
![shared-responsibility](https://github.com/user-attachments/assets/787360d5-ff57-41ba-a550-d571d611242e)

- On-Premise : Terkadang disebut sebagai private cloud, user bertanggung jawab untuk memanajemen seluruh resource yang ada, seperti virtualisasi server, perawatan hardware, cabling, penyusunan rak, update software, dan data center sendiri.
- IaaS (Infrastructure as a Service): User tidak perlu memanajemen infrastruktur (termasuk hardware), karena telah disediakan cloud provider. Contohnya adalah ketika menyewa virtual machine pada cloud provider. Kita tidak perlu membeli hardware sendiri secara fisik, tetapi kita dapat memilih OS yang ingin digunakan maupun aplikasi yang ingin diinstall pada VM tersebut.
- PaaS (Platform as a Service): Penyedia layanan mengelola infrastruktur, sistem operasi, dan middleware. user hanya perlu mengelola aplikasi dan data.
- SaaS (Software as a Service): Penyedia layanan mengelola semuanya, mulai dari infrastruktur hingga aplikasi. User hanya perlu menggunakan aplikasi dan mengelola datanya.

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

<img src="https://cdn-dynmedia-1.microsoft.com/is/image/microsoftcorp/azure-portal_valprop5?resMode=sharp2&op_usm=1.5,0.65,15,0&wid=2000&qlt=100" alt="Azure Portal" width="600"/>

Portal Azure adalah konsol berbasis web untuk membuat, mengelola, dan memantau semua sumber daya Azure melalui antarmuka grafis. Melalui portal ini, pengguna dapat mengatur database, menambah daya komputasi, memantau biaya, hingga membangun aplikasi dari sederhana hingga kompleks.

#### Azure CLI

<img width="544" height="474" alt="bash-azure-cli-2" src="https://github.com/user-attachments/assets/d9e64c97-f9f8-4952-a628-3951b80022bc" />

Azure Command-Line Interface (CLI) adalah alat lintas platform berbasis terminal untuk mengelola sumber daya Azure. CLI dapat digunakan secara interaktif melalui shell (cmd, Bash, atau lainnya) maupun otomatis dengan menyusun perintah ke dalam skrip untuk menjalankan tugas berulang.

#### Azure Resource Manager (ARM)
<img width="570" height="300" alt="consistent--layer" src="https://github.com/user-attachments/assets/2d1d2284-7835-4d43-86c2-88d9a3a5e22c" />

Azure Resource Manager (ARM) adalah layanan deployment dan manajemen di Azure yang memungkinkan pengguna membuat, memperbarui, dan menghapus resource. ARM juga menyediakan fitur seperti access control, locks, dan tags untuk mengamankan serta mengorganisasi resource setelah deployment.

#### Azure Marketplace
<img width="816" height="524" alt="browse-apps" src="https://github.com/user-attachments/assets/9fec994c-cbcb-43b7-b8cd-fe15c05a9038" />

Azure Marketplace adalah toko online yang menyediakan solusi berbasis Azure untuk profesional TI dan pengembang. Melalui portal Azure atau situs web, pengguna dapat menemukan produk dan layanan, seperti konsultasi serta layanan terkelola yang dikategorikan berdasarkan area seperti analitik, jaringan, keamanan, dan database, sehingga memudahkan pencarian solusi sesuai kebutuhan organisasi.

## Layanan Utama Microsoft Azure

### App Hosting and Compute

| **Layanan**               | **Deskripsi**                                                                                                                                                                                                                                                                                                              |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Azure App Service         | Host aplikasi web dan API .NET, Java, Node.js, dan Python menggunakan layanan Azure yang sepenuhnya dikelola. Pengguna hanya perlu men-deploy kode ke Azure, sementara Azure mengurus semua manajemen infrastruktur seperti high availability, load balancing, dan autoscaling.                                            |
| Azure Static Web Apps     | Host aplikasi web statis yang dibangun menggunakan framework seperti Gatsby, Hugo, atau VuePress, atau aplikasi web modern menggunakan Angular, React, Svelte, atau Vue. Aplikasi web statis ini secara otomatis membangun dan men-deploy berdasarkan perubahan kode, serta memiliki integrasi API dengan Azure Functions. |
| Azure Container Apps      | Azure Container Apps memungkinkan pengguna menjalankan aplikasi berbasis container tanpa perlu mengkhawatirkan orkestrasi atau infrastruktur, melalui platform serverless.                                                                                                                                                 |
| Azure Container Instances | Jalankan Docker container sesuai permintaan dalam lingkungan Azure yang dikelola dan serverless. Solusi ini cocok untuk skenario yang dapat berjalan di container terisolasi tanpa memerlukan orkestrasi.                                                                                                                  |
| Azure Kubernetes Services | Cepat deploy cluster Kubernetes siap produksi ke cloud dan serahkan beban operasional ke Azure. Azure menangani tugas penting seperti monitoring kesehatan dan pemeliharaan, sementara pengguna hanya perlu mengelola agent nodes.                                                                                         |
| Azure Virtual Machines    | Hosting aplikasi menggunakan virtual machine di Azure ketika membutuhkan kontrol lebih besar atas lingkungan komputasi. Azure VMs menyediakan lingkungan komputasi yang fleksibel dan skalabel untuk Linux maupun Windows.                                                                                                 |
| Azure Functions           | Platform komputasi serverless untuk membuat potongan kode kecil yang dapat dipicu oleh berbagai jenis event. Contoh penggunaan umum: membangun API serverless atau mengorkestrasi arsitektur berbasis event.                                                                                                               |
| Azure Spring Apps         | Hosting aplikasi Spring Boot microservice di Azure tanpa perlu perubahan kode. Menyediakan fitur seperti monitoring, manajemen konfigurasi, service discovery, integrasi CI/CD, dan lainnya.                                                                                                                               |

### Azure AI services

| **Layanan**                    | **Deskripsi**                                                                                                                                                |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Azure OpenAI                   | Gunakan model bahasa canggih seperti GPT-3, Codex, dan Embeddings untuk generasi konten, ringkasan, pencarian semantik, dan terjemahan bahasa alami ke kode. |
| Azure AI Speech                | Mengubah ucapan menjadi teks yang dapat dibaca dan dicari, atau mengubah teks menjadi suara yang terdengar alami untuk antarmuka yang lebih interaktif.      |
| Azure AI Language              | Memanfaatkan Natural Language Processing (NLP) untuk mengidentifikasi frasa penting dan melakukan analisis sentimen dari teks.                               |
| Azure AI Translator            | Menerjemahkan lebih dari 100 bahasa dan dialek.                                                                                                              |
| Azure AI Vision                | Menganalisis konten dalam gambar dan video, seperti objek, wajah, atau teks.                                                                                 |
| Azure AI Search                | Layanan pencarian informasi skala besar untuk aplikasi pencarian tradisional maupun percakapan, dengan opsi keamanan, AI enrichment, dan vektorisasi.        |
| Azure AI Document Intelligence | Layanan ekstraksi dokumen yang memahami form dan struktur dokumen, memungkinkan ekstraksi teks dan struktur secara cepat.                                    |

### Data

| **Layanan**                   | **Deskripsi**                                                                                                             |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Azure SQL                     | Keluarga produk engine database SQL Server yang tersedia di cloud.                                                        |
| Azure SQL Database            | Versi SQL Server berbasis cloud yang sepenuhnya dikelola oleh Azure.                                                      |
| Azure Cosmos DB               | Layanan database PostgreSQL berbasis cloud yang sepenuhnya dikelola, menggunakan PostgreSQL Community Edition.            |
| Azure Database for PostgreSQL | Layanan database PostgreSQL berbasis cloud yang sepenuhnya dikelola, menggunakan PostgreSQL Community Edition.            |
| Azure Database for MySQL      | Layanan database MySQL berbasis cloud yang sepenuhnya dikelola, menggunakan MySQL Community Edition.                      |
| Azure Database for MariaDB    | Layanan database MariaDB berbasis cloud yang sepenuhnya dikelola, menggunakan MariaDB Community Edition.                  |
| Azure Cache for Redis         | Cache data dan messaging broker yang aman, menyediakan akses data berlatensi rendah dan throughput tinggi untuk aplikasi. |

### Storage

| **Layanan**             | **Deskripsi**                                                                                                                                                                                                    |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Azure Blob Storage      | Memungkinkan aplikasi pengguna menyimpan dan mengambil file di cloud. Layanan ini sangat skalabel untuk menyimpan data dalam jumlah besar, dan data disimpan secara redundan untuk memastikan high availability. |
| Azure Data Lake Storage | Mendukung analitik big data dengan menyediakan penyimpanan yang skalabel dan hemat biaya untuk data terstruktur, semi-terstruktur, maupun tidak terstruktur.                                                     |

### Messaging

| **Layanan**         | **Deskripsi**                                                                                                                                                                                                                                           |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Azure Service Bus   | Layanan message broker enterprise yang sepenuhnya dikelola, mendukung integrasi point-to-point dan publish-subscribe. Cocok untuk membangun aplikasi yang terdekomposisi, queue-based load leveling, atau memfasilitasi komunikasi antar microservices. |
| Azure Event Hubs    | Layanan yang sepenuhnya dikelola untuk menerima dan memproses aliran data besar dari website, aplikasi, atau perangkat.                                                                                                                                 |
| Azure Queue Storage | Layanan queue sederhana dan andal yang dapat menangani beban kerja besar untuk antrean pesan.                                                                                                                                                           |

### Identity and Security

| **Layanan**        | **Deskripsi**                                                                                                                                                                    |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Microsoft Entra ID | Mengelola identitas pengguna dan mengontrol akses ke aplikasi, data, dan sumber daya pengguna.                                                                                   |
| Azure Key Vault    | Menyimpan dan mengakses rahasia aplikasi seperti connection string dan API key dalam vault terenkripsi dengan akses terbatas, sehingga rahasia dan aplikasi pengguna tetap aman. |
| App Configuration  | Layanan cepat dan skalabel untuk mengelola pengaturan aplikasi dan feature flags secara terpusat.                                                                                |

### Management

| **Layanan**          | **Deskripsi**                                                                                                                                                             |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Azure Monitor        | Solusi monitoring komprehensif untuk mengumpulkan, menganalisis, dan merespons data pemantauan dari lingkungan cloud maupun on-premises pengguna.                         |
| Application Insights | itur dari Azure Monitor yang menyediakan Application Performance Management (APM) untuk meningkatkan performa, keandalan, dan kualitas aplikasi web yang sedang berjalan. |

## Hosting Aplikasi di Azure

### Opsi Hosting di Azure
Azure menyediakan berbagai pilihan model hosting yang dapat disesuaikan dengan kebutuhan. Semakin sederhana layanan hosting, semakin mudah dikelola namun kontrol infrastruktur terbatas; sebaliknya, semakin kompleks, semakin besar pula kontrol yang dimiliki.
<table>
  <tr>
    <th style="width:50%">Opsi Sederhana</th>
    <th style="width:50%">Opsi Kompleks</th>
  </tr>
  <tr>
    <td>
      Konfigurasi & manajemen minimal, kontrol infrastruktur terbatas.  
      Biasanya menggunakan pendekatan <b>cloud-native</b> (misalnya container/Dapr)  
      yang bisa dijalankan lintas penyedia cloud.
    </td>
    <td>
      Lebih banyak konfigurasi & manajemen, dengan kontrol penuh atas infrastruktur.  
      Umumnya bersifat <b>Azure-native</b>, memanfaatkan layanan, alat,  
      dan integrasi khusus Azure.
    </td>
  </tr>
</table>

### Model Hosting di Azure
Azure menyediakan berbagai model hosting aplikasi, mulai dari solusi tanpa kode hingga kontrol penuh atas infrastruktur. Setiap model menawarkan keseimbangan berbeda antara kemudahan penggunaan, fleksibilitas, dan tanggung jawab manajemen.

#### Simplified Hosting
Solusi ini sepenuhnya dikelola oleh Azure. Pengguna hanya fokus pada kode dan konfigurasi lingkungan, sementara Azure mengurus runtime, infrastruktur, update, dan patch. Pendekatan ini umumnya Azure-native. Contoh layanannya adalah Logic Apps, Power Automate, Azure Static Web Apps, dan Azure Functions.

#### Balanced Hosting
Solusi ini menyeimbangkan antara kesederhanaan dan kontrol. Pengguna tetap mengelola kode dan konfigurasi, tetapi runtime dan infrastruktur ditangani Azure. Bisa juga membawa container sendiri. Model ini bisa Azure-native maupun Cloud-native. Contoh layanannya adalah Azure App Service, Azure Container Apps, dan Azure Spring Apps.

#### Controlled Hosting
Solusi ini memberi pengguna kontrol penuh atas infrastruktur. Pengguna bertanggung jawab penuh atas kode, aset, konfigurasi lingkungan, update, dan patch. Pendekatan ini lebih ke Cloud-native. Contoh layanannya adalah Azure Virtual Machines dan Azure Kubernetes Service (AKS).

#### Source Code Hosting
<img width="1108" height="519" alt="source-code-suggested-compute" src="https://github.com/user-attachments/assets/750d8f0b-75b7-4fac-aa7e-c06601666f52" />
Model ini ditujukan bagi developer yang baru memulai di Azure. Azure akan memberikan rekomendasi hosting sesuai jenis aplikasi, yaitu no-code/low-code, code, atau container.
##### No-code / Low-code
Cocok untuk membangun workflow atau otomatisasi bisnis tanpa banyak kode. Contohnya adalah Logic Apps dan Power Automate / Power Apps.
##### Code 
Digunakan ketika developer ingin mengembangkan langsung dengan kode. Contohnya adalah Azure Static Web Apps, Azure Functions, Azure App Service, dan Azure Spring Apps.
##### Container
Ditujukan untuk aplikasi berbasis container dengan berbagai tingkat kontrol.
- Orkestrasi: Azure Kubernetes Service (cloud-native), Azure Service Fabric (Azure-native)
- Preconfigured hosting: Azure App Service, Azure Spring Apps, Azure Container Apps, Azure Container Instances
- Manajemen image: Azure Container Registry

## Membuat Virtual Machine di Azure

Sebelum memulai praktik di Microsoft Azure, langkah pertama yang perlu dilakukan adalah membuat akun Azure. Microsoft menyediakan opsi akun gratis atau trial yang memungkinkan pengguna mengeksplorasi berbagai layanan cloud tanpa biaya awal, sehingga sangat cocok untuk pembelajaran dan percobaan layanan seperti Virtual Machine, Storage, dan Database.

### Persiapan

Berikut adalah langkah claimnya secara lengkap:

1. Akses https://azure.microsoft.com/en-us/free/students/
   ![Screenshot 2023-03-14 091825](https://user-images.githubusercontent.com/79139753/225182579-2a2bc316-3f91-4c8a-bd51-36f7f6cd94a5.png)
2. Pilih `Start free` lalu sign up menggunakan account bebas (bisa email ITS/email pribadi)
3. Lakukan verifikasi nomor HP, lalu isi data diri sesuai ketentuan termasuk email akademik (tidak perlu sama dengan email account pada poin 2). Kemudian lakukan verifikasi yang akan dikirimkan ke email akademik
   ![Screenshot 2023-03-14 122025](https://user-images.githubusercontent.com/79139753/225182930-86afc333-fbe4-4ed8-bad6-7cf4c8c443e6.png)
4. Jika sudah selesai, akses https://portal.azure.com/signin/index/ lalu cek pada bagian subscription. Maka subscription `Azure for Students` akan terlihat sudah aktif dan mendapat kredit 100$

### Langkah-langkah Membuat VM

Salah satu yang dapat dilakukan pada Microsoft Azure adalah melakukan penyewaan Virtual Machine dengan spesifikasi yang diinginkan. Berikut adalah langkah-langkahnya:

1. Klik `Virtual Machines`, lalu pilih `Create` -> `Azure virtual machine` pada bagian kiri atas
   ![Screenshot 2023-03-14 091939](https://user-images.githubusercontent.com/79139753/225183536-ff0f8874-5af1-4283-9693-79616a0262fd.png)
2. Isi nama VM, region (posisi data center VM), image (OS maupun image lain) yang ingin digunakan pada VM, dan size (CPU, cores)
   ![Screenshot 2023-03-14 092036](https://user-images.githubusercontent.com/79139753/225183573-0431c21f-f175-453d-91d7-599a55fc98d3.png)
3. Selanjutnya untuk autentikasi dapat menggunakan SSH public key (memerlukan file key ketika ingin mengakses secara remote menggunakan `ssh`) maupun password (lebih mudah karena dapat diakses dari mana saja tetapi lebih rentan), dalam contoh ini akan menggunakan password
   ![Screenshot 2023-03-14 092252](https://user-images.githubusercontent.com/79139753/225183831-d0222f67-6651-4d4d-abb3-927576febe22.png)
4. Lanjutkan proses review dan create
5. Ketika VM berhasil diinisialisasi, maka VM tersebut akan ditampilkan pada halaman `Virtual Machines`
   ![Screenshot 2023-03-14 094731](https://user-images.githubusercontent.com/79139753/225184137-8e7567f6-bea1-41a3-914c-fd806bd8961c.png)
   ![Screenshot 2023-03-14 094743](https://user-images.githubusercontent.com/79139753/225184169-e70acbd6-dc49-4ebd-9ed1-89729e58317b.png)

### Mengakses Virtual Machine

Setelah VM dibuat, berikut cara mengaksesnya:

1. Ketika ingin mengakses VM tersebut, kita dapat menggunakan terminal/command line pada perangkat yang kita miliki lalu gunakan ssh ([secure shell](https://www.biznetgio.com/en/news/apa-itu-ssh-pengertian-fungsi-dan-cara-kerjanya)) dengan command `ssh <username>@<public IP address>` sesuai dengan parameter username dan IP pada VM yang kita miliki
   ![Screenshot 2023-03-14 121244](https://user-images.githubusercontent.com/79139753/225184391-53b238bc-8542-4f29-8678-bc1beaf5612a.png)
2. VM sudah dapat diakses dan digunakan melalui terminal seperti penggunaan terminal pada umumnya
   ![Screenshot 2023-03-14 121257](https://user-images.githubusercontent.com/79139753/225184439-cc9ca4bc-61be-4b65-bb9d-29ebe2df8890.png)
