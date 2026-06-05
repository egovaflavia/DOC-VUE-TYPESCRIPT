# Tutorial Vue 3 TypeScript #5: Edit dan Update Data dengan REST API

Pada bagian kelima ini, kita akan mempelajari cara melakukan proses pengambilan detail data produk berdasarkan ID (*pre-filled form*), mengedit data tersebut, dan mengirimkannya kembali ke server menggunakan method HTTP PUT simulasi via `FormData`.

---

## 1. Langkah 1: Membuat Halaman Edit Produk
Buat file baru bernama `edit.vue` di dalam direktori `src/views/products/` (`src/views/products/edit.vue`):

```html
<script setup lang="ts">
// Import ref dan onMounted dari Vue
import { ref, onMounted } from "vue";
// Import useRoute untuk mengambil ID dari parameter URL, dan useRouter untuk navigasi halaman
import { useRoute, useRouter } from "vue-router";
// Import service API Axios
import Api from "../../api";

// 1. Definisikan Interface Errors
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
const price = ref("");
const stock = ref("");
const errors = ref<Errors>({});

// 3. Inisialisasi route dan router
const route = useRoute();
const router = useRouter();

// 4. Fungsi untuk mengambil detail data produk dari server
const fetchDetailProduct = async () => {
  try {
    // Ambil parameter id dari URL via route.params.id
    const response = await Api.get(`/api/products/${route.params.id}`);
    
    // Set data response ke masing-masing state untuk ditampilkan di form
    title.value = response.data.data.title;
    description.value = response.data.data.description;
    price.value = response.data.data.price;
    stock.value = response.data.data.stock;
  } catch (error) {
    console.error("Error fetching product details:", error);
  }
};

// 5. Ambil data produk saat pertama kali halaman dibuka (onMounted)
onMounted(() => {
  fetchDetailProduct();
});

// 6. Fungsi untuk menangani perubahan input file (gambar baru)
const handleFileChange = (event: Event) => {
  const target = event.target as HTMLInputElement;
  if (target.files && target.files[0]) {
    image.value = target.files[0];
  }
};

// 7. Fungsi untuk mengupdate data produk
const updateProduct = async () => {
  const formData = new FormData();
  
  // Masukkan file biner jika user memilih gambar baru
  if (image.value) {
    formData.append("image", image.value);
  }
  formData.append("title", title.value);
  formData.append("description", description.value);
  formData.append("price", price.value);
  formData.append("stock", stock.value);
  
  // Tambahkan key _method dengan value PUT untuk simulasi method PUT di REST API (Laravel)
  formData.append("_method", "PUT");

  try {
    // Kirim request POST (simulasi PUT) ke endpoint dengan ID produk
    await Api.post(`/api/products/${route.params.id}`, formData);
    
    // Redirect ke halaman daftar produk setelah sukses
    router.push("/products");
  } catch (error: any) {
    // Simpan error response validasi jika ada
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
            <form @submit.prevent="updateProduct">
              
              <!-- Input Image File -->
              <div class="mb-3">
                <label class="form-label fw-bold">Image</label>
                <input type="file" @change="handleFileChange" class="form-control" />
                <div v-if="errors.image" class="alert alert-danger mt-2">
                  {{ errors.image[0] }}
                </div>
              </div>
              
              <!-- Input Title -->
              <div class="mb-3">
                <label class="form-label fw-bold">Title</label>
                <input type="text" v-model="title" class="form-control" placeholder="Title Product" />
                <div v-if="errors.title" class="alert alert-danger mt-2">
                  {{ errors.title[0] }}
                </div>
              </div>
              
              <!-- Input Description -->
              <div class="mb-3">
                <label class="form-label fw-bold">Description</label>
                <textarea v-model="description" class="form-control" rows="5" placeholder="Description Product"></textarea>
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
              
              <!-- Tombol Update -->
              <button type="submit" class="btn btn-md btn-primary rounded-5 shadow border-0">Update</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
```

---

## 2. Kenapa menggunakan `_method = "PUT"` dengan `POST`?
> [!NOTE]
> Backend REST API (seperti PHP/Laravel) secara default tidak dapat membaca parameter file upload (`multipart/form-data`) secara langsung apabila menggunakan method HTTP `PUT` atau `PATCH`. 
> 
> Solusinya adalah mengirim request menggunakan method HTTP `POST` melalui `FormData`, dan menambahkan variabel `_method` bernilai `"PUT"` di dalamnya. Laravel secara otomatis akan mendeteksi ini sebagai request `PUT` asli.

---

## 3. Langkah 2: Daftarkan Route Products Edit
Buka file `src/routes/index.ts` dan daftarkan dynamic route `/products/edit/:id` ke dalam router:

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
  {
    path: '/products/create',
    name: 'products-create',
    component: () => import(/* webpackChunkName: "products-create" */ '../views/products/create.vue')
  },
  // Tambahkan dynamic route edit berikut:
  {
    path: '/products/edit/:id',
    name: 'products-edit',
    component: () => import(/* webpackChunkName: "products-edit" */ '../views/products/edit.vue')
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
```

Sekarang ketika Anda mengklik tombol **EDIT** pada data produk di tabel, Vue akan membuka form edit yang datanya telah diisi otomatis dari database sesuai dengan ID produk tersebut.
