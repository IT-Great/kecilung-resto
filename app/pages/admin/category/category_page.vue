<!-- <template>
  <div class="container mx-auto p-6 max-w-5xl">
    <div class="flex justify-between items-center mb-6">
      <h1 class="text-3xl font-bold text-gray-800">Manajemen Kategori</h1>
      <button
        @click="openModal('add')"
        class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg font-medium transition"
      >
        + Tambah Kategori
      </button>
    </div>

    <div class="bg-white rounded-lg shadow overflow-hidden">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              ID
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Kode
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Nama
            </th>
            <th
              class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase tracking-wider"
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
            v-for="cat in categories"
            :key="cat.id"
            class="hover:bg-gray-50"
          >
            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
              {{ cat.id }}
            </td>
            <td
              class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900"
            >
              {{ cat.code }}
            </td>
            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">
              {{ cat.name }}
            </td>
            <td
              class="px-6 py-4 whitespace-nowrap text-center text-sm font-medium space-x-3"
            >
              <NuxtLink
                :to="`/admin/category/detail/${cat.id}`"
                class="text-blue-600 hover:text-blue-900"
                >Detail</NuxtLink
              >
              <button
                @click="openModal('edit', cat)"
                class="text-amber-600 hover:text-amber-900"
              >
                Edit
              </button>
              <button
                @click="deleteCategory(cat.id)"
                class="text-red-600 hover:text-red-900"
              >
                Hapus
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div
      v-if="isModalOpen"
      class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
    >
      <div class="bg-white p-6 rounded-lg shadow-xl w-full max-w-md">
        <h2 class="text-xl font-bold mb-4">
          {{ modalMode === "add" ? "Tambah Kategori" : "Edit Kategori" }}
        </h2>
        <form @submit.prevent="saveCategory">
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2"
              >Kode Kategori</label
            >
            <input
              v-model="form.code"
              type="text"
              required
              class="w-full border rounded px-3 py-2 focus:outline-none focus:ring focus:border-orange-300"
              placeholder="Misal: MC"
            />
          </div>
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2"
              >Nama Kategori</label
            >
            <input
              v-model="form.name"
              type="text"
              required
              class="w-full border rounded px-3 py-2 focus:outline-none focus:ring focus:border-orange-300"
              placeholder="Misal: Main Course"
            />
          </div>
          <div class="mb-6">
            <label class="block text-gray-700 text-sm font-bold mb-2"
              >Deskripsi</label
            >
            <textarea
              v-model="form.description"
              rows="3"
              class="w-full border rounded px-3 py-2 focus:outline-none focus:ring focus:border-orange-300"
            ></textarea>
          </div>
          <div class="flex justify-end space-x-3">
            <button
              type="button"
              @click="closeModal"
              class="px-4 py-2 text-gray-600 bg-gray-200 rounded hover:bg-gray-300"
            >
              Batal
            </button>
            <button
              type="submit"
              class="px-4 py-2 text-white bg-orange-500 rounded hover:bg-orange-600"
            >
              Simpan
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
definePageMeta({
  layout: "admin",
});

const baseURL = "https://back.kecilungresto.com/api";
const {
  data: response,
  pending,
  refresh,
} = await useFetch(`${baseURL}/categories`);
const categories = computed(() => response.value?.data || []);

const isModalOpen = ref(false);
const modalMode = ref("add");
const form = ref({ id: null, code: "", name: "", description: "" });

const openModal = (mode, data = null) => {
  modalMode.value = mode;
  if (mode === "edit" && data) {
    form.value = { ...data };
  } else {
    form.value = { id: null, code: "", name: "", description: "" };
  }
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};

const saveCategory = async () => {
  try {
    if (modalMode.value === "add") {
      await $fetch(`${baseURL}/categories`, {
        method: "POST",
        body: form.value,
      });
    } else {
      await $fetch(`${baseURL}/categories/${form.value.id}`, {
        method: "PUT",
        body: form.value,
      });
    }
    closeModal();
    refresh(); // Memuat ulang data tabel
  } catch (error) {
    alert("Gagal menyimpan data: " + error.message);
  }
};

const deleteCategory = async (id) => {
  if (!confirm("Yakin ingin menghapus kategori ini?")) return;
  try {
    await $fetch(`${baseURL}/categories/${id}`, { method: "DELETE" });
    refresh();
  } catch (error) {
    alert("Gagal menghapus data");
  }
};
</script> -->

