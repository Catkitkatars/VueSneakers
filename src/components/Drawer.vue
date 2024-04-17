<script setup lang="ts">
import DrawerHead from "@/components/DrawerHead.vue";
import CartItemList from "@/components/CartItemList.vue";
import {computed, ref, inject} from "vue";
import InfoBlock from "@/components/InfoBlock.vue";
import axios from "axios";

const props = defineProps({
  totalCartPrice: Number,
  vatPrice: Number,
})

const {
  cart,
  closeDrawer
} = inject('cart')

const isCreating = ref(false);
const orderId = ref(null)
const createOrder = async () => {
  try
  {
    isCreating.value = true;
    const { data } = await axios.post(`https://fb9d01f7a2cfae30.mokky.dev/orders`, {
      items: cart.value,
      totalCartPrice: props.totalCartPrice.value
    })

    cart.value = [];
    orderId.value = data.id;
  }
  catch (err)
  {
    console.log(err);
  }
  finally
  {
    isCreating.value = false;
  }
}

const cartIsEmpty = computed(() => cart.value.length === 0)
const disabledCartButton = computed(() => isCreating.value || cartIsEmpty.value);

</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-50"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead/>

    <div
        v-if="!totalCartPrice || orderId"
        class="flex h-full items-center">
      <InfoBlock
          v-if="orderId"
          title="Заказ оформлен"
          :description="`Ваш заказ № ${orderId} скоро будет передан в доставку`"
          image-url="/order-success-icon.png"
      />
      <InfoBlock
          v-if="!totalCartPrice && !orderId"
          title="Корзина пустая"
          description="Добавьте товар в корзину"
          image-url="/package-icon.png"
      />
    </div>

    <CartItemList
      v-if="totalCartPrice"
    />



    <div v-if="totalCartPrice" class="flex flex-col gap-4 mb-6 mt-7">
      <div class="flex gap-2">
        <span>Итого</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ totalCartPrice }} руб.</b>
      </div>
      <div class="flex gap-2">
        <span>Налог 5%</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ vatPrice }} руб.</b>
      </div>
      <button
          :disabled="disabledCartButton"
          @click="createOrder"
          class="mt-4 cursor-pointer bg-lime-600 w-full rounded-xl py-3 disabled:bg-slate-300 text-white hover:bg-lime-700 active:bg-lime-800">
        Оформить заказ
      </button>
    </div>

  </div>

</template>

<style scoped>

</style>