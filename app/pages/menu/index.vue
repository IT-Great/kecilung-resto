<!-- <template>
  <div class="bg-stone-50 min-h-screen pb-20">
    <div
      class="bg-gray-900 text-white py-16 text-center relative overflow-hidden"
    >
      <div
        class="absolute top-0 left-0 w-64 h-64 bg-orange-500 rounded-full mix-blend-multiply filter blur-3xl opacity-20 -translate-x-1/2 -translate-y-1/2"
      ></div>
      <div
        class="absolute bottom-0 right-0 w-64 h-64 bg-orange-500 rounded-full mix-blend-multiply filter blur-3xl opacity-20 translate-x-1/2 translate-y-1/2"
      ></div>

      <div class="relative z-10">
        <h1 class="text-4xl md:text-5xl font-extrabold mb-4 tracking-tight">
          Our <span class="text-orange-500">Menu</span>
        </h1>
        <p class="text-gray-400 max-w-2xl mx-auto px-4 text-lg">
          Eksplorasi seluruh sajian istimewa kami, mulai dari hidangan pembuka
          hingga penutup yang disiapkan dengan penuh gairah dan bahan
          berkualitas tinggi.
        </p>
      </div>
    </div>

    <div class="container mx-auto px-6 mt-12">
      <div class="flex flex-wrap justify-center gap-3 mb-12">
        <button
          @click="selectedCategory = 'all'"
          :class="
            selectedCategory === 'all'
              ? 'bg-orange-500 text-white shadow-lg border-orange-500'
              : 'bg-white text-gray-700 hover:bg-orange-50 border-gray-200 hover:border-orange-300'
          "
          class="px-6 py-2.5 rounded-full font-bold transition-all duration-300 border focus:outline-none transform active:scale-95"
        >
          Semua Menu
        </button>

        <button
          v-for="cat in categories"
          :key="cat.id"
          @click="selectedCategory = cat.id"
          :class="
            selectedCategory === cat.id
              ? 'bg-orange-500 text-white shadow-lg border-orange-500'
              : 'bg-white text-gray-700 hover:bg-orange-50 border-gray-200 hover:border-orange-300'
          "
          class="px-6 py-2.5 rounded-full font-bold transition-all duration-300 border focus:outline-none transform active:scale-95"
        >
          {{ cat.name }}
        </button>
      </div>

      <div
        v-if="pendingMenus || pendingCategories"
        class="flex justify-center py-20"
      >
        <div
          class="animate-spin rounded-full h-12 w-12 border-b-2 border-orange-500"
        ></div>
      </div>

      <div
        v-else-if="filteredMenus.length === 0"
        class="text-center py-20 text-gray-500 bg-white rounded-2xl shadow-sm border border-gray-100 max-w-2xl mx-auto"
      >
        <span class="text-5xl mb-4 block opacity-50">🍽️</span>
        <p class="text-xl font-medium text-gray-800 mb-2">Tidak Ada Hidangan</p>
        <p>Belum ada menu yang tersedia untuk kategori ini.</p>
      </div>

      <div
        v-else
        class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8"
      >
        <div
          v-for="menu in filteredMenus"
          :key="menu.id"
          @click="openModal(menu)"
          class="bg-white rounded-2xl shadow-md overflow-hidden hover:shadow-xl transition-all duration-300 group cursor-pointer flex flex-col transform hover:-translate-y-1"
        >
          <div class="relative h-56 overflow-hidden flex-shrink-0">
            <img
              v-if="menu.image_url"
              :src="menu.image_url"
              :alt="menu.name"
              class="w-full h-full object-cover transform transition-transform duration-700 group-hover:scale-110"
            />
            <div
              v-else
              class="w-full h-full bg-gray-100 flex items-center justify-center text-gray-400"
            >
              <span class="text-2xl">🍲</span>
            </div>
            <div
              class="absolute top-4 right-4 bg-orange-500 text-white font-bold py-1 px-4 rounded-full shadow-lg backdrop-blur-sm bg-opacity-95 text-sm"
            >
              Rp {{ menu.price.toLocaleString("id-ID") }}
            </div>
          </div>

          <div class="p-6 flex-1 flex flex-col">
            <span
              v-if="selectedCategory === 'all'"
              class="text-xs font-extrabold text-orange-500 uppercase tracking-wider mb-2 block"
            >
              {{ getCategoryName(menu.category_id) }}
            </span>

            <h3
              class="text-xl font-bold text-gray-900 mb-2 group-hover:text-orange-600 transition-colors"
            >
              {{ menu.name }}
            </h3>
            <p
              class="text-gray-600 text-sm line-clamp-2 leading-relaxed flex-1"
            >
              {{
                menu.description ||
                "Hidangan spesial yang disiapkan khusus untuk memanjakan lidah Anda."
              }}
            </p>
          </div>
        </div>
      </div>
    </div>

    <div
      v-if="isModalOpen"
      class="fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-50 p-4 backdrop-blur-sm"
      @click.self="closeModal"
    >
      <div
        class="bg-white rounded-2xl shadow-2xl max-w-3xl w-full flex flex-col md:flex-row overflow-hidden relative transform transition-all scale-100 opacity-100"
      >
        <button
          @click="closeModal"
          class="absolute top-4 right-4 z-20 bg-white rounded-full p-2 text-gray-500 hover:text-red-500 shadow-md transition-colors focus:outline-none"
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

        <div class="md:w-1/2 h-64 md:h-auto relative bg-gray-50">
          <img
            v-if="selectedMenu?.image_url"
            :src="selectedMenu.image_url"
            :alt="selectedMenu.name"
            class="w-full h-full object-cover"
          />
          <div
            v-else
            class="w-full h-full flex flex-col items-center justify-center text-gray-400"
          >
            <span class="text-6xl mb-2">🍲</span>
            <span>Gambar tidak tersedia</span>
          </div>
        </div>

        <div class="md:w-1/2 p-8 flex flex-col justify-center">
          <div
            class="mb-2 text-sm font-extrabold text-orange-500 uppercase tracking-widest"
          >
            {{ getCategoryName(selectedMenu?.category_id) }}
          </div>
          <h2 class="text-3xl font-black text-gray-900 mb-4">
            {{ selectedMenu?.name }}
          </h2>
          <p
            class="text-2xl font-extrabold text-green-600 mb-6 bg-green-50 inline-block px-4 py-2 rounded-lg border border-green-100 w-fit"
          >
            Rp {{ selectedMenu?.price.toLocaleString("id-ID") }}
          </p>
          <div class="h-px w-full bg-gray-100 mb-6"></div>
          <p
            class="text-gray-700 leading-relaxed overflow-y-auto max-h-48 pr-2"
          >
            {{
              selectedMenu?.description ||
              "Hidangan spesial dari Kecilung Kitchen & Resto yang dibuat dengan bahan-bahan berkualitas untuk memanjakan lidah Anda."
            }}
          </p>

          <button
            @click="closeModal"
            class="mt-8 w-full bg-gray-900 hover:bg-black text-white font-bold py-4 rounded-xl transition-colors shadow-lg"
          >
            Tutup Jendela
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const baseURL = "https://back.kecilungresto.com/api";

