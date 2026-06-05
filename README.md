# 🚀 Panduan Belajar & Rangkuman: Vue 3 TypeScript + REST API

Selamat datang! Dokumentasi ini dirancang khusus untuk membantu Anda memahami pembuatan aplikasi CRUD (Create, Read, Update, Delete) menggunakan **Vue 3**, **TypeScript**, dan **Vite** dengan backend REST API (Laravel) berdasarkan seri tutorial dari **SantriKoding**.

Dokumentasi ini ditulis dengan bahasa yang sangat santai, terstruktur, dan **semudah mungkin dipahami** bahkan oleh pemula sekalipun.

---

## 💡 Analogi Sederhana: Apa Bedanya TypeScript vs JavaScript?

Banyak orang bingung kenapa harus menggunakan TypeScript jika JavaScript biasa sudah cukup. Mari kita gunakan analogi sederhana ini:

* **JavaScript biasa (JS) itu seperti "Naik Motor Tanpa Helm" 🏍️**
  * **Kelebihan:** Sangat cepat, praktis, tinggal starter langsung jalan tanpa ribet pasang perlengkapan di awal.
  * **Bahayanya:** Jika di tengah jalan ada lubang atau batu (bug/error), Anda baru sadar setelah menabraknya dan jatuh berantakan (aplikasi langsung crash di depan pengguna).
  
* **TypeScript (TS) itu seperti "Naik Motor Lengkap dengan Helm & Pelindung" 🪖**
  * **Kekurangannya:** Di awal sedikit repot karena harus memakai perlengkapan dulu (menulis tipe data, interface, casting variabel).
  * **Keamanannya:** Jika ada masalah di jalan, Anda aman karena helm melindungi Anda. Bahkan di TypeScript, jika ada potensi lubang di depan, motor Anda akan otomatis berbunyi alarm sebelum Anda menabraknya (editor VS Code langsung memberi garis merah tanda error sebelum kode dijalankan).

### 3 Keuntungan Nyata TypeScript untuk Pemula:
1. **Salah Ketik Langsung Ketahuan:** Jika Anda salah mengetik nama variabel (misal menulis `product.tittle` padahal yang benar `product.title`), TypeScript langsung mencoret merah kata tersebut di editor Anda. Anda tidak perlu membuang waktu membuka browser untuk tahu kenapa datanya tidak muncul.
2. **Auto-Complete Pintar (Intellisense):** Saat Anda mengetik nama objek diikuti tanda titik (`product.`), text editor Anda langsung menampilkan menu pilihan kolom yang tersedia (`id`, `title`, `price`, `stock`). Anda tidak perlu lagi bolak-balik melihat database atau struktur API.
3. **Membantu Mengingat Struktur Data:** Dengan mendefinisikan *Interface*, Anda secara tidak langsung mendokumentasikan bentuk data aplikasi Anda, sehingga kode Anda sangat rapi dan mudah dibaca orang lain.

