<!-- <template>
  <div class="container mx-auto p-6 md:p-8 space-y-8">
    <div
      class="flex items-center justify-between gap-4 border-b border-gray-200 pb-6 bg-white p-6 rounded-2xl shadow-sm"
    >
      <div>
        <h1 class="text-3xl font-extrabold text-gray-900 tracking-tight">
          Dashboard Overview
        </h1>
        <p class="text-gray-600 mt-1">
          Selamat datang kembali, {{ currentAdmin.name }}. Berikut ringkasan
          data Kecilung Resto.
        </p>
      </div>
      <div
        class="text-sm font-medium text-gray-500 bg-gray-100 px-4 py-2 rounded-full shadow-inner"
      >
        {{ currentDate }}
      </div>
    </div>

    <div v-if="pending" class="flex justify-center items-center py-20">
      <div
        class="animate-spin rounded-full h-12 w-12 border-b-2 border-orange-500"
      ></div>
    </div>

    <div
      v-else-if="error"
      class="bg-red-50 text-red-700 p-6 rounded-2xl border border-red-200 text-center shadow-lg"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-12 w-12 mx-auto mb-4"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"
        />
      </svg>
      <h3 class="font-bold text-lg">Gagal Memuat Data Dashboard</h3>
      <p class="mt-1 text-sm">
        {{ error.message || "Terjadi kesalahan pada server." }}
      </p>
    </div>

    <div v-else class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-3 gap-6">
      <NuxtLink
        to="/admin/category/category_page"
        class="stat-card group hover:border-orange-300 transition-all duration-300"
      >
        <div class="flex items-center gap-4">
          <div class="icon-box bg-orange-100 text-orange-600">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-7 w-7"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10"
              />
            </svg>
          </div>
          <div>
            <p
              class="text-sm font-medium text-gray-500 group-hover:text-orange-700 transition-colors"
            >
              Total Kategori Menu
            </p>
            <p class="text-3xl font-bold text-gray-900 mt-1">
              {{ stats.categories }}
            </p>
          </div>
        </div>
      </NuxtLink>

      <NuxtLink
        to="/admin/menu/menu_page"
        class="stat-card group hover:border-orange-300 transition-all duration-300"
      >
        <div class="flex items-center gap-4">
          <div class="icon-box bg-orange-100 text-orange-600">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-7 w-7"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"
              />
            </svg>
          </div>
          <div>
            <p
              class="text-sm font-medium text-gray-500 group-hover:text-orange-700 transition-colors"
            >
              Total Item Menu
            </p>
            <p class="text-3xl font-bold text-gray-900 mt-1">
              {{ stats.menus }}
            </p>
          </div>
        </div>
      </NuxtLink>

      <NuxtLink
        to="/admin/article/article_page"
        class="stat-card group hover:border-orange-300 transition-all duration-300"
      >
        <div class="flex items-center gap-4">
          <div class="icon-box bg-orange-100 text-orange-600">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-7 w-7"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M19 20H5a2 2 0 01-2-2V6a2 2 0 012-2h10a2 2 0 012 2v1m2 13a2 2 0 01-2-2V7m2 13a2 2 0 002-2V9.5a2.5 2.5 0 00-2.5-2.5H14"
              />
            </svg>
          </div>
          <div>
            <p
              class="text-sm font-medium text-gray-500 group-hover:text-orange-700 transition-colors"
            >
              Total Artikel/Berita
            </p>
            <p class="text-3xl font-bold text-gray-900 mt-1">
              {{ stats.articles }}
            </p>
          </div>
        </div>
      </NuxtLink>

      <NuxtLink
        to="/admin/contact/contact_page"
        class="stat-card group hover:border-orange-300 transition-all duration-300"
      >
        <div class="flex items-center gap-4">
          <div class="icon-box bg-orange-100 text-orange-600">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-7 w-7"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"
              />
            </svg>
          </div>
          <div>
            <p
              class="text-sm font-medium text-gray-500 group-hover:text-orange-700 transition-colors"
            >
              Pesan Belum Dibalas
            </p>
            <div class="flex items-baseline gap-2 mt-1">
              <p class="text-3xl font-bold text-gray-900">
                {{ stats.unrepliedMessages }}
              </p>
              <p class="text-sm text-gray-500">
                / {{ stats.totalMessages }} total
              </p>
            </div>
          </div>
        </div>
      </NuxtLink>

      <NuxtLink
        to="/admin/catering/booking_page"
        class="stat-card group hover:border-orange-300 transition-all duration-300 sm:col-span-2 xl:col-span-1"
      >
        <div class="flex items-center gap-4">
          <div class="icon-box bg-orange-100 text-orange-600">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-7 w-7"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M21 15.546c-.523 0-1.046.151-1.5.454a2.704 2.704 0 01-3 0 2.704 2.704 0 00-3 0 2.704 2.704 0 01-3 0 2.704 2.704 0 00-3 0 2.704 2.704 0 01-3 0 2.701 2.701 0 00-1.5-.454M9 6v2m3-2v2m3-2v2M9 3h.01M12 3h.01M15 3h.01M21 21v-7a2 2 0 00-2-2H5a2 2 0 00-2 2v7h18zm-3-9v-2a2 2 0 00-2-2H8a2 2 0 00-2 2v2h12z"
              />
            </svg>
          </div>
          <div class="flex-1">
            <p
              class="text-sm font-medium text-gray-500 group-hover:text-orange-700 transition-colors"
            >
              Booking Katering Aktif (APPROVED)
            </p>
            <p class="text-3xl font-bold text-gray-900 mt-1">
              {{ stats.cateringBookings }}
            </p>
          </div>
        </div>
      </NuxtLink>

      <NuxtLink
        to="/admin/moment/booking_page"
        class="stat-card group hover:border-orange-300 transition-all duration-300 sm:col-span-2 xl:col-span-1"
      >
        <div class="flex items-center gap-4">
          <div class="icon-box bg-orange-100 text-orange-600">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-7 w-7"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M5 3v4M3 5h4M6 17v4m-2-2h4m5-16l2.286 6.857L21 12l-5.714 2.143L13 21l-2.286-6.857L5 12l5.714-2.143L13 3z"
              />
            </svg>
          </div>
          <div class="flex-1">
            <p
              class="text-sm font-medium text-gray-500 group-hover:text-orange-700 transition-colors"
            >
              Booking Moment Aktif (APPROVED)
            </p>
            <p class="text-3xl font-bold text-gray-900 mt-1">
              {{ stats.momentBookings }}
            </p>
          </div>
        </div>
      </NuxtLink>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";