// 1. Fetch seluruh Kategori
const { data: catRes, pending: pendingCategories } = useFetch(
  `${baseURL}/categories`,
  {
    lazy: import.meta.client, // Sempurna untuk SEO
  },
);
const categories = computed(() => catRes.value?.data || []);

// 2. Fetch seluruh Menu Makanan
const { data: menuRes, pending: pendingMenus } = useFetch(`${baseURL}/menus`, {
  lazy: import.meta.client, // Sempurna untuk SEO
});
const allMenus = computed(() => menuRes.value?.data || []);

// 3. Logika Filter (Reaktif)
const selectedCategory = ref("all");

const filteredMenus = computed(() => {
  if (selectedCategory.value === "all") {
    return allMenus.value;
  }
  return allMenus.value.filter(
    (menu) => menu.category_id === selectedCategory.value,
  );
});

// Helper: Mencari nama kategori berdasarkan category_id
const getCategoryName = (id) => {
  if (!id) return "";
  const cat = categories.value.find((c) => c.id === id);
  return cat ? cat.name : "Uncategorized";
};

// 4. Logika Modal
const isModalOpen = ref(false);
const selectedMenu = ref(null);

const openModal = (menu) => {
  selectedMenu.value = menu;
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
  // Jeda untuk membiarkan animasi penutupan selesai sebelum mengosongkan data
  setTimeout(() => {
    selectedMenu.value = null;
  }, 300);
};
</script> -->

