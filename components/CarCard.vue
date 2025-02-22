<!-- @format -->

<template>
  <div class="flex flex-col h-full border border-gray-200 rounded-lg shadow-lg">
    <!-- Image Section -->
    <div class="w-full h-48 overflow-hidden">
      <img
        v-if="car.images?.[0]?.directus_files_id?.filename_disk"
        :src="`https://crm.quickrentals.co.za/assets/${car.images?.[0]?.directus_files_id?.filename_disk}`"
        alt="car"
        class="w-full h-full object-cover" />
      <img
        v-else
        :src="`https://www.shutterstock.com/image-vector/coming-soon-on-dark-background-600nw-2364512887.jpg`"
        alt="car"
        class="w-full h-full object-cover" />
    </div>

    <!-- Image Counter Section -->
    <div class="flex items-center bg-blue-100 text-blue-900 p-2">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
        class="w-6 h-6 mx-2">
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="m2.25 15.75 5.159-5.159a2.25 2.25 0 0 1 3.182 0l5.159 5.159m-1.5-1.5 1.409-1.409a2.25 2.25 0 0 1 3.182 0l2.909 2.909m-18 3.75h16.5a1.5 1.5 0 0 0 1.5-1.5V6a1.5 1.5 0 0 0-1.5-1.5H3.75A1.5 1.5 0 0 0 2.25 6v12a1.5 1.5 0 0 0 1.5 1.5Zm10.5-11.25h.008v.008h-.008V8.25Zm.375 0a.375.375 0 1 1-.75 0 .375.375 0 0 1 .75 0Z" />
      </svg>
      <p>{{ car.images.length }}</p>
    </div>

    <!-- Content Section -->
    <div class="flex flex-col p-4 flex-1">
      <h2 class="font-bold text-xl text-gray-800 mb-4">
        {{ car.vehicle_name }}
      </h2>

      <div class="flex-1 flex flex-col justify-end">
        <p
          v-if="car.monthly_rental != null || car.monthly_rental === 0"
          class="text-lg font-semibold text-blue-600 mb-2">
          R {{ car.monthly_rental }}
        </p>
        <p v-else class="text-lg font-semibold text-blue-600 mb-2">
          No price available
        </p>

        <div class="text-gray-600">
          <p>{{ car.location }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    car: {
      type: Object,
      required: true,
    },
  },
};
</script>