// Proteksi Halaman & Layout
definePageMeta({ layout: "admin", middleware: "auth" });

const config = useRuntimeConfig();
const baseURL = config.public.apiBase || "http://31.97.60.207:8246/api";

// State Data Profil Admin (dari localStorage)
const currentAdmin = ref({ name: "Admin" });

// State untuk ringkasan statistik
const stats = ref({
  categories: 0,
  menus: 0,
  articles: 0,
  cateringBookings: 0, // Count Approved
  momentBookings: 0, // Count Approved
  totalMessages: 0,
  unrepliedMessages: 0,
});

const currentDate = computed(() => {
  return new Date().toLocaleDateString("id-ID", {
    weekday: "long",
    year: "numeric",
    month: "long",
    day: "numeric",
  });
});

// Mengambil data admin saat halaman dimuat
onMounted(() => {
  const savedData = localStorage.getItem("admin_data");
  if (savedData) {
    currentAdmin.value = JSON.parse(savedData);
  }
});

// Pola Fallback: Ambil semua data lalu hitung jumlahnya (jika backend tidak punya endpoint stats)
// Diperlukan penyesuaian endpoint API yang sebenarnya di sini.

const { data: resCategories, pending: pCat } = useLazyFetch(
  `${baseURL}/categories`,
);
const { data: resMenus, pending: pMenu } = useLazyFetch(`${baseURL}/menus`);
const { data: resArticles, pending: pArt } = useLazyFetch(
  `${baseURL}/articles`,
);
const { data: resCatBookings, pending: pCBook } = useLazyFetch(
  `${baseURL}/catering/bookings`,
);
const { data: resMomBookings, pending: pMBook } = useLazyFetch(
  `${baseURL}/moments/bookings`,
);
const { data: resContacts, pending: pCont } = useLazyFetch(
  `${baseURL}/contacts`,
);