<template>
  <div class="bg-stone-50 min-h-screen pb-20">
    <!-- Banner Utama -->
    <div
      class="bg-gray-900 text-white py-16 text-center relative overflow-hidden"
    >
      <div
        class="absolute top-0 left-0 w-64 h-64 bg-orange-500 rounded-full mix-blend-multiply filter blur-3xl opacity-20 -translate-x-1/2 -translate-y-1/2"
      ></div>
      <div
        class="absolute bottom-0 right-0 w-64 h-64 bg-orange-500 rounded-full mix-blend-multiply filter blur-3xl opacity-20 translate-x-1/2 translate-y-1/2"
      ></div>

      <div class="relative z-10">
        <h1 class="text-4xl md:text-5xl font-extrabold mb-4 tracking-tight">
          Our <span class="text-orange-500">Menu</span>
        </h1>
        <p class="text-gray-400 max-w-2xl mx-auto px-4 text-lg">
          Eksplorasi seluruh sajian istimewa kami, mulai dari hidangan pembuka
          hingga penutup yang disiapkan dengan penuh gairah dan bahan
          berkualitas tinggi.
        </p>
      </div>
    </div>

    <div class="container mx-auto px-6 mt-12">
      <!-- Deretan Tombol Filter Kategori -->
      <div class="flex flex-wrap justify-center gap-3 mb-10">
        <button
          @click="selectedCategory = 'all'"
          :class="
            selectedCategory === 'all'
              ? 'bg-orange-500 text-white shadow-lg border-orange-500'
              : 'bg-white text-gray-700 hover:bg-orange-50 border-gray-200 hover:border-orange-300'
          "
          class="px-6 py-2.5 rounded-full font-bold transition-all duration-300 border focus:outline-none transform active:scale-95"
        >
          Semua Menu
        </button>

        <button
          v-for="cat in categories"
          :key="cat.id"
          @click="selectedCategory = cat.id"
          :class="
            selectedCategory === cat.id
              ? 'bg-orange-500 text-white shadow-lg border-orange-500'
              : 'bg-white text-gray-700 hover:bg-orange-50 border-gray-200 hover:border-orange-300'
          "
          class="px-6 py-2.5 rounded-full font-bold transition-all duration-300 border focus:outline-none transform active:scale-95"
        >
          {{ cat.name }}
        </button>
      </div>

      <!-- ========================================== -->
      <!-- FILTER BAR (Items per Page & Search)       -->
      <!-- ========================================== -->
      <div class="flex flex-col sm:flex-row justify-between items-center gap-4 mb-8">
        <!-- Kiri: Items Per Page -->
        <div class="flex items-center gap-2 text-sm text-gray-600 font-medium">
          <span>Tampilkan</span>
          <select
            v-model="itemsPerPage"
            class="border border-gray-300 rounded-lg px-3 py-1.5 focus:ring-2 focus:ring-orange-500 focus:border-orange-500 outline-none bg-white cursor-pointer shadow-sm"
          >
            <!-- Opsinya disesuaikan agar pas dengan grid 4/3 kolom -->
            <option :value="8">8</option>
            <option :value="12">12</option>
            <option :value="24">24</option>
            <option :value="48">48</option>
          </select>
          <span>hidangan</span>
        </div>

        <!-- Kanan: Search Bar -->
        <div class="relative w-full sm:w-72">
          <input
            v-model="searchQuery"
            type="text"
            placeholder="Cari hidangan atau harga..."
            class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-xl shadow-sm focus:ring-2 focus:ring-orange-500 focus:border-transparent outline-none transition-all"
          />
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-5 w-5 text-gray-400 absolute left-3 top-1/2 transform -translate-y-1/2"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
          </svg>
        </div>
      </div>

      <!-- Indikator Loading Global -->
      <div v-if="pendingMenus || pendingCategories" class="flex justify-center py-20">
        <div class="animate-spin rounded-full h-12 w-12 border-b-2 border-orange-500"></div>
      </div>

      <!-- Tampilan Jika Data Kosong -->
      <div
        v-else-if="processedMenus.length === 0"
        class="text-center py-20 text-gray-500 bg-white rounded-2xl shadow-sm border border-gray-100 max-w-2xl mx-auto"
      >
        <span class="text-5xl mb-4 block opacity-50">🍽️</span>
        <p class="text-xl font-bold text-gray-800 mb-2">Tidak Ada Hidangan</p>
        <p>Menu yang Anda cari tidak ditemukan atau belum tersedia.</p>
      </div>

      <!-- ========================================== -->
      <!-- GRID MENU MAKANAN (Data Ter-Paginate)      -->
      <!-- ========================================== -->
      <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8">
        <div
          v-for="menu in paginatedMenus"
          :key="menu.id"
          @click="openModal(menu)"
          class="bg-white rounded-2xl shadow-md overflow-hidden hover:shadow-xl transition-all duration-300 group cursor-pointer flex flex-col transform hover:-translate-y-1"
        >
          <div class="relative h-56 overflow-hidden flex-shrink-0">
            <img
              v-if="menu.image_url"
              :src="menu.image_url"
              :alt="menu.name"
              class="w-full h-full object-cover transform transition-transform duration-700 group-hover:scale-110"
            />
            <div
              v-else
              class="w-full h-full bg-gray-100 flex items-center justify-center text-gray-400"
            >
              <span class="text-2xl">🍲</span>
            </div>
            <!-- Label Harga -->
            <div
              class="absolute top-4 right-4 bg-orange-500 text-white font-bold py-1 px-4 rounded-full shadow-lg backdrop-blur-sm bg-opacity-95 text-sm"
            >
              Rp {{ menu.price.toLocaleString("id-ID") }}
            </div>
          </div>

          <div class="p-6 flex-1 flex flex-col">
            <!-- Label Kategori (Hanya muncul jika di tab "Semua Menu") -->
            <span
              v-if="selectedCategory === 'all'"
              class="text-xs font-extrabold text-orange-500 uppercase tracking-wider mb-2 block"
            >
              {{ getCategoryName(menu.category_id) }}
            </span>

            <h3 class="text-xl font-bold text-gray-900 mb-2 group-hover:text-orange-600 transition-colors">
              {{ menu.name }}
            </h3>
            <p class="text-gray-600 text-sm line-clamp-2 leading-relaxed flex-1">
              {{ menu.description || "Hidangan spesial yang disiapkan khusus untuk memanjakan lidah Anda." }}
            </p>
          </div>
        </div>
      </div>

      <!-- ========================================== -->
      <!-- PAGINATION FOOTER                          -->
      <!-- ========================================== -->
      <div
        v-if="processedMenus.length > 0"
        class="mt-12 flex flex-col md:flex-row justify-between items-center gap-4 border-t border-gray-200 pt-8"
      >
        <!-- Kiri: Info Halaman -->
        <div class="text-sm text-gray-600 font-medium bg-white px-4 py-2 rounded-lg shadow-sm border border-gray-100">
          Showing <span class="font-bold text-gray-900">{{ showingStart }}</span> 
          to <span class="font-bold text-gray-900">{{ showingEnd }}</span> 
          of <span class="font-bold text-gray-900">{{ processedMenus.length }}</span> items
        </div>

        <!-- Kanan: Tombol Navigasi Pagination -->
        <div class="flex items-center space-x-1" v-if="totalPages > 1">
          <!-- Tombol Prev -->
          <button
            @click="currentPage--"
            :disabled="currentPage === 1"
            class="px-3 py-1.5 border border-gray-300 rounded-lg bg-white hover:bg-orange-50 hover:border-orange-300 text-gray-600 disabled:opacity-50 disabled:cursor-not-allowed transition-all shadow-sm"
          >
            &laquo;
          </button>

          <!-- Deretan Kotak Angka (Maksimal 7, dengan "...") -->
          <template v-for="(pageItem, index) in paginationArray" :key="index">
            <span v-if="pageItem === '...'" class="px-2 py-1.5 text-gray-400 font-bold">...</span>
            <button
              v-else
              @click="currentPage = pageItem"
              :class="[
                'px-3 py-1.5 border rounded-lg transition-all shadow-sm',
                currentPage === pageItem
                  ? 'bg-orange-500 text-white border-orange-500 font-bold transform scale-105'
                  : 'bg-white border-gray-300 text-gray-700 hover:bg-orange-50 hover:border-orange-300',
              ]"
            >
              {{ pageItem }}
            </button>
          </template>

          <!-- Tombol Next -->
          <button
            @click="currentPage++"
            :disabled="currentPage === totalPages"
            class="px-3 py-1.5 border border-gray-300 rounded-lg bg-white hover:bg-orange-50 hover:border-orange-300 text-gray-600 disabled:opacity-50 disabled:cursor-not-allowed transition-all shadow-sm"
          >
            &raquo;
          </button>
        </div>
      </div>
    </div>

    <!-- ========================================== -->
    <!-- MODAL DETAIL MENU                          -->
    <!-- ========================================== -->
    <div
      v-if="isModalOpen"
      class="fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-50 p-4 backdrop-blur-sm"
      @click.self="closeModal"
    >
      <div class="bg-white rounded-2xl shadow-2xl max-w-3xl w-full flex flex-col md:flex-row overflow-hidden relative transform transition-all scale-100 opacity-100">
        <!-- Tombol Tutup (X) -->
        <button
          @click="closeModal"
          class="absolute top-4 right-4 z-20 bg-white rounded-full p-2 text-gray-500 hover:text-red-500 shadow-md transition-colors focus:outline-none"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>

        <!-- Sisi Kiri: Gambar -->
        <div class="md:w-1/2 h-64 md:h-auto relative bg-gray-50">
          <img
            v-if="selectedMenu?.image_url"
            :src="selectedMenu.image_url"
            :alt="selectedMenu.name"
            class="w-full h-full object-cover"
          />
          <div v-else class="w-full h-full flex flex-col items-center justify-center text-gray-400">
            <span class="text-6xl mb-2">🍲</span>
            <span>Gambar tidak tersedia</span>
          </div>
        </div>

        <!-- Sisi Kanan: Detail & Deskripsi -->
        <div class="md:w-1/2 p-8 flex flex-col justify-center">
          <div class="mb-2 text-sm font-extrabold text-orange-500 uppercase tracking-widest">
            {{ getCategoryName(selectedMenu?.category_id) }}
          </div>
          <h2 class="text-3xl font-black text-gray-900 mb-4">
            {{ selectedMenu?.name }}
          </h2>
          <p class="text-2xl font-extrabold text-green-600 mb-6 bg-green-50 inline-block px-4 py-2 rounded-lg border border-green-100 w-fit">
            Rp {{ selectedMenu?.price.toLocaleString("id-ID") }}
          </p>
          <div class="h-px w-full bg-gray-100 mb-6"></div>
          <p class="text-gray-700 leading-relaxed overflow-y-auto max-h-48 pr-2">
            {{ selectedMenu?.description || "Hidangan spesial dari Kecilung Kitchen & Resto yang dibuat dengan bahan-bahan berkualitas untuk memanjakan lidah Anda." }}
          </p>
          <button
            @click="closeModal"
            class="mt-8 w-full bg-gray-900 hover:bg-black text-white font-bold py-4 rounded-xl transition-colors shadow-lg"
          >
            Tutup Jendela
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";

