<script setup lang="ts">
import { RouterView } from "vue-router"
import Navbar from "@/components/navigation/AppNavbar.vue"
</script>

<template>
  <Navbar />
  <div class="app-container">
    <RouterView />
  </div>
</template>

<style lang="scss" scoped>
.app-container {
  padding: 32px;
  width: 100%;
}
</style>
