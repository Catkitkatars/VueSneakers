<script setup lang="ts">
import Header from "@/components/Header.vue";
import axios from 'axios';
import CardList from "@/components/CardList.vue";
import Drawer from "@/components/Drawer.vue";
import {onMounted, reactive, ref, watch, provide, computed} from "vue";

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

const onClickAddPlus = (item) => {
  if(!item.isAdded)
  {
    addToCart(item)
  }
  else
  {
    removeFromCart(item)
  }
}

// ==============================
// ==============================

// ==============================
// handled items
// ==============================
const items = ref<Array<object>>([]);

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
});

const fetchItems = async () => {
  try
  {
    const params = {
      sortBy: filters.sortBy,
    }
    if(filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get(
        `https://fb9d01f7a2cfae30.mokky.dev/items2`,
        {
          params,
        }
    );
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false,
    }));
  }
  catch (err)
  {
    console.log(err);
  }
}
const fetchFavorites = async () => {
  try
  {
    const { data: favorites } = await axios.get(`https://fb9d01f7a2cfae30.mokky.dev/favorites`);

    items.value = items.value.map(item => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)

      if(!favorite) {
        return item;
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      }
    });
  }
  catch (err)
  {
    console.log(err);
  }
};
const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value;
}
const onChangeSelect = (event) => {
  filters.sortBy = event.target.value;
}
const addToFavorite = async (item: object) => {
    try
    {
      if(!item.isFavorite)
      {
        item.isFavorite = true;
        const obj = {
          parentId: item.id
        }
        const { data } = await axios.post(`https://fb9d01f7a2cfae30.mokky.dev/favorites`, obj);
        item.favoriteId = data.id;
      }
      else
      {
        item.isFavorite = false;
        await axios.delete(`https://fb9d01f7a2cfae30.mokky.dev/favorites/${item.favoriteId}`);
        item.favoriteId = null;
      }
    }
    catch (err)
    {
      console.log('Response error');
    }

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
onMounted(() => {
  fetchItems();
  fetchFavorites();
});
watch(filters, () => {
  fetchItems();
  fetchFavorites();
});

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
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
        <div class="flex gap-4">
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option value="name">По названию</option>
            <option value="-price">По цене(дорогие)</option>
            <option value="price">По цене(дешевые)</option>
          </select>
          <div class="relative">
            <img class="absolute left-3 top-3" src="/search.svg"/>
            <input @input="onChangeSearchInput" class="border rounded-md py-2 pl-10 pr-4 outline-none focus: border-gray-400" placeholder="Поиск...">
          </div>
        </div>
      </div>
      <div class="mt-10">
        <CardList
            :items="items"
            @add-to-cart="onClickAddPlus"
            @add-to-favorite="addToFavorite"
        />
      </div>

    </div>

  </div>
</template>

<style scoped>

</style>
