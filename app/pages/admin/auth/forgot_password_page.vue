<template>
  <div
    class="min-h-screen bg-gray-50 flex flex-col justify-center items-center p-4"
  >
    <div
      class="max-w-md w-full bg-white rounded-2xl shadow-xl p-8 border border-gray-100"
    >
      <div class="text-center mb-8">
        <h2 class="text-2xl font-bold text-gray-900">Lupa Password?</h2>
        <p class="text-sm text-gray-500 mt-2">
          Masukkan email Anda. Kami akan mengirimkan kode OTP untuk mengatur
          ulang password Anda.
        </p>
      </div>

      <form @submit.prevent="handleRequestOTP" class="space-y-6">
        <div>
          <label class="block text-sm font-bold text-gray-700 mb-2"
            >Alamat Email</label
          >
          <input
            v-model="email"
            type="email"
            required
            class="w-full px-4 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-orange-500 focus:border-transparent transition-all"
            placeholder="admin@email.com"
          />
        </div>

        <button
          type="submit"
          :disabled="isLoading"
          class="w-full flex justify-center py-3 px-4 border border-transparent rounded-lg shadow-sm text-sm font-bold text-white bg-orange-600 hover:bg-orange-700 disabled:opacity-50 transition-all"
        >
          {{ isLoading ? "Mengirim Kode..." : "Kirim Kode OTP" }}
        </button>

        <div class="text-center mt-4">
          <NuxtLink
            to="/admin/auth/login_page"
            class="text-sm text-gray-500 hover:text-gray-800 font-semibold transition-colors"
          >
            &larr; Kembali ke Login
          </NuxtLink>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import Swal from "sweetalert2";

definePageMeta({ layout: false });

const router = useRouter();
const config = useRuntimeConfig();
const email = ref("");
const isLoading = ref(false);

const handleRequestOTP = async () => {
  isLoading.value = true;
  try {
    const baseURL = config.public.apiBase || "http://31.97.60.207:8246";
    await $fetch(`${baseURL}/api/auth/forgot-password`, {
      method: "POST",
      body: { email: email.value },
    });

    // Simpan email sementara di Session Storage untuk dipakai di halaman selanjutnya
    sessionStorage.setItem("reset_email", email.value);
    router.push("/admin/auth/otp_verification_page");
  } catch (error) {
    Swal.fire(
      "Gagal",
      error.response?._data?.error || "Email tidak ditemukan.",
      "error",
    );
  } finally {
    isLoading.value = false;
  }
};
</script>
