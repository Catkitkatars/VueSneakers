<script setup lang="ts">
import CardList from "@/components/CardList.vue";
import {inject, onMounted, reactive, ref, watch} from 'vue';
import debounce from 'lodash.debounce';
import axios from "axios";

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
});

const items = ref<Array<object>>([]);

const { cart,  addToCart, removeFromCart } = inject('cart');

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value;
}, 500)
const onChangeSelect = (event) => {
  filters.sortBy = event.target.value;
}
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
        `https://fb9d01f7a2cfae30.mokky.dev/items`,
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
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)

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


watch(filters, () => {
  fetchItems();
  fetchFavorites();
});



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
const addToFavorite = async (item: object) => {
  try
  {
    if(!item.isFavorite)
    {
      item.isFavorite = true;
      const obj = {
        item_id: item.id
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

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false,
  }))
})

onMounted(async () => {
  const localCart = localStorage.getItem('cart');
  cart.value = localCart ? JSON.parse(localCart) : [];

  await fetchItems();
  await fetchFavorites();

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
});


</script>

<template>
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
</template>

<style scoped>

</style>