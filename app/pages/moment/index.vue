<!-- <template>
  <div class="bg-stone-50 min-h-screen py-16">
    <div class="container mx-auto px-6 max-w-6xl">
      <div class="text-center mb-16">
        <h1 class="text-4xl md:text-5xl font-extrabold text-gray-900 mb-6">
          Kecilung <span class="text-orange-600">Moment</span>
        </h1>
        <p class="text-lg text-gray-700 max-w-4xl mx-auto leading-relaxed">
          Ciptakan kenangan tak terlupakan bersama orang-orang terkasih. Kami
          menyediakan ruang, suasana, dan pelayanan eksklusif untuk merayakan
          setiap momen berharga Anda.
        </p>
      </div>

      <div v-if="pending" class="flex justify-center py-20">
        <div
          class="animate-spin rounded-full h-12 w-12 border-b-2 border-orange-500"
        ></div>
      </div>

      <div
        v-else-if="moments.length === 0"
        class="text-center py-20 text-gray-500"
      >
        <p class="text-xl">Belum ada paket moment yang tersedia saat ini.</p>
      </div>

      <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
        <NuxtLink
          v-for="mom in moments"
          :key="mom.id"
          :to="`/moment/${mom.id}`"
          class="bg-white rounded-2xl shadow-md overflow-hidden group hover:shadow-2xl transition-all duration-300 transform hover:-translate-y-1 flex flex-col"
        >
          <div class="relative h-64 overflow-hidden">
            <img
              v-if="mom.images && mom.images.length > 0"
              :src="mom.images[0].image_url"
              :alt="mom.name"
              class="w-full h-full object-cover transition-transform duration-500 group-hover:scale-110"
            />
            <div
              v-else
              class="w-full h-full bg-gray-200 flex items-center justify-center text-gray-400"
            >
              [Tidak Ada Gambar]
            </div>
          </div>

          <div class="p-6 flex-1 flex flex-col">
            <h2
              class="text-2xl font-bold text-gray-800 mb-3 group-hover:text-orange-600 transition-colors"
            >
              {{ mom.name }}
            </h2>
            <p
              class="text-gray-600 text-sm line-clamp-3 leading-relaxed mb-4 flex-1"
            >
              {{ mom.description }}
            </p>

            <div class="mt-auto text-orange-600 font-bold flex items-center">
              Lihat Detail & Reservasi
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-5 w-5 ml-1 transition-transform group-hover:translate-x-1"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M14 5l7 7m0 0l-7 7m7-7H3"
                />
              </svg>
            </div>
          </div>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<script setup>
const baseURL = "https://back.kecilungresto.com/api";

// Fetch data paket moment dari backend
const { data: res, pending } = useFetch(`${baseURL}/moments/packages`, {
  lazy: import.meta.client,
});
const moments = computed(() => res.value?.data || []);
</script> -->

<template>
  <div class="bg-stone-50 min-h-screen py-16">
    <div class="container mx-auto px-6 max-w-6xl">
      <!-- Bagian Copywriting / Header -->
      <div class="text-center mb-16">
        <h1 class="text-4xl md:text-5xl font-extrabold text-gray-900 mb-6">
          Kecilung <span class="text-orange-600">Moment</span>
        </h1>
        <p class="text-lg text-gray-700 max-w-4xl mx-auto leading-relaxed">
          Ciptakan kenangan tak terlupakan bersama orang-orang terkasih. Kami
          menyediakan ruang, suasana, dan pelayanan eksklusif untuk merayakan
          setiap momen berharga Anda.
        </p>
      </div>

      <!-- SKELETON LOADING STATE -->
      <div
        v-if="pending"
        class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8"
      >
        <!-- Tampilkan 6 kerangka skeleton -->
        <div
          v-for="n in 6"
          :key="'skeleton-' + n"
          class="bg-white rounded-2xl shadow-md overflow-hidden animate-pulse flex flex-col h-full"
        >
          <!-- Skeleton Thumbnail -->
          <div class="h-64 bg-gray-300 w-full"></div>

          <div class="p-6 flex-1 flex flex-col">
            <!-- Skeleton Judul -->
            <div class="h-8 bg-gray-300 rounded w-3/4 mb-4"></div>

            <!-- Skeleton Deskripsi (3 baris) -->
            <div class="h-4 bg-gray-200 rounded w-full mb-2"></div>
            <div class="h-4 bg-gray-200 rounded w-11/12 mb-2"></div>
            <div class="h-4 bg-gray-200 rounded w-4/5 mb-6"></div>

            <!-- Skeleton Link Aksi di bagian bawah -->
            <div class="mt-auto h-5 bg-gray-300 rounded w-1/2"></div>
          </div>
        </div>
      </div>

      <!-- Pesan Jika Data Kosong -->
      <div
        v-else-if="moments.length === 0"
        class="text-center py-20 text-gray-500"
      >
        <p class="text-xl">Belum ada paket moment yang tersedia saat ini.</p>
      </div>

      <!-- Grid Daftar Moment -->
      <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
        <!-- Kartu yang bisa diklik -->
        <!-- <NuxtLink
          v-for="mom in moments"
          :key="mom.id"
          :to="`/moment/${mom.id}`"
          class="bg-white rounded-2xl shadow-md overflow-hidden group hover:shadow-2xl transition-all duration-300 transform hover:-translate-y-1 flex flex-col"
        > -->
        <NuxtLink
          v-for="mom in moments"
          :key="mom.id"
          :to="`/moment/${mom.slug}`" 
          class="bg-white rounded-2xl shadow-md overflow-hidden group hover:shadow-2xl transition-all duration-300 transform hover:-translate-y-1 flex flex-col"
        >
          <!-- Thumbnail Gambar -->
          <div class="relative h-64 overflow-hidden">
            <!-- Tampilkan gambar pertama dari array images -->
            <img
              v-if="mom.images && mom.images.length > 0"
              :src="mom.images[0].image_url"
              :alt="mom.name"
              class="w-full h-full object-cover transition-transform duration-500 group-hover:scale-110"
            />
            <div
              v-else
              class="w-full h-full bg-gray-200 flex items-center justify-center text-gray-400"
            >
              [Tidak Ada Gambar]
            </div>
          </div>

          <!-- Informasi Moment -->
          <div class="p-6 flex-1 flex flex-col">
            <h2
              class="text-2xl font-bold text-gray-800 mb-3 group-hover:text-orange-600 transition-colors"
            >
              {{ mom.name }}
            </h2>
            <!-- line-clamp-3 membatasi deskripsi hanya 3 baris -->
            <p
              class="text-gray-600 text-sm line-clamp-3 leading-relaxed mb-4 flex-1"
            >
              {{ mom.description }}
            </p>

            <div class="mt-auto text-orange-600 font-bold flex items-center">
              Lihat Detail & Reservasi
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-5 w-5 ml-1 transition-transform group-hover:translate-x-1"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M14 5l7 7m0 0l-7 7m7-7H3"
                />
              </svg>
            </div>
          </div>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<script setup>
const baseURL = "https://back.kecilungresto.com/api";

// Fetch data paket moment dari backend
const { data: res, pending } = useFetch(`${baseURL}/moments/packages`, {
  lazy: import.meta.client,
});
const moments = computed(() => res.value?.data || []);
</script>
