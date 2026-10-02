<!-- <template>
  <div class="p-6">
    <div class="flex justify-between items-center mb-6">
      <h1 class="text-2xl font-bold text-gray-800">Manajemen Artikel</h1>
      <NuxtLink
        to="/admin/article/add_article_page"
        class="bg-blue-600 text-white px-4 py-2 rounded shadow hover:bg-blue-700 transition"
      >
        + Tambah Artikel
      </NuxtLink>
    </div>

    <div class="bg-white rounded shadow p-4 overflow-x-auto">
      <table class="w-full text-left border-collapse">
        <thead>
          <tr class="bg-gray-100 text-gray-700">
            <th class="p-3 border-b">No</th>
            <th class="p-3 border-b">Kode</th>
            <th class="p-3 border-b">Nama Artikel</th>
            <th class="p-3 border-b">Deskripsi Singkat</th>
            <th class="p-3 border-b text-center">Aksi</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(item, index) in articles"
            :key="item.id"
            class="hover:bg-gray-50 border-b"
          >
            <td class="p-3">{{ index + 1 }}</td>
            <td class="p-3 font-semibold">{{ item.code }}</td>
            <td class="p-3">{{ item.name }}</td>
            <td class="p-3 truncate max-w-xs">{{ item.description }}</td>
            <td class="p-3 text-center space-x-2">
              <NuxtLink
                :to="`/admin/article/detail/${item.id}`"
                class="text-green-600 hover:underline"
                >Detail</NuxtLink
              >
              <NuxtLink
                :to="`/admin/article/edit/${item.id}`"
                class="text-blue-600 hover:underline"
                >Edit</NuxtLink
              >
              <button
                @click="confirmDelete(item.id)"
                class="text-red-600 hover:underline"
              >
                Hapus
              </button>
            </td>
          </tr>
          <tr v-if="articles.length === 0">
            <td colspan="5" class="p-6 text-center text-gray-500">
              Belum ada data artikel.
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import Swal from "sweetalert2";

definePageMeta({ layout: "admin", middleware: "auth" });

const config = useRuntimeConfig();
const articles = ref([]);

const fetchArticles = async () => {
  try {
    const res = await $fetch(
      `${config.public.apiBase || "http://31.97.60.207:8246"}/api/articles`,
    );
    articles.value = res.data || [];
  } catch (error) {
    Swal.fire("Error!", "Gagal mengambil data artikel", "error");
  }
};

const confirmDelete = (id) => {
  Swal.fire({
    title: "Apakah Anda yakin?",
    text: "Data yang dihapus tidak dapat dikembalikan!",
    icon: "warning",
    showCancelButton: true,
    confirmButtonColor: "#d33",
    cancelButtonColor: "#3085d6",
    confirmButtonText: "Ya, hapus!",
    cancelButtonText: "Batal",
  }).then(async (result) => {
    if (result.isConfirmed) {
      try {
        await $fetch(
          `${config.public.apiBase || "http://31.97.60.207:8246"}/api/articles/${id}`,
          { method: "DELETE" },
        );
        Swal.fire("Terhapus!", "Artikel berhasil dihapus.", "success");
        fetchArticles();
      } catch (error) {
        Swal.fire("Gagal!", "Terjadi kesalahan saat menghapus data.", "error");
      }
    }
  });
};

onMounted(() => {
  fetchArticles();
});
</script> -->

