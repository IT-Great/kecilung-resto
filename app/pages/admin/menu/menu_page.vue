<!-- <template>
  <div class="container mx-auto p-6 max-w-6xl">
    <div class="flex justify-between items-center mb-6">
      <h1 class="text-3xl font-bold text-gray-800">Manajemen Menu Makanan</h1>
      <NuxtLink
        to="/admin/menu/add_menu_page"
        class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg font-medium transition"
      >
        + Tambah Menu Baru
      </NuxtLink>
    </div>

    <div class="bg-white rounded-lg shadow overflow-x-auto">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase"
            >
              Nama Menu
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase"
            >
              Kategori
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase"
            >
              Harga
            </th>
            <th
              class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase"
            >
              Aksi
            </th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          <tr v-if="pending" class="text-center">
            <td colspan="4" class="py-4">Memuat data...</td>
          </tr>
          <tr
            v-else
            v-for="menu in menus"
            :key="menu.id"
            class="hover:bg-gray-50"
          >
            <td
              class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900"
            >
              {{ menu.name }}
            </td>
            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-600">
              {{ menu.category?.name || "Tanpa Kategori" }}
            </td>
            <td
              class="px-6 py-4 whitespace-nowrap text-sm text-green-600 font-semibold"
            >
              Rp {{ menu.price.toLocaleString("id-ID") }}
            </td>
            <td
              class="px-6 py-4 whitespace-nowrap text-center text-sm font-medium space-x-4"
            >
              <NuxtLink
                :to="`/admin/menu/detail/${menu.id}`"
                class="text-blue-600 hover:text-blue-900"
                >Detail</NuxtLink
              >
              <NuxtLink
                :to="`/admin/menu/edit/${menu.id}`"
                class="text-amber-600 hover:text-amber-900"
                >Edit</NuxtLink
              >
              <button
                @click="deleteMenu(menu.id)"
                class="text-red-600 hover:text-red-900"
              >
                Hapus
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
definePageMeta({
  layout: "admin",
});

const baseURL = "https://back.kecilungresto.com/api";
const { data: response, pending, refresh } = await useFetch(`${baseURL}/menus`);
const menus = computed(() => response.value?.data || []);

const deleteMenu = async (id) => {
  if (!confirm("Yakin ingin menghapus menu ini?")) return;
  try {
    await $fetch(`${baseURL}/menus/${id}`, { method: "DELETE" });
    refresh();
  } catch (error) {
    alert("Gagal menghapus data");
  }
};
</script> -->

<!-- <template>
  <div class="container mx-auto p-6 max-w-6xl">
    <div class="flex justify-between items-center mb-6">
      <h1 class="text-3xl font-bold text-gray-800">Manajemen Menu Makanan</h1>
      <NuxtLink to="/admin/menu/add_menu_page" class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg font-medium transition">
        + Tambah Menu Baru
      </NuxtLink>
    </div>

    <div class="bg-white rounded-lg shadow overflow-x-auto">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Gambar</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Nama Menu</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Kategori</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Harga</th>
            <th class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase">Aksi</th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          <tr v-if="pending" class="text-center"><td colspan="5" class="py-4">Memuat data...</td></tr>
          <tr v-else v-for="menu in menus" :key="menu.id" class="hover:bg-gray-50 items-center">
            
            <td class="px-6 py-4 whitespace-nowrap">
              <img v-if="menu.image_url" :src="menu.image_url" :alt="menu.name" class="w-16 h-16 object-cover rounded-md shadow-sm border" />
              <div v-else class="w-16 h-16 bg-gray-200 rounded-md flex items-center justify-center text-xs text-gray-400 border">No Image</div>
            </td>

            <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">{{ menu.name }}</td>
            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-600">{{ menu.category?.name || "Tanpa Kategori" }}</td>
            <td class="px-6 py-4 whitespace-nowrap text-sm text-green-600 font-semibold">Rp {{ menu.price.toLocaleString("id-ID") }}</td>
            <td class="px-6 py-4 whitespace-nowrap text-center text-sm font-medium space-x-4">
              <NuxtLink :to="`/admin/menu/detail/${menu.id}`" class="text-blue-600 hover:text-blue-900">Detail</NuxtLink>
              <NuxtLink :to="`/admin/menu/edit/${menu.id}`" class="text-amber-600 hover:text-amber-900">Edit</NuxtLink>
              <button @click="deleteMenu(menu.id)" class="text-red-600 hover:text-red-900">Hapus</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
