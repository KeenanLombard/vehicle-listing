<!-- @format -->

<script setup>
import { ref } from "vue";

const showModal = ref(false);

const openModal = () => {
  showModal.value = true;
};

const closeModal = () => {
  showModal.value = false;
};
</script>

<template>
  <!-- Header Section -->
  <div
    class="bg-gradient-to-l from-blue-600 to-blue-800 text-white shadow-lg sticky top-0 z-50">
    <div class="container mx-auto flex items-center justify-between py-6 px-8">
      <!-- Logo -->
      <img
        @click="$router.push('/')"
        class="w-45 cursor-pointer"
        src="https://werentcarssa.co.za/_nuxt/img/wrc-logo.b71ccfa.svg"
        alt="" />

      <!-- Info Modal Toggle -->
      <button
        class="bg-blue-700 cursor-pointer border-neutral-400 border-2 text-white font-semibold py-3 px-6 rounded-lg shadow-md hover:bg-blue-800 hover:scale-105 transition-transform duration-300"
        @click="openModal">
        Info
      </button>
    </div>
  </div>

  <!-- Modal / It's a joke ( i have a ford ) -->
  <Teleport to="body">
    <div
      v-if="showModal"
      class="fixed inset-0 bg-black bg-opacity-50 backdrop-blur-sm flex justify-center items-center z-50"
      @click="closeModal">
      <div
        class="bg-white p-8 rounded-lg max-w-lg w-full shadow-xl transform transition-all scale-95 hover:scale-100"
        @click.stop>
        <h1 class="text-2xl font-semibold text-gray-800 mb-4">
          Helpful Information
        </h1>
        <p class="text-gray-600 mb-6">
          Try not to rent a
          <span class="font-semibold text-blue-500">Ford</span>, they leak oil.
          Buy a <span class="font-semibold text-blue-500">Hilux</span> instead.
        </p>
        <button
          class="bg-blue-600 text-white px-6 py-3 rounded-lg hover:bg-blue-700 transition"
          @click="closeModal">
          Close
        </button>
      </div>
    </div>
  </Teleport>
</template>

<style scoped>
.bg-black.bg-opacity-50 {
  animation: fadeIn 0.3s ease-in-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.bg-white {
  animation: scaleUp 0.3s ease-in-out;
}

@keyframes scaleUp {
  from {
    transform: scale(0.95);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}
</style>
