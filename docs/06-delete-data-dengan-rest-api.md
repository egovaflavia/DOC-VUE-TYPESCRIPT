# Tutorial Vue 3 TypeScript #6: Delete Data dengan REST API

Pada bagian terakhir dari seri tutorial ini, kita akan mempelajari cara membuat proses hapus data produk (*Delete*) menggunakan method HTTP DELETE melalui Axios, serta melakukan *reactive state update* untuk memperbarui tampilan tabel secara instan setelah data terhapus.

---

## 1. Langkah 1: Implementasi Fungsi Hapus Data di Komponen
Untuk menghapus data, kita tidak membuat file view baru, melainkan menambahkan fungsi `deleteProduct` di dalam halaman daftar produk (`src/views/products/index.vue`).

Buka file `src/views/products/index.vue` dan ubah isinya menjadi seperti berikut:

```html
<script setup lang="ts">
// Import ref dan onMounted dari Vue
import { ref, onMounted } from "vue";
// Import service API Axios
import Api from "../../api";

// Definisikan Interface Product untuk Type Safety
interface Product {
  id: number;
  image: string;
  title: string;
  description: string;
  price: number;
  stock: number;
}

// State produk
const products = ref<Product[]>([]);

// Fungsi untuk mengambil data produk dari server
const fetchDataProducts = async () => {
  try {
    const response = await Api.get("/api/products");
    products.value = response.data.data.data;
  } catch (error) {
    console.error("Error fetching products:", error);
  }
};

// Ambil data saat component dimount
onMounted(() => {
  fetchDataProducts();
});

// ==========================================
// BARU: Fungsi untuk menghapus data produk
// ==========================================
const deleteProduct = async (id: number) => {
  // Peringatan konfirmasi browser sebelum menghapus (opsional namun direkomendasikan)
  if (confirm("Apakah Anda yakin ingin menghapus produk ini?")) {
    try {
      // Mengirim request dengan method DELETE ke REST API Laravel
      await Api.delete(`/api/products/${id}`);
      
      // Ambil data produk terbaru untuk memperbarui tampilan tabel
      fetchDataProducts();
    } catch (error) {
      console.error("Error deleting product:", error);
    }
  }
};
</script>

<template>
  <div class="container mt-5 mb-5">
    <div class="row">
      <div class="col-md-12">
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
                <tr v-if="products.length === 0">
                  <td colspan="6" class="text-center">
                    <div class="alert alert-danger mb-0">No data available</div>
                  </td>
                </tr>
                <tr v-for="product in products" :key="product.id">
                  <td class="text-center">
                    <img :src="product.image" :alt="product.title" width="200" class="rounded-3" />
                  </td>
                  <td>{{ product.title }}</td>
                  <td>{{ product.description }}</td>
                  <td>{{ product.price.toLocaleString('id-ID') }}</td>
                  <td>{{ product.stock }}</td>
                  <td class="text-center">
                    <router-link :to="`/products/edit/${product.id}`" class="btn btn-sm btn-primary rounded-5 shadow border-0 me-2">
                      EDIT
                    </router-link>
                    
                    <!-- ========================================== -->
                    <!-- BARU: Binding Event Handler Click di Tombol -->
                    <!-- ========================================== -->
                    <button @click="deleteProduct(product.id)" class="btn btn-sm btn-danger rounded-5 shadow border-0">
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

## 2. Alur Proses Hapus Data
1. Pengguna mengklik tombol **DELETE** pada baris produk tertentu.
2. Event `@click="deleteProduct(product.id)"` terpicu dan mengirimkan ID produk tersebut sebagai argumen.
3. Fungsi `deleteProduct` melakukan konfirmasi konfirmasi dialog browser.
4. Request HTTP `DELETE` dikirimkan ke URL `/api/products/{id}` menggunakan Axios instance `Api.delete()`.
5. Jika server berhasil menghapus data produk dari database, server mengembalikan status sukses (200/204).
6. State diperbarui secara reaktif dengan memanggil ulang fungsi `fetchDataProducts()`, sehingga produk yang baru dihapus langsung hilang dari tabel di browser tanpa perlu reload halaman web.
