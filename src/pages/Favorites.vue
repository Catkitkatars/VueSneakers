<script setup lang="ts">
  import axios  from "axios";
  import {inject, onMounted, ref} from 'vue';
  import CardList from "@/components/CardList.vue";

  const favorites = ref([]);

  onMounted( async () => {
    try
    {
        const { data } = await axios.get('https://fb9d01f7a2cfae30.mokky.dev/favorites?_relations=items')

        favorites.value = data.map((obj) => obj.item);
    }
    catch (err)
    {
      console.log(err)
    }
  })

</script>

<template>

  <h1>Favorites</h1>
  <CardList
      :items="favorites"

  />
</template>

<style scoped>

</style>