<!-- <template>
  <div class="bg-stone-50 min-h-screen pb-16">
    
    <div class="bg-gray-900 text-white py-16 text-center">
      <h1 class="text-4xl md:text-5xl font-bold mb-4">
        {{ currentCategoryName }}
      </h1>
      <p class="text-gray-400 max-w-2xl mx-auto px-4">
        Nikmati pilihan hidangan terbaik dari kategori {{ currentCategoryName }} yang disiapkan khusus untuk memanjakan lidah Anda.
      </p>
    </div>

    <div class="container mx-auto px-6 mt-12">
      <div v-if="pending" class="flex justify-center py-20">
        <div class="animate-spin rounded-full h-12 w-12 border-b-2 border-orange-500"></div>
      </div>

      <div v-else-if="menus.length === 0" class="text-center py-20 text-gray-500">
        <p class="text-2xl mb-2">🍽️</p>
        <p>Belum ada hidangan pada kategori ini.</p>
      </div>

      <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8">
        <div 
          v-for="menu in menus" 
          :key="menu.id" 
          class="bg-white rounded-xl shadow-md overflow-hidden hover:shadow-xl transition-shadow duration-300 group"
        >
          <div class="relative h-56 overflow-hidden">
            <img 
              v-if="menu.image_url" 
              :src="menu.image_url" 
              :alt="menu.name" 
              class="w-full h-full object-cover transform transition-transform duration-500 group-hover:scale-110" 
            />
            <div v-else class="w-full h-full bg-gray-200 flex items-center justify-center text-gray-400">
              [No Image]
            </div>
            <div class="absolute top-4 right-4 bg-orange-500 text-white font-bold py-1 px-3 rounded-full shadow-lg">
              Rp {{ menu.price.toLocaleString("id-ID") }}
            </div>
          </div>
          
          <div class="p-6">
            <h3 class="text-xl font-bold text-gray-800 mb-2 group-hover:text-orange-600 transition-colors">
              {{ menu.name }}
            </h3>
            <p class="text-gray-600 text-sm line-clamp-2 leading-relaxed">
              {{ menu.description || "Hidangan spesial dari Kecilung Kitchen & Resto." }}
            </p>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script setup>
const route = useRoute();
const categoryId = route.params.id;
const baseURL = "https://back.kecilungresto.com/api";

// 1. Fetch Menu berdasarkan ID Kategori
const { data: menuResponse, pending } = useFetch(`${baseURL}/menus/category/${categoryId}`, {
  lazy: import.meta.client // <-- Kunci Rahasianya ada di sini
});
const menus = computed(() => menuResponse.value?.data || []);

// 2. Fetch data nama kategori secara paralel untuk judul banner
const { data: categoryResponse } = useFetch(`${baseURL}/categories`, {
  lazy: import.meta.client // <-- Kunci Rahasianya ada di sini
});
const currentCategoryName = computed(() => {
  const cats = categoryResponse.value?.data || [];
  const found = cats.find(c => c.id == categoryId);
  return found ? found.name : 'Daftar Menu';
});
</script> -->

