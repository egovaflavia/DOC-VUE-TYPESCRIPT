# Tutorial Vue 3 TypeScript #3: Menampilkan Data dari REST API

Pada bagian ketiga ini, kita akan mempelajari cara menginstal **Axios**, membuat konfigurasi endpoint API global, mendefinisikan interface data dengan TypeScript, serta menampilkan data dari REST API (Laravel backend) di halaman produk Vue 3.

---

## 1. Langkah 1: Instalasi Axios
**Axios** adalah library HTTP client berbasis Promise yang digunakan untuk melakukan request (GET, POST, PUT, DELETE) ke server/API.

Jalankan perintah instalasi di terminal:

```bash
npm install axios@1.8.4
```

---

## 2. Langkah 2: Konfigurasi Endpoint API Global
Agar lebih mudah mengelola perubahan URL domain API di masa mendatang, kita akan membuat instance Axios global.

Buat folder baru bernama `api` di dalam direktori `src`, kemudian buat file baru bernama `index.ts` di dalamnya (`src/api/index.ts`):

```typescript
// Import axios
import axios from 'axios';

// Membuat instance Axios dengan konfigurasi baseURL default
const Api = axios.create({
  // URL dasar server API Laravel
  baseURL: 'http://localhost:8000'
})

export default Api
```

---

## 3. Langkah 3: Membuat Halaman Daftar Produk
Buat folder baru bernama `products` di dalam direktori `src/views/`, lalu buat file baru bernama `index.vue` di dalamnya (`src/views/products/index.vue`):

```html
<script setup lang="ts">
// Import ref dan onMounted dari Vue
import { ref, onMounted } from "vue";
// Import service API Axios yang sudah dibuat
import Api from "../../api";

// 1. Definisikan Interface Product untuk tipe data yang ketat (Type Safety)
interface Product {
  id: number;
  image: string;
  title: string;
  description: string;
  price: number;
  stock: number;
}

// 2. Deklarasikan State dengan generic type interface Product
const products = ref<Product[]>([]);

// 3. Fungsi Asynchronous untuk mengambil data dari REST API
const fetchDataProducts = async () => {
  try {
    const response = await Api.get("/api/products");
    // Mengambil data dari response API (disesuaikan dengan format pagination Laravel)
    products.value = response.data.data.data;
  } catch (error) {
    console.error("Error fetching products:", error);
  }
};

// 4. Jalankan method fetchDataProducts ketika komponen selesai dimount (onMounted)
onMounted(() => {
  fetchDataProducts();
});
</script>

<template>
  <div class="container mt-5 mb-5">
    <div class="row">
      <div class="col-md-12">
        <!-- Tombol Tambah Produk -->
        <router-link to="/products/create" class="btn btn-md btn-success rounded-5 shadow border-0 mb-3">
          ADD NEW PRODUCT
        </router-link>
        
        <div class="card border-0 rounded-3 shadow">
          <div class="card-body">
            <table class="table table-bordered">
              <thead class="bg-dark text-white">
                <tr>
                  <th scope="col">Image</th>
                  <th scope="col">Title</th>
                  <th scope="col">Description</th>
                  <th scope="col">Price</th>
                  <th scope="col">Stock</th>
                  <th scope="col" style="width: 15%">Actions</th>
                </tr>
              </thead>
              <tbody>
                <!-- Tampilkan alert jika data kosong -->
                <tr v-if="products.length === 0">
                  <td colspan="6" class="text-center">
                    <div class="alert alert-danger mb-0">No data available</div>
                  </td>
                </tr>
                
                <!-- Looping data produk menggunakan v-for -->
                <tr v-for="product in products" :key="product.id">
                  <td class="text-center">
                    <img :src="product.image" :alt="product.title" width="200" class="rounded-3" />
                  </td>
                  <td>{{ product.title }}</td>
                  <td>{{ product.description }}</td>
                  <td>{{ product.price.toLocaleString('id-ID') }}</td>
                  <td>{{ product.stock }}</td>
                  <td class="text-center">
                    <!-- Tombol Edit -->
                    <router-link :to="`/products/edit/${product.id}`" class="btn btn-sm btn-primary rounded-5 shadow border-0 me-2">
                      EDIT
                    </router-link>
                    <!-- Tombol Delete (Fungsionalitas ditambahkan di Part 6) -->
                    <button class="btn btn-sm btn-danger rounded-5 shadow border-0">
                      DELETE
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
```

---

## 4. Langkah 4: Daftarkan Route Products di Router Configuration
Buka file `src/routes/index.ts` dan tambahkan route `/products` ke dalam daftar router:

```typescript
import { createRouter, createWebHistory, type RouteRecordRaw } from 'vue-router'

const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    name: 'home',
    component: () => import(/* webpackChunkName: "home" */ '../views/home.vue')
  },
  // Tambahkan route products berikut:
  {
    path: '/products',
    name: 'products',
    component: () => import(/* webpackChunkName: "products" */ '../views/products/index.vue')
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
```

Sekarang ketika Anda menavigasi ke halaman **PRODUCTS** via navbar (atau URL `http://localhost:5173/products`), Vue akan mengirim request GET ke Laravel API untuk menampilkan daftar produk.
