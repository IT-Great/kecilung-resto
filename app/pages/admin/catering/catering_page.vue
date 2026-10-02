<!-- <template>
  <div class="container mx-auto p-6 max-w-6xl">
    <div class="flex justify-between items-center mb-6">
      <h1 class="text-3xl font-bold text-gray-800">Manajemen Paket Katering</h1>
      <button @click="openModal('add')" class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg font-medium transition">
        + Tambah Paket
      </button>
    </div>

    <div class="bg-white shadow rounded-lg overflow-hidden">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase">Gambar Utama</th>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase">Nama Paket</th>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase">Deskripsi</th>
            <th class="px-6 py-3 text-center text-xs font-bold text-gray-500 uppercase">Aksi</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-200">
          <tr v-if="pending">
            <td colspan="4" class="text-center py-8 text-gray-500">Memuat data...</td>
          </tr>
          <tr v-else v-for="cat in caterings" :key="cat.id" class="hover:bg-gray-50">
            <td class="px-6 py-4">
              <img v-if="cat.images && cat.images.length > 0" :src="cat.images[0].image_url" class="w-20 h-20 object-cover rounded-md shadow-sm border" />
              <div v-else class="w-20 h-20 bg-gray-200 flex items-center justify-center text-xs text-gray-400 rounded-md border">No Image</div>
            </td>
            <td class="px-6 py-4 font-bold text-gray-900">{{ cat.name }}</td>
            <td class="px-6 py-4 text-sm text-gray-600">
              <p class="line-clamp-2">{{ cat.description }}</p>
              <span class="text-xs text-orange-500 font-semibold mt-1 block">{{ cat.images?.length || 0 }} Gambar tersimpan</span>
            </td>
            <td class="px-6 py-4 text-center space-x-3">
              <button @click="openModal('edit', cat)" class="text-amber-600 hover:text-amber-900 font-bold">Edit</button>
              <button @click="deleteCatering(cat.id)" class="text-red-600 hover:text-red-900 font-bold">Hapus</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div v-if="isModalOpen" class="fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-50">
      <div class="bg-white p-8 rounded-xl w-full max-w-lg shadow-2xl transform transition-all">
        <h2 class="text-2xl font-bold mb-6 text-gray-800">
          {{ modalMode === 'add' ? 'Tambah Paket Baru' : 'Edit Paket Katering' }}
        </h2>
        <form @submit.prevent="saveCatering">
          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Nama Paket</label>
            <input v-model="form.name" type="text" required class="w-full border rounded-lg px-4 py-2 focus:ring-2 focus:ring-orange-300 focus:outline-none" placeholder="Misal: Paket Pernikahan Mewah" />
          </div>
          
          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Deskripsi</label>
            <textarea v-model="form.description" rows="4" required class="w-full border rounded-lg px-4 py-2 focus:ring-2 focus:ring-orange-300 focus:outline-none" placeholder="Jelaskan isi paket ini..."></textarea>
          </div>

          <div class="mb-8">
            <label class="block text-gray-700 font-bold mb-2">
              Upload Gambar {{ modalMode === 'edit' ? '(Opsional)' : '' }}
            </label>
            <input type="file" multiple @change="handleFileChange" accept="image/*" class="w-full border rounded-lg px-4 py-2 bg-gray-50" />
            <p class="text-xs text-gray-500 mt-2">* Tahan tombol CTRL (Windows) atau Command (Mac) untuk memilih lebih dari satu gambar.</p>
          </div>

          <div class="flex justify-end space-x-4">
            <button type="button" @click="closeModal" class="bg-gray-200 text-gray-700 px-6 py-2 rounded-lg font-bold hover:bg-gray-300 transition">Batal</button>
            <button type="submit" :disabled="isSaving" class="bg-orange-500 text-white px-6 py-2 rounded-lg font-bold hover:bg-orange-600 transition disabled:opacity-50">
              {{ isSaving ? 'Menyimpan...' : 'Simpan Paket' }}
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import Swal from 'sweetalert2';
definePageMeta({ layout: "admin" });

const baseURL = "http://31.97.60.207:8246/api";

