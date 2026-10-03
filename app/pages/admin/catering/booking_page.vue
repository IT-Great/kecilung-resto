<!-- <template>
  <div class="container mx-auto p-6 max-w-6xl">
    <h1 class="text-3xl font-bold text-gray-800 mb-6">Manajemen Booking Katering</h1>
    
    <div class="bg-white shadow rounded-lg overflow-hidden">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase">Pelanggan</th>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase">Jadwal</th>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase">Status</th>
            <th class="px-6 py-3 text-center text-xs font-bold text-gray-500 uppercase">Aksi</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-200">
          <tr v-for="book in bookings" :key="book.id">
            <td class="px-6 py-4">
              <p class="font-bold text-gray-900">{{ book.customer_name }}</p>
              <p class="text-sm text-gray-500">{{ book.phone }}</p>
              <p class="text-xs text-orange-600">Paket: {{ book.catering?.name }}</p>
            </td>
            <td class="px-6 py-4 font-medium text-gray-700">
              {{ new Date(book.booking_date).toLocaleDateString('id-ID', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' }) }}
            </td>
            <td class="px-6 py-4">
              <span :class="{'bg-yellow-100 text-yellow-800': book.status === 'PENDING', 'bg-green-100 text-green-800': book.status === 'APPROVED', 'bg-red-100 text-red-800': book.status === 'REJECTED'}" class="px-3 py-1 rounded-full text-xs font-bold">
                {{ book.status }}
              </span>
            </td>
            <td class="px-6 py-4 text-center space-x-2">
              <button v-if="book.status === 'PENDING'" @click="updateStatus(book.id, 'approve')" class="bg-green-500 text-white px-3 py-1 rounded text-sm font-bold hover:bg-green-600">Terima</button>
              <button v-if="book.status === 'PENDING'" @click="updateStatus(book.id, 'reject')" class="bg-red-500 text-white px-3 py-1 rounded text-sm font-bold hover:bg-red-600">Tolak</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import Swal from 'sweetalert2';
definePageMeta({ layout: "admin" });

const baseURL = "https://back.kecilungresto.com/api";
const { data: res, refresh } = useLazyFetch(`${baseURL}/catering/bookings`);
const bookings = computed(() => res.value?.data || []);

const updateStatus = async (id, action) => {
  const isApprove = action === 'approve';
  const confirm = await Swal.fire({
    title: isApprove ? 'Terima Booking?' : 'Tolak Booking?',
    text: isApprove ? "Jadwal ini akan dikunci untuk pelanggan ini." : "Pelanggan akan ditolak.",
    icon: 'warning', showCancelButton: true
  });

  if (confirm.isConfirmed) {
    await $fetch(`${baseURL}/catering/bookings/${id}/${action}`, { method: 'PUT' });
    Swal.fire('Berhasil!', `Booking telah di-${isApprove ? 'Setujui' : 'Tolak'}.`, 'success');
    refresh();
  }
};
</script> -->

<!-- <template>
  <div class="container mx-auto p-6 max-w-6xl">
    <h1 class="text-3xl font-bold text-gray-800 mb-6">Manajemen Booking Katering</h1>
    
    <div class="bg-white shadow rounded-lg overflow-hidden">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase">Pelanggan</th>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase">Jadwal</th>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase">Status</th>
            <th class="px-6 py-3 text-center text-xs font-bold text-gray-500 uppercase">Aksi</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-200">
          <tr v-for="book in bookings" :key="book.id">
            <td class="px-6 py-4">
              <p class="font-bold text-gray-900">{{ book.customer_name }}</p>
              <p class="text-sm text-gray-500">{{ book.phone }}</p>
              <p class="text-xs text-orange-600">Paket: {{ book.catering?.name }}</p>
            </td>
            <td class="px-6 py-4 font-medium text-gray-700">
              <span class="block">
                {{ new Date(book.booking_date).toLocaleDateString('id-ID', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' }) }}
              </span>
              <span class="text-sm text-orange-600 font-bold block mt-1">
                Jam: {{ new Date(book.booking_date).toLocaleTimeString('id-ID', { hour: '2-digit', minute: '2-digit' }) }} WIB
              </span>
            </td>
            <td class="px-6 py-4">
              <span :class="{'bg-yellow-100 text-yellow-800': book.status === 'PENDING', 'bg-green-100 text-green-800': book.status === 'APPROVED', 'bg-red-100 text-red-800': book.status === 'REJECTED'}" class="px-3 py-1 rounded-full text-xs font-bold">
                {{ book.status }}
              </span>
            </td>
            <td class="px-6 py-4 text-center space-x-2">
              <button v-if="book.status === 'PENDING'" @click="updateStatus(book.id, 'approve')" class="bg-green-500 text-white px-3 py-1 rounded text-sm font-bold hover:bg-green-600">Terima</button>
              <button v-if="book.status === 'PENDING'" @click="updateStatus(book.id, 'reject')" class="bg-red-500 text-white px-3 py-1 rounded text-sm font-bold hover:bg-red-600">Tolak</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import Swal from 'sweetalert2';
definePageMeta({ layout: "admin" });

const baseURL = "https://back.kecilungresto.com/api";
const { data: res, refresh } = useLazyFetch(`${baseURL}/catering/bookings`);
const bookings = computed(() => res.value?.data || []);

const updateStatus = async (id, action) => {
  const isApprove = action === 'approve';
  const confirm = await Swal.fire({
    title: isApprove ? 'Terima Booking?' : 'Tolak Booking?',
    text: isApprove ? "Jadwal ini akan dikunci untuk pelanggan ini." : "Pelanggan akan ditolak.",
    icon: 'warning', showCancelButton: true
  });

  if (confirm.isConfirmed) {
    await $fetch(`${baseURL}/catering/bookings/${id}/${action}`, { method: 'PUT' });
    Swal.fire('Berhasil!', `Booking telah di-${isApprove ? 'Setujui' : 'Tolak'}.`, 'success');
    refresh();
  }
};
</script> -->

<!-- <template>
  <div class="container mx-auto p-6 max-w-6xl">
    <h1 class="text-3xl font-bold text-gray-800 mb-6">Manajemen Booking Katering</h1>
    <div class="bg-white shadow rounded-lg overflow-x-auto">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase whitespace-nowrap">Pelanggan</th>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase whitespace-nowrap">Detail Acara</th>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase whitespace-nowrap">Jadwal</th>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase whitespace-nowrap">Status</th>
            <th class="px-6 py-3 text-center text-xs font-bold text-gray-500 uppercase whitespace-nowrap">Aksi</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-200">
          <tr v-for="book in bookings" :key="book.id" class="hover:bg-gray-50">
            <td class="px-6 py-4">
              <p class="font-bold text-gray-900">{{ book.customer_name }}</p>
              <p class="text-sm text-gray-500">{{ book.phone }}</p>
            </td>
            
            <td class="px-6 py-4">
              <p class="text-xs font-bold text-orange-600 mb-1">Paket: {{ book.catering?.name }}</p>
              <p class="text-sm text-gray-800"><span class="font-semibold text-gray-500">Acara:</span> {{ book.description }}</p>
              <p class="text-sm text-gray-800"><span class="font-semibold text-gray-500">Peserta:</span> {{ book.member_count }} Pax</p>
            </td>

            <td class="px-6 py-4 font-medium text-gray-700 whitespace-nowrap">
              <span class="block text-sm text-gray-900 font-semibold mb-1">
                {{ new Date(book.booking_date).toLocaleDateString('id-ID', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' }) }}
              </span>
              <div class="flex items-center gap-2 text-sm text-orange-600 font-bold bg-orange-50 px-2 py-1 rounded inline-block">
                <span>{{ new Date(book.booking_date).toLocaleTimeString('id-ID', { hour: '2-digit', minute: '2-digit' }) }}</span>
                <span class="text-gray-400">-</span>
                <span class="text-red-600">{{ new Date(book.booking_end_date).toLocaleTimeString('id-ID', { hour: '2-digit', minute: '2-digit' }) }} WIB</span>
              </div>
            </td>

            <td class="px-6 py-4 whitespace-nowrap">
              <span 
                :class="{'bg-yellow-100 text-yellow-800 border border-yellow-200': book.status === 'PENDING', 'bg-green-100 text-green-800 border border-green-200': book.status === 'APPROVED', 'bg-red-100 text-red-800 border border-red-200': book.status === 'REJECTED'}" 
                class="px-3 py-1 rounded-full text-xs font-bold"
              >
                {{ book.status }}
              </span>
            </td>

            <td class="px-6 py-4 text-center space-x-2 whitespace-nowrap">
              <button v-if="book.status === 'PENDING'" @click="updateStatus(book.id, 'approve')" class="bg-green-500 text-white px-3 py-1.5 rounded text-sm font-bold hover:bg-green-600 shadow-sm transition">Terima</button>
              <button v-if="book.status === 'PENDING'" @click="updateStatus(book.id, 'reject')" class="bg-red-500 text-white px-3 py-1.5 rounded text-sm font-bold hover:bg-red-600 shadow-sm transition">Tolak</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import Swal from 'sweetalert2';
definePageMeta({ layout: "admin", middleware: "auth" });

const baseURL = "https://back.kecilungresto.com/api";
const { data: res, refresh } = useLazyFetch(`${baseURL}/catering/bookings`);
const bookings = computed(() => res.value?.data || []);

const updateStatus = async (id, action) => {
  const isApprove = action === 'approve';
  const confirm = await Swal.fire({
    title: isApprove ? 'Terima Booking?' : 'Tolak Booking?',
    text: isApprove ? "Jadwal ini akan dikunci dan disetujui." : "Pelanggan akan ditolak.",
    icon: 'warning', showCancelButton: true
  });

  if (confirm.isConfirmed) {
    try {
      await $fetch(`${baseURL}/catering/bookings/${id}/${action}`, { method: 'PUT' });
      Swal.fire('Berhasil!', `Booking telah di-${isApprove ? 'Setujui' : 'Tolak'}.`, 'success');
      refresh();
    } catch (err) {
      Swal.fire('Gagal!', 'Terjadi kesalahan saat memproses data.', 'error');
    }
  }
};
</script> -->

<template>
  <div class="container mx-auto p-6 max-w-6xl">
    <div
      class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-6 gap-4"
    >
      <h1 class="text-3xl font-bold text-gray-800">
        Manajemen Booking Katering
      </h1>

      <!-- TAMBAHAN: Tombol Export Excel -->
      <button
        @click="exportExcel"
        :disabled="isExporting"
        class="bg-green-600 hover:bg-green-700 text-white px-5 py-2 rounded-lg font-bold shadow-md transition flex items-center gap-2 disabled:opacity-50"
      >
        <svg
          v-if="!isExporting"
          class="w-5 h-5"
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"
          ></path>
        </svg>
        <svg
          v-else
          class="animate-spin h-5 w-5 text-white"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
        >
          <circle
            class="opacity-25"
            cx="12"
            cy="12"
            r="10"
            stroke="currentColor"
            stroke-width="4"
          ></circle>
          <path
            class="opacity-75"
            fill="currentColor"
            d="M4 12a8 8 0 018-8v8H4z"
          ></path>
        </svg>
        {{ isExporting ? "Memproses..." : "Export Excel" }}
      </button>
    </div>

    <!-- ... (Tabel Booking persis sama seperti sebelumnya) ... -->
    <div class="bg-white shadow rounded-lg overflow-x-auto">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th
              class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase whitespace-nowrap"
            >
              Pelanggan
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase whitespace-nowrap"
            >
              Detail Acara
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase whitespace-nowrap"
            >
              Jadwal
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase whitespace-nowrap"
            >
              Status
            </th>
            <th
              class="px-6 py-3 text-center text-xs font-bold text-gray-500 uppercase whitespace-nowrap"
            >
              Aksi
            </th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-200">
          <tr v-for="book in bookings" :key="book.id" class="hover:bg-gray-50">
            <!-- Kolom Pelanggan -->
            <td class="px-6 py-4">
              <p class="font-bold text-gray-900">{{ book.customer_name }}</p>
              <p class="text-sm text-gray-500">{{ book.phone }}</p>
            </td>

            <!-- Kolom Detail Acara -->
            <td class="px-6 py-4">
              <p class="text-xs font-bold text-orange-600 mb-1">
                Paket: {{ book.catering?.name }}
              </p>
              <p class="text-sm text-gray-800">
                <span class="font-semibold text-gray-500">Acara:</span>
                {{ book.description }}
              </p>
              <p class="text-sm text-gray-800">
                <span class="font-semibold text-gray-500">Peserta:</span>
                {{ book.member_count }} Pax
              </p>
            </td>

            <!-- Kolom Jadwal -->
            <td class="px-6 py-4 font-medium text-gray-700 whitespace-nowrap">
              <span class="block text-sm text-gray-900 font-semibold mb-1">
                {{
                  new Date(book.booking_date).toLocaleDateString("id-ID", {
                    weekday: "long",
                    year: "numeric",
                    month: "long",
                    day: "numeric",
                  })
                }}
              </span>
              <div
                class="flex items-center gap-2 text-sm text-orange-600 font-bold bg-orange-50 px-2 py-1 rounded inline-block"
              >
                <span>{{
                  new Date(book.booking_date).toLocaleTimeString("id-ID", {
                    hour: "2-digit",
                    minute: "2-digit",
                  })
                }}</span>
                <span class="text-gray-400">-</span>
                <span class="text-red-600"
                  >{{
                    new Date(book.booking_end_date).toLocaleTimeString(
                      "id-ID",
                      { hour: "2-digit", minute: "2-digit" },
                    )
                  }}
                  WIB</span
                >
              </div>
            </td>

            <!-- Kolom Status -->
            <td class="px-6 py-4 whitespace-nowrap">
              <span
                :class="{
                  'bg-yellow-100 text-yellow-800 border border-yellow-200':
                    book.status === 'PENDING',
                  'bg-green-100 text-green-800 border border-green-200':
                    book.status === 'APPROVED',
                  'bg-red-100 text-red-800 border border-red-200':
                    book.status === 'REJECTED',
                }"
                class="px-3 py-1 rounded-full text-xs font-bold"
              >
                {{ book.status }}
              </span>
            </td>

            <!-- Kolom Aksi -->
            <td class="px-6 py-4 text-center space-x-2 whitespace-nowrap">
              <button
                v-if="book.status === 'PENDING'"
                @click="updateStatus(book.id, 'approve')"
                class="bg-green-500 text-white px-3 py-1.5 rounded text-sm font-bold hover:bg-green-600 shadow-sm transition"
              >
                Terima
              </button>
              <button
                v-if="book.status === 'PENDING'"
                @click="updateStatus(book.id, 'reject')"
                class="bg-red-500 text-white px-3 py-1.5 rounded text-sm font-bold hover:bg-red-600 shadow-sm transition"
              >
                Tolak
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import Swal from "sweetalert2";
definePageMeta({ layout: "admin", middleware: "auth" });

const baseURL = "https://back.kecilungresto.com/api";
const { data: res, refresh } = useLazyFetch(`${baseURL}/catering/bookings`);
const bookings = computed(() => res.value?.data || []);

const isExporting = ref(false); // State Loading untuk tombol Export

// --- LOGIKA BLOB HANDLING UNTUK EXPORT EXCEL ---
const exportExcel = async () => {
  isExporting.value = true;
  try {
    // Kita gunakan Fetch native agar bisa menangkap respon sebagai Blob biner
    const response = await fetch(`${baseURL}/catering/bookings/export`, {
      method: "GET",
      headers: {
        // Jika backend kelak menggunakan validasi JWT per-route, tambahkan:
        // 'Authorization': `Bearer ${localStorage.getItem('admin_token')}`
      },
    });

    if (!response.ok) throw new Error("Gagal mengunduh file");

    // 1. Ubah stream HTTP menjadi Blob
    const blob = await response.blob();

    // 2. Buat URL objek sementara dari memori browser
    const url = window.URL.createObjectURL(blob);

    // 3. Buat elemen <a> fiktif dan klik secara programatis
    const a = document.createElement("a");
    a.href = url;
    a.download = `Rekap_Booking_Katering_${new Date().toISOString().slice(0, 10)}.xlsx`;
    document.body.appendChild(a);
    a.click();

    // 4. Bersihkan memori dan elemen
    a.remove();
    window.URL.revokeObjectURL(url);

    Swal.fire({
      toast: true,
      position: "top-end",
      icon: "success",
      title: "File Excel berhasil diunduh",
      showConfirmButton: false,
      timer: 3000,
    });
  } catch (error) {
    Swal.fire("Gagal", "Tidak dapat mengunduh file Excel.", "error");
  } finally {
    isExporting.value = false;
  }
};

const updateStatus = async (id, action) => {
  const isApprove = action === "approve";
  const confirm = await Swal.fire({
    title: isApprove ? "Terima Booking?" : "Tolak Booking?",
    text: isApprove
      ? "Jadwal ini akan dikunci dan disetujui."
      : "Pelanggan akan ditolak.",
    icon: "warning",
    showCancelButton: true,
  });

  if (confirm.isConfirmed) {
    try {
      await $fetch(`${baseURL}/catering/bookings/${id}/${action}`, {
        method: "PUT",
      });
      Swal.fire(
        "Berhasil!",
        `Booking telah di-${isApprove ? "Setujui" : "Tolak"}.`,
        "success",
      );
      refresh();
    } catch (err) {
      Swal.fire("Gagal!", "Terjadi kesalahan saat memproses data.", "error");
    }
  }
};
</script>
