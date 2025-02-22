<!-- @format -->
<!-- Author: Keenan Lombard -->

<script setup>
import { ref } from "vue";
import { Teleport } from "vue";

const showModal = ref(false);

// Function to open the modal
const openModal = () => {
  showModal.value = true;
};

// Function to close the modal
const closeModal = () => {
  showModal.value = false;
};
</script>

<template>
  <div class="text-blue-900 shadow-md sticky top-0 z-50">
    <div class="container mx-auto flex items-center justify-between py-4 px-6">
      <!-- Logo -->
      <h1
        class="text-2xl font-semibold cursor-pointer"
        @click="$router.push('/')">
        Keenan Lombard
      </h1>

      <!-- Info Button -->
      <button
        class="bg-blue-100 text-blue-900 cursor-pointer font-semibold p-2 rounded-lg hover:bg-blue-200 transition-colors"
        @click="openModal">
        info
      </button>
    </div>
  </div>

  <!-- I used teloport because ive had styling conflicts before and i think its a good habit -->
  <Teleport to="body">
    <div
      v-if="showModal"
      class="fixed inset-0 bg-neutral-600 bg-opacity-10 flex justify-center items-center z-50"
      @click="closeModal">
      <div class="bg-white p-6 rounded-lg max-w-lg w-full" @click.stop>
        <h1 class="text-xl font-bold mb-4">Helpfull Information</h1>

        <p class="pb-4">
          Try not to rent a
          <span class="font-semibold text-blue-500">Ford</span>, they leak oil,
          buy a <span class="font-semibold text-blue-500">Hilux</span>
        </p>

        <button
          class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700"
          @click="closeModal">
          Close
        </button>
      </div>
    </div>
  </Teleport>
</template>

<style scoped>
/* Navbar styling */
header {
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

header h1 {
  font-family: "Roboto", sans-serif;
  cursor: pointer;
}

header button {
  font-size: 1.2rem;
  transition: background-color 0.3s;
}

/* Modal styling */
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
  animation: fadeUp 0.3s ease-in-out;
}

@keyframes fadeUp {
  from {
    transform: translateY(10px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}
</style>
