<script setup lang="ts">
  import {inject} from "vue";

  const { closeDrawer } = inject('cart' )
</script>

<template>
  <div  class="flex items-center gap-5">

    <svg @click="closeDrawer"
        class="opacity-70 cursor-pointer rotate-180 hover:opacity-100 hover: transition mb-8"
        width="7" height="12" viewBox="0 0 7 12" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M1 0.999999L6 6L1 11" stroke="#C8C8C8" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </svg>
    <h2 class="text-2xl font-bold">Корзина</h2>

  </div>
</template>

<style scoped>

</style>