// Ambil Data Paket
const { data: res, pending, refresh } = useLazyFetch(`${baseURL}/catering/packages`);
const caterings = computed(() => res.value?.data || []);

// State Modal & Form
const isModalOpen = ref(false);
const modalMode = ref("add");
const isSaving = ref(false);
const selectedFiles = ref([]);

const form = ref({ id: null, name: "", description: "" });

const openModal = (mode, data = null) => {
  modalMode.value = mode;
  selectedFiles.value = []; // Reset file
  
  if (mode === "edit" && data) {
    form.value = { id: data.id, name: data.name, description: data.description };
  } else {
    form.value = { id: null, name: "", description: "" };
  }
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};

// Menangani pemilihan multi-gambar
const handleFileChange = (e) => {
  if (e.target.files.length > 0) {
    selectedFiles.value = Array.from(e.target.files);
  } else {
    selectedFiles.value = [];
  }
};

// Simpan Data (Add / Edit) menggunakan FormData untuk multi-file
const saveCatering = async () => {
  isSaving.value = true;
  try {
    const formData = new FormData();
    formData.append("name", form.value.name);
    formData.append("description", form.value.description);
    
    // Append setiap gambar ke dalam FormData dengan key "images"
    selectedFiles.value.forEach((file) => {
      formData.append("images", file);
    });

    if (modalMode.value === "add") {
      await $fetch(`${baseURL}/catering/packages`, { method: "POST", body: formData });
      Swal.fire({ icon: 'success', title: 'Berhasil!', text: 'Paket katering baru ditambahkan.', timer: 1500, showConfirmButton: false });
    } else {
      // Pastikan Anda sudah membuat endpoint PUT ini di Backend Go Anda
      await $fetch(`${baseURL}/catering/packages/${form.value.id}`, { method: "PUT", body: formData });
      Swal.fire({ icon: 'success', title: 'Diperbarui!', text: 'Paket katering berhasil diubah.', timer: 1500, showConfirmButton: false });
    }
    
    closeModal();
    refresh();
  } catch (error) {
    Swal.fire({ icon: 'error', title: 'Gagal!', text: 'Gagal menyimpan data: ' + error.message });
  } finally {
    isSaving.value = false;
  }
};

// Hapus Data dengan SweetAlert2
const deleteCatering = async (id) => {
  const result = await Swal.fire({
    title: 'Hapus Paket Ini?',
    text: "Semua gambar dan data terkait paket ini akan terhapus permanen!",
    icon: 'warning',
    showCancelButton: true,
    confirmButtonColor: '#ef4444',
    cancelButtonColor: '#6b7280',
    confirmButtonText: 'Ya, Hapus!',
    cancelButtonText: 'Batal'
  });

  if (result.isConfirmed) {
    try {
      // Pastikan Anda sudah membuat endpoint DELETE ini di Backend Go Anda
      await $fetch(`${baseURL}/catering/packages/${id}`, { method: "DELETE" });
      refresh();
      Swal.fire({ icon: 'success', title: 'Terhapus!', text: 'Paket katering berhasil dihapus.', timer: 1500, showConfirmButton: false });
    } catch (error) {
      Swal.fire({ icon: 'error', title: 'Gagal!', text: 'Gagal menghapus paket katering.' });
    }
  }
};
</script> -->