definePageMeta({ layout: "admin" });

const baseURL = "https://back.kecilungresto.com/api";
const { data: response, pending, refresh } = await useFetch(`${baseURL}/menus`);
const menus = computed(() => response.value?.data || []);

const deleteMenu = async (id) => {
  if (!confirm("Yakin ingin menghapus menu ini?")) return;
  try {
    await $fetch(`${baseURL}/menus/${id}`, { method: "DELETE" });
    refresh();
  } catch (error) {
    alert("Gagal menghapus data");
  }
};
</script> -->

<!-- <template>
  <div class="container mx-auto p-6 max-w-6xl">
    <div class="flex justify-between items-center mb-6">
      <h1 class="text-3xl font-bold text-gray-800">Manajemen Menu Makanan</h1>
      <NuxtLink to="/admin/menu/add_menu_page" class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg font-medium transition">
        + Tambah Menu Baru
      </NuxtLink>
    </div>

    <div class="bg-white rounded-lg shadow overflow-x-auto">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Gambar</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Nama Menu</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Kategori</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Harga</th>
            <th class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase">Aksi</th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          
          <template v-if="pending">
            <tr v-for="n in 5" :key="'skel-' + n" class="animate-pulse slide-up-anim hover:bg-gray-50">
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="w-16 h-16 bg-gray-200 rounded-md"></div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="h-4 bg-gray-200 rounded w-3/4"></div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="h-4 bg-gray-200 rounded w-1/2"></div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="h-4 bg-gray-200 rounded w-1/3"></div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-center flex justify-center space-x-4 mt-5">
                <div class="h-4 bg-gray-200 rounded w-10"></div>
                <div class="h-4 bg-gray-200 rounded w-10"></div>
                <div class="h-4 bg-gray-200 rounded w-10"></div>
              </td>
            </tr>
          </template>

          <template v-else>
            <tr v-for="menu in menus" :key="menu.id" class="hover:bg-gray-50 items-center">
              <td class="px-6 py-4 whitespace-nowrap">
                <img v-if="menu.image_url" :src="menu.image_url" :alt="menu.name" class="w-16 h-16 object-cover rounded-md shadow-sm border" />
                <div v-else class="w-16 h-16 bg-gray-200 rounded-md flex items-center justify-center text-xs text-gray-400 border">No Image</div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">{{ menu.name }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-600">{{ menu.category?.name || "Tanpa Kategori" }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-green-600 font-semibold">Rp {{ menu.price.toLocaleString("id-ID") }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-center text-sm font-medium space-x-4">
                <NuxtLink :to="`/admin/menu/detail/${menu.id}`" class="text-blue-600 hover:text-blue-900">Detail</NuxtLink>
                <NuxtLink :to="`/admin/menu/edit/${menu.id}`" class="text-amber-600 hover:text-amber-900">Edit</NuxtLink>
                <button @click="deleteMenu(menu.id)" class="text-red-600 hover:text-red-900">Hapus</button>
              </td>
            </tr>
          </template>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
definePageMeta({ layout: "admin", middleware: "auth" });

const baseURL = "https://back.kecilungresto.com/api";

// PERUBAHAN: Gunakan useLazyFetch tanpa "await"
const { data: response, pending, refresh } = useLazyFetch(`${baseURL}/menus`);
const menus = computed(() => response.value?.data || []);

const deleteMenu = async (id) => {
  if (!confirm("Yakin ingin menghapus menu ini?")) return;
  try {
    await $fetch(`${baseURL}/menus/${id}`, { method: "DELETE" });
    refresh();
  } catch (error) {
    alert("Gagal menghapus data");
  }
};
</script>

<style scoped>
/* Animasi Float / Slide Up */
.slide-up-anim {
  animation: slideUp 0.4s ease-out forwards;
  opacity: 0;
}
@keyframes slideUp {
  0% { transform: translateY(15px); opacity: 0; }
  100% { transform: translateY(0); opacity: 1; }
}
</style> -->

<!-- <template>
  <div class="container mx-auto p-6 max-w-6xl">
    <div
      class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 gap-4"
    >
      <h1 class="text-3xl font-bold text-gray-800">Manajemen Menu Makanan</h1>
      <NuxtLink
        to="/admin/menu/add_menu_page"
        class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg font-medium transition whitespace-nowrap"
      >
        + Tambah Menu Baru
      </NuxtLink>
    </div>

    <div
      class="bg-white p-4 rounded-t-lg shadow-sm border-b border-gray-100 flex flex-col sm:flex-row justify-between items-center gap-4"
    >
      <div class="flex items-center gap-2 text-sm text-gray-600">
        <span>Tampilkan</span>
        <select
          v-model="itemsPerPage"
          class="border border-gray-300 rounded px-2 py-1 focus:ring-orange-500 focus:border-orange-500 outline-none"
        >
          <option :value="5">5</option>
          <option :value="10">10</option>
          <option :value="25">25</option>
          <option :value="50">50</option>
          <option :value="100">100</option>
        </select>
        <span>item</span>
      </div>

      <div class="relative w-full sm:w-64">
        <input
          v-model="searchQuery"
          type="text"
          placeholder="Cari nama, harga, atau kategori..."
          class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-orange-500 focus:border-transparent outline-none text-sm transition-all"
        />
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-5 w-5 text-gray-400 absolute left-3 top-1/2 transform -translate-y-1/2"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
          />
        </svg>
      </div>
    </div>

    <div class="bg-white shadow overflow-x-auto">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase"
            >
              Gambar
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase"
            >
              Nama Menu
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase"
            >
              Kategori
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase"
            >
              Harga
            </th>
            <th
              class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase"
            >
              Aksi
            </th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          <template v-if="pending">
            <tr
              v-for="n in 5"
              :key="'skel-' + n"
              class="animate-pulse hover:bg-gray-50"
            >
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="w-16 h-16 bg-gray-200 rounded-md"></div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="h-4 bg-gray-200 rounded w-3/4"></div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="h-4 bg-gray-200 rounded w-1/2"></div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="h-4 bg-gray-200 rounded w-1/3"></div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-center space-x-4">
                <div class="h-4 bg-gray-200 rounded w-20 inline-block"></div>
              </td>
            </tr>
          </template>

          <template v-else-if="paginatedMenus.length === 0">
            <tr>
              <td colspan="5" class="px-6 py-12 text-center text-gray-500">
                Data tidak ditemukan.
              </td>
            </tr>
          </template>

          <template v-else>
            <tr
              v-for="menu in paginatedMenus"
              :key="menu.id"
              class="hover:bg-gray-50 items-center transition-colors"
            >
              <td class="px-6 py-4 whitespace-nowrap">
                <img
                  v-if="menu.image_url"
                  :src="menu.image_url"
                  :alt="menu.name"
                  class="w-16 h-16 object-cover rounded-md shadow-sm border border-gray-200"
                />
                <div
                  v-else
                  class="w-16 h-16 bg-gray-100 rounded-md flex items-center justify-center text-xs text-gray-400 border border-gray-200"
                >
                  No Img
                </div>
              </td>
              <td
                class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900"
              >
                {{ menu.name }}
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-600">
                <span class="bg-gray-100 px-2 py-1 rounded text-xs">{{
                  menu.category?.name || "Tanpa Kategori"
                }}</span>
              </td>
              <td
                class="px-6 py-4 whitespace-nowrap text-sm text-green-600 font-bold"
              >
                Rp {{ menu.price.toLocaleString("id-ID") }}
              </td>
              <td
                class="px-6 py-4 whitespace-nowrap text-center text-sm font-medium space-x-3"
              >
                <NuxtLink
                  :to="`/admin/menu/detail/${menu.id}`"
                  class="text-blue-600 hover:text-blue-900 bg-blue-50 px-2 py-1 rounded transition"
                  >Detail</NuxtLink
                >
                <NuxtLink
                  :to="`/admin/menu/edit/${menu.id}`"
                  class="text-amber-600 hover:text-amber-900 bg-amber-50 px-2 py-1 rounded transition"
                  >Edit</NuxtLink
                >
                <button
                  @click="deleteMenu(menu.id)"
                  class="text-red-600 hover:text-red-900 bg-red-50 px-2 py-1 rounded transition"
                >
                  Hapus
                </button>
              </td>
            </tr>
          </template>
        </tbody>
      </table>
    </div>

    <div
      class="bg-white p-4 rounded-b-lg shadow-sm flex flex-col md:flex-row justify-between items-center gap-4 text-sm text-gray-600 border-t border-gray-100"
    >
      <div>
        Showing
        <span class="font-bold text-gray-900">{{ showingStart }}</span> to
        <span class="font-bold text-gray-900">{{ showingEnd }}</span> of
        <span class="font-bold text-gray-900">{{ filteredMenus.length }}</span>
        items
      </div>

      <div class="flex items-center space-x-1" v-if="totalPages > 1">
        <button
          @click="currentPage--"
          :disabled="currentPage === 1"
          class="px-3 py-1.5 border border-gray-300 rounded hover:bg-gray-50 disabled:opacity-50 disabled:cursor-not-allowed transition"
        >
          &laquo;
        </button>

        <template v-for="(pageItem, index) in paginationArray" :key="index">
          <span v-if="pageItem === '...'" class="px-2 py-1.5 text-gray-400"
            >...</span
          >

          <button
            v-else
            @click="currentPage = pageItem"
            :class="[
              'px-3 py-1.5 border rounded transition',
              currentPage === pageItem
                ? 'bg-orange-500 text-white border-orange-500 font-bold'
                : 'border-gray-300 text-gray-700 hover:bg-gray-50',
            ]"
          >
            {{ pageItem }}
          </button>
        </template>

        <button
          @click="currentPage++"
          :disabled="currentPage === totalPages"
          class="px-3 py-1.5 border border-gray-300 rounded hover:bg-gray-50 disabled:opacity-50 disabled:cursor-not-allowed transition"
        >
          &raquo;
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";
definePageMeta({ layout: "admin", middleware: "auth" });