<!-- <template>
  <div class="container mx-auto p-6 max-w-5xl">
    <div class="flex justify-between items-center mb-6">
      <h1 class="text-3xl font-bold text-gray-800">Manajemen Kategori</h1>
      <button @click="openModal('add')" class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg font-medium transition">
        + Tambah Kategori
      </button>
    </div>

    <div class="bg-white rounded-lg shadow overflow-hidden">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">ID</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Kode</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Nama</th>
            <th class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase tracking-wider">Aksi</th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          
          <template v-if="pending">
            <tr v-for="n in 4" :key="'skel-cat-' + n" class="animate-pulse slide-up-anim hover:bg-gray-50">
              <td class="px-6 py-4 whitespace-nowrap"><div class="h-4 bg-gray-200 rounded w-8"></div></td>
              <td class="px-6 py-4 whitespace-nowrap"><div class="h-4 bg-gray-200 rounded w-16"></div></td>
              <td class="px-6 py-4 whitespace-nowrap"><div class="h-4 bg-gray-200 rounded w-32"></div></td>
              <td class="px-6 py-4 whitespace-nowrap text-center space-x-3 flex justify-center">
                <div class="h-4 bg-gray-200 rounded w-12"></div>
                <div class="h-4 bg-gray-200 rounded w-12"></div>
                <div class="h-4 bg-gray-200 rounded w-12"></div>
              </td>
            </tr>
          </template>

          <template v-else>
            <tr v-for="cat in categories" :key="cat.id" class="hover:bg-gray-50">
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">{{ cat.id }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">{{ cat.code }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">{{ cat.name }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-center text-sm font-medium space-x-3">
                <NuxtLink :to="`/admin/category/detail/${cat.id}`" class="text-blue-600 hover:text-blue-900">Detail</NuxtLink>
                <button @click="openModal('edit', cat)" class="text-amber-600 hover:text-amber-900">Edit</button>
                <button @click="deleteCategory(cat.id)" class="text-red-600 hover:text-red-900">Hapus</button>
              </td>
            </tr>
          </template>

        </tbody>
      </table>
    </div>

  </div>
</template>

<script setup>
definePageMeta({ layout: "admin" });

const baseURL = "https://back.kecilungresto.com/api";

// PERUBAHAN: Gunakan useLazyFetch tanpa "await"
const { data: response, pending, refresh } = useLazyFetch(`${baseURL}/categories`);
const categories = computed(() => response.value?.data || []);

const isModalOpen = ref(false);
const modalMode = ref("add");
const form = ref({ id: null, code: "", name: "", description: "" });

const openModal = (mode, data = null) => {
  modalMode.value = mode;
  if (mode === "edit" && data) {
    form.value = { ...data };
  } else {
    form.value = { id: null, code: "", name: "", description: "" };
  }
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};

const saveCategory = async () => {
  try {
    if (modalMode.value === "add") {
      await $fetch(`${baseURL}/categories`, { method: "POST", body: form.value });
    } else {
      await $fetch(`${baseURL}/categories/${form.value.id}`, { method: "PUT", body: form.value });
    }
    closeModal();
    refresh();
  } catch (error) {
    alert("Gagal menyimpan data: " + error.message);
  }
};

const deleteCategory = async (id) => {
  if (!confirm("Yakin ingin menghapus kategori ini?")) return;
  try {
    await $fetch(`${baseURL}/categories/${id}`, { method: "DELETE" });
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
  <div class="container mx-auto p-6 max-w-5xl">
    <div class="flex justify-between items-center mb-6">
      <h1 class="text-3xl font-bold text-gray-800">Manajemen Kategori</h1>
      <button @click="openModal('add')" class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg font-medium transition">
        + Tambah Kategori
      </button>
    </div>

    <div class="bg-white rounded-lg shadow overflow-hidden">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">ID</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Kode</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Nama</th>
            <th class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase tracking-wider">Aksi</th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          
          <template v-if="pending">
            <tr v-for="n in 4" :key="'skel-cat-' + n" class="animate-pulse slide-up-anim hover:bg-gray-50">
              <td class="px-6 py-4 whitespace-nowrap"><div class="h-4 bg-gray-200 rounded w-8"></div></td>
              <td class="px-6 py-4 whitespace-nowrap"><div class="h-4 bg-gray-200 rounded w-16"></div></td>
              <td class="px-6 py-4 whitespace-nowrap"><div class="h-4 bg-gray-200 rounded w-32"></div></td>
              <td class="px-6 py-4 whitespace-nowrap text-center space-x-3 flex justify-center">
                <div class="h-4 bg-gray-200 rounded w-12"></div>
                <div class="h-4 bg-gray-200 rounded w-12"></div>
                <div class="h-4 bg-gray-200 rounded w-12"></div>
              </td>
            </tr>
          </template>

          <template v-else>
            <tr v-for="cat in categories" :key="cat.id" class="hover:bg-gray-50">
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">{{ cat.id }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">{{ cat.code }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">{{ cat.name }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-center text-sm font-medium space-x-3">
                <NuxtLink :to="`/admin/category/detail/${cat.id}`" class="text-blue-600 hover:text-blue-900">Detail</NuxtLink>
                <button @click="openModal('edit', cat)" class="text-amber-600 hover:text-amber-900">Edit</button>
                <button @click="deleteCategory(cat.id)" class="text-red-600 hover:text-red-900">Hapus</button>
              </td>
            </tr>
          </template>

        </tbody>
      </table>
    </div>

    <div v-if="isModalOpen" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
      <div class="bg-white p-6 rounded-lg shadow-xl w-full max-w-md slide-up-anim">
        <h2 class="text-xl font-bold mb-4">
          {{ modalMode === "add" ? "Tambah Kategori" : "Edit Kategori" }}
        </h2>
        <form @submit.prevent="saveCategory">
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2">Kode Kategori</label>
            <input v-model="form.code" type="text" required class="w-full border rounded px-3 py-2 focus:outline-none focus:ring focus:border-orange-300" placeholder="Misal: MC" />
          </div>
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2">Nama Kategori</label>
            <input v-model="form.name" type="text" required class="w-full border rounded px-3 py-2 focus:outline-none focus:ring focus:border-orange-300" placeholder="Misal: Main Course" />
          </div>
          <div class="mb-6">
            <label class="block text-gray-700 text-sm font-bold mb-2">Deskripsi</label>
            <textarea v-model="form.description" rows="3" class="w-full border rounded px-3 py-2 focus:outline-none focus:ring focus:border-orange-300"></textarea>
          </div>
          <div class="flex justify-end space-x-3">
            <button type="button" @click="closeModal" class="px-4 py-2 text-gray-600 bg-gray-200 rounded hover:bg-gray-300">Batal</button>
            <button type="submit" class="px-4 py-2 text-white bg-orange-500 rounded hover:bg-orange-600">Simpan</button>
          </div>
        </form>
      </div>
    </div>

  </div>
</template>

<script setup>
import Swal from 'sweetalert2'; // Import SweetAlert2

definePageMeta({ layout: "admin" });

const baseURL = "https://back.kecilungresto.com/api";

const { data: response, pending, refresh } = useLazyFetch(`${baseURL}/categories`);
const categories = computed(() => response.value?.data || []);

const isModalOpen = ref(false);
const modalMode = ref("add");
const form = ref({ id: null, code: "", name: "", description: "" });

const openModal = (mode, data = null) => {
  modalMode.value = mode;
  if (mode === "edit" && data) {
    form.value = { ...data };
  } else {
    form.value = { id: null, code: "", name: "", description: "" };
  }
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};

// Modifikasi saveCategory dengan Swal
const saveCategory = async () => {
  try {
    if (modalMode.value === "add") {
      await $fetch(`${baseURL}/categories`, { method: "POST", body: form.value });
    } else {
      await $fetch(`${baseURL}/categories/${form.value.id}`, { method: "PUT", body: form.value });
    }
    
    closeModal();
    refresh();
    
    // Notifikasi Sukses
    Swal.fire({
      icon: 'success',
      title: 'Berhasil!',
      text: `Kategori berhasil ${modalMode.value === 'add' ? 'ditambahkan' : 'diperbarui'}.`,
      timer: 1500,
      showConfirmButton: false
    });

  } catch (error) {
    // Notifikasi Error
    Swal.fire({
      icon: 'error',
      title: 'Gagal!',
      text: 'Gagal menyimpan data: ' + error.message
    });
  }
};

// Modifikasi deleteCategory dengan Swal Konfirmasi
const deleteCategory = async (id) => {
  const result = await Swal.fire({
    title: 'Hapus Kategori?',
    text: "Data yang dihapus tidak dapat dikembalikan!",
    icon: 'warning',
    showCancelButton: true,
    confirmButtonColor: '#ef4444', // red-500
    cancelButtonColor: '#6b7280',  // gray-500
    confirmButtonText: 'Ya, Hapus!',
    cancelButtonText: 'Batal'
  });

  if (result.isConfirmed) {
    try {
      await $fetch(`${baseURL}/categories/${id}`, { method: "DELETE" });
      refresh();
      
      Swal.fire({
        icon: 'success',
        title: 'Terhapus!',
        text: 'Kategori berhasil dihapus.',
        timer: 1500,
        showConfirmButton: false
      });
    } catch (error) {
      Swal.fire({
        icon: 'error',
        title: 'Gagal!',
        text: 'Gagal menghapus data kategori.'
      });
    }
  }
};
</script>

<style scoped>
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
    <div class="flex justify-between items-center mb-6">
      <h1 class="text-3xl font-bold text-gray-800">Manajemen Kategori</h1>
      <button @click="openModal('add')" class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg font-medium transition">
        + Tambah Kategori
      </button>
    </div>

    <div class="bg-white rounded-lg shadow overflow-hidden">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Gambar</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Kode</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Nama</th>
            <th class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase tracking-wider">Aksi</th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          
          <template v-if="pending">
            <tr v-for="n in 4" :key="'skel-cat-' + n" class="animate-pulse slide-up-anim hover:bg-gray-50">
              <td class="px-6 py-4 whitespace-nowrap"><div class="h-12 bg-gray-200 rounded w-16"></div></td>
              <td class="px-6 py-4 whitespace-nowrap"><div class="h-4 bg-gray-200 rounded w-16"></div></td>
              <td class="px-6 py-4 whitespace-nowrap"><div class="h-4 bg-gray-200 rounded w-32"></div></td>
              <td class="px-6 py-4 whitespace-nowrap text-center space-x-3 flex justify-center">
                <div class="h-4 bg-gray-200 rounded w-12"></div>
                <div class="h-4 bg-gray-200 rounded w-12"></div>
              </td>
            </tr>
          </template>

          <template v-else>
            <tr v-for="cat in categories" :key="cat.id" class="hover:bg-gray-50">
              <td class="px-6 py-4 whitespace-nowrap">
                <img v-if="cat.image_url" :src="cat.image_url" class="w-16 h-12 object-cover rounded shadow-sm border" />
                <div v-else class="w-16 h-12 bg-gray-100 flex items-center justify-center text-xs text-gray-400 rounded border">No Img</div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">{{ cat.code }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">{{ cat.name }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-center text-sm font-medium space-x-3">
                <button @click="openModal('edit', cat)" class="text-amber-600 hover:text-amber-900">Edit</button>
                <button @click="deleteCategory(cat.id)" class="text-red-600 hover:text-red-900">Hapus</button>
              </td>
            </tr>
          </template>

        </tbody>
      </table>
    </div>

    <div v-if="isModalOpen" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
      <div class="bg-white p-6 rounded-lg shadow-xl w-full max-w-md slide-up-anim">
        <h2 class="text-xl font-bold mb-4">
          {{ modalMode === "add" ? "Tambah Kategori" : "Edit Kategori" }}
        </h2>
        <form @submit.prevent="saveCategory">
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2">Kode Kategori</label>
            <input v-model="form.code" type="text" required class="w-full border rounded px-3 py-2 focus:outline-none focus:ring focus:border-orange-300" placeholder="Misal: MC" />
          </div>
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2">Nama Kategori</label>
            <input v-model="form.name" type="text" required class="w-full border rounded px-3 py-2 focus:outline-none focus:ring focus:border-orange-300" placeholder="Misal: Main Course" />
          </div>
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2">Deskripsi</label>
            <textarea v-model="form.description" rows="2" class="w-full border rounded px-3 py-2 focus:outline-none focus:ring focus:border-orange-300"></textarea>
          </div>
          <div class="mb-6">
            <label class="block text-gray-700 text-sm font-bold mb-2">Gambar Kategori (Opsional)</label>
            <input type="file" @change="handleFileChange" accept="image/*" class="w-full border rounded px-3 py-2 text-sm bg-gray-50" />
          </div>
          <div class="flex justify-end space-x-3">
            <button type="button" @click="closeModal" class="px-4 py-2 text-gray-600 bg-gray-200 rounded hover:bg-gray-300">Batal</button>
            <button type="submit" :disabled="isSaving" class="px-4 py-2 text-white bg-orange-500 rounded hover:bg-orange-600 disabled:opacity-50">
              {{ isSaving ? 'Menyimpan...' : 'Simpan' }}
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

const baseURL = "https://back.kecilungresto.com/api";

const { data: response, pending, refresh } = useLazyFetch(`${baseURL}/categories`);
const categories = computed(() => response.value?.data || []);

const isModalOpen = ref(false);
const modalMode = ref("add");
const isSaving = ref(false);
const form = ref({ id: null, code: "", name: "", description: "" });
const selectedFile = ref(null);

const openModal = (mode, data = null) => {
  modalMode.value = mode;
  selectedFile.value = null; 
  if (mode === "edit" && data) {
    form.value = { ...data };
  } else {
    form.value = { id: null, code: "", name: "", description: "" };
  }
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};

const handleFileChange = (e) => {
  if (e.target.files.length > 0) {
    selectedFile.value = e.target.files[0];
  }
};

const saveCategory = async () => {
  isSaving.value = true;
  try {
    const formData = new FormData();
    formData.append("code", form.value.code);
    formData.append("name", form.value.name);
    formData.append("description", form.value.description || "");
    if (selectedFile.value) {
      formData.append("image", selectedFile.value);
    }

    if (modalMode.value === "add") {
      await $fetch(`${baseURL}/categories`, { method: "POST", body: formData });
    } else {
      await $fetch(`${baseURL}/categories/${form.value.id}`, { method: "PUT", body: formData });
    }
    
    closeModal();
    refresh();
    
    Swal.fire({
      icon: 'success',
      title: 'Berhasil!',
      text: `Kategori berhasil ${modalMode.value === 'add' ? 'ditambahkan' : 'diperbarui'}.`,
      timer: 1500,
      showConfirmButton: false
    });

  } catch (error) {
    Swal.fire({ icon: 'error', title: 'Gagal!', text: 'Gagal menyimpan data: ' + error.message });
  } finally {
    isSaving.value = false;
  }
};

const deleteCategory = async (id) => {
  const result = await Swal.fire({
    title: 'Hapus Kategori?',
    text: "Data yang dihapus tidak dapat dikembalikan!",
    icon: 'warning',
    showCancelButton: true,
    confirmButtonColor: '#ef4444',
    cancelButtonColor: '#6b7280',
    confirmButtonText: 'Ya, Hapus!',
    cancelButtonText: 'Batal'
  });

  if (result.isConfirmed) {
    try {
      await $fetch(`${baseURL}/categories/${id}`, { method: "DELETE" });
      refresh();
      Swal.fire({ icon: 'success', title: 'Terhapus!', text: 'Kategori dihapus.', timer: 1500, showConfirmButton: false });
    } catch (error) {
      Swal.fire({ icon: 'error', title: 'Gagal!', text: 'Gagal menghapus data kategori.' });
    }
  }
};
</script>

<style scoped>
.slide-up-anim {
  animation: slideUp 0.4s ease-out forwards;
  opacity: 0;
}
@keyframes slideUp {
  0% { transform: translateY(15px); opacity: 0; }
  100% { transform: translateY(0); opacity: 1; }
}
</style> -->

<template>
  <div class="container mx-auto p-6 max-w-6xl">
    <div
      class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 gap-4"
    >
      <h1 class="text-3xl font-bold text-gray-800">Manajemen Kategori</h1>
      <button
        @click="openModal('add')"
        class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg font-medium transition whitespace-nowrap"
      >
        + Tambah Kategori
      </button>
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
          placeholder="Cari kode atau nama..."
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

    <!-- Tabel Kategori -->
    <div class="bg-white shadow overflow-x-auto">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Gambar
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Kode
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Nama
            </th>
            <th
              class="px-6 py-3 text-center text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Aksi
            </th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          <template v-if="pending">
            <tr
              v-for="n in 4"
              :key="'skel-cat-' + n"
              class="animate-pulse hover:bg-gray-50"
            >
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="h-12 bg-gray-200 rounded w-16"></div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="h-4 bg-gray-200 rounded w-16"></div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="h-4 bg-gray-200 rounded w-32"></div>
              </td>
              <td
                class="px-6 py-4 whitespace-nowrap text-center space-x-3 flex justify-center"
              >
                <div class="h-4 bg-gray-200 rounded w-24"></div>
              </td>
            </tr>
          </template>

          <template v-else-if="paginatedCategories.length === 0">
            <tr>
              <td colspan="4" class="px-6 py-12 text-center text-gray-500">
                Data tidak ditemukan.
              </td>
            </tr>
          </template>

          <template v-else>
            <tr
              v-for="cat in paginatedCategories"
              :key="cat.id"
              class="hover:bg-gray-50 transition-colors"
            >
              <td class="px-6 py-4 whitespace-nowrap">
                <img
                  v-if="cat.image_url"
                  :src="cat.image_url"
                  class="w-16 h-12 object-cover rounded shadow-sm border border-gray-200"
                />
                <div
                  v-else
                  class="w-16 h-12 bg-gray-100 flex items-center justify-center text-xs text-gray-400 rounded border border-gray-200"
                >
                  No Img
                </div>
              </td>
              <td
                class="px-6 py-4 whitespace-nowrap text-sm font-bold text-gray-900"
              >
                <span class="bg-gray-100 px-2 py-1 rounded text-xs">{{
                  cat.code
                }}</span>
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">
                {{ cat.name }}
              </td>
              <td
                class="px-6 py-4 whitespace-nowrap text-center text-sm font-medium space-x-3"
              >
                <button
                  @click="openModal('edit', cat)"
                  class="text-amber-600 hover:text-amber-900 bg-amber-50 px-3 py-1.5 rounded transition"
                >
                  Edit
                </button>
                <button
                  @click="deleteCategory(cat.id)"
                  class="text-red-600 hover:text-red-900 bg-red-50 px-3 py-1.5 rounded transition"
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
          filteredCategories.length
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

    <!-- MODAL FORM -->
    <div
      v-if="isModalOpen"
      class="fixed inset-0 bg-black bg-opacity-60 backdrop-blur-sm flex items-center justify-center z-50 p-4"
    >
      <div
        class="bg-white p-6 md:p-8 rounded-2xl shadow-2xl w-full max-w-md slide-up-anim"
      >
        <h2 class="text-2xl font-bold mb-6 text-gray-800">
          {{ modalMode === "add" ? "Tambah Kategori" : "Edit Kategori" }}
        </h2>
        <form @submit.prevent="saveCategory">
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2"
              >Kode Kategori</label
            >
            <input
              v-model="form.code"
              type="text"
              required
              class="w-full border border-gray-300 rounded-lg px-4 py-3 focus:outline-none focus:ring-2 focus:ring-orange-500 transition"
              placeholder="Misal: MC"
            />
          </div>
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2"
              >Nama Kategori</label
            >
            <input
              v-model="form.name"
              type="text"
              required
              class="w-full border border-gray-300 rounded-lg px-4 py-3 focus:outline-none focus:ring-2 focus:ring-orange-500 transition"
              placeholder="Misal: Main Course"
            />
          </div>
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2"
              >Deskripsi</label
            >
            <textarea
              v-model="form.description"
              rows="3"
              class="w-full border border-gray-300 rounded-lg px-4 py-3 focus:outline-none focus:ring-2 focus:ring-orange-500 transition"
              placeholder="Tuliskan deskripsi kategori..."
            ></textarea>
          </div>
          <div class="mb-6">
            <label class="block text-gray-700 text-sm font-bold mb-2"
              >Gambar Kategori (Opsional)</label
            >
            <input
              type="file"
              @change="handleFileChange"
              accept="image/*"
              class="w-full border border-gray-300 rounded-lg px-4 py-2.5 text-sm bg-gray-50 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-orange-50 file:text-orange-700 hover:file:bg-orange-100 transition"
            />
          </div>
          <div class="flex justify-end gap-3 mt-8">
            <button
              type="button"
              @click="closeModal"
              class="px-6 py-2.5 text-gray-700 bg-gray-100 rounded-lg font-bold hover:bg-gray-200 transition"
            >
              Batal
            </button>
            <button
              type="submit"
              :disabled="isSaving"
              class="px-6 py-2.5 text-white bg-orange-500 rounded-lg font-bold hover:bg-orange-600 disabled:opacity-50 transition"
            >
              {{ isSaving ? "Menyimpan..." : "Simpan" }}
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

const baseURL = "https://back.kecilungresto.com/api";
const {
  data: response,
  pending,
  refresh,
} = useLazyFetch(`${baseURL}/categories`);
const allCategories = computed(() => response.value?.data || []);

// --- STATE FILTER & PAGINATION ---
const searchQuery = ref("");
const itemsPerPage = ref(10);
const currentPage = ref(1);

watch([searchQuery, itemsPerPage], () => {
  currentPage.value = 1;
});

// --- LOGIKA FILTER (SEARCH BAR) ---
const filteredCategories = computed(() => {
  if (!searchQuery.value) return allCategories.value;
  const q = searchQuery.value.toLowerCase();
  return allCategories.value.filter(
    (cat) =>
      cat.name.toLowerCase().includes(q) || cat.code.toLowerCase().includes(q),
  );
});

// --- LOGIKA PAGINATION ---
const totalPages = computed(() =>
  Math.ceil(filteredCategories.value.length / itemsPerPage.value),
);

const paginatedCategories = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage.value;
  return filteredCategories.value.slice(start, start + itemsPerPage.value);
});

// Logika "Showing X to Y of Z"
const showingStart = computed(() =>
  filteredCategories.value.length === 0
    ? 0
    : (currentPage.value - 1) * itemsPerPage.value + 1,
);
const showingEnd = computed(() =>
  Math.min(
    currentPage.value * itemsPerPage.value,
    filteredCategories.value.length,
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
const form = ref({ id: null, code: "", name: "", description: "" });
const selectedFile = ref(null);

const openModal = (mode, data = null) => {
  modalMode.value = mode;
  selectedFile.value = null;
  if (mode === "edit" && data) form.value = { ...data };
  else form.value = { id: null, code: "", name: "", description: "" };
  isModalOpen.value = true;
};

const closeModal = () => (isModalOpen.value = false);

const handleFileChange = (e) => {
  if (e.target.files.length > 0) selectedFile.value = e.target.files[0];
};

const saveCategory = async () => {
  isSaving.value = true;
  try {
    const formData = new FormData();
    formData.append("code", form.value.code);
    formData.append("name", form.value.name);
    formData.append("description", form.value.description || "");
    if (selectedFile.value) formData.append("image", selectedFile.value);

    if (modalMode.value === "add") {
      await $fetch(`${baseURL}/categories`, { method: "POST", body: formData });
    } else {
      await $fetch(`${baseURL}/categories/${form.value.id}`, {
        method: "PUT",
        body: formData,
      });
    }

    closeModal();
    refresh();
    Swal.fire({
      icon: "success",
      title: "Berhasil!",
      text: `Kategori berhasil disimpan.`,
      timer: 1500,
      showConfirmButton: false,
    });
  } catch (error) {
    Swal.fire({
      icon: "error",
      title: "Gagal!",
      text: "Gagal menyimpan data.",
    });
  } finally {
    isSaving.value = false;
  }
};

const deleteCategory = async (id) => {
  const result = await Swal.fire({
    title: "Hapus Kategori?",
    text: "Data yang dihapus tidak dapat dikembalikan!",
    icon: "warning",
    showCancelButton: true,
    confirmButtonColor: "#ef4444",
    cancelButtonColor: "#6b7280",
    confirmButtonText: "Ya, Hapus!",
  });

  if (result.isConfirmed) {
    try {
      await $fetch(`${baseURL}/categories/${id}`, { method: "DELETE" });
      refresh();
      Swal.fire({
        icon: "success",
        title: "Terhapus!",
        text: "Kategori dihapus.",
        timer: 1500,
        showConfirmButton: false,
      });
    } catch (error) {
      Swal.fire({
        icon: "error",
        title: "Gagal!",
        text: "Gagal menghapus data kategori.",
      });
    }
  }
};
</script>

<style scoped>
.slide-up-anim {
  animation: slideUp 0.3s ease-out forwards;
}
@keyframes slideUp {
  0% {
    transform: translateY(20px);
    opacity: 0;
  }
  100% {
    transform: translateY(0);
    opacity: 1;
  }
}
</style>