<!-- <template>
  <div class="container mx-auto p-6 max-w-6xl">
    
    <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-6 gap-4">
      <h1 class="text-3xl font-bold text-gray-800">Manajemen Paket Katering</h1>
      <div class="flex space-x-3">
        <NuxtLink to="/admin/catering/booking_page" class="bg-gray-800 hover:bg-gray-900 text-white px-4 py-2 rounded-lg font-medium transition shadow flex items-center">
          Lihat Data Booking &rarr;
        </NuxtLink>
        <button @click="openModal('add')" class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg font-medium transition shadow">
          + Tambah Paket
        </button>
      </div>
    </div>

    <div class="bg-white shadow rounded-lg overflow-hidden">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase">Gambar Utama</th>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase">Nama Paket</th>
            <th class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase">Deskripsi</th>
            <th class="px-6 py-3 text-center text-xs font-bold text-gray-500 uppercase">Aksi</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-200">
          <tr v-if="pending">
            <td colspan="4" class="text-center py-8 text-gray-500">Memuat data...</td>
          </tr>
          <tr v-else v-for="cat in caterings" :key="cat.id" class="hover:bg-gray-50">
            <td class="px-6 py-4">
              <img v-if="cat.images && cat.images.length > 0" :src="cat.images[0].image_url" class="w-20 h-20 object-cover rounded-md shadow-sm border" />
              <div v-else class="w-20 h-20 bg-gray-200 flex items-center justify-center text-xs text-gray-400 rounded-md border">No Image</div>
            </td>
            <td class="px-6 py-4 font-bold text-gray-900">{{ cat.name }}</td>
            <td class="px-6 py-4 text-sm text-gray-600">
              <p class="line-clamp-2">{{ cat.description }}</p>
              <span class="text-xs text-orange-500 font-semibold mt-1 block">{{ cat.images?.length || 0 }} Gambar tersimpan</span>
            </td>
            <td class="px-6 py-4 text-center space-x-3 whitespace-nowrap">
              <button @click="showDetail(cat)" class="text-blue-600 hover:text-blue-900 font-bold">Detail</button>
              <button @click="openModal('edit', cat)" class="text-amber-600 hover:text-amber-900 font-bold">Edit</button>
              <button @click="deleteCatering(cat.id)" class="text-red-600 hover:text-red-900 font-bold">Hapus</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div v-if="isModalOpen" class="fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-50">
      <div class="bg-white p-8 rounded-xl w-full max-w-lg shadow-2xl transform transition-all">
        <h2 class="text-2xl font-bold mb-6 text-gray-800">
          {{ modalMode === 'add' ? 'Tambah Paket Baru' : 'Edit Paket Katering' }}
        </h2>
        <form @submit.prevent="saveCatering">
          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Nama Paket</label>
            <input v-model="form.name" type="text" required class="w-full border rounded-lg px-4 py-2 focus:ring-2 focus:ring-orange-300 focus:outline-none" placeholder="Misal: Paket Pernikahan Mewah" />
          </div>
          
          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Deskripsi</label>
            <textarea v-model="form.description" rows="4" required class="w-full border rounded-lg px-4 py-2 focus:ring-2 focus:ring-orange-300 focus:outline-none" placeholder="Jelaskan isi paket ini..."></textarea>
          </div>

          <div class="mb-8">
            <label class="block text-gray-700 font-bold mb-2">
              Upload Gambar {{ modalMode === 'edit' ? '(Opsional)' : '' }}
            </label>
            <input type="file" multiple @change="handleFileChange" accept="image/*" class="w-full border rounded-lg px-4 py-2 bg-gray-50" />
            <p class="text-xs text-gray-500 mt-2">* Tahan tombol CTRL (Windows) atau Command (Mac) untuk memilih lebih dari satu gambar.</p>
          </div>

          <div class="flex justify-end space-x-4">
            <button type="button" @click="closeModal" class="bg-gray-200 text-gray-700 px-6 py-2 rounded-lg font-bold hover:bg-gray-300 transition">Batal</button>
            <button type="submit" :disabled="isSaving" class="bg-orange-500 text-white px-6 py-2 rounded-lg font-bold hover:bg-orange-600 transition disabled:opacity-50">
              {{ isSaving ? 'Menyimpan...' : 'Simpan Paket' }}
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import Swal from 'sweetalert2';
definePageMeta({ layout: "admin", middleware: "auth" });

const baseURL = "http://31.97.60.207:8246/api";

const { data: res, pending, refresh } = useLazyFetch(`${baseURL}/catering/packages`);
const caterings = computed(() => res.value?.data || []);

const isModalOpen = ref(false);
const modalMode = ref("add");
const isSaving = ref(false);
const selectedFiles = ref([]);

const form = ref({ id: null, name: "", description: "" });