const baseURL = "https://back.kecilungresto.com/api";

const { data: response, pending, refresh } = useLazyFetch(`${baseURL}/menus`);
const allMenus = computed(() => response.value?.data || []);

// --- STATE FILTER & PAGINATION ---
const searchQuery = ref("");
const itemsPerPage = ref(10);
const currentPage = ref(1);

// Reset ke halaman 1 jika user mengetik sesuatu di search bar atau mengubah items per page
watch([searchQuery, itemsPerPage], () => {
  currentPage.value = 1;
});

// --- LOGIKA FILTER (SEARCH BAR) ---
const filteredMenus = computed(() => {
  if (!searchQuery.value) return allMenus.value;

  const q = searchQuery.value.toLowerCase();
  return allMenus.value.filter((menu) => {
    return (
      menu.name.toLowerCase().includes(q) ||
      (menu.category?.name && menu.category.name.toLowerCase().includes(q)) ||
      menu.price.toString().includes(q)
    );
  });
});

// --- LOGIKA PAGINATION ---
const totalPages = computed(() => {
  return Math.ceil(filteredMenus.value.length / itemsPerPage.value);
});

const paginatedMenus = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage.value;
  const end = start + itemsPerPage.value;
  return filteredMenus.value.slice(start, end);
});

// Logika "Showing X to Y of Z"
const showingStart = computed(() =>
  filteredMenus.value.length === 0
    ? 0
    : (currentPage.value - 1) * itemsPerPage.value + 1,
);
const showingEnd = computed(() =>
  Math.min(currentPage.value * itemsPerPage.value, filteredMenus.value.length),
);

