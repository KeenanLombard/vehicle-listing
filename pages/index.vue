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
const totalItems = ref(0);

// Function to fetch data
const fetchCars = async () => {
  pending.value = true;
  error.value = null;

  //struggled abit here but resolved using a default url string
  //and adding filter statements if selected
  try {
    let url = `https://crm.quickrentals.co.za/items/stock/?fields=*.*.*&meta=*&filter[monthly_rental][_gt]=0&limit=${limit}&page=${currentPage.value}&sort[]=${sortOrder.value}`;

    if (selectedBrand.value) {
      url += `&filter[_or][0][make][_contains]=${selectedBrand.value}`;
    }
    const response = await $fetch(url);
    data.value = response?.data || [];
    totalItems.value = response?.meta.filter_count || [];
    console.log(response);
  } catch (err) {
    error.value = err;
  } finally {
    pending.value = false;
  }
};

//watch filter values and does a new fetch if values changes
//also resets page state
watch([selectedBrand, currentPage, sortOrder], () => {
  if (selectedBrand.value) {
    currentPage.value = 1;
  }
  fetchCars();
});

fetchCars();
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
  <div class="container mx-auto">
    <!-- Filter Section -->
    <div
      class="border-blue-400 bg-blue-100 border-b mb-4 rounded-b p-6 flex flex-col sm:flex-row lg:flex-row gap-4 justify-between items-center">
      <div class="flex space-x-4">
        <div class="flex flex-col sm:flex-row gap-4 sm:w-auto w-full">
          <select
            v-model="selectedBrand"
            id="brand-filter"
            class="p-4 w-full sm:w-auto border-2 rounded-xl border-blue-800">
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
          </select>
        </div>
        <!-- Sort Dropdown -->
        <div class="flex flex-col sm:flex-row gap-4 sm:w-auto w-full">
          <select
            v-model="sortOrder"
            id="sort-cars"
            class="p-4 w-full sm:w-auto border-2 rounded-xl border-blue-800"
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
      <!-- Reset Button -->
      <div
        class="w-full sm:w-auto p-4 flex h-full justify-center sm:justify-start">
        <button
          @click="resetFilters"
          class="p-4 w-full sm:w-auto border-2 rounded-xl border-blue-800 cursor-pointer font-semibold text-blue-900 bg-blue-200 hover:bg-blue-300">
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

    <!-- Render Car Cards -->
    <div
      v-else
      class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 px-4">
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
      <p class="text-lg font-semibold">
        Page {{ currentPage }} of {{ Math.ceil(totalItems / 12) }}
      </p>
      <button
        @click="changePage(currentPage + 1)"
        :disabled="data.length < limit"
        class="px-4 py-2 bg-gray-300 rounded disabled:opacity-50">
        Next
      </button>
    </div>
  </div>
</template>