// Menampilkan Detail Paket menggunakan SweetAlert2
const showDetail = (cat) => {
  // Merakit HTML untuk galeri gambar
  let imagesHtml = '';
  if (cat.images && cat.images.length > 0) {
    const imgTags = cat.images.map(img => 
      `<img src="${img.image_url}" class="w-24 h-24 object-cover rounded-md border shadow-sm" />`
    ).join('');
    imagesHtml = `<div class="flex gap-2 overflow-x-auto justify-center mb-4 p-2 bg-gray-50 rounded-lg">${imgTags}</div>`;
  } else {
    imagesHtml = `<div class="text-gray-400 italic mb-4 bg-gray-50 py-4 rounded-lg">Tidak ada gambar yang tersimpan</div>`;
  }

  Swal.fire({
    title: `<span class="text-2xl font-bold text-gray-900">${cat.name}</span>`,
    html: `
      ${imagesHtml}
      <div class="text-left bg-orange-50 text-gray-800 p-4 rounded-lg max-h-64 overflow-y-auto border border-orange-100 whitespace-pre-line text-sm leading-relaxed">
        ${cat.description}
      </div>
    `,
    width: '600px',
    showCloseButton: true,
    focusConfirm: false,
    confirmButtonText: 'Tutup',
    confirmButtonColor: '#f97316',
    customClass: {
      popup: 'rounded-xl shadow-2xl',
      title: 'pb-2 border-b'
    }
  });
};

const openModal = (mode, data = null) => {
  modalMode.value = mode;
  selectedFiles.value = []; 
  
  if (mode === "edit" && data) {
    form.value = { id: data.id, name: data.name, description: data.description };
  } else {
    form.value = { id: null, name: "", description: "" };
  }
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};

const handleFileChange = (e) => {
  if (e.target.files.length > 0) {
    selectedFiles.value = Array.from(e.target.files);
  } else {
    selectedFiles.value = [];
  }
};

const saveCatering = async () => {
  isSaving.value = true;
  try {
    const formData = new FormData();
    formData.append("name", form.value.name);
    formData.append("description", form.value.description);
    
    selectedFiles.value.forEach((file) => {
      formData.append("images", file);
    });

    if (modalMode.value === "add") {
      await $fetch(`${baseURL}/catering/packages`, { method: "POST", body: formData });
      Swal.fire({ icon: 'success', title: 'Berhasil!', text: 'Paket katering baru ditambahkan.', timer: 1500, showConfirmButton: false });
    } else {
      await $fetch(`${baseURL}/catering/packages/${form.value.id}`, { method: "PUT", body: formData });
      Swal.fire({ icon: 'success', title: 'Diperbarui!', text: 'Paket katering berhasil diubah.', timer: 1500, showConfirmButton: false });
    }
    
    closeModal();
    refresh();
  } catch (error) {
    Swal.fire({ icon: 'error', title: 'Gagal!', text: 'Gagal menyimpan data: ' + error.message });
  } finally {
    isSaving.value = false;
  }
};

const deleteCatering = async (id) => {
  const result = await Swal.fire({
    title: 'Hapus Paket Ini?',
    text: "Semua gambar dan data terkait paket ini akan terhapus permanen!",
    icon: 'warning',
    showCancelButton: true,
    confirmButtonColor: '#ef4444',
    cancelButtonColor: '#6b7280',
    confirmButtonText: 'Ya, Hapus!',
    cancelButtonText: 'Batal'
  });

  if (result.isConfirmed) {
    try {
      await $fetch(`${baseURL}/catering/packages/${id}`, { method: "DELETE" });
      refresh();
      Swal.fire({ icon: 'success', title: 'Terhapus!', text: 'Paket katering berhasil dihapus.', timer: 1500, showConfirmButton: false });
    } catch (error) {
      Swal.fire({ icon: 'error', title: 'Gagal!', text: 'Gagal menghapus paket katering.' });
    }
  }
};
</script> -->

