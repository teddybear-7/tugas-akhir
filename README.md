# Kulinerku - Peta Kuliner Semarang

![Kulinerku Banner](docs/banner.png)

Aplikasi web untuk menemukan dan mem Contribution tempat kuliner di Semarang. Dibangun dengan CodeIgniter 4 dan Leaflet Maps.

---

## 📋 Daftar Isi

- [Instalasi](#instalasi--setup)
- [Konfigurasi](#konfigurasi)
- [Akun Demo](#akun-demo)
- [Fitur Utama](#fitur-utama)
- [Screenshot](#screenshot)
- [API Documentation](#api-documentation)

---

## Instalasi & Setup

### Prasyarat
- PHP ≥ 8.2
- MySQL/MariaDB
- Composer
- Ekstensi PHP: `intl`, `mbstring`, `curl`, `gd`, `mysqli`

### Langkah instalasi

```bash
# 1. Install Codeigniter4
composer create-project codeigniter4/appstarter kuliner_map
cd kuliner_map

# 2. Install dependency
composer install

# 3. Siapkan environment
cp env .env
# lalu edit .env — sesuaikan koneksi database & KULINER_API_KEY

# 4A. Opsi A — Import SQL dump langsung
mysql -u root -p -e "CREATE DATABASE kuliner_map"
mysql -u root -p kuliner_map < kuliner_map.sql

# 4B. Opsi B — Migration + Seeder (direkomendasikan, lebih bersih & bisa rollback)
php spark migrate
php spark db:seed MainSeeder

# 5. Jalankan server lokal
php spark serve
# akses di http://localhost:8080
```

---

## Konfigurasi

### File `.env`

Setel koneksi database:

```env
database.default.hostname = localhost
database.default.database = kuliner_map
database.default.username = root
database.default.password = your_password
```

Setel API Key untuk akses API pihak ketiga:

```env
KULINER_API_KEY = your-secure-api-key
```

### Cache

Pastikan folder `writable/cache` dapat ditulis:

```bash
chmod -R 777 writable
```

---

## Akun Demo

| Role        | Username        | Password          | URL Login |
|-------------|-----------------|------------------|-----------|
| Admin       | `admin`         | `admin123`       | `/login` (tab Admin) |
| Kontributor | `budi_semarang` | `kontributor123`  | `/login` (tab Kontributor) |
| Kontributor | `siti_kuliner`  | `kontributor123`  | `/login` (tab Kontributor) |

---

## Fitur Utama

### 🗺️ Peta Interaktif
- Tampilan peta dengan Leaflet Maps
- Custom markers untuk setiap tempat kuliner
- Klik marker untuk melihat popup detail
- Filter berdasarkan area yang terlihat di peta

### 🔍 Pencarian & Filter
- **Search** - Cari berdasarkan nama, alamat, atau jenis makanan
- **Filter Kategori** - Warteg, Kafe & Kopi, Street Food, dll.
- **Filter Rating** - Rating minimum 3+, 4+, 4.5+
- **Filter Harga** - Range harga Rp 10.000 - 100.000+
- **Sort** - Terdekat, Rating Tertinggi, A-Z, Review Terbanyak

### 👤 Role & Akses

| Fitur | Pengunjung | Kontributor | Admin |
|-------|------------|-------------|-------|
| Lihat Peta | ✅ | ✅ | ✅ |
| Lihat Detail | ✅ | ✅ | ✅ |
| Baca Review | ✅ | ✅ | ✅ |
| Submit Tempat | ❌ | ✅ | ✅ |
| Tulis Review | ❌ | ✅ | ✅ |
| Favorit | ❌ | ✅ | ✅ |
| Moderasi | ❌ | ❌ | ✅ |
| Kelola Data | ❌ | ❌ | ✅ |

### 📱 UI/UX
- **Responsif** - Berfungsi di desktop, tablet, dan mobile
- **Konsisten** - Layout dan styling unified di semua halaman
- **Navigasi Jelas** - Topbar dengan search dan link navigasi

### 🔗 API
- RESTful API untuk integrasi pihak ketiga
- Endpoint publik (tanpa API key)
- Endpoint dengan API Key untuk display/poster kampus

---

## Screenshot

### 🏠 Halaman Utama - Peta

![Halaman Peta](docs/screenshots/01-homepage.png)

*Halaman utama dengan peta interaktif, sidebar daftar tempat, filter, dan pagination.*

### 📍 Detail Tempat Kuliner

![Detail Kuliner](docs/screenshots/02-detail.png)

*Halaman detail dengan foto gallery, info tempat, rating, review, dan peta mini.*

### 🔐 Halaman Login

![Login](docs/screenshots/03-login.png)

*Login terpadu untuk Admin dan Kontributor dalam satu halaman.*

### 👨‍💼 Dashboard Admin

![Dashboard Admin](docs/screenshots/04-admin-dashboard.png)

*Dashboard admin dengan statistik, grafik aktivitas, dan tabel data.*


---

## API Documentation

Dokumentasi lengkap tersedia di `/api/docs`.

### Endpoint Publik (Tanpa API Key)

| Method | Endpoint | Deskripsi |
|--------|----------|-----------|
| GET | `/kuliner/getData` | Daftar kuliner dengan filter |
| GET | `/kuliner/detail/{id}` | Detail satu kuliner |
| GET | `/kuliner/geocode` | Konversi alamat ke koordinat |

### Endpoint dengan API Key

| Method | Endpoint | Deskripsi |
|--------|----------|-----------|
| GET | `/api/kuliner` | Daftar kuliner (approved) |
| GET | `/api/kuliner/{id}` | Detail kuliner lengkap |
| GET | `/api/kategori` | Daftar kategori |
| GET | `/api/tags` | Daftar tag |

### Contoh Penggunaan

```bash
# Daftar kuliner dengan filter
curl "http://localhost:8080/api/kuliner?lat=-7.0498&lng=110.4380&radius=2" \
     -H "X-API-KEY: kuliner-demo-key"

# Detail kuliner
curl "http://localhost:8080/api/kuliner/5" \
     -H "X-API-KEY: kuliner-demo-key"
```

---

## Struktur Proyek

```
kuliner_map/
├── app/
│   ├── Config/          # Konfigurasi CodeIgniter
│   ├── Controllers/     # Logic aplikasi
│   │   ├── Admin/       # Controller Admin
│   │   └── ...
│   ├── Models/          # Model database
│   └── Views/           # Template view
│       ├── admin/       # View halaman admin
│       ├── layout/      # Layout utama
│       └── public/      # View halaman publik
├── docs/
│   └── screenshots/     # Screenshot fitur
├── writable/           # Cache, logs, uploads
├── .env                # Environment variables
└── README.md
```

---

## Teknologi

|----------|-----|
| CodeIgniter 4 | PHP Framework |
| Leaflet Maps | Peta Interaktif |
| Bootstrap 5 | CSS Framework |
| Plus Jakarta Sans | Typography |
| MySQL | Database |
| Nominatim | Geocoding Service |

