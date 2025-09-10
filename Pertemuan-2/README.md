# Pertemuan 2 LBE NCC 2025

## Daftar Isi

- [Network Service Azure](#network-service-azure)
   - [Fondasi Jaringan](#fondasi-jaringan)
   - [Penyeimbangan Beban dan Pengiriman Konten](#penyeimbangan-beban-dan-pengiriman-konten)
   - [Konektivitas Hibrid](#konektivitas-hibrid)
   - [Keamanan Jaringan](#keamanan-jaringan)
   - [Manajemen dan Pemantauan Jaringan](#manajemen-dan-pemantauan-jaringan)
- [Membangun Infrastruktur Azure Sederhana](#membangun-infrastruktur-azure-sederhana)
   - [Konfigurasi Jaringan](#konfigurasi-jaringan)
   - [Web Server](#web-server)
   - [Database](#database)
- [Deploy Aplikasi](deploy-aplikasi)
   - [Konfigurasi Aplikasi](#konfigurasi-aplikasi)
   - [Port Forwarding pada NGINX](#port-Forwarding-pada-nginx)
   - [Menjalankan Aplikasi sebagai Layanan dengan Systemd](#menjalankan-aplikasi-sebagai-layanan-dengan-systemd)


## Network Service Azure

Azure Networking Services adalah kumpulan layanan yang disediakan Microsoft Azure untuk membangun, menghubungkan, mengamankan, dan mengelola jaringan di cloud. Kumpulan layanan ini memberikan fondasi agar aplikasi dan layanan di Azure bisa saling terhubung dengan aman, cepat, dan stabil, baik antar layanan di cloud, ke internet, maupun ke jaringan lokal.

### Ringkasan Dalam Tabel

| Kategori                                      | Layanan Utama Azure                                                                                        |
| --------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Fondasi Jaringan**                          | VNet, NSG, Service Endpoints, Private Link, Azure DNS, Bastion, Route Server, NAT Gateway, Traffic Manager |
| **Penyeimbangan Beban dan Pengiriman Konten** | Load Balancer, Application Gateway, Azure Front Door                                                       |
| **Konektivitas Hibrida**                      | VPN Gateway, ExpressRoute, Virtual WAN, Peering Service                                                    |
| **Keamanan Jaringan**                         | Azure Firewall, Firewall Manager, WAF, DDoS Protection                                                     |
| **Manajemen & Pemantauan**                    | Network Watcher, Azure Monitor, Virtual Network Manager, NPM                                               |

### Fondasi jaringan

Bagian ini menjelaskan layanan yang menyediakan blok penyusun untuk merancang lingkungan jaringan di Azure seperti Virtual Network (VNet), Network Security Groups, Service Endpoints, Private Link, Azure DNS, Azure Bastion, Route Server, NAT Gateway, dan Traffic Manager.

#### 1. Virtual Network (VNet)

<img width="849" height="436" alt="v-net" src="https://github.com/user-attachments/assets/0fba1ee8-c40b-4f86-8bba-c48babd2a3cc" />

Azure Virtual Network (VNet) adalah jaringan pribadi (private network) di dalam Azure. Ibaratnya seperti "kompleks perumahan" khusus yang di dalamnya ada rumah-rumah seperti Virtual Machine (VM), Azure Kubernetes Service (AKS), App Service Environment, dll yang bisa saling mengobrol secara privat.

Fungsinya antara lain:

- **Berkomunikasi antara sumber daya Azure**: VM atau aplikasi dalam satu VNet bisa saling berkomunikasi secara privat.
- **Berkomunikasi satu sama lain**: VNet bisa dihubungkan satu sama lain menggunakan peering, baik di wilayah (region) yang sama maupun berbeda.
- **Berkomunikasi ke internet**: Semua sumber daya (VM, AKS, dll) bisa keluar ke internet. Namun, untuk menerima koneksi dari luar (inbound), dibutuhkan Public IP atau Load Balancer.
- **Berkomunikasi dengan jaringan lokal**: VNet dapat dihubungkan ke jaringan kantor menggunakan VPN Gateway atau ExpressRoute.
- **Mengenkripsi lalu lintas antar sumber daya**:VNet mendukung enkripsi untuk lalu lintas antar sumber daya di dalamnya.

Singkatnya, VNet adalah pondasi utama untuk semua komunikasi jaringan di Azure. Semua arsitektur jaringan Azure hampir pasti mulai dari bikin VNet + subnet.

#### 2. Network Security Groups

NSG adalah mekanisme keamanan untuk mengatur izin lalu lintas ke dalam (inbound) dan keluar (outbound) dari sumber daya di dalam VNet. NSG dapat ditempelkan pada subnet atau langsung ke network interface dari VM.

Dengan NSG, kita bisa membuat aturan seperti:

- Hanya mengizinkan akses ke port tertentu (misalnya 443 untuk HTTPS).
- Membatasi akses RDP/SSH hanya dari alamat IP kantor.
- Menolak semua lalu lintas yang tidak sesuai protokol yang berlaku.

#### 3. Service Endpoints

<img width="612" height="445" alt="vnet-service-endpoints-overview" src="https://github.com/user-attachments/assets/1793472a-1686-4780-8dec-d92b2e55791f" />

Service Endpoints membuat resource dalam VNet yang dapat langsung terhubung ke layanan Azure seperti Storage atau SQL Database melalui jalur privat Microsoft.

Kelebihannya antara lain:

- Trafik tidak lewat internet publik, jadi lebih aman.

- Bisa membatasi layanan Azure supaya hanya bisa diakses dari VNet tertentu.

#### 4. Azure Private Link

<img width="956" height="511" alt="private-endpoint" src="https://github.com/user-attachments/assets/d5b4a3bb-0bf7-4121-9958-5b7e09685d80" />

Azure Private Link lebih privat daripada Service Endpoints. Dengan ini, layanan Azure (misalnya Storage atau SQL) punya alamat IP privat langsung di dalam VNet kita.

Keuntungannya antara lain:

- Resource terasa seperti ada di jaringan lokal kita sendiri.

- Tidak perlu mengekspos layanan ke internet publik.

- Bisa juga dipakai untuk berbagi layanan dengan partner atau pelanggan.

#### 5. DNS Azure

Azure DNS menyediakan hosting dan resolusi DNS menggunakan infrastruktur Microsoft Azure. Layanan ini menghubungkan nama (domain) ke alamat IP.

Azure DNS terdiri dari tiga layanan:

- Public DNS → untuk domain publik, misalnya perusahaan.com.

- Private DNS → untuk nama domain internal yang hanya berlaku di dalam VNet.

- DNS Private Resolver → jembatan supaya DNS Azure bisa terhubung dengan DNS di jaringan lokal (dan sebaliknya).

#### 6. Azure Bastion

<img width="929" height="339" alt="architecture" src="https://github.com/user-attachments/assets/54103169-34fc-47a4-9964-7de6183ba581" />

Azure Bastion adalah cara aman untuk mengakses VM tanpa harus memberi Public IP ke VM tersebut.

Keuntungan dari Azure Bastion:

- Bisa masuk ke VM lewat browser atau portal Azure.

- Koneksi aman karena sudah lewat TLS.

- Tidak perlu software tambahan di sisi VM.

#### 7. Azure Route Server

<img width="1142" height="720" alt="route-server-overview" src="https://github.com/user-attachments/assets/d5994ee6-f2be-4046-8071-80e1247ffa17" />

Route Server memudahkan pengaturan rute jaringan secara otomatis menggunakan BGP (Border Gateway Protocol) tanpa perlu mengonfigurasi atau memelihara tabel rute secara manual.

Kegunaan dari Azure Route Server:

- Memungkinkan resource dan perangkat virtual jaringan (seperti firewall atau router) bertukar informasi rute tanpa perlu setting manual.

- Membuat integrasi jaringan lebih mudah.

#### 8. NAT Gateway

<img width="472" height="603" alt="flow-map" src="https://github.com/user-attachments/assets/c3875f2a-db3a-4a5b-91f7-4643033aa4c4" />

NAT Gateway berfungsi sebagai “pintu keluar bersama” untuk resource dalam subnet.

Manfaat dari NAT Gateway:

- Semua resource keluar ke internet lewat satu alamat IP publik statis.

- Tidak perlu memberi Public IP ke setiap VM.

- Membantu kalau butuh IP keluar (outbound) yang konsisten.

#### 9. Azure Traffic Manager

<img width="780" height="600" alt="priority" src="https://github.com/user-attachments/assets/ce2c4697-3d84-4ffa-b06c-a9697056969b" />

Azure Traffic Manager adalah penyeimbang beban lalu lintas berbasis DNS yang berfungsi mengatur distribusi trafik pengguna ke resouce di berbagai lokasi (region).

Kelebihan Azure Traffic Manager:

- Membuat trafik lebih cepat karena diarahkan ke lokasi terdekat atau tercepat.

- Menyediakan failover, jadi layanan tetap jalan meskipun ada yang mati.

- Bisa atur strategi distribusi trafik sesuai kebutuhan (misalnya prioritas atau lokasi geografis).

### Penyeimbangan Beban dan Pengiriman Konten

Bagian ini menjelaskan layanan jaringan di Azure yang membantu mengirimkan aplikasi dan beban kerja seperti Load Balancer, Application Gateway, dan Azure Front Door Service.

#### 1. Load Balancer

Azure Load Balancer digunakan untuk membagi lalu lintas ke beberapa server agar tidak ada satu server yang terlalu terbebani. Layanan ini bekerja di lapisan 4 (transport) dan mendukung protokol TCP dan UDP. Misalnya, jika terdapat 1000 orang yang membuka aplikasi dalam waktu bersamaan, permintaan mereka akan dibagi rata ke beberapa server agar tidak ada server yang kewalahan.

Azure Load Balancer tersedia dalam SKU Standar, Regional, dan Gateway.

Gambar berikut menunjukkan aplikasi multi-tingkat yang menggunakan load balancer eksternal dan internal:

<img width="441" height="597" alt="load-balancer" src="https://github.com/user-attachments/assets/9a0cb865-048d-42e0-a096-4c988f2bb3ee" />

#### 2. Application Gateway

Azure Application Gateway adalah layanan penyeimbang beban yang bekerja di lapisan aplikasi (Layer 7). Berbeda dengan Load Balancer yang hanya mengatur lalu lintas di tingkat jaringan, Application Gateway bisa mengarahkan lalu lintas berdasarkan isi permintaan web. Misalnya, permintaan untuk namadomain.com/images bisa diarahkan ke server khusus gambar, sementara namadomain.com/api diarahkan ke server API.

Selain itu, Application Gateway juga dilengkapi dengan Web Application Firewall (WAF) yang melindungi aplikasi dari berbagai ancaman keamanan di web. Layanan ini sangat cocok untuk aplikasi web modern yang membutuhkan kontrol lalu lintas yang lebih detail.

Gambar berikut memperlihatkan perutean berbasis jalur URL dengan Application Gateway.

<img width="597" height="233" alt="figure1-720" src="https://github.com/user-attachments/assets/849790e6-6ad4-42fa-b598-5f150d3ea5fc" />


#### 3. Azure Front Door

Azure Front Door berfungsi sebagai pintu depan global untuk aplikasi. Layanan ini mempercepat akses aplikasi dengan memilih jalur tercepat di jaringan global Microsoft, sehingga pengguna di mana pun bisa mendapatkan pengalaman akses yang optimal. Jika pengguna berasal dari Indonesia, misalnya, Front Door akan mengarahkan permintaan ke server atau jalur yang paling dekat dan paling cepat.

Tidak hanya itu, layanan ini juga menyediakan failover otomatis. Artinya, jika terjadi gangguan di salah satu wilayah, permintaan pengguna bisa segera dialihkan ke wilayah lain yang masih aktif. Dengan fitur ini, aplikasi bisa tetap tersedia meskipun ada masalah di sebagian infrastruktur. Front Door sangat cocok digunakan untuk aplikasi berskala besar yang melayani pengguna lintas negara dan membutuhkan performa tinggi serta ketersediaan global.

<img width="520" height="530" alt="front-door-visual-diagram" src="https://github.com/user-attachments/assets/85440f66-48d5-4566-9c42-35a0bd1ce4d4" />


### Konektivitas Hibrid

Bagian ini menjelaskan layanan konektivitas jaringan yang menyediakan komunikasi aman antara jaringan lokal dan jaringan di Azure seperti VPN Gateway, ExpressRoute, Virtual WAN, dan Peering Service.

#### 1. VPN Gateway

VPN Gateway adalah layanan yang memungkinkan kita membuat koneksi terenkripsi (aman) antara jaringan lokal dengan Virtual Network (VNet) di Azure. Dengan VPN Gateway, kita juga bisa menghubungkan beberapa VNet yang berbeda. Ada beberapa jenis koneksi yang bisa dibuat, seperti:

- Situs ke situs (site-to-site): menghubungkan jaringan kantor langsung ke Azure.

- Titik ke situs (point-to-site): memungkinkan perangkat individu (seperti laptop) terhubung langsung ke Azure.

- VNet ke VNet: menghubungkan beberapa jaringan virtual di Azure agar bisa saling berkomunikasi.

Diagram berikut mengilustrasikan beberapa koneksi VPN situs-ke-situs ke jaringan virtual yang sama.

<img width="1487" height="377" alt="multi-site" src="https://github.com/user-attachments/assets/51f3349a-af26-4603-9d51-d6788ae555d3" />


#### 2. ExpressRoute

ExpressRoute adalah layanan untuk membuat koneksi privat langsung dari jaringan lokal ke Azure tanpa lewat internet. Karena jalurnya privat, koneksi ini biasanya lebih aman, stabil, dan memiliki kecepatan tinggi. ExpressRoute juga bisa digunakan untuk mengakses layanan lain dari Microsoft, seperti Microsoft 365 dan Dynamics 365. Layanan ini biasanya dipakai oleh perusahaan yang punya kebutuhan koneksi besar dan kritis.

<img width="1215" height="581" alt="expressroute-connection-overview" src="https://github.com/user-attachments/assets/8dcfd879-e82d-4e7e-b973-64793bcf26e1" />


#### 3. Virtual WAN

Azure Virtual WAN adalah layanan yang menggabungkan berbagai kebutuhan konektivitas dan keamanan jaringan dalam satu tempat. Dengan Virtual WAN, perusahaan bisa menghubungkan kantor cabang, pengguna jarak jauh, bahkan mengatur rute antar jaringan virtual dengan lebih mudah. Layanan ini juga mendukung koneksi site-to-site VPN, point-to-site VPN, ExpressRoute, serta konektivitas antar-VNet. Selain itu, ada juga fitur tambahan seperti firewall, enkripsi, dan perutean otomatis yang membantu menjaga koneksi tetap aman dan efisien.

<img width="688" height="409" alt="virtual-wan-diagram" src="https://github.com/user-attachments/assets/751b322e-cec8-4ebc-8ccf-007757addcdb" />


#### 4. Layanan Peering

Azure Peering Service adalah layanan yang membantu meningkatkan kualitas koneksi dari jaringan pengguna ke layanan Microsoft, seperti Microsoft 365, Dynamics 365, dan Azure. Dengan menggunakan Peering Service, jalur koneksi ke layanan Microsoft bisa menjadi lebih cepat, lebih stabil, dan lebih andal karena diarahkan lewat jaringan mitra yang sudah bekerja sama langsung dengan Microsoft.

### Keamanan Jaringan

Bagian ini menjelaskan layanan jaringan di Azure yang melindungi dan memantau sumber daya jaringan Anda - Firewall Manager, Firewall, Web Application Firewall, dan DDoS Protection.

#### 1. Firewall Manager

<img width="494" height="578" alt="trusted-security-partners" src="https://github.com/user-attachments/assets/f82f5dd2-da14-479a-b138-e785a09c57e3" />

Azure Firewall Manager adalah layanan untuk mengatur keamanan secara terpusat. Dengan layanan ini, kita bisa membuat satu kebijakan keamanan yang berlaku di banyak jaringan dan wilayah Azure. Firewall Manager juga mendukung arsitektur jaringan yang berbeda, baik untuk hub virtual yang aman maupun jaringan hub biasa. Selain itu, layanan ini bisa digunakan untuk mengelola Azure Firewall di berbagai lokasi, mengaktifkan perlindungan DDoS, hingga mengatur kebijakan Web Application Firewall (WAF).


#### 2. Azure Firewall

<img width="536" height="349" alt="firewall-threat" src="https://github.com/user-attachments/assets/5309ea4e-17a9-4f37-a62a-c4694ec31bfc" />

Azure Firewall adalah layanan firewall berbasis cloud yang terkelola penuh oleh Azure. Fungsi utamanya adalah melindungi resource di dalam Virtual Network (VNet). Dengan Azure Firewall, kita bisa membuat aturan untuk mengatur lalu lintas masuk dan keluar jaringan, baik untuk aplikasi maupun koneksi lain. Layanan ini juga menyediakan alamat IP publik statis, sehingga semua lalu lintas keluar dari jaringan bisa dikenali dengan jelas.


#### 3. Web Application Firewall

<img width="456" height="306" alt="waf-overview" src="https://github.com/user-attachments/assets/4bc50a68-10fe-4b37-b6e9-4e34477c5e64" />

Azure WAF adalah layanan khusus yang melindungi aplikasi web dari serangan umum, misalnya SQL injection atau cross-site scripting (XSS). WAF ini menggunakan aturan bawaan untuk melindungi dari 10 kerentanan paling berbahaya menurut OWASP, dan kita juga bisa menambahkan aturan sendiri sesuai kebutuhan (misalnya berdasarkan IP, cookie, atau query string). WAF bisa dipasang bersama Azure Application Gateway untuk perlindungan regional, atau dengan Azure Front Door untuk perlindungan global di titik akses publik.

#### 4. Perlindungan DDoS

<img width="1196" height="553" alt="ddos-protection-overview-architecture" src="https://github.com/user-attachments/assets/ed11d549-3050-4134-8036-91ace0becac2" />

Azure DDoS Protection adalah layanan untuk melindungi aplikasi dari serangan Distributed Denial of Service (DDoS). Layanan ini secara otomatis mendeteksi dan memitigasi serangan yang mencoba membanjiri sistem dengan lalu lintas berlebihan. Ada dua jenis layanan DDoS di Azure:

- DDoS Network Protection, yang memberikan perlindungan otomatis ke resource dalam VNet.

- DDoS IP Protection, yang melindungi alamat IP tertentu dengan biaya berdasarkan jumlah IP yang diamankan.


#### 5. Keamanan Jaringan Kontainer

Keamanan jaringan kontainer adalah fitur tambahan untuk melindungi aplikasi yang berjalan di dalam Azure Kubernetes Service (AKS). Layanan ini memungkinkan penerapan aturan keamanan berbasis domain (FQDN) sehingga lebih detail dan sesuai prinsip Zero Trust. Dengan begitu, setiap komunikasi antar kontainer bisa diawasi dan dibatasi sesuai kebutuhan, tanpa memberi akses berlebihan.

### Manajemen dan Pemantauan Jaringan

Bagian ini menjelaskan layanan manajemen dan pemantauan jaringan di Azure - Network Watcher, Azure Monitor, dan Azure Virtual Network Manager.

#### 1. Azure Network Watcher

<img width="600" height="219" alt="network-watcher-capabilities" src="https://github.com/user-attachments/assets/9f304761-ed6b-480e-920e-2280a83b45a3" />

Azure Network Watcher adalah layanan yang dipakai untuk memantau dan mendiagnosis jaringan di Azure. Dengan layanan ini, kita bisa melihat metrik lalu lintas, mengecek apakah ada masalah koneksi, sampai mengaktifkan log agar aktivitas jaringan bisa tercatat. Network Watcher membantu kita memahami apa yang sebenarnya terjadi di dalam jaringan, jadi lebih mudah untuk menemukan dan memperbaiki masalah.

#### 2. Azure Monitor

Azure Monitor berfokus pada performa aplikasi dan infrastruktur. Layanan ini mengumpulkan data dari berbagai sumber, lalu menyajikan informasi yang bisa dipakai untuk analisis. Dengan begitu, kita bisa mengetahui apakah aplikasi berjalan dengan baik, menemukan masalah sebelum menjadi lebih besar, dan menjaga ketersediaan layanan tetap stabil.

#### 3. Azure Virtual Network Manager

<img width="409" height="577" alt="virtual-network-manager-resources-diagram" src="https://github.com/user-attachments/assets/90bfed7b-452b-4176-8ce5-f387c3de2c0b" />

Azure Virtual Network Manager berfungsi untuk mengelola banyak jaringan virtual (VNet) secara terpusat. Dengan layanan ini, kita bisa mengelompokkan jaringan, mengatur konfigurasi konektivitas, hingga menerapkan aturan keamanan sekaligus ke banyak VNet. Jadi, pengelolaan jaringan dalam skala besar bisa dilakukan lebih cepat, konsisten, dan efisien.

#### 4. Pengamantan Jaringan Kontainer

Pengamatan Jaringan Kontainer khusus digunakan untuk lingkungan Kubernetes (AKS). Layanan ini memberikan pandangan detail tentang lalu lintas dan performa jaringan dalam cluster, mulai dari level node, pod, sampai DNS. Dengan begitu, kita bisa memantau kondisi jaringan kontainer secara real-time dan memastikan aplikasi berbasis kontainer tetap berjalan lancar.

<img width="586" height="365" alt="advanced-network-observability" src="https://github.com/user-attachments/assets/a893647c-f998-4910-b97a-61ce0f7973f9" />


## Membangun Infrastruktur Azure Sederhana

Setelah mengetahui berbagai macam layanan pada Azure, kita akan mencoba membuat infrastruktur Azure sederhana. Infrastruktur ini terdiri dari dua komponen utama:

1. Web server yang ditempatkan di public subnet agar dapat diakses dari internet. Karena VM di Azure tidak otomatis mendapatkan public IP DNS name, kita perlu membuat Public IP DNS name secara terpisah untuk web server agar pengguna dapat mengaksesnya lebih mudah.

2. Database yang ditempatkan di private subnet, sehingga tidak dapat diakses langsung dari internet. Database hanya dapat diakses oleh VM tertentu, misalnya web server, melalui jaringan internal (VNet).

Untuk mengatur hak akses masing-masing resource, kita menggunakan Network Security Group (NSG). NSG berfungsi sebagai firewall virtual yang mengatur traffic masuk dan keluar:

- Dari internet ke public subnet hanya membuka port 80/443 agar web server dapat diakses.

- Dari public subnet ke private subnet hanya mengizinkan web server mengakses database pada port tertentu, seperti port 5432 untuk PostgreSQL.

Struktur infrasturktur dapat lebih jelas dilihat pada ilustrasi berikut:<br>

<center>

<img width="849" height="436" alt="v-net" src="https://github.com/user-attachments/assets/710e4439-d8f7-4f10-b36f-245738ca07fb" />

<img width="2513" height="1486" alt="Screenshot 2025-09-08 044842" src="https://github.com/user-attachments/assets/bf83dc4a-6140-4a7a-bd93-0fa0526b37c9" />

</center>

### Konfigurasi Awal

#### Resource Group

Sebelum membuat konfigurasi jaringan, kita perlu membuat resource group terlebih dahulu untuk menampung semua resources seperti VM, VNet, NSG, dan Public IP DNS name agar lebih mudah dikelola nantinya.

Pertama, cari resources group pada search bar atau menu bar
1. Masuk ke `Resources Group`
<img width="1263" height="1005" alt="image" src="https://github.com/user-attachments/assets/a6f6dfad-4fdc-4b30-bff3-d37333726212" />

2. Klik `Create`, lalu masukkan nama Resource Group dan pilih Region yang paling dekat dengan lokasi client. Setelah itu, tekan tombol `Review + Create`.

<img width="1381" height="720" alt="image" src="https://github.com/user-attachments/assets/9d674243-e3f5-4af5-9ba6-0c8a62fbbe27" />


#### Network Security Group (NSG)

Selanjutnya, buat Network Security Group (NSG) untuk mengatur lalu lintas jaringan masuk dan keluar. Karena nantinya kita akan memiliki 2 jenis subnet, yakni private dan public, maka NSG yang kita buat juga terdiri dari NSG private dan public. Berikut adalah langkah-langkahnya:

1. Masuk ke `Network security groups`
2. Lalu klik `Create`
3. Pilih `Resource Group` yang telah dibuat sebelumnya, lalu masukkan nama untuk resource yang akan dibuat.
Pada contoh di bawah, kita membuat NSG untuk subnet public.

<img width="1402" height="747" alt="image" src="https://github.com/user-attachments/assets/f0ecabbf-1226-45dc-b4aa-212e16f4a15f" />

4. Setelah itu, tekan tombol `Review + Create`.
5. Lakukan hal yang sama untuk NSG subnet private.

#### Virtual Networks (VNet)

Kita perlu membuat sebuah subnet agar Virtual Machine (VM) dan database cloud berada dalam jaringan yang sama, sehingga keduanya bisa saling terhubung dengan aman dan efisien. Dengan subnet ini, komunikasi antara VM dan database lebih cepat, terlindungi dari akses luar, dan memudahkan pengelolaan koneksi jaringan tanpa mengganggu layanan lain di Azure. Berikut langkah-langkahnya:
1. Masuk ke `Virtual Networks`
2. Lalu klik `Create`
3. Pilih `Resource Group` yang telah dibuat sebelumnya, lalu masukkan nama untuk Virtual network yang akan dibuat. Pastikan memilih region yang sama dengan semua resource yang sudah dibuat.
<img width="1398" height="1148" alt="image" src="https://github.com/user-attachments/assets/b5d0ebe6-58e9-4246-b4d2-a55c71488e79" />

4. Pada bagian `IP addresses`, buat 2 subnet untuk public dan private. Tekan add a subnet untuk menambah subnet. Sebelum itu, default subnet yang ada bisa dihapus terlebih dahulu.

<center>
<img width="1406" height="382" alt="image" src="https://github.com/user-attachments/assets/24642ac4-ea73-4930-8230-3d5767ed4d18" />
<img width="1097" height="863" alt="image" src="https://github.com/user-attachments/assets/b1ddeee6-ccd1-4114-8bf4-3bee056bfc8d" />
<img width="1372" height="477" alt="image" src="https://github.com/user-attachments/assets/175510d4-9eb8-4a0a-addb-dce8be9b9594" />
</center>
<br>
5. Lakukan hal yang sama untuk subnet private. Namun, kita perlu memberikan konfigurasi tambahan pada bagian security dan subnet delegation.<br>
<img width="1370" height="821" alt="image" src="https://github.com/user-attachments/assets/ce8b53e9-af90-4e32-befd-e5b3605bb58a" />

<img width="1119" height="159" alt="image" src="https://github.com/user-attachments/assets/6e605c78-33c9-4d7a-aef0-17ab9b5e7210" />

6. Berikut hasil tampilan ketika VNet dan subnet telah selesai dikonfigurasi:
<img width="1108" height="1172" alt="image" src="https://github.com/user-attachments/assets/99391c7a-77f4-4e9a-a6ff-c353ef0a10e7" />

#### Firewall
Saat membuat sebuah network security group, secara default hanya port 65000 dan 65001 yang diizinkan untuk koneksi masuk. Agar dapat melakukan remote access menggunakan port 22 (SSH) serta memungkinkan aplikasi web diakses publik melalui port 80 (HTTP) dan 443 (HTTPS), kita perlu menambahkan aturan akses. Kita juga perlu membuka port 5432 agar dapat berkomunikasi dengan database. Ikuti langkah-langkah berikut:
1. Masuk ke `Network security groups`
2. Masuk ke network security group yang nantinya akan digunakan untuk virtual utama (publik)
3. Pilih `Settings` -> `Inbound security rules`

<img width="1152" height="832" alt="image" src="https://github.com/user-attachments/assets/797538b1-3a51-45c2-b8d5-d4c0711c5d36" />

4. Klik `add` dan pilih service `SSH`. Berikan nama pada security rule dan tekan `add` untuk menambah rule baru. Lakukan hal yang sama untuk `HTTP`, `HTTPS`, dan `PostgreSQL`
<img width="1118" height="761" alt="image" src="https://github.com/user-attachments/assets/4eb24db5-8ff6-45b3-9c41-270f34c7a816" />

5. Pilih `Settings` -> `Outbound security rules`
6. Klik `add` dan pilih service `PostgreSQL`. Berikan nama pada security rule dan tekan `add` untuk menambah rule baru.
<center>
<img width="1356" height="1293" alt="image" src="https://github.com/user-attachments/assets/2101aa7a-241e-4cb3-8a41-46a5e0fee129" />
</center>

Selain itu kita juga perlu membuat aturan untuk database yang akan kita buat. Ikuti langkah-langkah berikut:
1. Masuk ke network security group yang nantinya akan digunakan untuk database (privat)
2. Pilih `Settings` -> `Inbound security rules`
3. Klik `add` dan pilih service `PostgreSQL`. Berikan nama pada security rule dan tekan `add` untuk menambah rule baru.
4. Lakukan hal yang sama untuk  `Outbound security rules`

<center>
<img width="1375" height="1286" alt="image" src="https://github.com/user-attachments/assets/511da59b-6e8a-40df-882a-51a2d1dbfb2e" />
</center>

#### Public IP

Karena VM di Azure tidak secara otomatis memiliki public IP, kita perlu membuat Public IP secara terpisah untuk VM web server agar dapat diakses dari internet. Berikut langkah-langkahnya:

1. Masuk ke `Public IP addresses`
2. Klik `+ Create public IP address`
3. Pilih resource group utama kemudian berikan nama pada public IP.

<img width="1388" height="1009" alt="image" src="https://github.com/user-attachments/assets/7af50003-6d46-4fc3-a875-2f52e01891ea" />
<img width="1377" height="702" alt="image" src="https://github.com/user-attachments/assets/1310dd39-4b7b-44c2-b992-c6bc87eb43c5" />

4. Beri nama DNS pada IP sehingga lebih mudah diakses oleh client.<br>

<img width="1364" height="118" alt="image" src="https://github.com/user-attachments/assets/90d23676-5141-432b-9725-de07e0e5c36c" />

5. Setelah itu, tekan tombol `Review + Create`.

### Web Server

Web server merupakan perangkat yang menyediakan konten web pada pengguna melalui internet. Kali ini, kita akan menggunakan `linux` dan `Nginx` sebagai web server.

#### Launch VM

Buat Virtual Machine dengan langkah-langkah yang sama seperti pada modul sebelumnya. Tambahkan konfigurasi berikut:
1. Pada menu `disks`, pilih OS disk type `Standard SSD locally-redundant storage`.
   
<img width="1382" height="277" alt="image" src="https://github.com/user-attachments/assets/44167479-b67d-42a9-9167-287cd55377e7" />

3. Setelah itu, pada menu `networking` pilih VNet, Subnet, Public IP, dan NSG yang telah dibuat tadi. 
<img width="1375" height="859" alt="image" src="https://github.com/user-attachments/assets/0cef7fd9-04ea-4634-861a-06d77bd73698" />
<img width="1400" height="113" alt="image" src="https://github.com/user-attachments/assets/e9ff4dc9-f855-4cea-acf6-f85b5560ee48" />
<br>

4. Pada menu `Advanced` di bagian Custom data and cloud-init dengan skrip berikut:
```
#!/bin/bash
apt-get update -y
apt-get install -y nginx postgresql-client-16 nodejs npm
```

5. Biarkan konfigurasi lainnya tetap secara default. Berikut tampilan jika VM telah berhasil dibuat:<br>
<img width="2860" height="1011" alt="image" src="https://github.com/user-attachments/assets/85a05e7b-1fcb-4386-95c6-fd977469deaa" />

#### Menjalankan NGINX

Meskipun port HTTP (80) sudah dibuka di Azure, kita tetap tidak bisa mengakses apa-apa karena tidak ada proses yang menjalankan layanan pada port 80. Oleh karena itu kita perlu menjalankan Nginx di VM sudah kita install pada saat VM dibuat. Berikut langkah-langkahnya:

<center>
<img width="2866" height="1246" alt="image" src="https://github.com/user-attachments/assets/c2acc691-e14b-4eed-aa1c-c9c7cda3e524" />
</center>

1. Kita dapat mengaktifkannya dengan command berikut.

```
systemctl enable nginx
systemctl start nginx
```

2. Lalu kita dapat mengecek apakah nginx sudah berjalan dengan command berikut
```
systemctl status nginx
```
<img width="2311" height="716" alt="image" src="https://github.com/user-attachments/assets/48b05d18-f561-4860-b047-b929159b92a2" />

3. Sekarang, kita dapat mengakses web pada browser seperti berikut

<img width="1431" height="694" alt="image" src="https://github.com/user-attachments/assets/ecc3b958-bd57-42d8-aab1-725877e8ae30" />

### Database

Sebagian besar aplikasi web membutuhkan database untuk menyimpan data yang dibutuhkan. Pada kali ini, kita akan menggunakan database postgresql pada `Azure Database for PostgreSQL flexible servers`.

#### Launch Azure PostgreSQL Database 

Pilih create `Azure Database for PostgreSQL flexible servers` lalu isi dengan konfigurasi berikut: <br>
<img width="1366" height="1170" alt="image" src="https://github.com/user-attachments/assets/84309c57-7003-4092-ae6f-954fec07d297" />

Pada bagian `Compute + storage`, buka menu `configure server` untuk mengatur spesifikasi penyimpanan database. Pilih compute size dengan tipe `Standard_B1ms (1 vCore, 2 GiB memory, 640 max iops)`. Dengan memilih konfigurasi ini, penggunaan sumber daya jadi lebih efisien sehingga biaya operasional database menjadi lebih rendah. Setelah itu, tekan save untuk menyimpan perubahan.
<img width="1368" height="764" alt="image" src="https://github.com/user-attachments/assets/eb90f67a-1f8e-4ed1-9f17-21863150acbb" />

Lalu, pada bagian `Authentication`, pilih `PostgreSQL authentication only` dan masukkan username dan password database yang diinginkan.
<img width="1359" height="811" alt="image" src="https://github.com/user-attachments/assets/4b6cb2b3-a170-4f93-a865-2894e26788e4" />

Setelah itu, tekan tombol `next: networking` lalu pilih connectivity method `private access` dan masukkan VNet dan subnet private yang telah dibuat sebelumnya. 
<img width="1361" height="926" alt="image" src="https://github.com/user-attachments/assets/2fabcc67-c7b7-485b-8603-169da77193d4" />

Biarkan konfigurasi lainnya tetap secara default dan tekan review + create. 

#### Mengakses database dari web server

Karena berada di 1 VNet dan kita telah mengatur security group, maka web server seharusnya dapat mengakses private IP database. Untuk mengakses, kita perlu menginstal postgresql pada web server.

<center>
<img width="1409" height="194" alt="image" src="https://github.com/user-attachments/assets/ad323220-cb4d-4c79-a056-2996755c2ecf" />
</center>

Selanjutnya, kita dapat mencoba akses ke database menggunakan `psql` sesuai dengan nama host/endpoint dan portnya. Pertama, masuk ke Azure PostgreSQL database yang telah dibuat, lalu masuk ke menu `connect` dan salin script untuk menghubungkan VM dengan database.

<center>
<img width="1381" height="1150" alt="image" src="https://github.com/user-attachments/assets/8ec5f1db-6b99-49b0-8998-13303235e854" />
<img width="1371" height="282" alt="image" src="https://github.com/user-attachments/assets/c735638f-6330-4549-ba14-ec272b349d82" />
</center>
Selamat, anda telah berhasil membuat database di Azure dan mengaksesnya dari VM.
<img width="1821" height="332" alt="image" src="https://github.com/user-attachments/assets/584dcc68-96a9-4db3-8d1f-9a1c695fa0c9" />


## Deploy Aplikasi

Setelah berhasil membangun infrastruktur di AWS, kita akan mencoba melakukan deploy aplikasi. Aplikasi yang akan dideploy merupakan aplikasi CRUD sederhana dan dapat diclone dari https://github.com/arizki787/API-Project.

<center>
<img width="1557" height="304" alt="image" src="https://github.com/user-attachments/assets/a1faced3-6eb6-4688-a88e-444e259ca00f" />
</center>

### Konfigurasi Aplikasi

Aplikasi yang akan dideploy merupakan aplikasi nodejs sehingga kita perlu menginstall `nodejs` dan `npm` terlebih dahulu. Jalankan script berikut:

```
cd API-Project/
sudo apt install nodejs npm -y
```

<center>
<img width="1327" height="195" alt="image" src="https://github.com/user-attachments/assets/2e0b689b-8793-4b4e-9b00-e408414c7141" />
</center>

Selanjutnya, buat database beserta tabelnya dengan terlebih dahulu mengakses postgresql di Azure Database. Buat database `school` dan tabel `student` dengan query berikut:

```R
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

Jika berhasil, maka akan terlihat seperti ini.
<center>
<img width="2872" height="1372" alt="image" src="https://github.com/user-attachments/assets/6dd3f4da-3ed6-446e-8655-6dd851e20424" />

</center>

Kita juga akan membuat file `.env` untuk mendefinisikan `environment variable` yang diperlukan dengan template sebagai berikut. Jangan lupa untuk mengubah `DB EndPoint`, `DB User`, dan `DB Password` sesuai dengan Database yang telah kalian buat.

```R
DB_HOST=<DB EndPoint>
DB_USER=<DB User>
DB_PORT=5432
DB_PASSWORD=<DB Password>
DB_NAME=school
```

Dengan menggunakan `nano`, kita dapat menulis file `.env` seperti berikut.

<center>
   <img width="1422" height="151" alt="image" src="https://github.com/user-attachments/assets/595acc5b-332e-4e34-aec6-704124a470cb" />

<img width="1419" height="229" alt="image" src="https://github.com/user-attachments/assets/8a30df16-a4cf-4f75-97b3-8bb479b701bc" />

</center>

Setelah itu, install modul dotenv agar aplikasi Node.js di dalam VM dapat membaca konfigurasi database

```
npm install dotenv
```

### Port Forwarding pada NGINX

Sebenarnya, aplikasi di atas sudah bisa dijalankan dengan command `npm start`, maka server akan mendengar pada port 3000. Namun, pada praktiknya, tidak lazim kita membuka port secara custom. Biasanya port yang dibuka untuk akses api ada pada port 80 (http) atau 443 (https). Hal tersebut sesuai dengan aturan `security group` pada instance `webServer` yang hanya menerima koneksi ke port 22 (ssh) dan 80 (http). Oleh karena itu, kita perlu melakukan port forwarding dari port 80 ke port 3000 di mana aplikasi kita berjalan.

1. Buat salinan konfigurasi server Nginx dengan command

```R
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/backup
```

2. Modifikasi file `/etc/nginx/sites-available/default` dengan menggunakan `nano` menjadi sebagai berikut:

```R
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

```R
sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/
```

4. Restart Nginx dan jalankan aplikasi

```R
sudo systemctl restart nginx
node ./index.js
```

5. Akses web server pada browser, maka aplikasi kita telah dapat terlihat.
<center>
<img width="2872" height="1372" alt="image" src="https://github.com/user-attachments/assets/d47ca755-5a95-42ab-af79-c5f47b007480" />
</center>

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

<img width="1396" height="561" alt="image" src="https://github.com/user-attachments/assets/7027d98e-cff6-4331-ac26-19ce1c3952c5" />


2. Restart daemon dan jalankan service server. Periksa juga apakah layanan server telah berjalan.

```R
sudo systemctl daemon-reload
sudo systemctl start server
sudo systemctl status server
```

3. Akses kembali public ip. Kalian akan mendapati bahwa server berjalan walaupun kalian tidak mengeksekusi npm start pada root project. Dengan demikian, project kalian sudah berjalan pada background.
