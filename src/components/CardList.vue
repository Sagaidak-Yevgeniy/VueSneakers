<script setup>
// Импортируем компонент Card.vue и функцию inject из Vue
import Card from './Card.vue'
// import { inject } from 'vue'

// Определяем пропсы (свойства) компонента
defineProps({
  items: Array // Массив элементов, которые будут отображаться
})

// Определяем функцию для генерации событий (emit) addToFavorite и addToCart
const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>

<template>
  <!-- Обертка для карточек, используется сетка с четырьмя колонками -->
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <!-- Используем компонент Card для отображения каждого элемента в массиве items -->
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :isFavorite="item.isFavorite"
      :isAdded="item.isAdded"
      :onclickAdd="() => emit('addToCart', item)"
      :onclickFavorite="() => emit('addToFavorite', item)"
    />
  </div>
</template>
