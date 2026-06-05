# Tutorial Vue 3 TypeScript #2: Install dan Konfigurasi Vue Router

Pada bagian kedua ini, kita akan mempelajari cara menginstal dan mengonfigurasi **Vue Router** untuk mengelola navigasi halaman (routing) di Vue 3 dengan TypeScript, serta mengintegrasikan Bootstrap 5 untuk styling dasar.

---

## 1. Apa itu Vue Router?
**Vue Router** adalah library resmi untuk Vue.js yang berfungsi mengelola sistem navigasi dan routing. Dengan Vue Router, kita dapat membangun *Single Page Application* (SPA) di mana perpindahan halaman terjadi secara dinamis di sisi klien tanpa perlu memuat ulang seluruh halaman (*no full page reload*), memberikan pengalaman pengguna yang lebih mulus dan cepat.

---

## 2. Langkah 1: Instalasi Vue Router
Jalankan perintah berikut di dalam terminal proyek Anda (`crud-vue3-ts`):

```bash
npm install vue-router@4.5.0
```

---

## 3. Langkah 2: Integrasi Bootstrap 5 via CDN
Buka file `index.html` di root project Anda, lalu ubah isinya untuk mengintegrasikan Bootstrap CSS, JS, dan Google Font Quicksand.

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite + Vue + TS</title>
    <!-- Google Fonts: Quicksand -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@300..700&display=swap" rel="stylesheet">
    <!-- Bootstrap 5 CSS CDN -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
      body {
        background-color: lightgray;
        font-family: 'Quicksand', sans-serif;
      }
    </style>
  </head>
  <body>
    <div id="app"></div>
    <!-- Bootstrap 5 JS Bundle CDN -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"></script>
    <script type="module" src="/src/main.ts"></script>
  </body>
</html>
```

---

## 4. Langkah 3: Membuat View Home
Buat folder baru bernama `views` di dalam direktori `src`, kemudian buat file baru bernama `home.vue` di dalamnya (`src/views/home.vue`):

```html
<script setup lang="ts">
  // Script setup menggunakan TypeScript (lang="ts")
</script>

<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12">
        <div class="p-5 mt-5 mb-4 bg-light rounded-5">
          <div class="container-fluid py-5">
            <h1 class="display-5 fw-bold">VITE + VUE 3 + TS</h1>
            <p class="col-md-8 fs-4">
              Belajar CRUD dengan Vue 3 dan TypeScript di SantriKoding.com
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
```

---

## 5. Langkah 4: Konfigurasi Router (routes/index.ts)
Buat folder baru bernama `routes` di dalam direktori `src`, kemudian buat file baru bernama `index.ts` di dalamnya (`src/routes/index.ts`):

```typescript
// Import modul router dan tipe RouteRecordRaw dari vue-router
import { createRouter, createWebHistory, type RouteRecordRaw } from 'vue-router'

// Mendefinisikan array route dengan tipe anotasi eksplisit RouteRecordRaw
const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    name: 'home',
    component: () => import(/* webpackChunkName: "home" */ '../views/home.vue')
  }
]

// Membuat instance router
const router = createRouter({
  history: createWebHistory(), // Menggunakan history mode (tanpa tanda hash '#')
  routes
})

export default router
```

> [!NOTE]
> Penggunaan `createWebHistory()` menghasilkan URL bersih seperti `http://localhost:5173/posts` dibandingkan `createWebHashHistory()` yang menghasilkan URL `http://localhost:5173/#/posts`.

---

## 6. Langkah 5: Registrasi Route Secara Global
Buka file `src/main.ts` dan daftarkan konfigurasi router agar dapat digunakan di seluruh aplikasi:

```typescript
import { createApp } from 'vue';
import App from './App.vue';

// Import konfigurasi router
import routes from './routes';

const app = createApp(App);

// Registrasi router menggunakan plugin "use"
app.use(routes);

app.mount('#app');
```

---

## 7. Langkah 6: Konfigurasi Router View di App.vue
Buka file `src/App.vue` dan ubah isinya untuk membuat navigasi (navbar) menggunakan Bootstrap dan merender komponen halaman menggunakan `<router-view />`.

```html
<template>
  <div>
    <nav class="navbar navbar-expand-lg bg-dark" data-bs-theme="dark">
      <div class="container">
        <!-- router-link untuk navigasi tanpa refresh -->
        <router-link :to="{ name: 'home' }" class="navbar-brand">HOME</router-link>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <router-link to="/products" class="nav-link active" aria-current="page">PRODUCTS</router-link>
            </li>
          </ul>
          <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
            <a href="https://santrikoding.com" target="_blank" class="btn btn-success">SANTRIKODING.COM</a>
          </ul>
        </div>
      </div>
    </nav>
    
    <!-- Tempat merender view halaman aktif berdasarkan route -->
    <router-view />
  </div>
</template>
```

Sekarang jika Anda menjalankan project, halaman Home akan otomatis dirender dan navbar akan siap digunakan untuk halaman `/products` di bagian selanjutnya.
