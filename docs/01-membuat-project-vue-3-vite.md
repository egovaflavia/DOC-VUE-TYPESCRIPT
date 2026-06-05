# Tutorial Vue 3 TypeScript #1: Membuat Project Vue 3 (Vite)

Pada bagian pertama ini, kita akan belajar cara membuat project Vue 3 baru menggunakan Vite dengan template TypeScript.

## 1. Apa itu Vite?
**Vite** adalah *build tool* modern yang dirancang untuk pengembangan frontend yang sangat cepat dan efisien. Vite dikembangkan oleh Evan You (pencipta Vue.js) untuk menggantikan build tool lama seperti Webpack pada proses development. Vite menggunakan ES Modules (ESM) bawaan browser agar proses start server dan *Hot Module Replacement* (HMR) berjalan instan.

---

## 2. Prasyarat & Instalasi Node.js
Sebelum memulai, pastikan Anda telah menginstal Node.js di komputer Anda. Jika belum, silakan unduh di [Situs Resmi Node.js](https://nodejs.org/).

Untuk memverifikasi instalasi Node.js dan NPM, jalankan perintah berikut di Terminal atau Command Prompt (CMD):

```bash
# Cek versi Node.js
node --version

# Cek versi NPM
npm --version
```

> [!IMPORTANT]
> Pastikan juga Anda sudah menyiapkan backend REST API (misalnya menggunakan Laravel) yang akan menyediakan endpoint data produk untuk dikonsumsi oleh aplikasi Vue 3 ini.

---

## 3. Membuat Project Vue 3 TypeScript
Pilihlah folder direktori tempat Anda ingin menyimpan proyek, buka terminal di folder tersebut, lalu jalankan perintah berikut:

```bash
npm create vite@6.2.0 crud-vue3-ts -- --template vue-ts
```

**Penjelasan Perintah:**
- `create vite@6.2.0`: Menentukan versi generator Vite yang digunakan.
- `crud-vue3-ts`: Nama folder project yang akan dibuat.
- `--template vue-ts`: Menggunakan template Vue 3 yang sudah dikonfigurasi dengan TypeScript.

---

## 4. Menjalankan Project
Setelah proses pembuatan struktur project selesai, masuklah ke direktori project Anda dan instal seluruh dependencies yang dibutuhkan:

```bash
# Masuk ke folder project
cd crud-vue3-ts

# Menginstal seluruh dependency (package.json)
npm install
```

Tunggu hingga proses instalasi selesai (memerlukan koneksi internet). Setelah selesai, jalankan server development lokal dengan perintah:

```bash
npm run dev
```

Jika berhasil, Vite akan menjalankan server lokal pada port default `5173`. Anda dapat mengaksesnya di browser Anda melalui alamat:
[http://localhost:5173](http://localhost:5173)

---

## 5. Rangkuman Perintah Cepat
| Langkah | Deskripsi Perintah |
| :--- | :--- |
| **Inisialisasi** | `npm create vite@6.2.0 crud-vue3-ts -- --template vue-ts` |
| **Navigasi** | `cd crud-vue3-ts` |
| **Install** | `npm install` |
| **Run** | `npm run dev` |