<template>
  <div class="container mx-auto p-6 max-w-6xl">
    <!-- Header dan Navigasi -->
    <div
      class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-6 gap-4"
    >
      <h1 class="text-3xl font-bold text-gray-800">Manajemen Paket Katering</h1>
      <div class="flex space-x-3">
        <NuxtLink
          to="/admin/catering/booking_page"
          class="bg-gray-800 hover:bg-gray-900 text-white px-4 py-2 rounded-lg font-medium transition shadow flex items-center whitespace-nowrap"
        >
          Lihat Data Booking &rarr;
        </NuxtLink>
        <button
          @click="openModal('add')"
          class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg font-medium transition shadow whitespace-nowrap"
        >
          + Tambah Paket
        </button>
      </div>
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
          placeholder="Cari nama paket..."
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

    <!-- Tabel Paket Katering -->
    <div class="bg-white shadow overflow-x-auto">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th
              class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase"
            >
              Gambar Utama
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase"
            >
              Nama Paket
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-bold text-gray-500 uppercase"
            >
              Deskripsi
            </th>
            <th
              class="px-6 py-3 text-center text-xs font-bold text-gray-500 uppercase"
            >
              Aksi
            </th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-200">
          <template v-if="pending">
            <tr
              v-for="n in 3"
              :key="'skel-cat-' + n"
              class="animate-pulse hover:bg-gray-50"
            >
              <td class="px-6 py-4">
                <div class="w-20 h-20 bg-gray-200 rounded-md"></div>
              </td>
              <td class="px-6 py-4">
                <div class="h-4 bg-gray-200 rounded w-48"></div>
              </td>
              <td class="px-6 py-4">
                <div class="h-4 bg-gray-200 rounded w-full mb-2"></div>
                <div class="h-4 bg-gray-200 rounded w-2/3"></div>
              </td>
              <td class="px-6 py-4 text-center">
                <div class="h-4 bg-gray-200 rounded w-32 inline-block"></div>
              </td>
            </tr>
          </template>

          <template v-else-if="paginatedCaterings.length === 0">
            <tr>
              <td colspan="4" class="px-6 py-12 text-center text-gray-500">
                Data tidak ditemukan.
              </td>
            </tr>
          </template>

          <template v-else>
            <tr
              v-for="cat in paginatedCaterings"
              :key="cat.id"
              class="hover:bg-gray-50 transition-colors"
            >
              <td class="px-6 py-4 whitespace-nowrap">
                <img
                  v-if="cat.images && cat.images.length > 0"
                  :src="cat.images[0].image_url"
                  class="w-20 h-20 object-cover rounded-md shadow-sm border border-gray-200"
                />
                <div
                  v-else
                  class="w-20 h-20 bg-gray-100 flex items-center justify-center text-xs text-gray-400 rounded-md border border-gray-200"
                >
                  No Image
                </div>
              </td>
              <td class="px-6 py-4 font-bold text-gray-900 whitespace-nowrap">
                {{ cat.name }}
              </td>
              <td class="px-6 py-4 text-sm text-gray-600">
                <p class="line-clamp-2">{{ cat.description }}</p>
                <span class="text-xs text-orange-500 font-semibold mt-1 block"
                  >{{ cat.images?.length || 0 }} Gambar tersimpan</span
                >
              </td>
              <td class="px-6 py-4 text-center space-x-3 whitespace-nowrap">
                <button
                  @click="showDetail(cat)"
                  class="text-blue-600 hover:text-blue-900 font-bold bg-blue-50 px-3 py-1.5 rounded transition"
                >
                  Detail
                </button>
                <button
                  @click="openModal('edit', cat)"
                  class="text-amber-600 hover:text-amber-900 font-bold bg-amber-50 px-3 py-1.5 rounded transition"
                >
                  Edit
                </button>
                <button
                  @click="deleteCatering(cat.id)"
                  class="text-red-600 hover:text-red-900 font-bold bg-red-50 px-3 py-1.5 rounded transition"
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
        <span class="font-bold text-gray-900">{{
          filteredCaterings.length
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

    <!-- MODAL FORM (ADD & EDIT) TETAP SAMA -->
    <div
      v-if="isModalOpen"
      class="fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-50 p-4"
    >
      <div
        class="bg-white p-6 md:p-8 rounded-xl w-full max-w-lg shadow-2xl transform transition-all"
      >
        <h2 class="text-2xl font-bold mb-6 text-gray-800">
          {{
            modalMode === "add" ? "Tambah Paket Baru" : "Edit Paket Katering"
          }}
        </h2>
        <form @submit.prevent="saveCatering">
          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Nama Paket</label>
            <input
              v-model="form.name"
              type="text"
              required
              class="w-full border rounded-lg px-4 py-2 focus:ring-2 focus:ring-orange-300 focus:outline-none"
              placeholder="Misal: Paket Pernikahan Mewah"
            />
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Deskripsi</label>
            <textarea
              v-model="form.description"
              rows="4"
              required
              class="w-full border rounded-lg px-4 py-2 focus:ring-2 focus:ring-orange-300 focus:outline-none"
              placeholder="Jelaskan isi paket ini..."
            ></textarea>
          </div>

          <div class="mb-8">
            <label class="block text-gray-700 font-bold mb-2">
              Upload Gambar {{ modalMode === "edit" ? "(Opsional)" : "" }}
            </label>
            <input
              type="file"
              multiple
              @change="handleFileChange"
              accept="image/*"
              class="w-full border rounded-lg px-4 py-2 bg-gray-50"
            />
            <p class="text-xs text-gray-500 mt-2">
              * Tahan tombol CTRL (Windows) atau Command (Mac) untuk memilih
              lebih dari satu gambar.
            </p>
          </div>

          <div class="flex justify-end space-x-4">
            <button
              type="button"
              @click="closeModal"
              class="bg-gray-200 text-gray-700 px-6 py-2 rounded-lg font-bold hover:bg-gray-300 transition"
            >
              Batal
            </button>
            <button
              type="submit"
              :disabled="isSaving"
              class="bg-orange-500 text-white px-6 py-2 rounded-lg font-bold hover:bg-orange-600 transition disabled:opacity-50"
            >
              {{ isSaving ? "Menyimpan..." : "Simpan Paket" }}
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import Swal from "sweetalert2";
import { ref, computed, watch } from "vue";

definePageMeta({ layout: "admin", middleware: "auth" });

const baseURL = "http://31.97.60.207:8246/api";

const {
  data: res,
  pending,
  refresh,
} = useLazyFetch(`${baseURL}/catering/packages`);
const allCaterings = computed(() => res.value?.data || []);

// --- STATE FILTER & PAGINATION ---
const searchQuery = ref("");
const itemsPerPage = ref(5); // Default 5 untuk paket katering agar baris tidak terlalu panjang
const currentPage = ref(1);

watch([searchQuery, itemsPerPage], () => {
  currentPage.value = 1;
});

// --- LOGIKA FILTER (SEARCH BAR) ---
const filteredCaterings = computed(() => {
  if (!searchQuery.value) return allCaterings.value;
  const q = searchQuery.value.toLowerCase();
  return allCaterings.value.filter(
    (cat) =>
      cat.name.toLowerCase().includes(q) ||
      cat.description.toLowerCase().includes(q),
  );
});

// --- LOGIKA PAGINATION ---
const totalPages = computed(() =>
  Math.ceil(filteredCaterings.value.length / itemsPerPage.value),
);

const paginatedCaterings = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage.value;
  return filteredCaterings.value.slice(start, start + itemsPerPage.value);
});