<!-- <template>
  <div class="bg-stone-50 min-h-screen pb-16">
    <div class="bg-red-900 text-white py-16 text-center">
      <h1 class="text-4xl md:text-5xl font-bold mb-4">
        {{ currentCategoryName }}
      </h1>
      <p class="text-white max-w-2xl mx-auto px-4">
        Nikmati pilihan hidangan terbaik dari kategori
        {{ currentCategoryName }} yang disiapkan khusus untuk memanjakan lidah
        Anda.
      </p>
    </div>

    <div class="container mx-auto px-6 mt-12">
      <div v-if="pending" class="flex justify-center py-20">
        <div
          class="animate-spin rounded-full h-12 w-12 border-b-2 border-orange-500"
        ></div>
      </div>

      <div
        v-else-if="menus.length === 0"
        class="text-center py-20 text-gray-500"
      >
        <p class="text-2xl mb-2">🍽️</p>
        <p>Belum ada hidangan pada kategori ini.</p>
      </div>

      <div
        v-else
        class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8"
      >
        <div
          v-for="menu in menus"
          :key="menu.id"
          @click="openModal(menu)"
          class="bg-white rounded-xl shadow-md overflow-hidden hover:shadow-xl transition-shadow duration-300 group cursor-pointer"
        >
          <div class="relative h-56 overflow-hidden">
            <img
              v-if="menu.image_url"
              :src="menu.image_url"
              :alt="menu.name"
              class="w-full h-full object-cover transform transition-transform duration-500 group-hover:scale-110"
            />
            <div
              v-else
              class="w-full h-full bg-gray-200 flex items-center justify-center text-gray-400"
            >
              [No Image]
            </div>
            <div
              class="absolute top-4 right-4 bg-orange-500 text-white font-bold py-1 px-3 rounded-full shadow-lg"
            >
              Rp {{ menu.price.toLocaleString("id-ID") }}
            </div>
          </div>

          <div class="p-6">
            <h3
              class="text-xl font-bold text-gray-800 mb-2 group-hover:text-orange-600 transition-colors"
            >
              {{ menu.name }}
            </h3>
            <p class="text-gray-600 text-sm line-clamp-2 leading-relaxed">
              {{
                menu.description ||
                "Hidangan spesial dari Kecilung Kitchen & Resto."
              }}
            </p>
          </div>
        </div>
      </div>
    </div>

    <div
      v-if="isModalOpen"
      class="fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-50 p-4"
      @click.self="closeModal"
    >
      <div
        class="bg-white rounded-2xl shadow-2xl max-w-3xl w-full flex flex-col md:flex-row overflow-hidden relative transform transition-all"
      >
        <button
          @click="closeModal"
          class="absolute top-4 right-4 z-10 bg-white rounded-full p-2 text-gray-500 hover:text-red-500 shadow-md transition-colors focus:outline-none"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M6 18L18 6M6 6l12 12"
            />
          </svg>
        </button>

        <div class="md:w-1/2 h-64 md:h-auto relative">
          <img
            v-if="selectedMenu?.image_url"
            :src="selectedMenu.image_url"
            :alt="selectedMenu.name"
            class="w-full h-full object-cover"
          />
          <div
            v-else
            class="w-full h-full bg-gray-200 flex items-center justify-center text-gray-500"
          >
            Tidak ada gambar tersedia
          </div>
        </div>

        <div class="md:w-1/2 p-8 flex flex-col justify-center">
          <div
            class="mb-2 text-sm font-semibold text-orange-500 uppercase tracking-wider"
          >
            {{ currentCategoryName }}
          </div>
          <h2 class="text-3xl font-bold text-gray-900 mb-4">
            {{ selectedMenu?.name }}
          </h2>
          <p class="text-2xl font-extrabold text-green-600 mb-6">
            Rp {{ selectedMenu?.price.toLocaleString("id-ID") }}
          </p>
          <div class="h-px w-full bg-gray-200 mb-6"></div>
          <p class="text-gray-700 leading-relaxed overflow-y-auto max-h-48">
            {{
              selectedMenu?.description ||
              "Hidangan spesial dari Kecilung Kitchen & Resto yang dibuat dengan bahan-bahan berkualitas untuk memanjakan lidah Anda."
            }}
          </p>

          <button
            @click="closeModal"
            class="mt-8 w-full bg-gray-900 hover:bg-black text-white font-bold py-3 rounded-xl transition-colors"
          >
            Tutup Detail
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const route = useRoute();
const categoryId = route.params.id;
const baseURL = "https://back.kecilungresto.com/api";

// 1. Fetch Menu berdasarkan ID Kategori
const { data: menuResponse, pending } = useFetch(
  `${baseURL}/menus/category/${categoryId}`,
  {
    lazy: import.meta.client, // <-- Kunci Rahasia SEO
  },
);
const menus = computed(() => menuResponse.value?.data || []);

// 2. Fetch data nama kategori secara paralel untuk judul banner
const { data: categoryResponse } = useFetch(`${baseURL}/categories`, {
  lazy: import.meta.client,
});
const currentCategoryName = computed(() => {
  const cats = categoryResponse.value?.data || [];
  const found = cats.find((c) => c.id == categoryId);
  return found ? found.name : "Daftar Menu";
});

// 3. Manajemen Modal
const isModalOpen = ref(false);
const selectedMenu = ref(null);

const openModal = (menu) => {
  selectedMenu.value = menu;
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
  // Sedikit jeda sebelum menghapus data agar animasi penutupan modal (jika ada) tidak kehilangan data secara instan
  setTimeout(() => {
    selectedMenu.value = null;
  }, 200);
};
</script> -->

