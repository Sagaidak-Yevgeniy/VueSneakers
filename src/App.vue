<script setup>
import Header from './components/Header.vue' // Импорт компонента заголовка

import Drawer from './components/Drawer.vue' // Импорт компонента выдвижного меню
import { watch, provide, ref, computed } from 'vue' // Импорт необходимых функций и хуков из Vue

/*Корзина START*/
const cart = ref([])

const drawerVisible = ref(false)

const totalPrice = computed(() => cart.value.reduce((sum, item) => sum + item.price, 0))

const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const toggleDrawer = () => {
  drawerVisible.value = !drawerVisible.value // Инвертирование значения видимости
}

const addToCart = (item) => {
  cart.value.push(item) // Добавляем элемент в корзину
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false // Удаляем элемент из корзины
}

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value)) // Сохраняем состояние корзины в localStorage
  },

  { deep: true }
)

provide('cart', {
  toggleDrawer, // Передаем функцию для переключения видимости выдвижного меню
  cart,
  addToCart,
  removeFromCart,
  totalPrice
})
/*Корзина END*/
</script>

<template>
  <Drawer v-if="drawerVisible" :total-price="totalPrice" :vat-price="vatPrice" />

  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header :total-price="totalPrice" @toggle-drawer="toggleDrawer" />

    <div class="p-10">
      <RouterView />
    </div>
  </div>
</template>

<style scoped></style>
