# Laravel + Filament v5 Environment (GitHub Codespaces)

Repository ini berisi konfigurasi **Dev Containers** untuk menjalankan Laravel dan Filament v5 di GitHub Codespaces secara otomatis. Semua ekstensi PHP yang dibutuhkan (termasuk `intl`, `gd`, `pdo_mysql`) akan terpasang saat container dibangun, sehingga Anda tidak perlu lagi mengompilasi ekstensi secara manual.

## 📌 Latar Belakang
Instalasi Laravel dan Filament sering gagal karena environment standar Codespaces tidak memiliki ekstensi PHP wajib. Dengan `Dockerfile` kustom, kita memastikan ekstensi `intl`, `zip`, `gd`, `bcmath`, dan `pdo_mysql` aktif sejak awal.

---

## 🛠️ Konfigurasi Environment

### 1. `.devcontainer/Dockerfile`
```dockerfile
FROM mcr.microsoft.com/devcontainers/php:8.4

# Install dependensi sistem untuk kompilasi ekstensi
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get install -y libicu-dev libzip-dev libpng-dev libjpeg-dev libfreetype6-dev libwebp-dev \
    && apt-get clean -y && rm -rf /var/lib/apt/lists/*

# Konfigurasi dan install ekstensi PHP wajib Laravel & Filament
RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j$(nproc) intl zip gd bcmath pdo_mysql