<template>
  <div class="bg-stone-50 min-h-screen pb-16">
    <!-- Banner Kategori -->
    <div class="bg-red-900 text-white py-16 text-center">
      <h1 class="text-4xl md:text-5xl font-bold mb-4">
        {{ currentCategoryName }}
      </h1>
      <p class="text-white max-w-2xl mx-auto px-4">
        Nikmati pilihan hidangan terbaik dari kategori
        {{ currentCategoryName }} yang disiapkan khusus untuk memanjakan lidah
        Anda.
      </p>
    </div>

    <!-- Grid Menu Makanan -->
    <div class="container mx-auto px-6 mt-12">
      <!-- SKELETON LOADING STATE (Pengganti Spinner) -->
      <div
        v-if="pending"
        class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8"
      >
        <!-- Tampilkan 8 kerangka skeleton agar grid terlihat penuh -->
        <div
          v-for="n in 8"
          :key="'skeleton-' + n"
          class="bg-white rounded-xl shadow-md overflow-hidden animate-pulse"
        >
          <!-- Skeleton Gambar -->
          <div class="h-56 bg-gray-300 relative">
            <!-- Skeleton Harga Mengambang -->
            <div
              class="absolute top-4 right-4 bg-gray-400 h-6 w-24 rounded-full"
            ></div>
          </div>

          <div class="p-6">
            <!-- Skeleton Judul -->
            <div class="h-6 bg-gray-300 rounded w-3/4 mb-4"></div>

            <!-- Skeleton Deskripsi (2 baris) -->
            <div class="h-3 bg-gray-200 rounded w-full mb-2"></div>
            <div class="h-3 bg-gray-200 rounded w-5/6"></div>
          </div>
        </div>
      </div>

      <!-- KONDISI KOSONG -->
      <div
        v-else-if="menus.length === 0"
        class="text-center py-20 text-gray-500"
      >
        <p class="text-2xl mb-2">🍽️</p>
        <p>Belum ada hidangan pada kategori ini.</p>
      </div>

      <!-- DATA AKTUAL (Sudah Dimuat) -->
      <div
        v-else
        class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8"
      >
        <!-- Card Menu -->
        <div
          v-for="menu in menus"
          :key="menu.id"
          @click="openModal(menu)"
          class="bg-white rounded-xl shadow-md overflow-hidden hover:shadow-xl transition-shadow duration-300 group cursor-pointer"
        >
          <div class="relative h-56 overflow-hidden">
            <img
              v-if="menu.image_url"
              :src="menu.image_url"
              :alt="menu.name"
              class="w-full h-full object-cover transform transition-transform duration-500 group-hover:scale-110"
            />
            <div
              v-else
              class="w-full h-full bg-gray-200 flex items-center justify-center text-gray-400"
            >
              [No Image]
            </div>
            <!-- Label Harga Mengambang -->
            <div
              class="absolute top-4 right-4 bg-orange-500 text-white font-bold py-1 px-3 rounded-full shadow-lg"
            >
              Rp {{ menu.price.toLocaleString("id-ID") }}
            </div>
          </div>

          <div class="p-6">
            <h3
              class="text-xl font-bold text-gray-800 mb-2 group-hover:text-orange-600 transition-colors"
            >
              {{ menu.name }}
            </h3>
            <p class="text-gray-600 text-sm line-clamp-2 leading-relaxed">
              {{
                menu.description ||
                "Hidangan spesial dari Kecilung Kitchen & Resto."
              }}
            </p>
          </div>
        </div>
      </div>
    </div>

    <!-- MODAL DETAIL MENU -->
    <div
      v-if="isModalOpen"
      class="fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-50 p-4"
      @click.self="closeModal"
    >
      <div
        class="bg-white rounded-2xl shadow-2xl max-w-3xl w-full flex flex-col md:flex-row overflow-hidden relative transform transition-all"
      >
        <!-- Tombol Tutup (X) -->
        <button
          @click="closeModal"
          class="absolute top-4 right-4 z-10 bg-white rounded-full p-2 text-gray-500 hover:text-red-500 shadow-md transition-colors focus:outline-none"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M6 18L18 6M6 6l12 12"
            />
          </svg>
        </button>

        <!-- Sisi Kiri: Gambar -->
        <div class="md:w-1/2 h-64 md:h-auto relative">
          <img
            v-if="selectedMenu?.image_url"
            :src="selectedMenu.image_url"
            :alt="selectedMenu.name"
            class="w-full h-full object-cover"
          />
          <div
            v-else
            class="w-full h-full bg-gray-200 flex items-center justify-center text-gray-500"
          >
            Tidak ada gambar tersedia
          </div>
        </div>

        <!-- Sisi Kanan: Detail & Deskripsi -->
        <div class="md:w-1/2 p-8 flex flex-col justify-center">
          <div
            class="mb-2 text-sm font-semibold text-orange-500 uppercase tracking-wider"
          >
            {{ currentCategoryName }}
          </div>
          <h2 class="text-3xl font-bold text-gray-900 mb-4">
            {{ selectedMenu?.name }}
          </h2>
          <p class="text-2xl font-extrabold text-green-600 mb-6">
            Rp {{ selectedMenu?.price.toLocaleString("id-ID") }}
          </p>
          <div class="h-px w-full bg-gray-200 mb-6"></div>
          <p class="text-gray-700 leading-relaxed overflow-y-auto max-h-48">
            {{
              selectedMenu?.description ||
              "Hidangan spesial dari Kecilung Kitchen & Resto yang dibuat dengan bahan-bahan berkualitas untuk memanjakan lidah Anda."
            }}
          </p>

          <button
            @click="closeModal"
            class="mt-8 w-full bg-gray-900 hover:bg-black text-white font-bold py-3 rounded-xl transition-colors"
          >
            Tutup Detail
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// const route = useRoute();
// const categoryId = route.params.id;
// const baseURL = "https://back.kecilungresto.com/api";

