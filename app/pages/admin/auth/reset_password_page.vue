<template>
  <div
    class="min-h-screen bg-gray-50 flex flex-col justify-center items-center p-4"
  >
    <div
      class="max-w-md w-full bg-white rounded-2xl shadow-xl p-8 border border-gray-100"
    >
      <div class="text-center mb-8">
        <h2 class="text-2xl font-bold text-gray-900">Buat Password Baru</h2>
        <p class="text-sm text-gray-500 mt-2">
          Buat kata sandi baru yang kuat untuk akun Anda.
        </p>
      </div>

      <form @submit.prevent="handleResetPassword" class="space-y-6">
        <div>
          <label class="block text-sm font-bold text-gray-700 mb-2"
            >Password Baru</label
          >
          <input
            v-model="newPassword"
            type="password"
            minlength="6"
            required
            class="w-full px-4 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-orange-500 focus:border-transparent transition-all"
            placeholder="Minimal 6 karakter"
          />
        </div>

        <div>
          <label class="block text-sm font-bold text-gray-700 mb-2"
            >Konfirmasi Password Baru</label
          >
          <input
            v-model="confirmPassword"
            type="password"
            minlength="6"
            required
            class="w-full px-4 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-orange-500 focus:border-transparent transition-all"
            placeholder="Ketik ulang password"
          />
        </div>

        <button
          type="submit"
          :disabled="isLoading"
          class="w-full flex justify-center py-3 px-4 border border-transparent rounded-lg shadow-sm text-sm font-bold text-white bg-green-600 hover:bg-green-700 disabled:opacity-50 transition-all"
        >
          {{ isLoading ? "Menyimpan..." : "Simpan Password & Login" }}
        </button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";
import Swal from "sweetalert2";

definePageMeta({ layout: false });

const router = useRouter();
const config = useRuntimeConfig();
const newPassword = ref("");
const confirmPassword = ref("");
const email = ref("");
const isLoading = ref(false);

onMounted(() => {
  const savedEmail = sessionStorage.getItem("reset_email");
  if (!savedEmail) {
    router.push("/admin/auth/login_page");
  } else {
    email.value = savedEmail;
  }
});

const handleResetPassword = async () => {
  if (newPassword.value !== confirmPassword.value) {
    Swal.fire("Error", "Konfirmasi password tidak cocok!", "error");
    return;
  }

  isLoading.value = true;
  try {
    const baseURL = config.public.apiBase || "http://31.97.60.207:8246";
    await $fetch(`${baseURL}/api/auth/reset-password`, {
      method: "POST",
      body: { email: email.value, new_password: newPassword.value },
    });

    // Selesai, hapus session dan arahkan ke login
    sessionStorage.removeItem("reset_email");

    Swal.fire({
      icon: "success",
      title: "Berhasil!",
      text: "Password Anda telah diperbarui. Silakan login.",
    }).then(() => {
      router.push("/admin/auth/login_page");
    });
  } catch (error) {
    Swal.fire(
      "Gagal",
      error.response?._data?.error || "Gagal mereset password.",
      "error",
    );
  } finally {
    isLoading.value = false;
  }
};
</script>