const baseURL = "https://back.kecilungresto.com/api";

// 1. Fetch seluruh Kategori
const { data: catRes, pending: pendingCategories } = useFetch(
  `${baseURL}/categories`,
  { lazy: import.meta.client }
);
const categories = computed(() => catRes.value?.data || []);

// 2. Fetch seluruh Menu Makanan
const { data: menuRes, pending: pendingMenus } = useFetch(`${baseURL}/menus`, {
  lazy: import.meta.client,
});
const allMenus = computed(() => menuRes.value?.data || []);


// ====================================================
// LOGIKA FILTER, PENCARIAN, & PAGINATION
// ====================================================
const selectedCategory = ref("all");
const searchQuery = ref("");
const itemsPerPage = ref(12); // Default 12 kotak agar rapi di grid 3/4 kolom
const currentPage = ref(1);

// Kembalikan ke halaman 1 jika ada perubahan filter, pencarian, atau items per page
watch([selectedCategory, searchQuery, itemsPerPage], () => {
  currentPage.value = 1;
});

// A. Filter Data Inti (Berdasarkan Kategori & Pencarian)
const processedMenus = computed(() => {
  let filtered = allMenus.value;

  // Filter 1: Berdasarkan Kategori
  if (selectedCategory.value !== "all") {
    filtered = filtered.filter((m) => m.category_id === selectedCategory.value);
  }

  // Filter 2: Berdasarkan Kata Kunci Pencarian (Nama / Deskripsi / Harga)
  if (searchQuery.value) {
    const q = searchQuery.value.toLowerCase();
    filtered = filtered.filter((m) => {
      return (
        m.name.toLowerCase().includes(q) ||
        (m.description && m.description.toLowerCase().includes(q)) ||
        m.price.toString().includes(q)
      );
    });
  }

  return filtered;
});