<template>
  <div class="p-6">
    <div
      class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 gap-4"
    >
      <h1 class="text-2xl font-bold text-gray-800">Manajemen Artikel</h1>
      <NuxtLink
        to="/admin/article/add_article_page"
        class="bg-blue-600 text-white px-4 py-2 rounded shadow hover:bg-blue-700 transition whitespace-nowrap"
      >
        + Tambah Artikel
      </NuxtLink>
    </div>

    <!-- FILTER BAR (Search & Items per page) -->
    <div
      class="bg-white p-4 rounded-t shadow-sm border-b border-gray-100 flex flex-col sm:flex-row justify-between items-center gap-4"
    >
      <!-- Items Per Page Dropdown -->
      <div class="flex items-center gap-2 text-sm text-gray-600">
        <span>Tampilkan</span>
        <select
          v-model="itemsPerPage"
          class="border border-gray-300 rounded px-2 py-1 focus:ring-blue-500 focus:border-blue-500 outline-none"
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
          placeholder="Cari kode, nama, deskripsi..."
          class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none text-sm transition-all"
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

    <div class="bg-white shadow p-0 overflow-x-auto">
      <table class="w-full text-left border-collapse">
        <thead>
          <tr class="bg-gray-100 text-gray-700">
            <th class="p-3 border-b">No</th>
            <th class="p-3 border-b">Kode</th>
            <th class="p-3 border-b">Nama Artikel</th>
            <th class="p-3 border-b">Deskripsi Singkat</th>
            <th class="p-3 border-b text-center">Aksi</th>
          </tr>
        </thead>
        <tbody>
          <!-- PERBAIKAN: Pisahkan v-if dan v-for menggunakan tag <template> -->
          <template v-if="isLoading">
            <tr
              v-for="n in 5"
              :key="'skel-' + n"
              class="animate-pulse hover:bg-gray-50 border-b"
            >
              <td class="p-3">
                <div class="h-4 bg-gray-200 rounded w-8"></div>
              </td>
              <td class="p-3">
                <div class="h-4 bg-gray-200 rounded w-16"></div>
              </td>
              <td class="p-3">
                <div class="h-4 bg-gray-200 rounded w-48"></div>
              </td>
              <td class="p-3">
                <div class="h-4 bg-gray-200 rounded w-64"></div>
              </td>
              <td class="p-3">
                <div class="h-4 bg-gray-200 rounded w-24 mx-auto"></div>
              </td>
            </tr>
          </template>

          <template v-else-if="paginatedArticles.length === 0">
            <tr>
              <td colspan="5" class="p-6 text-center text-gray-500">
                Belum ada data artikel.
              </td>
            </tr>
          </template>

          <template v-else>
            <tr
              v-for="(item, index) in paginatedArticles"
              :key="item.id"
              class="hover:bg-gray-50 border-b transition-colors"
            >
              <!-- Perhitungan No urut agar sesuai dengan pagination -->
              <td class="p-3">
                {{ (currentPage - 1) * itemsPerPage + index + 1 }}
              </td>
              <td class="p-3 font-semibold">
                <span class="bg-gray-100 px-2 py-1 rounded text-xs">{{
                  item.code
                }}</span>
              </td>
              <td class="p-3">{{ item.name }}</td>
              <td class="p-3 truncate max-w-xs">{{ item.description }}</td>
              <td class="p-3 text-center space-x-2 whitespace-nowrap">
                <NuxtLink
                  :to="`/admin/article/detail/${item.id}`"
                  class="text-green-600 hover:text-green-800 bg-green-50 px-2 py-1 rounded"
                  >Detail</NuxtLink
                >
                <NuxtLink
                  :to="`/admin/article/edit/${item.id}`"
                  class="text-blue-600 hover:text-blue-800 bg-blue-50 px-2 py-1 rounded"
                  >Edit</NuxtLink
                >
                <button
                  @click="confirmDelete(item.id)"
                  class="text-red-600 hover:text-red-800 bg-red-50 px-2 py-1 rounded"
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
      class="bg-white p-4 rounded-b shadow-sm flex flex-col md:flex-row justify-between items-center gap-4 text-sm text-gray-600 border-t border-gray-100 mt-0"
    >
      <div>
        Showing
        <span class="font-bold text-gray-900">{{ showingStart }}</span> to
        <span class="font-bold text-gray-900">{{ showingEnd }}</span> of
        <span class="font-bold text-gray-900">{{
          filteredArticles.length
        }}</span>
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
                ? 'bg-blue-600 text-white border-blue-600 font-bold'
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
import { ref, computed, watch, onMounted } from "vue";
import Swal from "sweetalert2";

definePageMeta({ layout: "admin", middleware: "auth" });

const config = useRuntimeConfig();
const allArticles = ref([]);
const isLoading = ref(true);

// --- STATE FILTER & PAGINATION ---
const searchQuery = ref("");
const itemsPerPage = ref(10);
const currentPage = ref(1);

watch([searchQuery, itemsPerPage], () => {
  currentPage.value = 1;
});

// --- LOGIKA FILTER (SEARCH BAR) ---
const filteredArticles = computed(() => {
  if (!searchQuery.value) return allArticles.value;
  const q = searchQuery.value.toLowerCase();
  return allArticles.value.filter(
    (item) =>
      item.name.toLowerCase().includes(q) ||
      item.code.toLowerCase().includes(q) ||
      item.description.toLowerCase().includes(q),
  );
});

// --- LOGIKA PAGINATION ---
const totalPages = computed(() =>
  Math.ceil(filteredArticles.value.length / itemsPerPage.value),
);

const paginatedArticles = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage.value;
  return filteredArticles.value.slice(start, start + itemsPerPage.value);
});

const showingStart = computed(() =>
  filteredArticles.value.length === 0
    ? 0
    : (currentPage.value - 1) * itemsPerPage.value + 1,
);
const showingEnd = computed(() =>
  Math.min(
    currentPage.value * itemsPerPage.value,
    filteredArticles.value.length,
  ),
);

const paginationArray = computed(() => {
  const current = currentPage.value;
  const total = totalPages.value;
  if (total <= 7) return Array.from({ length: total }, (_, i) => i + 1);
  if (current <= 4) return [1, 2, 3, 4, 5, "...", total];
  if (current >= total - 3)
    return [1, "...", total - 4, total - 3, total - 2, total - 1, total];
  return [1, "...", current - 1, current, current + 1, "...", total];
});

// --- API ACTIONS ---
const fetchArticles = async () => {
  isLoading.value = true;
  try {
    const res = await $fetch(
      `${config.public.apiBase || "http://31.97.60.207:8246"}/api/articles`,
    );
    allArticles.value = res.data || [];
  } catch (error) {
    Swal.fire("Error!", "Gagal mengambil data artikel", "error");
  } finally {
    isLoading.value = false;
  }
};

const confirmDelete = (id) => {
  Swal.fire({
    title: "Apakah Anda yakin?",
    text: "Data yang dihapus tidak dapat dikembalikan!",
    icon: "warning",
    showCancelButton: true,
    confirmButtonColor: "#d33",
    cancelButtonColor: "#3085d6",
    confirmButtonText: "Ya, hapus!",
    cancelButtonText: "Batal",
  }).then(async (result) => {
    if (result.isConfirmed) {
      try {
        await $fetch(
          `${config.public.apiBase || "http://31.97.60.207:8246"}/api/articles/${id}`,
          { method: "DELETE" },
        );
        Swal.fire("Terhapus!", "Artikel berhasil dihapus.", "success");
        fetchArticles();
      } catch (error) {
        Swal.fire("Gagal!", "Terjadi kesalahan saat menghapus data.", "error");
      }
    }
  });
};

onMounted(() => {
  fetchArticles();
});
</script>