// Logika "Showing X to Y of Z"
const showingStart = computed(() =>
  filteredCaterings.value.length === 0
    ? 0
    : (currentPage.value - 1) * itemsPerPage.value + 1,
);
const showingEnd = computed(() =>
  Math.min(
    currentPage.value * itemsPerPage.value,
    filteredCaterings.value.length,
  ),
);

// Algoritma Pagination Dinamis (Maksimal 7 Kotak)
const paginationArray = computed(() => {
  const current = currentPage.value;
  const total = totalPages.value;
  if (total <= 7) return Array.from({ length: total }, (_, i) => i + 1);
  if (current <= 4) return [1, 2, 3, 4, 5, "...", total];
  if (current >= total - 3)
    return [1, "...", total - 4, total - 3, total - 2, total - 1, total];
  return [1, "...", current - 1, current, current + 1, "...", total];
});

// --- MODAL & API ACTIONS ---
const isModalOpen = ref(false);
const modalMode = ref("add");
const isSaving = ref(false);
const selectedFiles = ref([]);
const form = ref({ id: null, name: "", description: "" });

const showDetail = (cat) => {
  let imagesHtml = "";
  if (cat.images && cat.images.length > 0) {
    const imgTags = cat.images
      .map(
        (img) =>
          `<img src="${img.image_url}" class="w-24 h-24 object-cover rounded-md border shadow-sm" />`,
      )
      .join("");
    imagesHtml = `<div class="flex gap-2 overflow-x-auto justify-center mb-4 p-2 bg-gray-50 rounded-lg">${imgTags}</div>`;
  } else {
    imagesHtml = `<div class="text-gray-400 italic mb-4 bg-gray-50 py-4 rounded-lg">Tidak ada gambar yang tersimpan</div>`;
  }

  Swal.fire({
    title: `<span class="text-2xl font-bold text-gray-900">${cat.name}</span>`,
    html: `
      ${imagesHtml}
      <div class="text-left bg-orange-50 text-gray-800 p-4 rounded-lg max-h-64 overflow-y-auto border border-orange-100 whitespace-pre-line text-sm leading-relaxed">
        ${cat.description}
      </div>
    `,
    width: "600px",
    showCloseButton: true,
    focusConfirm: false,
    confirmButtonText: "Tutup",
    confirmButtonColor: "#f97316",
    customClass: { popup: "rounded-xl shadow-2xl", title: "pb-2 border-b" },
  });
};