// Computed gabungan untuk status loading
const pending = computed(
  () =>
    pCat.value ||
    pMenu.value ||
    pArt.value ||
    pCBook.value ||
    pMBook.value ||
    pCont.value,
);

// Computed gabungan untuk error (jika salah satu gagal)
const error = computed(
  () =>
    resCategories.value?.error ||
    resMenus.value?.error ||
    resArticles.value?.error ||
    resCatBookings.value?.error ||
    resMomBookings.value?.error ||
    resContacts.value?.error,
);

// Watch data yang masuk untuk memperbarui statistik
watchEffect(() => {
  if (resCategories.value?.data)
    stats.value.categories = resCategories.value.data.length;
  if (resMenus.value?.data) stats.value.menus = resMenus.value.data.length;
  if (resArticles.value?.data)
    stats.value.articles = resArticles.value.data.length;

  // Hitung yang APPROVED saja untuk Catering
  if (resCatBookings.value?.data) {
    stats.value.cateringBookings = resCatBookings.value.data.filter(
      (b) => b.status === "APPROVED",
    ).length;
  }

  // Hitung yang APPROVED saja untuk Moment
  if (resMomBookings.value?.data) {
    stats.value.momentBookings = resMomBookings.value.data.filter(
      (b) => b.status === "APPROVED",
    ).length;
  }

  // Pesan (Unreplied vs Total)
  if (resContacts.value?.data) {
    const contacts = resContacts.value.data;
    stats.value.totalMessages = contacts.length;
    stats.value.unrepliedMessages = contacts.filter(
      (c) => !c.is_replied,
    ).length;
  }
});
</script>

<style scoped>
.stat-card {
  @apply bg-white p-6 rounded-2xl shadow-sm border border-gray-100 flex items-center gap-4 block;
}
.icon-box {
  @apply p-3 rounded-full flex-shrink-0;
}
</style> -->

