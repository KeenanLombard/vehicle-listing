<!-- @format -->

<script setup>
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";
import Spinner from "~/components/Spinner.vue";

const route = useRoute();

const car = ref(null);
//To check is there is images, There was a bug where-
//application was stuck in loading state after refresh-
//this was the fix
const images = ref(0);
const loading = ref(false);
const error = ref(null);

const modalVisible = ref(false);
const selectedImage = ref(null);

//check formats are correct and images available
const fetchCarDetails = async () => {
  loading.value = true;
  error.value = null;
  try {
    const response = await $fetch(
      `https://crm.quickrentals.co.za/items/stock/?filter[uid][_eq]=${route.params.uid}&fields=*.*.*`
    );
    car.value = response?.data?.[0] || null;
    images.value = car.value.images.length;
  } catch (err) {
    error.value = "Failed to load car details.";
  } finally {
    loading.value = false;
  }
};

const openModal = (image) => {
  selectedImage.value = image;
  modalVisible.value = true;
};

const closeModal = () => {
  modalVisible.value = false;
  selectedImage.value = null;
};

onMounted(fetchCarDetails);
</script>

<template>
  <div class="container mx-auto p-6">
    <!-- Loading Animation -->
    <div v-if="loading" class="flex justify-center items-center h-48">
      <Spinner />
    </div>

    <!-- Error Handling -->
    <div v-else-if="error" class="text-red-500 text-center text-lg">
      {{ error }}
    </div>

    <div v-else-if="car && images > 0">
      <!-- Heading -->
      <div class="mb-6">
        <h1 class="text-4xl font-bold text-gray-900">
          {{ car.vehicle_name }}
        </h1>
        <p class="text-2xl font-semibold text-blue-600 mt-2">
          R {{ car.monthly_rental ?? "No price available" }} / month
        </p>
      </div>

      <CarBanner :car="car" />

      <!-- Images Section -->
      <div>
        <div class="grid grid-cols-2 md:grid-cols-3 gap-4">
          <img
            v-for="(image, index) in car.images"
            :key="index"
            :src="`https://crm.quickrentals.co.za/assets/${image.directus_files_id.filename_disk}`"
            alt="Car Image"
            class="w-full h-40 object-cover rounded shadow-md hover:scale-105 transition-transform cursor-pointer"
            @click="openModal(image)" />
        </div>
      </div>
    </div>

    <div v-else class="text-center text-gray-500 text-lg">
      There is no imnages for this vehicle
      <NuxtLink class="text-blue-500" to="/">Browse Other Vehicles</NuxtLink>
    </div>

    <!-- Modal for Enlarged Image -->
    <Teleport to="body">
      <div
        v-if="modalVisible"
        class="fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center z-50">
        <div class="relative bg-white p-6 rounded-lg shadow-lg max-w-lg">
          <button
            @click="closeModal"
            class="absolute top-2 right-4 text-2xl font-bold text-gray-700 hover:text-red-500 transition">
            ✖
          </button>
          <img
            v-if="selectedImage"
            :src="`https://crm.quickrentals.co.za/assets/${selectedImage.directus_files_id.filename_disk}`"
            alt="Enlarged Car Image"
            class="w-full h-[600px] object-cover rounded-md" />
        </div>
      </div>
    </Teleport>
  </div>
</template>
