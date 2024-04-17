<script setup lang="ts">
import Header from "@/components/Header.vue";
import axios from 'axios';
import Drawer from "@/components/Drawer.vue";
import { ref, watch, provide, computed} from "vue";

// ==============================
// handled order
// ==============================









// ==============================
// ==============================


// ==============================
// handled cart
// ==============================
const cart = ref([]);

const totalCartPrice = computed(
    () => cart.value.reduce((acc, item) => acc + item.price, 0)
);

const vatPrice = computed(
    () => Math.round((totalCartPrice.value * 5) / 100)
);

const addToCart = (item) => {
  cart.value.push(item);
  item.isAdded = true;
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1);
  item.isAdded = false;
}


// ==============================
// handled Drawer
// ==============================
const drawerStatus = ref(false);

const openDrawer = () => {
  drawerStatus.value = true;
}
const closeDrawer = () => {
  drawerStatus.value = false;
}

provide('cart',  {
  cart,
  openDrawer,
  closeDrawer,
  addToCart,
  removeFromCart
})
// ==============================
// ==============================



// onMounted(async () => {
//   // Асинхронная функция
//   // try
//   // {
//   //   const { data } = await axios.get('https://fb9d01f7a2cfae30.mokky.dev/items2')
//   //   items.value = data;
//   // }
//   // catch (err)
//   // {
//   //   console.log(err);
//   // }
//
//   // Вариант фетч
//   // fetch('https://fb9d01f7a2cfae30.mokky.dev/items2')
//   //     .then(response => response.json)
//   //     .then(data => {
//   //       console.log(data);
//   //     })
//
//   // Вариант с аксиос(библиотека)
//   // axios.get('https://fb9d01f7a2cfae30.mokky.dev/items2')
//   //     .then(res => console.log(res.data))
// });





watch(cart,
    () => {
      localStorage.setItem('cart', JSON.stringify(cart.value));
    },
    { deep: true }
);

// watch(filters, async () => {
//   try
//   {
//     const { data } = await axios.get('https://fb9d01f7a2cfae30.mokky.dev/items2?sortBy=' + filters.sortBy);
//     items.value = data;
//   }
//   catch (err)
//   {
//     console.log(err);
//   }
// });


</script>

<template>
  <Drawer
      v-if="drawerStatus"
      :total-cart-price="totalCartPrice"
      :vat-price="vatPrice"

  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-2xl mt-10">
    <Header
        :total-cart-price="totalCartPrice"
        @open-drawer="openDrawer"
    />
    <div class="p-10">

      <router-view></router-view>

    </div>

  </div>
</template>

<style scoped>

</style>
