<!-- @format -->

<script setup>
import { ref } from "vue";
import { Teleport } from "vue";

const showModal = ref(false);

const openModal = () => {
  showModal.value = true;
};

const closeModal = () => {
  showModal.value = false;
};
</script>

<template>
  <div
    class="bg-gradient-to-r from-blue-800 to-blue-900 text-white shadow-lg sticky top-0 z-50">
    <div class="container mx-auto flex items-center justify-between py-5 px-8">
      <!-- Logo -->
      <h1
        class="text-3xl font-bold tracking-wide cursor-pointer hover:opacity-80 transition"
        @click="$router.push('/')">
        Keenan Lombard
      </h1>

      <!-- Info Button -->
      <button
        class="bg-white text-blue-900 font-semibold py-2 px-4 rounded-lg shadow-md hover:bg-blue-200 hover:scale-105 transition-transform"
        @click="openModal">
        Info
      </button>
    </div>
  </div>

  <!-- Modal -->
  <Teleport to="body">
    <div
      v-if="showModal"
      class="fixed inset-0 bg-opacity-40 backdrop-blur-md flex justify-center items-center z-50"
      @click="closeModal">
      <div
        class="bg-white p-6 rounded-lg max-w-lg w-full shadow-xl transform transition-all scale-95 hover:scale-100"
        @click.stop>
        <h1 class="text-xl font-bold mb-4">Helpful Information</h1>

        <p class="pb-4 text-gray-700">
          Try not to rent a
          <span class="font-semibold text-blue-500">Ford</span>, they leak oil.
          Buy a <span class="font-semibold text-blue-500">Hilux</span>.
        </p>

        <button
          class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition"
          @click="closeModal">
          Close
        </button>
      </div>
    </div>
  </Teleport>
</template>

<style scoped>
/* Animations */
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
