# Final Project LBE NCC 2025

## Tujuan

Membangun aplikasi sederhana berbasis cloud menggunakan beberapa layanan utama Azure: Virtual Machine, Blob Storage, Azure Functions, dan Azure Database for PostgreSQL Flexible Server.

### Arsitektur

- VM → menjalankan frontend web.
- Blob Storage → menyimpan file statis (gambar, dokumen, dsb).
- PostgreSQL Flexible Server → menyimpan data aplikasi..
- Azure Functions → menjalankan backend API.

![Arsitektur Final Project](assets/fp-lbe-ncc.png)

### Langkah-langkah

1. Deploy Frontend di Virtual Machine

- Buat resource group baru untuk final project.
- Deploy VM dengan OS Linux (Ubuntu).
- Install nginx dan postgresql-client.
- Clone repository frontend yang telah disediakan `https://github.com/hamasfaa/crud-lbe-ncc-2025/tree/frontend`
- Pastikan membuka port 22 (SSH), 80 (HTTP), dan 443 (HTTPS).
- Konfigurasikan nginx dengan file berikut:

```
server {
    listen 80;
    server_name _;

    root /var/www/[nama-folder];
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

> Ganti [nama-folder] dengan folder tempat menyimpan file frontend.

> Jangan lupa update `env.js` di frontend agar mengarah ke URL Azure Functions yang nanti dibuat.

2. Deploy Database PostgreSQL Flexible Server

- Deploy Azure Database for PostgreSQL Flexible Server.
- Gunakan VNet Integration agar aman terhubung dengan VM dan Azure Functions.
- Buat database dan tabel dengan struktur berikut:
  | Kolom | Tipe Data | Keterangan |
  |---------|-------------|------------------------|
  | id | SERIAL | Primary key, auto increment |
  | nama | VARCHAR(100)| Nama user, wajib diisi |
  | foto_url| VARCHAR(500)| URL foto yang tersimpan di Blob |

3. Deploy Backend dengan Azure Functions

- Clone repository backend yang telah disediakan: `https://github.com/hamasfaa/crud-lbe-ncc-2025/tree/backend`
- Lengkapi bagian TODO yang ada di kode.
- Tambahkan environment variables berikut untuk deployment pada Azure Functions:

  - DB_HOST
  - DB_USER
  - DB_PASSWORD
  - DB_NAME
  - DB_PORT
  - AZURE_STORAGE_CONNECTION_STRING
  - BLOB_CONTAINER_NAME

Jika ingin mengembangkan backend secara lokal sebelum deploy:

    1. Pastikan sudah menginstal Azure Functions Core Tools v4.
    2. Sesuaikan konfigurasi pada file `local.settings.json` sesuai environment yang akan digunakan di Azure Functions.
    3. Jalankan perintah `func start` atau gunakan fitur **Run and Debug** pada VSCode.
    4. Azure Functions akan berjalan di http://localhost:7071 sehingga bisa diuji secara lokal.

4. Deploy Blob Storage

- Saat membuat Azure Functions, Azure biasanya otomatis membuat Storage Account yang diperlukan untuk menyimpan state, logs, dan artifacts. Kalian bisa menggunakan Storage Account ini untuk menyimpan file aplikasi.
- Tambahkan Blob Container untuk menyimpan file upload dari aplikasi.
