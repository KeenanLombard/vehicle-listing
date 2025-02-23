<!-- @format -->

<template>
  <div class="bg-white rounded-lg shadow-lg overflow-hidden mb-4">
    <!-- Car Info Section -->
    <div class="p-6 bg-blue-100 text-blue-900 flex flex-col gap-4">
      <p><strong class="font-semibold">Year:</strong> {{ car.year }}</p>
      <p>
        <strong class="font-semibold">Color:</strong> {{ car.color ?? "N/A" }}
      </p>
      <p>
        <strong class="font-semibold">Body Type:</strong>
        {{ car.mm_data.body_type ?? "N/A" }}
      </p>
      <p>
        <strong class="font-semibold">Fuel Type:</strong>
        {{ car.mm_data.fuel_type ?? "N/A" }}
      </p>
      <p>
        <strong class="font-semibold">Doors:</strong>
        {{ car.mm_data.no_of_doors ?? "N/A" }}
      </p>
      <p><strong class="font-semibold">KM:</strong> {{ car.km ?? "N/A" }}</p>
      <p>
        <strong class="font-semibold">Model:</strong> {{ car.model ?? "N/A" }}
      </p>
      <p>
        <strong class="font-semibold">Variant:</strong>
        {{ car.variant ?? "N/A" }} HP
      </p>
    </div>

    <!-- Car Image Section -->
    <div class="w-full relative">
      <img
        v-if="car.images?.length > 0"
        :src="`https://crm.quickrentals.co.za/assets/${car.images[0].directus_files_id.filename_disk}`"
        alt="Featured Car Image"
        class="w-full h-[400px] object-cover rounded-b-lg shadow-md transition-all duration-300 ease-in-out hover:scale-105" />
      <img
        v-else
        src="https://via.placeholder.com/1200x600?text=No+Image+Available"
        alt="Placeholder"
        class="w-full h-[400px] object-cover rounded-b-lg shadow-md" />
      <div
        class="absolute top-0 right-0 p-4 bg-black bg-opacity-50 text-white rounded-bl-lg text-lg">
        Featured Image
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