<template>
  <div class="container mx-auto p-6 md:p-8 space-y-8">
    <!-- Header -->
    <div
      class="flex items-center justify-between gap-4 border-b border-gray-200 pb-6 bg-white p-6 rounded-2xl shadow-sm"
    >
      <div>
        <h1 class="text-3xl font-extrabold text-gray-900 tracking-tight">
          Dashboard Overview
        </h1>
        <p class="text-gray-600 mt-1">
          Selamat datang kembali, {{ currentAdmin.name }}. Berikut ringkasan
          data Kecilung Resto.
        </p>
      </div>
      <div
        class="text-sm font-medium text-gray-500 bg-gray-100 px-4 py-2 rounded-full shadow-inner"
      >
        {{ currentDate }}
      </div>
    </div>

    <!-- Loading State -->
    <div v-if="pending" class="flex justify-center items-center py-20">
      <div
        class="animate-spin rounded-full h-12 w-12 border-b-2 border-orange-500"
      ></div>
    </div>

    <!-- Error State -->
    <div
      v-else-if="error"
      class="bg-red-50 text-red-700 p-6 rounded-2xl border border-red-200 text-center shadow-lg"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-12 w-12 mx-auto mb-4"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"
        />
      </svg>
      <h3 class="font-bold text-lg">Gagal Memuat Data Dashboard</h3>
      <p class="mt-1 text-sm">
        Terjadi kesalahan saat memuat data dari server.
      </p>
    </div>

    <!-- Konten Dashboard Utama -->
    <div v-else class="space-y-8">
      <!-- Stat Cards Grid -->
      <div class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-3 gap-6">
        <!-- Card: Category -->
        <NuxtLink to="/admin/category/category_page" class="stat-card group">
          <div class="flex items-center gap-4">
            <div class="icon-box bg-orange-100 text-orange-600">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-7 w-7" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10" />
              </svg>
            </div>
            <div>
              <p class="text-sm font-medium text-gray-500 group-hover:text-orange-700">Total Kategori Menu</p>
              <p class="text-3xl font-bold text-gray-900 mt-1">{{ stats.categories }}</p>
            </div>
          </div>
        </NuxtLink>

        <!-- Card: Menu -->
        <NuxtLink to="/admin/menu/menu_page" class="stat-card group">
          <div class="flex items-center gap-4">
            <div class="icon-box bg-orange-100 text-orange-600">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-7 w-7" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
              </svg>
            </div>
            <div>
              <p class="text-sm font-medium text-gray-500 group-hover:text-orange-700">Total Item Menu</p>
              <p class="text-3xl font-bold text-gray-900 mt-1">{{ stats.menus }}</p>
            </div>
          </div>
        </NuxtLink>

        <!-- Card: Article -->
        <NuxtLink to="/admin/article/article_page" class="stat-card group">
          <div class="flex items-center gap-4">
            <div class="icon-box bg-orange-100 text-orange-600">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-7 w-7" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 20H5a2 2 0 01-2-2V6a2 2 0 012-2h10a2 2 0 012 2v1m2 13a2 2 0 01-2-2V7m2 13a2 2 0 002-2V9.5a2.5 2.5 0 00-2.5-2.5H14" />
              </svg>
            </div>
            <div>
              <p class="text-sm font-medium text-gray-500 group-hover:text-orange-700">Total Artikel/Berita</p>
              <p class="text-3xl font-bold text-gray-900 mt-1">{{ stats.articles }}</p>
            </div>
          </div>
        </NuxtLink>

        <!-- Card: Pesan -->
        <NuxtLink to="/admin/contact/contact_page" class="stat-card group">
          <div class="flex items-center gap-4">
            <div class="icon-box bg-orange-100 text-orange-600">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-7 w-7" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
              </svg>
            </div>
            <div>
              <p class="text-sm font-medium text-gray-500 group-hover:text-orange-700">Pesan Belum Dibalas</p>
              <div class="flex items-baseline gap-2 mt-1">
                <p class="text-3xl font-bold text-gray-900">{{ stats.unrepliedMessages }}</p>
                <p class="text-sm text-gray-500">/ {{ stats.totalMessages }} total</p>
              </div>
            </div>
          </div>
        </NuxtLink>

        <!-- Card: Catering Booking -->
        <NuxtLink to="/admin/catering/booking_page" class="stat-card group sm:col-span-2 xl:col-span-1">
          <div class="flex items-center gap-4">
            <div class="icon-box bg-orange-100 text-orange-600">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-7 w-7" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 15.546c-.523 0-1.046.151-1.5.454a2.704 2.704 0 01-3 0 2.704 2.704 0 00-3 0 2.704 2.704 0 01-3 0 2.704 2.704 0 00-3 0 2.704 2.704 0 01-3 0 2.701 2.701 0 00-1.5-.454M9 6v2m3-2v2m3-2v2M9 3h.01M12 3h.01M15 3h.01M21 21v-7a2 2 0 00-2-2H5a2 2 0 00-2 2v7h18zm-3-9v-2a2 2 0 00-2-2H8a2 2 0 00-2 2v2h12z" />
              </svg>
            </div>
            <div class="flex-1">
              <p class="text-sm font-medium text-gray-500 group-hover:text-orange-700">Katering Aktif (APPROVED)</p>
              <p class="text-3xl font-bold text-gray-900 mt-1">{{ stats.cateringBookings }}</p>
            </div>
          </div>
        </NuxtLink>

        <!-- Card: Moment Booking -->
        <NuxtLink to="/admin/moment/booking_page" class="stat-card group sm:col-span-2 xl:col-span-1">
          <div class="flex items-center gap-4">
            <div class="icon-box bg-orange-100 text-orange-600">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-7 w-7" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 3v4M3 5h4M6 17v4m-2-2h4m5-16l2.286 6.857L21 12l-5.714 2.143L13 21l-2.286-6.857L5 12l5.714-2.143L13 3z" />
              </svg>
            </div>
            <div class="flex-1">
              <p class="text-sm font-medium text-gray-500 group-hover:text-orange-700">Moment Aktif (APPROVED)</p>
              <p class="text-3xl font-bold text-gray-900 mt-1">{{ stats.momentBookings }}</p>
            </div>
          </div>
        </NuxtLink>
      </div>

      <!-- SECTION CHART -->
      <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
        
        <!-- Line Chart: Pertumbuhan Booking -->
        <div class="lg:col-span-2 bg-white p-6 rounded-2xl shadow-sm border border-gray-100">
          <h2 class="text-lg font-bold text-gray-800 mb-4">Tren Booking (6 Bulan Terakhir)</h2>
          <div class="h-72">
            <!-- ClientOnly mencegah error SSR karena Chart.js butuh DOM Canvas -->
            <ClientOnly>
              <LineChart :data="bookingChartData" :options="lineChartOptions" />
            </ClientOnly>
          </div>
        </div>

        <!-- Doughnut Chart: Proporsi Kategori Menu -->
        <div class="bg-white p-6 rounded-2xl shadow-sm border border-gray-100">
          <h2 class="text-lg font-bold text-gray-800 mb-4">Produk per Kategori</h2>
          <div class="h-72 flex justify-center">
            <ClientOnly>
              <DoughnutChart :data="categoryChartData" :options="doughnutChartOptions" />
            </ClientOnly>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watchEffect } from "vue";
