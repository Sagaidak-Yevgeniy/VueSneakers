<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import CardList from '../components/CardList.vue';



const favorites = ref([])

onMounted( async () => {
   try {
    const { data } = await axios.get('https://c62d441103464330.mokky.dev/favorites')
   
    favorites.value = data.map(obj => obj.item) 
} catch (error) {
    console.log(error)
   }
  
})

const emit = defineEmits(['addToFavorite', 'addToCart', 'onClickaddToPlus'])
</script>

<template>
  <h2 class="text-3xl font-bold mb-8" v-auto-animate>Мои закладки</h2>

  <CardList :items="favorites" @add-to-favorite="addToFavorite" @add-to-cart="onClickaddToPlus" />
</template>


<style scoped>

</style>