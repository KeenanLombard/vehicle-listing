<!-- @format -->

<script setup>
import { ref, computed, watch } from "vue";
import CarCard from "~/components/CarCard.vue";
import Spinner from "~/components/Spinner.vue";

const limit = 12; // Limits items per page
const currentPage = ref(1);
const selectedBrand = ref(""); // Default is 'All Brands'
const sortOrder = ref("+make");
const data = ref([]); // To store the fetched data
const pending = ref(false); // To track loading state
const error = ref(null); // To track error state

// Function to fetch data
const fetchCars = async () => {
  pending.value = true;
  error.value = null;

  try {
    let url = `https://crm.quickrentals.co.za/items/stock/?fields=*.*.*&limit=${limit}&page=${currentPage.value}&sort[]=${sortOrder.value}`;

    if (selectedBrand.value) {
      url += `&filter[_or][0][make][_contains]=${selectedBrand.value}`;
    }

    const response = await $fetch(url);
    data.value = response?.data || [];
  } catch (err) {
    error.value = err;
  } finally {
    pending.value = false;
  }
};

watch([selectedBrand, currentPage, sortOrder], () => {
  if (selectedBrand.value) {
    // Reset currentPage to 1 when selectedBrand changes
    currentPage.value = 1;
  }
  fetchCars(); // Call fetchCars after the change
});
// Fetch data initially
fetchCars();

// Computed property to filter cars based on search query
const filteredCars = computed(() => {
  return data.value;
});

// Increment or Decrement currentPage
const changePage = (page) => {
  currentPage.value = page;
};

const resetFilters = () => {
  selectedBrand.value = "";
  sortOrder.value = "+make";
  currentPage.value = 1;
};

// Function to change sort order
const changeSort = (sort) => {
  sortOrder.value = sort;
  currentPage.value = 1;
};
</script>

<template>
  <div class="container mx-auto p-6">
    <!-- Filter by Brand Dropdown -->

    <div
      class="flex flex-col justify-between lg:flex-row md:flex-col sm:flex-col">
      <div class="mb-6 flex flex-wrap justify-end gap-4">
        <label for="brand-filter" class="text-lg my-auto font-semibold"
          >Filter by Brand:</label
        >
        <select
          v-model="selectedBrand"
          id="brand-filter"
          class="border border-gray-300 p-2 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"
          @change="fetchCars">
          <option value="">All Brands</option>
          <option value="Toyota">Toyota</option>
          <option value="Ford">Ford</option>
          <option value="Nissan">Nissan</option>
          <option value="Hyundai">Hyundai</option>
          <option value="Aprilia">Aprilia</option>
          <option value="Audi">Audi</option>
          <option value="Renault">Renault</option>
          <option value="Maserati">Maserati</option>
          <option value="Mercedes">Mercedes</option>
          <option value="Kia">Kia</option>
          <!-- Add more brands as needed -->
        </select>
      </div>

      <!-- Sort By : Dropdown -->
      <div class="mb-6 flex flex-wrap justify-end gap-4">
        <div class="flex items-center space-x-2">
          <label for="sort-cars" class="text-lg font-semibold">Sort by:</label>
          <select
            v-model="sortOrder"
            id="sort-cars"
            class="border border-gray-300 p-2 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"
            @change="changeSort(sortOrder)">
            <option value="+make">Make (A-Z)</option>
            <option value="-make">Make (Z-A)</option>
            <option value="+year">Year (Oldest to Newest)</option>
            <option value="-year">Year (Newest to Oldest)</option>
            <option value="+monthly_rental">Price (Low to High)</option>
            <option value="-monthly_rental">Price (High to Low)</option>
          </select>
        </div>
      </div>

      <div class="mb-6 flex flex-wrap justify-end gap-4">
        <button
          @click="resetFilters()"
          class="bg-blue-100 hover:bg-blue-200 text-blue-900 font-semibold rounded-xl p-4">
          Reset All Filters
        </button>
      </div>
    </div>

    <!-- Loading Animation -->
    <div
      v-if="pending"
      class="flex mx-auto justify-center w-screen items-center h-48">
      <Spinner />
    </div>

    <!-- Error Handling -->
    <div v-else-if="error">
      <p class="text-red-500">
        Error:
        {{ error?.message || "Failed to load cars. Please try again later." }}
      </p>
    </div>

    <!-- Render Cars -->
    <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <div
        v-for="car in filteredCars"
        :key="car.id"
        class="bg-white cursor-pointer rounded-lg shadow-md hover:shadow-xl overflow-hidden transition-transform transform hover:scale-105 duration-300">
        <NuxtLink :to="`${car.uid}`">
          <CarCard :car="car" />
        </NuxtLink>
      </div>
    </div>

    <!-- Pagination Controls -->
    <div class="flex justify-between items-center my-8">
      <button
        @click="changePage(currentPage - 1)"
        :disabled="currentPage === 1"
        class="px-4 py-2 bg-gray-300 rounded disabled:opacity-50">
        Prev
      </button>
      <p class="text-lg font-semibold">Page {{ currentPage }}</p>
      <button
        @click="changePage(currentPage + 1)"
        :disabled="data.length < limit"
        class="px-4 py-2 bg-gray-300 rounded disabled:opacity-50">
        Next
      </button>
    </div>
  </div>
</template>