// Algoritma Pagination Dinamis (Maksimal 7 Kotak)
const paginationArray = computed(() => {
  const current = currentPage.value;
  const total = totalPages.value;

  // Jika halaman sedikit (<= 7), tampilkan semuanya: [1] [2] [3] [4] [5] [6] [7]
  if (total <= 7) {
    return Array.from({ length: total }, (_, i) => i + 1);
  }

  // Jika posisi di awal (contoh current = 1, 2, 3, 4): [1] [2] [3] [4] [5] [...] [100]
  if (current <= 4) {
    return [1, 2, 3, 4, 5, "...", total];
  }

  // Jika posisi di akhir (contoh current = 97, 98, 99, 100): [1] [...] [96] [97] [98] [99] [100]
  if (current >= total - 3) {
    return [1, "...", total - 4, total - 3, total - 2, total - 1, total];
  }

  // Jika posisi di tengah (contoh current = 5): [1] [...] [4] [5] [6] [...] [100]
  return [1, "...", current - 1, current, current + 1, "...", total];
});

// --- API ACTIONS ---
const deleteMenu = async (id) => {
  if (!confirm("Yakin ingin menghapus menu ini?")) return;
  try {
    await $fetch(`${baseURL}/menus/${id}`, { method: "DELETE" });
    refresh();
  } catch (error) {
    alert("Gagal menghapus data");
  }
};
</script> -->

