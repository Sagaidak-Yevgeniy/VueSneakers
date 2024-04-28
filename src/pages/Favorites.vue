<script setup>
import { ref, onMounted, inject } from 'vue';
import axios from 'axios';
import CardList from '../components/CardList.vue';

const { addToCart} = inject('cart')

const favorites = ref([])


onMounted( async () => {
   try {
    const { data } = await axios.get('https://c62d441103464330.mokky.dev/favorites')
   
    favorites.value = data.map(obj => obj.item) 
} catch (error) {
    console.log(error)
   }
  
})


const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id,
        item
      }
      item.isFavorite = true
      const { data } = await axios.post(`https://c62d441103464330.mokky.dev/favorites`, obj) // Отправляем POST-запрос для добавления элемента в избранное

      item.favoriteId = data.id // Присваиваем элементу идентификатор избранного
    } else {
      // Если элемент уже является избранным
      item.isFavorite = false // Устанавливаем флаг isFavorite в false

      await axios.delete(`https://c62d441103464330.mokky.dev/favorites/${item.favoriteId}`) // Отправляем DELETE-запрос для удаления элемента из избранного
      item.favoriteId = null // Сбрасываем идентификатор избранного
    }
  } catch (error) {
    console.log(error) // Выводим ошибку в консоль, если запрос не удался
  }
}

const onClickaddToPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const removeFromCart = (item) => {
    favorites.value.splice(favorites.value.indexOf(item), 1)
  item.isAdded = false 
  console.log(favorites)
  
}

</script>

<template>
  <h2 class="text-3xl font-bold mb-8" v-auto-animate>Мои закладки</h2>


  <CardList :items="favorites" @add-to-favorite="addToFavorite" @add-to-cart="onClickaddToPlus" @remove-from-cart="removeFromCart" />
</template>


<style scoped>

</style>