// // 1. Fetch Menu berdasarkan ID Kategori
// const { data: menuResponse, pending } = useFetch(
//   `${baseURL}/menus/category/${categoryId}`,
//   {
//     lazy: import.meta.client, // <-- Kunci Rahasia SEO
//   },
// );
// const menus = computed(() => menuResponse.value?.data || []);

// // 2. Fetch data nama kategori secara paralel untuk judul banner
// const { data: categoryResponse } = useFetch(`${baseURL}/categories`, {
//   lazy: import.meta.client,
// });
// const currentCategoryName = computed(() => {
//   const cats = categoryResponse.value?.data || [];
//   const found = cats.find((c) => c.id == categoryId);
//   return found ? found.name : "Daftar Menu";
// });

// // 3. Manajemen Modal
// const isModalOpen = ref(false);
// const selectedMenu = ref(null);

// const openModal = (menu) => {
//   selectedMenu.value = menu;
//   isModalOpen.value = true;
// };

// const closeModal = () => {
//   isModalOpen.value = false;
//   // Sedikit jeda sebelum menghapus data agar animasi penutupan modal (jika ada) tidak kehilangan data secara instan
//   setTimeout(() => {
//     selectedMenu.value = null;
//   }, 200);
// };

import { ref, computed } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();
const categorySlug = route.params.slug; // Ambil slug dari URL
const baseURL = "https://back.kecilungresto.com/api";

// 1. FETCH DATA KATEGORI BERDASARKAN SLUG (Smart Request)
// Kita panggil API baru yang dibuat di backend agar mendapatkan ID aslinya.
const { data: catResponse, pending: pendingCat } = useFetch(
  `${baseURL}/categories/${categorySlug}`,
  {
    lazy: import.meta.client,
  }
);
const categoryData = computed(() => catResponse.value?.data);
const currentCategoryName = computed(() => categoryData.value?.name || "Daftar Menu");
const categoryId = computed(() => categoryData.value?.id);

// 2. FETCH DAFTAR MENU (Safe Null Fetch)
// Setelah categoryId didapatkan, otomatis API Menu akan menembak data yang sesuai.
const { data: menuResponse, pending: pendingMenu } = useFetch(
  () => categoryId.value ? `${baseURL}/menus/category/${categoryId.value}` : null,
  {
    lazy: import.meta.client,
  }
);
const menus = computed(() => menuResponse.value?.data || []);

// Gabungkan status loading
const pending = computed(() => pendingCat.value || pendingMenu.value);

// 3. MANAJEMEN MODAL
const isModalOpen = ref(false);
const selectedMenu = ref(null);

const openModal = (menu) => {
  selectedMenu.value = menu;
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
  setTimeout(() => {
    selectedMenu.value = null;
  }, 200);
};
</script>