<template>
  <div class="container mx-auto p-6 max-w-6xl">
    <div
      class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 gap-4"
    >
      <h1 class="text-3xl font-bold text-gray-800">Manajemen Menu Makanan</h1>
      <NuxtLink
        to="/admin/menu/add_menu_page"
        class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg font-medium transition whitespace-nowrap"
      >
        + Tambah Menu Baru
      </NuxtLink>
    </div>

    <!-- FILTER BAR (Search & Items per page) -->
    <div
      class="bg-white p-4 rounded-t-lg shadow-sm border-b border-gray-100 flex flex-col sm:flex-row justify-between items-center gap-4"
    >
      <!-- Items Per Page Dropdown -->
      <div class="flex items-center gap-2 text-sm text-gray-600">
        <span>Tampilkan</span>
        <select
          v-model="itemsPerPage"
          class="border border-gray-300 rounded px-2 py-1 focus:ring-orange-500 focus:border-orange-500 outline-none"
        >
          <option :value="5">5</option>
          <option :value="10">10</option>
          <option :value="25">25</option>
          <option :value="50">50</option>
          <option :value="100">100</option>
        </select>
        <span>item</span>
      </div>

      <!-- Search Bar -->
      <div class="relative w-full sm:w-64">
        <input
          v-model="searchQuery"
          type="text"
          placeholder="Cari nama, harga, atau kategori..."
          class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-orange-500 focus:border-transparent outline-none text-sm transition-all"
        />
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-5 w-5 text-gray-400 absolute left-3 top-1/2 transform -translate-y-1/2"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
          />
        </svg>
      </div>
    </div>

    <!-- TABEL DATA -->
    <div class="bg-white shadow overflow-x-auto">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Gambar</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Nama Menu</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Kategori</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Harga</th>
            <th class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase">Aksi</th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          <!-- SKELETON LOADING -->
          <template v-if="pending">
            <tr v-for="n in 5" :key="'skel-' + n" class="animate-pulse hover:bg-gray-50">
              <td class="px-6 py-4 whitespace-nowrap"><div class="w-16 h-16 bg-gray-200 rounded-md"></div></td>
              <td class="px-6 py-4 whitespace-nowrap"><div class="h-4 bg-gray-200 rounded w-3/4"></div></td>
              <td class="px-6 py-4 whitespace-nowrap"><div class="h-4 bg-gray-200 rounded w-1/2"></div></td>
              <td class="px-6 py-4 whitespace-nowrap"><div class="h-4 bg-gray-200 rounded w-1/3"></div></td>
              <td class="px-6 py-4 whitespace-nowrap text-center space-x-4"><div class="h-4 bg-gray-200 rounded w-20 inline-block"></div></td>
            </tr>
          </template>

          <!-- PESAN KOSONG -->
          <template v-else-if="paginatedMenus.length === 0">
            <tr>
              <td colspan="5" class="px-6 py-12 text-center text-gray-500">
                Data tidak ditemukan.
              </td>
            </tr>
          </template>

          <!-- DATA ASLI -->
          <template v-else>
            <tr
              v-for="menu in paginatedMenus"
              :key="menu.id"
              class="hover:bg-gray-50 items-center transition-colors"
            >
              <td class="px-6 py-4 whitespace-nowrap">
                <img
                  v-if="menu.image_url"
                  :src="menu.image_url"
                  :alt="menu.name"
                  class="w-16 h-16 object-cover rounded-md shadow-sm border border-gray-200"
                />
                <div
                  v-else
                  class="w-16 h-16 bg-gray-100 rounded-md flex items-center justify-center text-xs text-gray-400 border border-gray-200"
                >
                  No Img
                </div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">
                {{ menu.name }}
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-600">
                <span class="bg-gray-100 px-2 py-1 rounded text-xs">{{
                  menu.category?.name || "Tanpa Kategori"
                }}</span>
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-green-600 font-bold">
                Rp {{ menu.price.toLocaleString("id-ID") }}
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-center text-sm font-medium space-x-3">
                <NuxtLink
                  :to="`/admin/menu/detail/${menu.id}`"
                  class="text-blue-600 hover:text-blue-900 bg-blue-50 px-2 py-1 rounded transition"
                  >Detail</NuxtLink
                >
                <NuxtLink
                  :to="`/admin/menu/edit/${menu.id}`"
                  class="text-amber-600 hover:text-amber-900 bg-amber-50 px-2 py-1 rounded transition"
                  >Edit</NuxtLink
                >
                <button
                  @click="deleteMenu(menu.id)"
                  class="text-red-600 hover:text-red-900 bg-red-50 px-2 py-1 rounded transition"
                >
                  Hapus
                </button>
              </td>
            </tr>
          </template>
        </tbody>
      </table>
    </div>

    <!-- PAGINATION FOOTER -->
    <div
      class="bg-white p-4 rounded-b-lg shadow-sm flex flex-col md:flex-row justify-between items-center gap-4 text-sm text-gray-600 border-t border-gray-100"
    >
      <div>
        Showing
        <span class="font-bold text-gray-900">{{ showingStart }}</span> to
        <span class="font-bold text-gray-900">{{ showingEnd }}</span> of
        <span class="font-bold text-gray-900">{{ filteredMenus.length }}</span>
        items
      </div>

      <div class="flex items-center space-x-1" v-if="totalPages > 1">
        <!-- Tombol Prev -->
        <button
          @click="currentPage--"
          :disabled="currentPage === 1"
          class="px-3 py-1.5 border border-gray-300 rounded hover:bg-gray-50 disabled:opacity-50 disabled:cursor-not-allowed transition"
        >
          &laquo;
        </button>

        <!-- Deretan Kotak Angka -->
        <template v-for="(pageItem, index) in paginationArray" :key="index">
          <span v-if="pageItem === '...'" class="px-2 py-1.5 text-gray-400">...</span>
          <button
            v-else
            @click="currentPage = pageItem"
            :class="[
              'px-3 py-1.5 border rounded transition',
              currentPage === pageItem
                ? 'bg-orange-500 text-white border-orange-500 font-bold'
                : 'border-gray-300 text-gray-700 hover:bg-gray-50',
            ]"
          >
            {{ pageItem }}
          </button>
        </template>

        <!-- Tombol Next -->
        <button
          @click="currentPage++"
          :disabled="currentPage === totalPages"
          class="px-3 py-1.5 border border-gray-300 rounded hover:bg-gray-50 disabled:opacity-50 disabled:cursor-not-allowed transition"
        >
          &raquo;
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";
import Swal from "sweetalert2"; // TAMBAHAN: Import SweetAlert2

