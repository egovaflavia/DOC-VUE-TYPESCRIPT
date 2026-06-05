# Perbandingan: Vue 3 dengan TypeScript vs JavaScript Biasa

Saat mengembangkan aplikasi dengan Vue 3, kita memiliki pilihan untuk menggunakan **TypeScript (TS)** atau tetap menggunakan **JavaScript biasa (JS)**. Vue 3 sendiri ditulis ulang sepenuhnya menggunakan TypeScript, sehingga integrasi TypeScript di Vue 3 sangatlah mulus dan didukung secara penuh.

Berikut adalah penjelasan mendalam tentang perbedaan, kelebihan, kekurangan, serta komparasi kode di antara keduanya.

---

## 1. Perbedaan Utama secara Konseptual

| Fitur | TypeScript (TS) | JavaScript Biasa (JS) |
| :--- | :--- | :--- |
| **Sistem Tipe** | Statis (*Static Typing*). Tipe data variabel didefinisikan secara eksplisit. | Dinamis (*Dynamic Typing*). Tipe data ditentukan secara otomatis saat runtime. |
| **Pengecekan Error** | Saat kompilasi (*Compile-time*). Error langsung ditandai oleh IDE sebelum kode dijalankan. | Saat dijalankan (*Runtime*). Error baru diketahui ketika baris kode tersebut dieksekusi di browser. |
| **Dukungan Tooling** | Sangat Kuat. Menyediakan autocomplete otomatis (*Intellisense*) yang presisi dan navigasi kode yang mudah. | Terbatas. Autocomplete bergantung pada tebakan IDE (*heuristik*), sering kali tidak akurat pada objek yang kompleks. |
| **Skalabilitas Proyek** | Sangat cocok untuk proyek skala menengah hingga besar dengan banyak developer. | Lebih cepat untuk proyek kecil, prototyping, atau proyek personal sederhana. |

---

## 2. Perbandingan Implementasi Kode (Vue 3)

Berikut adalah beberapa perbandingan kode nyata yang diambil dari studi kasus CRUD Produk di seri tutorial ini.

### A. Deklarasi Script Setup
Pada TypeScript, kita menambahkan atribut `lang="ts"` pada tag `<script>`.
* **TypeScript:**
  ```html
  <script setup lang="ts">
    // TypeScript Compiler aktif di sini
  </script>
  ```
* **JavaScript:**
  ```html
  <script setup>
    // JavaScript biasa
  </script>
  ```

### B. Deklarasi State & Type Safety (Data List)
Mendefinisikan bentuk data (*data shape*) produk yang didapatkan dari REST API menggunakan Interface.
* **TypeScript:**
  ```typescript
  // 1. Membuat kontrak tipe data objek
  interface Product {
    id: number;
    title: string;
    price: number;
    stock: number;
  }

  // 2. State hanya menerima array objek Product
  const products = ref<Product[]>([]);
  
  // Jika kita mencoba memasukkan data yang tidak sesuai format:
  // products.value = [{ id: "abc", title: 123 }]; -> compiler TS akan memunculkan ERROR di IDE.
  ```
* **JavaScript:**
  ```javascript
  // State dideklarasikan tanpa batasan tipe data
  const products = ref([]);

  // Tidak ada peringatan jika struktur data salah, error baru muncul di browser
  // products.value = [{ id: "abc", title: 123 }]; -> diizinkan, namun berpotensi merusak UI.
  ```

### C. Penanganan Event (*File Upload/Casting*)
Saat menangani file input, TypeScript mewajibkan kita melakukan casting tipe target agar IDE tahu bahwa target tersebut memiliki properti `files`.
* **TypeScript:**
  ```typescript
  const handleFileChange = (event: Event) => {
    // Casting event.target menjadi HTMLInputElement
    const target = event.target as HTMLInputElement;
    if (target.files && target.files[0]) {
      image.value = target.files[0]; // Aman, tipe data dijamin File
    }
  };
  ```
* **JavaScript:**
  ```javascript
  const handleFileChange = (event) => {
    // Tanpa casting, namun tidak ada auto-complete properti .files di editor
    if (event.target.files && event.target.files[0]) {
      image.value = event.target.files[0];
    }
  };
  ```

### D. Parameter Route & Navigasi
Mengakses route params dan memanggil router navigasi.
* **TypeScript:**
  ```typescript
  import { useRoute, useRouter } from "vue-router";
  
  const route = useRoute();
  const router = useRouter();

  // TypeScript memastikan route dan router memiliki autocomplete untuk metode seperti router.push()
  const productId = route.params.id as string; // casting parameter
  ```
* **JavaScript:**
  ```javascript
  import { useRoute, useRouter } from "vue-router";
  
  const route = useRoute();
  const router = useRouter();

  const productId = route.params.id; // Tanpa proteksi tipe data
  ```

---

## 3. Kelebihan dan Kekurangan

### Menggunakan TypeScript di Vue 3

> [!TIP]
> **Kelebihan:**
> 1. **Zero Runtime Type Bugs**: Hampir semua bug salah ketik properti (misalnya `product.prrice` seharusnya `product.price`) akan dicegah langsung di text editor.
> 2. **Dokumentasi Mandiri (Self-Documenting)**: Developer lain tidak perlu menebak-nebak apa isi variabel `product` karena strukturnya sudah terdokumentasi jelas di dalam `interface Product`.
> 3. **Refactoring Sangat Aman**: Jika Anda mengubah nama properti (misal `price` menjadi `harga`), IDE akan mendeteksi seluruh file yang menggunakan properti tersebut dan memperbaruinya secara otomatis tanpa terlewat.
> 4. **Standardisasi Tim**: Memaksa tim menulis kode dengan struktur dan tipe data yang konsisten.

> [!WARNING]
> **Kekurangan:**
> 1. **Kurva Belajar (Learning Curve)**: Memerlukan pemahaman konsep object-oriented, generics, interfaces, dan type assertions.
> 2. **Waktu Setup & Penulisan Awal**: Menulis interface dan tipe data di awal membutuhkan waktu sedikit lebih lama dibandingkan langsung menulis logika JavaScript.
> 3. **Kompilasi Tambahan**: Kode TypeScript harus dikompilasi (ditranspilasi) ke JavaScript biasa agar bisa dijalankan di browser, meskipun proses ini sudah ditangani otomatis oleh Vite dengan sangat cepat.

---

## 4. Kesimpulan: Kapan Harus Memilih?

- **Pilihlah JavaScript jika:** Anda sedang dikejar *deadline* ketat untuk membuat aplikasi skala kecil (MVP), proyek personal sederhana, atau jika Anda baru pertama kali belajar dasar-dasar Vue 3.
- **Pilihlah TypeScript jika:** Anda membangun aplikasi skala produksi (*Enterprise*), aplikasi dikerjakan oleh tim developer yang terdiri dari beberapa orang, atau jika Anda ingin memastikan aplikasi minim bug *runtime* dan mudah dipelihara untuk jangka panjang.
