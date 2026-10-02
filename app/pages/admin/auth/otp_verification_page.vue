<template>
  <div
    class="min-h-screen bg-gray-50 flex flex-col justify-center items-center p-4"
  >
    <div
      class="max-w-md w-full bg-white rounded-2xl shadow-xl p-8 border border-gray-100"
    >
      <div class="text-center mb-8">
        <h2 class="text-2xl font-bold text-gray-900">Verifikasi OTP</h2>
        <p class="text-sm text-gray-500 mt-2">
          Kode 6 digit telah dikirim ke <br /><span
            class="font-bold text-orange-600"
            >{{ email }}</span
          >
        </p>
      </div>

      <form @submit.prevent="handleVerifyOTP" class="space-y-6">
        <div>
          <label class="block text-sm font-bold text-gray-700 mb-2 text-center"
            >Masukkan Kode OTP</label
          >
          <input
            v-model="otpCode"
            type="text"
            maxlength="6"
            required
            class="w-full px-4 py-3 text-center tracking-[1em] text-2xl font-bold rounded-lg border border-gray-300 focus:ring-2 focus:ring-orange-500 focus:border-transparent transition-all"
            placeholder="------"
          />
        </div>

        <button
          type="submit"
          :disabled="isLoading || otpCode.length !== 6"
          class="w-full flex justify-center py-3 px-4 border border-transparent rounded-lg shadow-sm text-sm font-bold text-white bg-orange-600 hover:bg-orange-700 disabled:opacity-50 transition-all"
        >
          {{ isLoading ? "Memverifikasi..." : "Verifikasi & Lanjut" }}
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
const otpCode = ref("");
const email = ref("");
const isLoading = ref(false);

onMounted(() => {
  const savedEmail = sessionStorage.getItem("reset_email");
  if (!savedEmail) {
    router.push("/admin/auth/forgot_password_page"); // Tendang jika akses langsung tanpa email
  } else {
    email.value = savedEmail;
  }
});

const handleVerifyOTP = async () => {
  isLoading.value = true;
  try {
    const baseURL = config.public.apiBase || "http://31.97.60.207:8246";
    await $fetch(`${baseURL}/api/auth/verify-otp`, {
      method: "POST",
      body: { email: email.value, otp: otpCode.value },
    });

    // Valid, lanjut ke reset password
    router.push("/admin/auth/reset_password_page");
  } catch (error) {
    Swal.fire(
      "Kode Salah",
      error.response?._data?.error || "Kode OTP salah atau telah kedaluwarsa.",
      "error",
    );
  } finally {
    isLoading.value = false;
  }
};
</script>
