<!-- @format -->
<!-- Author: Keenan Lombard -->

<script setup>
import { ref, computed } from "vue";
import CarCard from "~/components/CarCard.vue";
import Spinner from "~/components/Spinner.vue";

const limit = 12; // limits items per page
const currentPage = ref(1);
const searchQuery = ref("");
const sortOrder = ref("+make");

// Fetch data based on current page, limit and sorting order
const { data, pending, error, refresh } = useAsyncData("stock", async () => {
  const response = await $fetch(
    `https://crm.quickrentals.co.za/items/stock/?fields=*.*.*&limit=${limit}&page=${currentPage.value}&sort[]=${sortOrder.value}`
  );
  return response?.data || [];
});

// Computed property to filter cars based on searchQuery
const filteredCars = computed(() => {
  if (!searchQuery.value) return data.value; // Return all cars if no search query
  return data.value.filter((car) =>
    `${car.make} ${car.model} ${car.year}`
      .toLowerCase()
      .includes(searchQuery.value.toLowerCase())
  );
});

// Increment or Decrement currentPage
const changePage = (page) => {
  currentPage.value = page;
  refresh(); // Fetch next page
};

// Function to change sort order
const changeSort = (sort) => {
  sortOrder.value = sort;
  refresh(); // Fetch ordered data
};
</script>

<template>
  <div class="container mx-auto p-6">
    <!-- Search Bar -->
    <div class="mb-6 flex">
      <input
        v-model="searchQuery"
        placeholder="Search..."
        class="w-full border border-gray-300 p-2 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"
        type="text" />

      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
        class="size-10 my-auto text-blue-900 m-2 p-1">
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="m21 21-5.197-5.197m0 0A7.5 7.5 0 1 0 5.196 5.196a7.5 7.5 0 0 0 10.607 10.607Z" />
      </svg>
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

    <!-- Loading Animation -->
    <div v-if="pending" class="flex justify-center items-center h-48">
      <Spinner />
    </div>

    <!-- Error Handling -->
    <div v-else-if="error">
      <p class="text-red-500">
        Error:
        {{ error?.message || "Failed to load cars. Please try again later." }}
      </p>
    </div>

    <!-- Render Cards-->
    <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <div
        v-if="filteredCars.length != 0"
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
        :disabled="data.length < 12"
        class="px-4 py-2 bg-gray-300 rounded disabled:opacity-50">
        Next
      </button>
    </div>
  </div>
</template>