const openModal = (mode, data = null) => {
  modalMode.value = mode;
  selectedFiles.value = [];

  if (mode === "edit" && data) {
    form.value = {
      id: data.id,
      name: data.name,
      description: data.description,
    };
  } else {
    form.value = { id: null, name: "", description: "" };
  }
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};

const handleFileChange = (e) => {
  if (e.target.files.length > 0) {
    selectedFiles.value = Array.from(e.target.files);
  } else {
    selectedFiles.value = [];
  }
};

const saveCatering = async () => {
  isSaving.value = true;
  try {
    const formData = new FormData();
    formData.append("name", form.value.name);
    formData.append("description", form.value.description);

    selectedFiles.value.forEach((file) => {
      formData.append("images", file);
    });

    if (modalMode.value === "add") {
      await $fetch(`${baseURL}/catering/packages`, {
        method: "POST",
        body: formData,
      });
      Swal.fire({
        icon: "success",
        title: "Berhasil!",
        text: "Paket katering baru ditambahkan.",
        timer: 1500,
        showConfirmButton: false,
      });
    } else {
      await $fetch(`${baseURL}/catering/packages/${form.value.id}`, {
        method: "PUT",
        body: formData,
      });
      Swal.fire({
        icon: "success",
        title: "Diperbarui!",
        text: "Paket katering berhasil diubah.",
        timer: 1500,
        showConfirmButton: false,
      });
    }

    closeModal();
    refresh();
  } catch (error) {
    Swal.fire({
      icon: "error",
      title: "Gagal!",
      text: "Gagal menyimpan data: " + error.message,
    });
  } finally {
    isSaving.value = false;
  }
};

const deleteCatering = async (id) => {
  const result = await Swal.fire({
    title: "Hapus Paket Ini?",
    text: "Semua gambar dan data terkait paket ini akan terhapus permanen!",
    icon: "warning",
    showCancelButton: true,
    confirmButtonColor: "#ef4444",
    cancelButtonColor: "#6b7280",
    confirmButtonText: "Ya, Hapus!",
    cancelButtonText: "Batal",
  });

  if (result.isConfirmed) {
    try {
      await $fetch(`${baseURL}/catering/packages/${id}`, { method: "DELETE" });
      refresh();
      Swal.fire({
        icon: "success",
        title: "Terhapus!",
        text: "Paket katering berhasil dihapus.",
        timer: 1500,
        showConfirmButton: false,
      });
    } catch (error) {
      Swal.fire({
        icon: "error",
        title: "Gagal!",
        text: "Gagal menghapus paket katering.",
      });
    }
  }
};
</script>