*Baca penjelasan komparasi kodenya secara mendalam di sini: [Perbandingan Lengkap TypeScript vs JavaScript](file:///f:/DOC/DOC-VUE-TYPESCRIPT/docs/typescript-vs-javascript.md)*

---

## 📁 Menu Navigasi Belajar (Langkah demi Langkah)

Silakan klik salah satu materi di bawah ini untuk mulai belajar. Semua materi telah diringkas secara praktis lengkap dengan penjelasan kode baris demi baris:

1. **[Part 1: Membuat Project Vue 3 (Vite)](file:///f:/DOC/DOC-VUE-TYPESCRIPT/docs/01-membuat-project-vue-3-vite.md)**
1. **[Part 1: Membuat Project Vue 3 (Vite)](./docs/01-membuat-project-vue-3-vite.md)**
   *Belajar menginstal Node.js, membuat project Vue 3 TypeScript baru menggunakan Vite, dan menjalankan server lokal.*
   
2. **[Part 2: Instalasi & Konfigurasi Vue Router](file:///f:/DOC/DOC-VUE-TYPESCRIPT/docs/02-install-dan-konfigurasi-vue-router.md)**
2. **[Part 2: Instalasi & Konfigurasi Vue Router](./docs/02-install-dan-konfigurasi-vue-router.md)**
   *Belajar membuat halaman Single Page Application (SPA) tanpa reload, membuat halaman Home, mendesain navbar dengan Bootstrap 5 CDN, dan merender halaman.*
   
3. **[Part 3: Menampilkan Data dari REST API](file:///f:/DOC/DOC-VUE-TYPESCRIPT/docs/03-menampilkan-data-dari-rest-api.md)**
3. **[Part 3: Menampilkan Data dari REST API](./docs/03-menampilkan-data-dari-rest-api.md)**
   *Belajar menginstal Axios, membuat konfigurasi endpoint API global, mendefinisikan tipe data Interface Product, dan menampilkan daftar produk dalam tabel.*
   
4. **[Part 4: Tambah Data & Upload Gambar](file:///f:/DOC/DOC-VUE-TYPESCRIPT/docs/04-insert-data-dengan-rest-api.md)**
4. **[Part 4: Tambah Data & Upload Gambar](./docs/04-insert-data-dengan-rest-api.md)**
   *Belajar membuat form input produk baru, menangani input file biner (gambar), mengirim data menggunakan FormData ke REST API, dan menangani error validasi dari server.*
   
5. **[Part 5: Mengubah & Update Data](file:///f:/DOC/DOC-VUE-TYPESCRIPT/docs/05-edit-dan-update-data-dengan-rest-api.md)**
5. **[Part 5: Mengubah & Update Data](./docs/05-edit-dan-update-data-dengan-rest-api.md)**
   *Belajar mengambil detail data berdasarkan ID untuk mengisi form secara otomatis (*pre-filled*), dan mengupdate data ke server menggunakan trik _method PUT.*
   
6. **[Part 6: Menghapus Data (Delete)](file:///f:/DOC/DOC-VUE-TYPESCRIPT/docs/06-delete-data-dengan-rest-api.md)**
6. **[Part 6: Menghapus Data (Delete)](./docs/06-delete-data-dengan-rest-api.md)**
   *Belajar membuat tombol hapus produk dengan konfirmasi browser, mengirim request HTTP DELETE via Axios, dan mengupdate isi tabel secara instan tanpa reload halaman.*

---

## 🌐 Referensi & Sumber Materi Asli

Dokumentasi ini dirangkum dari seri artikel tutorial **SantriKoding**:

* 🔗 **Part 1:** [Tutorial Vue 3 TypeScript #1: Membuat Project Vue 3 (Vite)](https://santrikoding.com/tutorial-vue-3-typescript-1-membuat-project-vue-3-vite)
* 🔗 **Part 2:** [Tutorial Vue 3 TypeScript #2: Install dan Konfigurasi Vue Router](https://santrikoding.com/tutorial-vue-3-typescript-2-install-dan-konfigurasi-vue-router)
* 🔗 **Part 3:** [Tutorial Vue 3 TypeScript #3: Menampilkan Data dari Rest API](https://santrikoding.com/tutorial-vue-3-typescript-3-menampilkan-data-dari-rest-api)
* 🔗 **Part 4:** [Tutorial Vue 3 TypeScript #4: Insert Data Dengan Rest API](https://santrikoding.com/tutorial-vue-3-typescript-4-insert-data-dengan-rest-api)
* 🔗 **Part 5:** [Tutorial Vue 3 TypeScript #5: Edit dan Update Data Dengan Rest API](https://santrikoding.com/tutorial-vue-3-typescript-5-edit-dan-update-data-dengan-rest-api)
* 🔗 **Part 6:** [Tutorial Vue 3 TypeScript #6: Delete Data Dengan Rest API](https://santrikoding.com/tutorial-vue-3-typescript-6-delete-data-dengan-rest-api)

---

## 🛠️ Tech Stack yang Digunakan

* **Vue 3 (Composition API):** Framework JavaScript modern yang reaktif dan cepat.
* **TypeScript:** Memberikan keamanan tipe data (*type safety*) agar kode bebas bug salah ketik.
* **Vite:** Build tool super cepat untuk pengembangan aplikasi frontend.
* **Vue Router:** Mengelola perpindahan halaman aplikasi secara instan (SPA).
* **Axios:** Melakukan koneksi dan pertukaran data dengan Laravel REST API.
* **Bootstrap 5:** Kerangka CSS untuk membuat tampilan layout dan form yang responsif dan rapi secara instan.