// B. Potong Data Sesuai Halaman (Pagination Slice)
const paginatedMenus = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage.value;
  const end = start + itemsPerPage.value;
  return processedMenus.value.slice(start, end);
});

// C. Perhitungan Statistik Pagination (Showing X to Y)
const totalPages = computed(() => Math.ceil(processedMenus.value.length / itemsPerPage.value));

const showingStart = computed(() => 
  processedMenus.value.length === 0 ? 0 : (currentPage.value - 1) * itemsPerPage.value + 1
);
const showingEnd = computed(() => 
  Math.min(currentPage.value * itemsPerPage.value, processedMenus.value.length)
);

// D. Algoritma Pagination Dinamis (Maksimal 7 Kotak / Element Array)
const paginationArray = computed(() => {
  const current = currentPage.value;
  const total = totalPages.value;

  // Jika total <= 7, tampilkan semuanya: [1] [2] [3] [4] [5] [6] [7]
  if (total <= 7) return Array.from({ length: total }, (_, i) => i + 1);
  
  // Jika di awal: [1] [2] [3] [4] [5] [...] [100]
  if (current <= 4) return [1, 2, 3, 4, 5, "...", total];
  
  // Jika di akhir: [1] [...] [96] [97] [98] [99] [100]
  if (current >= total - 3) return [1, "...", total - 4, total - 3, total - 2, total - 1, total];
  
  // Jika di tengah: [1] [...] [4] [5] [6] [...] [100]
  return [1, "...", current - 1, current, current + 1, "...", total];
});


// ====================================================
// HELPER & MODAL LOGIC
// ====================================================
const getCategoryName = (id) => {
  if (!id) return "";
  const cat = categories.value.find((c) => c.id === id);
  return cat ? cat.name : "Uncategorized";
};

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
  }, 300);
};
</script>
