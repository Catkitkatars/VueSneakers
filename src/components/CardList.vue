<script setup lang="ts">
  import Card from "@/components/Card.vue";

  defineProps({
    items: Array,
  })
  const emit = defineEmits(['addToFavorite', 'addToCart']);


</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
        v-for="item in items"
        :key="item.id"
        :id="item.id"
        :title="item.title"
        :img-url="item.imageUrl"
        :price="item.price"
        :is-added="false"
        :onClickFavorite="() => emit('addToFavorite', item)"
        :on-click-add="() => emit('addToCart', item)"
        :is-favorite="item.isFavorite"
        :isAdded = 'item.isAdded'
    />
  </div>
</template>

<style scoped>

</style>