definePageMeta({ layout: "admin", middleware: "auth" });

const baseURL = "https://back.kecilungresto.com/api";

const { data: response, pending, refresh } = useLazyFetch(`${baseURL}/menus`);
const allMenus = computed(() => response.value?.data || []);

// --- STATE FILTER & PAGINATION ---
const searchQuery = ref("");
const itemsPerPage = ref(10);
const currentPage = ref(1);

watch([searchQuery, itemsPerPage], () => {
  currentPage.value = 1;
});

// --- LOGIKA FILTER (SEARCH BAR) ---
const filteredMenus = computed(() => {
  if (!searchQuery.value) return allMenus.value;

  const q = searchQuery.value.toLowerCase();
  return allMenus.value.filter((menu) => {
    return (
      menu.name.toLowerCase().includes(q) ||
      (menu.category?.name && menu.category.name.toLowerCase().includes(q)) ||
      menu.price.toString().includes(q)
    );
  });
});

// --- LOGIKA PAGINATION ---
const totalPages = computed(() => {
  return Math.ceil(filteredMenus.value.length / itemsPerPage.value);
});

const paginatedMenus = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage.value;
  const end = start + itemsPerPage.value;
  return filteredMenus.value.slice(start, end);
});

const showingStart = computed(() =>
  filteredMenus.value.length === 0 ? 0 : (currentPage.value - 1) * itemsPerPage.value + 1
);
const showingEnd = computed(() =>
  Math.min(currentPage.value * itemsPerPage.value, filteredMenus.value.length)
);