// Import ChartJS
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend,
  ArcElement
} from "chart.js";
import { Line as LineChart, Doughnut as DoughnutChart } from "vue-chartjs";

// Registrasi modul ChartJS
ChartJS.register(
  CategoryScale, LinearScale, PointElement, LineElement, Title, Tooltip, Legend, ArcElement
);

definePageMeta({ layout: "admin", middleware: "auth" });

const config = useRuntimeConfig();
const baseURL = config.public.apiBase || "http://31.97.60.207:8246/api";
const currentAdmin = ref({ name: "Admin" });

const stats = ref({
  categories: 0,
  menus: 0,
  articles: 0,
  cateringBookings: 0,
  momentBookings: 0,
  totalMessages: 0,
  unrepliedMessages: 0,
});

const currentDate = computed(() => {
  return new Date().toLocaleDateString("id-ID", {
    weekday: "long", year: "numeric", month: "long", day: "numeric",
  });
});

onMounted(() => {
  const savedData = localStorage.getItem("admin_data");
  if (savedData) currentAdmin.value = JSON.parse(savedData);
});

// Fetch Data
const { data: resCategories, pending: pCat } = useLazyFetch(`${baseURL}/categories`);
const { data: resMenus, pending: pMenu } = useLazyFetch(`${baseURL}/menus`);
const { data: resArticles, pending: pArt } = useLazyFetch(`${baseURL}/articles`);
const { data: resCatBookings, pending: pCBook } = useLazyFetch(`${baseURL}/catering/bookings`);
const { data: resMomBookings, pending: pMBook } = useLazyFetch(`${baseURL}/moments/bookings`);
const { data: resContacts, pending: pCont } = useLazyFetch(`${baseURL}/contacts`);

const pending = computed(() => pCat.value || pMenu.value || pArt.value || pCBook.value || pMBook.value || pCont.value);
const error = computed(() => resCategories.value?.error || resMenus.value?.error || resArticles.value?.error || resCatBookings.value?.error || resMomBookings.value?.error || resContacts.value?.error);

// ----------------------------------------------------
// LOGIKA PEMROSESAN DATA GRAFIK
// ----------------------------------------------------

// 1. Setup Data Line Chart (Booking Trend)
const bookingChartData = ref({
  labels: [], // e.g. ["Mei", "Jun", "Jul", "Ags", "Sep", "Okt"]
  datasets: [
    { label: "Catering", data: [], borderColor: "#f97316", backgroundColor: "#f97316", tension: 0.3 },
    { label: "Moment", data: [], borderColor: "#3b82f6", backgroundColor: "#3b82f6", tension: 0.3 }
  ]
});

const lineChartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: { legend: { position: "bottom" } },
  scales: { y: { beginAtZero: true, ticks: { stepSize: 1 } } }
};

// 2. Setup Data Doughnut Chart (Menu Category Proportion)
const categoryChartData = ref({
  labels: [],
  datasets: [
    {
      data: [],
      backgroundColor: ["#f97316", "#3b82f6", "#10b981", "#ef4444", "#8b5cf6", "#f59e0b", "#64748b"],
      hoverOffset: 4
    }
  ]
});

const doughnutChartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: { legend: { position: "bottom" } }
};

