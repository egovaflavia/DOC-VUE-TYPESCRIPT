# Tutorial Vue 3 TypeScript #4: Insert Data dengan REST API

Pada bagian keempat ini, kita akan mempelajari cara membuat form tambah data produk baru beserta proses **upload gambar** ke server/API. Kita juga akan belajar cara menangani validasi error dari backend menggunakan TypeScript.

---

## 1. Langkah 1: Membuat Halaman Form Tambah Produk
Buat file baru bernama `create.vue` di dalam direktori `src/views/products/` (`src/views/products/create.vue`):

```html
<script setup lang="ts">
// Import ref dari Vue
import { ref } from "vue";
// Import useRouter untuk melakukan navigasi halaman setelah sukses insert
import { useRouter } from "vue-router";
// Import service API Axios
import Api from "../../api";

// 1. Definisikan Interface Errors untuk type-checking pesan validasi backend
interface Errors {
  image?: string[];
  title?: string[];
  description?: string[];
  price?: string[];
  stock?: string[];
}

// 2. Deklarasikan State form produk
const image = ref<File | null>(null);
const title = ref("");
const description = ref("");
// Tipe data form price dan stock adalah string karena FormData hanya menerima data string/blob
const price = ref("");
const stock = ref("");

// 3. Deklarasikan State untuk menyimpan response error validasi
const errors = ref<Errors>({});

// 4. Inisialisasi router
const router = useRouter();

// 5. Fungsi untuk menangani perubahan input file (gambar)
const handleFileChange = (event: Event) => {
  // Melakukan casting event target sebagai HTMLInputElement
  const target = event.target as HTMLInputElement;
  if (target.files && target.files[0]) {
    image.value = target.files[0];
  }
};

// 6. Fungsi untuk menyimpan data produk ke REST API
const storeProduct = async () => {
  // Inisialisasi FormData karena kita menyertakan file biner (gambar)
  const formData = new FormData();
  
  if (image.value) {
    formData.append("image", image.value);
  }
  formData.append("title", title.value);
  formData.append("description", description.value);
  formData.append("price", price.value);
  formData.append("stock", stock.value);

  try {
    // Kirim request POST ke server API Laravel
    await Api.post("/api/products", formData);
    
    // Redirect ke halaman daftar produk setelah sukses
    router.push("/products");
  } catch (error: any) {
    // Tangkap error validasi dari server dan simpan ke state errors
    errors.value = error.response.data;
  }
};
</script>

<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-12">
        <div class="card border-0 rounded-3 shadow">
          <div class="card-body">
            <!-- Menghubungkan fungsi submit form -->
            <form @submit.prevent="storeProduct">
              
              <!-- Input Image File -->
              <div class="mb-3">
                <label class="form-label fw-bold">Image</label>
                <input type="file" @change="handleFileChange" class="form-control" />
                <!-- Alert error validasi gambar -->
                <div v-if="errors.image" class="alert alert-danger mt-2">
                  {{ errors.image[0] }}
                </div>
              </div>
              
              <!-- Input Title -->
              <div class="mb-3">
                <label class="form-label fw-bold">Title</label>
                <input type="text" v-model="title" class="form-control" placeholder="Title Product" />
                <!-- Alert error validasi title -->
                <div v-if="errors.title" class="alert alert-danger mt-2">
                  {{ errors.title[0] }}
                </div>
              </div>
              
              <!-- Input Description -->
              <div class="mb-3">
                <label class="form-label fw-bold">Description</label>
                <textarea v-model="description" class="form-control" rows="5" placeholder="Description Product"></textarea>
                <!-- Alert error validasi deskripsi -->
                <div v-if="errors.description" class="alert alert-danger mt-2">
                  {{ errors.description[0] }}
                </div>
              </div>
              
              <div class="row">
                <!-- Input Price -->
                <div class="col-md-6">
                  <div class="mb-3">
                    <label class="form-label fw-bold">Price</label>
                    <input type="number" v-model="price" class="form-control" placeholder="Price Product" />
                    <div v-if="errors.price" class="alert alert-danger mt-2">
                      {{ errors.price[0] }}
                    </div>
                  </div>
                </div>
                
                <!-- Input Stock -->
                <div class="col-md-6">
                  <div class="mb-3">
                    <label class="form-label fw-bold">Stock</label>
                    <input type="number" v-model="stock" class="form-control" placeholder="Stock Product" />
                    <div v-if="errors.stock" class="alert alert-danger mt-2">
                      {{ errors.stock[0] }}
                    </div>
                  </div>
                </div>
              </div>
              
              <!-- Tombol Submit -->
              <button type="submit" class="btn btn-md btn-primary rounded-5 shadow border-0">Save</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
```

---

## 2. Langkah 2: Daftarkan Route Products Create
Buka file `src/routes/index.ts` dan tambahkan route `/products/create` ke konfigurasi router Anda:

```typescript
import { createRouter, createWebHistory, type RouteRecordRaw } from 'vue-router'

const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    name: 'home',
    component: () => import(/* webpackChunkName: "home" */ '../views/home.vue')
  },
  {
    path: '/products',
    name: 'products',
    component: () => import(/* webpackChunkName: "products" */ '../views/products/index.vue')
  },
  // Tambahkan route create product berikut:
  {
    path: '/products/create',
    name: 'products-create',
    component: () => import(/* webpackChunkName: "products-create" */ '../views/products/create.vue')
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
```

Sekarang ketika Anda mengklik tombol **ADD NEW PRODUCT** di halaman produk, form pengisian data akan terbuka. Form ini sudah mendukung *client-side event handling* untuk input file biner dan validasi error *realtime* jika server menolak input data.
