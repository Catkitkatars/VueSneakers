<script setup lang="ts">

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
  <div class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl">

    <img :src="!isFavorite ? '/like-2.svg' : '/like-1.svg'"
         @click="onClickFavorite"
         class="absolute top-8 left-8"
         alt="plus"
    />

    <img :src="imgUrl" alt="Sneaker">
    <p class="mt-2">{{ title }}</p>
    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена:</span>
        <b>{{ price }} руб.</b>
      </div>
      <img
          :src="!isAdded ? '/plus.svg' : '/checked.svg'"
          @click="onClickAdd"
          alt="add"
      />
    </div>
  </div>
</template>
