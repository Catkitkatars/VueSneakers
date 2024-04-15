<script setup lang="ts">
defineProps({
  title: String,
  imgUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function,
})

</script>

<template>
  <div class="flex items-center border border-slate-200 p-4 rounded-xl gap-4">
    <img class="w-16 h-16" src="/sneakers/sneakers-1.jpg" alt="Sneaker">
    <div class="flex flex-col">
      <p>Кроссовки Nike Air max 270</p>
      <div class="flex justify-between mt-2 w-full">
        <b>12990 руб.</b>
        <img class="opacity-50 cursor-pointer hover:opacity-100 hover: transition" src="/close.svg" alt='close'>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>