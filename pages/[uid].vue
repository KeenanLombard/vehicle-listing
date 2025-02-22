<!-- @format -->
<!-- Author: Keenan Lombard -->

<script setup>
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";
import Spinner from "~/components/Spinner.vue";

const route = useRoute();

const car = ref(null);
const loading = ref(false);
const error = ref(null);

//modal toggle
const modalVisible = ref(false);
const selectedImage = ref(null);

//get car details by uid
const fetchCarDetails = async () => {
  loading.value = true;
  error.value = null;

  try {
    const response = await $fetch(
      `https://crm.quickrentals.co.za/items/stock/?filter[uid][_eq]=${route.params.uid}&fields=*.*.*`
    );
    car.value = response?.data?.[0] || null;
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

    <div v-else-if="car">
      <!-- Heading -->
      <div class="text-center mb-6">
        <h1 class="text-4xl font-bold text-gray-900">
          {{ car.vehicle_name }}
        </h1>
        <p class="text-2xl font-semibold text-blue-600 mt-2">
          R {{ car.monthly_rental ?? "No price available" }} / month
        </p>
      </div>

      <CarBanner :car="car" />

      <div class="">
        <!-- Images Grid -->
        <div class="md:col-span-2 grid grid-cols-2 md:grid-cols-3 gap-4">
          <img
            v-for="(image, index) in car.images.slice(1)"
            :key="index"
            :src="`https://crm.quickrentals.co.za/assets/${image.directus_files_id.filename_disk}`"
            alt="Car Image"
            class="w-full h-40 object-cover rounded shadow-md hover:scale-105 transition-transform cursor-pointer"
            @click="openModal(image)" />
        </div>
      </div>
    </div>

    <div v-else class="text-center text-gray-500 text-lg">No car found.</div>

    <!-- Modal for Enlarged Image -->
    <Teleport to="body">
      <div
        v-if="modalVisible"
        class="fixed inset-0 bg-neutral-100 bg-opacity-50 flex justify-center items-center z-50">
        <div class="bg-white p-4 rounded shadow-lg max-w-lg">
          <button
            @click="closeModal"
            class="absolute font-bold top-2 right-2 text-3xl">
            CLOSE
          </button>
          <img
            v-if="selectedImage"
            :src="`https://crm.quickrentals.co.za/assets/${selectedImage.directus_files_id.filename_disk}`"
            alt="Enlarged Car Image"
            class="w-full h-[600px] object-cover" />
        </div>
      </div>
    </Teleport>
  </div>
</template>
