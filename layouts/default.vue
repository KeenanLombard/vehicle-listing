<!-- @format -->

<template>
  <div>
    <!-- Navigation -->
    <header>
      <nav>
        <Navbar />
      </nav>
    </header>
    <!-- Page Content -->
    <main>
      <NuxtPage />
    </main>
  </div>
</template>

<script setup>
import Navbar from "~/components/Navbar.vue";
</script>
