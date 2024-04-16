<script setup lang="ts">
import CartItem from "@/components/CartItem.vue";
import { inject } from 'vue';

const { cart, removeFromCart } = inject('cart')


</script>

<template>
  <div class="flex flex-col gap-4">
    <CartItem
        v-for="item in cart"
        :key="item.id"
        :title="item.title"
        :price="item.price"
        :img-url="item.imageUrl"
        @on-click-remove="() => removeFromCart(item)"
    />

  </div>

</template>

<style scoped>

</style>