// ----------------------------------------------------
// WATCHER (Mengolah API Menjadi Statistik & Grafik)
// ----------------------------------------------------
watchEffect(() => {
  // Update Basic Stats
  if (resCategories.value?.data) stats.value.categories = resCategories.value.data.length;
  if (resMenus.value?.data) stats.value.menus = resMenus.value.data.length;
  if (resArticles.value?.data) stats.value.articles = resArticles.value.data.length;
  if (resContacts.value?.data) {
    stats.value.totalMessages = resContacts.value.data.length;
    stats.value.unrepliedMessages = resContacts.value.data.filter(c => !c.is_replied).length;
  }

  // A. OLAH DATA DOUGHNUT CHART (Hitung jumlah menu per category_id)
  if (resCategories.value?.data && resMenus.value?.data) {
    const cats = resCategories.value.data;
    const menus = resMenus.value.data;
    
    // Siapkan array penghitung
    const catLabels = [];
    const catCounts = [];

    cats.forEach(c => {
      catLabels.push(c.name);
      // Hitung berapa menu yang category_id-nya sama dengan id kategori ini
      const count = menus.filter(m => m.category_id === c.id).length;
      catCounts.push(count);
    });

    categoryChartData.value = {
      labels: catLabels,
      datasets: [{ ...categoryChartData.value.datasets[0], data: catCounts }]
    };
  }

  // B. OLAH DATA LINE CHART (Ekstrak bulan dari CreatedAt)
  if (resCatBookings.value?.data && resMomBookings.value?.data) {
    const catBookings = resCatBookings.value.data;
    const momBookings = resMomBookings.value.data;

    // Filter hanya yang APPROVED untuk statistik atas
    stats.value.cateringBookings = catBookings.filter(b => b.status === "APPROVED").length;
    stats.value.momentBookings = momBookings.filter(b => b.status === "APPROVED").length;

    // Buat rentang 6 bulan terakhir
    const monthNames = ["Jan", "Feb", "Mar", "Apr", "Mei", "Jun", "Jul", "Ags", "Sep", "Okt", "Nov", "Des"];
    const now = new Date();
    const last6MonthsLabels = [];
    
    // Array untuk menampung hitungan per bulan
    const catTrend = [0, 0, 0, 0, 0, 0];
    const momTrend = [0, 0, 0, 0, 0, 0];

    // Mundur 5 bulan dari bulan ini (total 6 titik data)
    for (let i = 5; i >= 0; i--) {
      const d = new Date(now.getFullYear(), now.getMonth() - i, 1);
      last6MonthsLabels.push(monthNames[d.getMonth()]);
    }

    // Fungsi penolong: Mencari index grafik berdasarkan bulan tanggal
    const getMonthIndex = (dateString) => {
      const d = new Date(dateString);
      const diffMonths = (now.getFullYear() - d.getFullYear()) * 12 + (now.getMonth() - d.getMonth());
      // Jika diffMonths 0, artinya bulan ini (index 5)
      // Jika diffMonths 1, artinya bulan lalu (index 4)
      if (diffMonths >= 0 && diffMonths <= 5) {
        return 5 - diffMonths;
      }
      return -1; // Di luar 6 bulan terakhir
    };

    // Hitung distribusi booking Catering
    catBookings.forEach(b => {
      const idx = getMonthIndex(b.created_at);
      if (idx !== -1) catTrend[idx]++;
    });

    // Hitung distribusi booking Moment
    momBookings.forEach(b => {
      const idx = getMonthIndex(b.created_at);
      if (idx !== -1) momTrend[idx]++;
    });

    // Perbarui chart
    bookingChartData.value = {
      labels: last6MonthsLabels,
      datasets: [
        { ...bookingChartData.value.datasets[0], data: catTrend },
        { ...bookingChartData.value.datasets[1], data: momTrend }
      ]
    };
  }
});
</script>

<style scoped>
.stat-card {
  @apply bg-white p-6 rounded-2xl shadow-sm border border-gray-100 flex items-center gap-4 transition-all duration-300 hover:border-orange-300 hover:-translate-y-1 block;
}
.icon-box {
  @apply p-3 rounded-full flex-shrink-0 transition-transform duration-300 group-hover:scale-110;
}
</style>
