<!-- @format -->

<template>
  <div>
    <div
      class="p-6 text-blue-900 flex-row sm:flex-col gap-2 justify-between bg-blue-100 rounded-t shadow-lg w-full">
      <p><strong>Year:</strong> {{ car.year }}</p>
      <p><strong>Color:</strong> {{ car.color ?? "N/A" }}</p>
      <p><strong>Body:</strong> {{ car.mm_data.body_type ?? "N/A" }} km</p>
      <p><strong>Fuel Type:</strong> {{ car.mm_data.fuel_type ?? "N/A" }}</p>
      <p><strong>Doors:</strong> {{ car.mm_data.no_of_doors ?? "N/A" }}</p>
      <p><strong>KM:</strong> {{ car.km ?? "N/A" }}</p>
      <p><strong>Model:</strong> {{ car.model ?? "N/A" }}</p>
      <p><strong>Variant:</strong> {{ car.variant ?? "N/A" }} HP</p>
    </div>

    <!-- Featured Image -->
    <div class="w-full mb-6">
      <img
        v-if="car.images?.length >= 0"
        :src="`https://crm.quickrentals.co.za/assets/${car.images[0].directus_files_id.filename_disk}`"
        alt="Featured Car Image"
        class="w-full h-[500px] object-cover rounded-b shadow-lg" />
      <img
        v-else
        src="https://via.placeholder.com/1200x600?text=No+Image+Available"
        alt="Placeholder"
        class="w-full h-[500px] object-cover rounded-b shadow-lg" />
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
