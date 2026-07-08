# WAD Task Manager - Frontend

Aplikasi frontend (Client-side) untuk mengelola daftar pekerjaan (Task Manager). Proyek ini dibangun menggunakan **React (Vite)** dan terhubung dengan RESTful API dari backend untuk mengelola autentikasi dan data tugas-tugas secara *real-time*.

🌐 **Live Demo:** [https://syahrulawaludin.my.id](https://syahrulawaludin.my.id)

---

## 🚀 Teknologi yang Digunakan

| Teknologi | Keterangan |
|---|---|
| **React 19** | UI Library utama |
| **Vite** | Build Tool & Dev Server |
| **React Router DOM v7** | Routing halaman (SPA) |
| **Axios** | HTTP Client dengan *Interceptors* token otomatis |
| **React Hook Form** | Manajemen state form dan validasi |
| **Socket.IO Client** | Koneksi WebSocket untuk update *real-time* |

---

## ✨ Fitur Utama

1. **Autentikasi (Auth)**
   - Login & Register
   - JWT Access Token & Refresh Token terintegrasi secara otomatis via Axios Interceptors.
   - Proteksi Rute (Halaman login tidak bisa diakses jika sudah masuk, halaman tasks tidak bisa diakses jika belum masuk).

2. **Manajemen Task (CRUD)**
   - Melihat daftar task milik pengguna.
   - Menambah task baru.
   - Mengedit (update) data task seperti judul, deskripsi, status, prioritas, dan tenggat waktu.
   - Menghapus task.

3. **Filter & Navigasi**
   - Filter task berdasarkan status (`TODO`, `IN_PROGRESS`, `DONE`).

4. **Real-Time Updates (Socket.IO)**
   - Koneksi otomatis ke backend melalui `window.location.origin` (mendukung HTTP maupun HTTPS).
   - Menerima notifikasi real-time saat task dibuat, diperbarui, atau dihapus.

---

## ⚙️ Prasyarat (Prerequisites)

Sebelum menjalankan aplikasi ini secara lokal, pastikan Anda telah menginstal:
- [Node.js](https://nodejs.org/en/) (Versi 18+ direkomendasikan)
- [npm](https://www.npmjs.com/) (Biasanya sudah terpasang bersama Node.js)
- Server Backend berjalan di lokal (menggunakan port `3000`).

---

## 🛠 Instalasi dan Menjalankan Proyek (Development)

1. **Clone repositori:**
   ```bash
   git clone <URL_REPOSITORI_INI>
   cd wad-frontend
   ```

2. **Instal seluruh dependensi:**
   ```bash
   npm install
   ```

3. **Menjalankan Development Server:**
   ```bash
   npm run dev
   ```
   Aplikasi akan berjalan di `http://localhost:5173/`. Vite secara otomatis mem-proxy request yang diawali dengan `/api` ke server backend (localhost:3000).

4. **Build untuk Production:**
   ```bash
   npm run build
   ```
   Hasil build akan tersimpan di folder `dist/`.

---

## 📁 Struktur Folder Utama

```
src/
├── components/     # Komponen UI yang digunakan berulang (Navbar, TaskCard, TaskForm, ProtectedRoute)
├── contexts/       # React Context API (AuthContext & SocketContext untuk global state)
├── hooks/          # Custom React Hooks
├── lib/            # Konfigurasi library pihak ketiga (Instansiasi Axios & Interceptor)
├── pages/          # Komponen halaman (LoginPage, RegisterPage, TasksPage)
├── services/       # Service untuk memanggil endpoint API (auth.service, task.service)
├── App.jsx         # Root component & Konfigurasi routing
├── index.css       # Styling aplikasi (Vanilla CSS)
└── main.jsx        # Entry point aplikasi React
```

---

## 🔐 Konfigurasi Proxy (Vite)

Pada file `vite.config.js`, proyek ini telah diatur untuk menggunakan proxy ke backend guna menghindari masalah CORS selama pengembangan lokal:

```javascript
export default defineConfig({
  plugins: [react()],
  server: {
    proxy: {
      '/api': {
        target: 'http://localhost:3000',
        changeOrigin: true,
      }
    }
  }
})
```
> **Catatan:** Konfigurasi proxy ini **hanya berlaku saat development** (`npm run dev`). Di production, Nginx Reverse Proxy yang menangani routing antara frontend dan backend dalam satu domain.

---

## 🎨 Tampilan UI / Styling

Proyek ini menggunakan standard **Vanilla CSS** (di dalam `index.css` dan `App.css`) untuk menyusun layout dan desain visual tanpa ketergantungan pada *framework CSS* pihak ketiga.

---

## 🧪 Panduan Pengetesan (Manual / UAT)

Aplikasi dapat diuji secara *End-to-End* (E2E) melalui browser. Berikut adalah skenario pengujiannya:

### Akun Default (Seed Data)
| Email | Password | Role |
|---|---|---|
| `budi@example.com` | `P@ssw0rd!` | USER |
| `siti@example.com` | `P@ssw0rd!` | USER |
| `admin@example.com` | `P@ssw0rd!` | ADMIN |

### Skenario Pengujian

#### 1. Pendaftaran & Autentikasi (Register / Login)
- Buka `https://syahrulawaludin.my.id/register`. Masukkan nama, email, dan password Anda.
- Setelah berhasil mendaftar, Anda akan diarahkan untuk Login di halaman `/login`.

#### 2. State Kosong (Empty State) & Profil
- Jika ini adalah akun baru, halaman `Tasks` akan menampilkan layar bersih dengan pesan *"Belum ada task. Buat task pertamamu!"*.
- Tekan menu **Profil** di Navbar atas untuk melihat detail akun yang sedang masuk.

#### 3. Pembuatan & Pembaruan (Create & Edit Task)
- **Create**: Klik tombol **+ Task Baru**. Isi formulir judul, deskripsi, status, prioritas, dan tenggat waktu. Setelah sukses, task akan langsung muncul di daftar.
- **Edit**: Pada *Task Card*, klik ikon pensil ✏️ untuk mengubah detailnya.

#### 4. Menghapus Task (Delete) & Logout
- **Delete**: Klik ikon tempat sampah 🗑️ pada *Task Card*. Konfirmasi akan muncul sebelum penghapusan permanen.
- **Logout**: Klik tombol **Keluar** di pojok kanan atas. Sesi akan berakhir dan token di-revoke dari server.

---

## ☁️ Deployment & CI/CD

Proyek ini telah dikonfigurasi untuk ter-deploy secara otomatis ke VPS menggunakan **GitHub Actions**.

### Arsitektur Deployment

```
Internet (HTTPS)
      │
      ▼
┌─────────────────────────────┐
│  Nginx Reverse Proxy        │  ← syahrulawaludin.my.id (Port 443/80)
│  (SSL/TLS via Certbot)      │
└──────────┬──────────────────┘
           │
    ┌──────┴──────┐
    │             │
    ▼             ▼
Frontend      Backend (API)
(PM2:3001)    (PM2:3000)
```

### Detail Infrastruktur
| Komponen | Detail |
|---|---|
| **Cloud Provider** | Biznet Gio NEO Lite |
| **OS** | Ubuntu 22.04 LTS |
| **Web Server** | Nginx (Reverse Proxy) |
| **SSL/TLS** | Let's Encrypt (via Certbot) - Auto-renew |
| **Domain** | `https://syahrulawaludin.my.id` |
| **Proses Manager** | PM2 |
| **Port Frontend** | `3001` (internal, diproxy oleh Nginx) |

### Alur CI/CD
1. Setiap kali kode baru di-push ke branch `main`, workflow `.github/workflows/deploy.yml` akan berjalan.
2. GitHub Actions masuk ke VPS secara otomatis menggunakan SSH Key (Base64 Secret).
3. Mengeksekusi perintah: `git pull`, `npm install`, `npm run build`, dan `pm2 restart frontend`.
4. Website langsung ter-update di server publik tanpa perlu campur tangan manual!

### GitHub Secrets yang Diperlukan
| Secret | Keterangan |
|---|---|
| `SSH_PRIVATE_KEY` | Private key SSH (di-encode Base64) |
| `VPS_HOST` | Alamat IP VPS (`103.93.135.88`) |
| `VPS_USER` | Username VPS (`syahrulaw`) |
| `VPS_PORT` | Port SSH (default: `22`) |
