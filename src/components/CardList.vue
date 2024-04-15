<script setup lang="ts">
  import Card from "@/components/Card.vue";
  defineProps({
    items: Array,
  })
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
        v-for="item in items"
        :key="item.id"
        :title="item.title"
        :img-url="item.imageUrl"
        :price="item.price"
        :is-added="false"
        :is-favorite="false"
    />
  </div>
</template>

<style scoped>

</style>