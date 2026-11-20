
# Laravel + Filament v5 Environment (GitHub Codespaces)

Repository ini berisi konfigurasi **Dev Containers** untuk menjalankan Laravel dan Filament v5 di GitHub Codespaces secara otomatis. Semua ekstensi PHP yang dibutuhkan (termasuk `intl`, `gd`, `pdo_mysql`) akan terpasang saat container dibangun, sehingga Anda tidak perlu lagi mengompilasi ekstensi secara manual.

## 📌 Latar Belakang
Instalasi Laravel dan Filament sering gagal karena environment standar Codespaces tidak memiliki ekstensi PHP wajib. Dengan `Dockerfile` kustom, kita memastikan ekstensi `intl`, `zip`, `gd`, `bcmath`, dan `pdo_mysql` aktif sejak awal.


## 🛠️ Konfigurasi Environment

### 1. `.devcontainer/Dockerfile`

```dockerfile
FROM mcr.microsoft.com/devcontainers/php:8.4

# Install dependensi sistem untuk kompilasi ekstensi
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get install -y libicu-dev libzip-dev libpng-dev libjpeg-dev libfreetype6-dev libwebp-dev \
    && apt-get clean -y && rm -rf /var/lib/apt/lists/*

# Konfigurasi dan install ekstensi PHP wajib Laravel & Filament 5
RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j$(nproc) intl zip gd bcmath pdo_mysql
```

### 2. `.devcontainer/devcontainer.json`
```json
{
  "name": "Laravel Filament PHP 8.4",
  "build": {
    "dockerfile": "Dockerfile"
  },
  "forwardPorts": [8000],
  "postCreateCommand": "composer install",
  "remoteEnv": {
    "XDEBUG_MODE": "off"
  }
}
```

---

## 🚀 Cara Setup di Repository Baru

1. **Buat Repository Baru** di GitHub (centang opsi "Add a README file" agar tombol Codespace langsung muncul).
2. Buka repository tersebut, klik tombol **Code** -> **Codespaces** -> **Create codespace on main**.
3. Setelah Codespace terbuka, buat folder `.devcontainer`.
4. Buat file `Dockerfile` dan `devcontainer.json` di dalamnya (salin isi dari bagian Konfigurasi di atas).
5. Tekan `Ctrl+Shift+P` -> pilih **Codespaces: Rebuild Container**.
6. Tunggu proses build selesai (sekitar 2-5 menit) hingga terminal baru terbuka.

### Verifikasi Environment
Jalankan perintah berikut untuk memastikan ekstensi sudah aktif:
```bash
php -m | grep -E "intl|gd|pdo_mysql|zip|bcmath"
```
*(Harus muncul: bcmath, gd, intl, pdo_mysql, zip)*

---

## 📦 Instalasi Laravel & Filament v5

Karena folder root sudah berisi `.devcontainer` dan `README.md`, kita akan menginstal Laravel di **sub-folder baru** bernama `app`.

### 1. Buat Project Laravel di Sub-folder
```bash
composer create-project laravel/laravel app
cd app
```

### 2. Install Filament v5
```bash
composer require filament/filament:"^5.0" -W
```

### 3. Setup Panel Admin
```bash
php artisan filament:install --panels
```

### 4. Buat Akun Admin
```bash
php artisan make:filament-user
```
*(Ikuti instruksi untuk memasukkan nama, email, dan password).*

### 5. Jalankan Server
```bash
php artisan serve
```
Buka tab **Ports** di Codespaces, klik URL yang tertera (port `8000`). Tambahkan `/admin` di belakang URL untuk mengakses panel Filament.

---

## ⚠️ Troubleshooting

### Masalah URL / Proxy Codespaces
Jika Filament tidak muncul atau redirect berulang, edit file `.env` di dalam folder `app` dan `bootstrap/app.php`:

**File `.env`:**
```env
APP_URL=https://<url-codespaces-anda>.app.github.dev
ASSET_URL="${APP_URL}"
```

**File `bootstrap/app.php`:**
```php
->withMiddleware(function (Middleware $middleware) {
    $middleware->trustProxies(at: '*'); 
})
```

### Membersihkan Cache Laravel
Setiap kali mengubah `.env` atau konfigurasi, jalankan:
```bash
php artisan optimize:clear
```

### Jika Ekstensi Tiba-tiba Hilang
Jika muncul error `Unable to load dynamic library`, lakukan:
1. `Ctrl+Shift+P` -> **Codespaces: Rebuild Container**.
2. Cek ulang dengan `php -m`.

---

## 📂 Struktur Folder
```text
├── .devcontainer/
│   ├── Dockerfile
│   └── devcontainer.json
├── app/            <-- Folder Laravel & Filament
│   ├── app/
│   ├── database/
│   ├── public/
│   └── ...
└── README.md
```

---
*Dibuat berdasarkan pengalaman setup environment Laravel & Filament v5 di GitHub Codespaces.*
```

---

### 