const paginationArray = computed(() => {
  const current = currentPage.value;
  const total = totalPages.value;

  if (total <= 7) return Array.from({ length: total }, (_, i) => i + 1);
  if (current <= 4) return [1, 2, 3, 4, 5, "...", total];
  if (current >= total - 3) return [1, "...", total - 4, total - 3, total - 2, total - 1, total];
  return [1, "...", current - 1, current, current + 1, "...", total];
});

// --- API ACTIONS (SWEETALERT) ---
const deleteMenu = async (id) => {
  // UBAH: Menggunakan konfirmasi SweetAlert2
  const result = await Swal.fire({
    title: "Apakah Anda Yakin?",
    text: "Menu makanan ini akan dihapus secara permanen!",
    icon: "warning",
    showCancelButton: true,
    confirmButtonColor: "#d33",
    cancelButtonColor: "#9ca3af",
    confirmButtonText: "Ya, Hapus!",
    cancelButtonText: "Batal",
  });

  if (result.isConfirmed) {
    try {
      await $fetch(`${baseURL}/menus/${id}`, { method: "DELETE" });
      Swal.fire("Terhapus!", "Data menu berhasil dihapus.", "success");
      refresh();
    } catch (error) {
      Swal.fire("Gagal", "Gagal menghapus data menu.", "error");
    }
  }
};
</script>
