<script setup lang="ts">
  defineProps({
    imageUrl: String,
    description: String,
    title: String,
  })
</script>

<template>
  <div class="flex flex-col items-center text-center w-72 mx-auto ">
    <img height="100" width="100" class="" :src="imageUrl" alt="info image">
    <h2 class="mt-4 text-2xl font-medium">
      {{ title }}
    </h2>
    <p class="text-gray-400 mt-2">
      {{ description }}
    </p>
  </div>

</template>

<style scoped>

</style>