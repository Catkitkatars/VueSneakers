<script setup lang="ts">

const emit = defineEmits(['onClickRemove'])

defineProps({
  id: Number,
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
    <img class="w-16 h-16" :src="imgUrl" :alt="title">
    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex justify-between mt-2 w-full">
        <b class="flex-1">{{ price }} руб.</b>
        <img @click="emit('onClickRemove')" class="opacity-50 cursor-pointer hover:opacity-100 hover: transition" src="/close.svg" alt='close'>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>