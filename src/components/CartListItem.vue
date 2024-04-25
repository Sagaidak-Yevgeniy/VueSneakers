<script setup>
// Импортируем функцию inject из Vue
import { inject } from 'vue'
// Импортируем компонент CartItem из файла CartItem.vue
import CartItem from './CartItem.vue'

// Используем функцию inject для получения доступа к контексту приложения и извлечения значения cart

const { cart, removeFromCart} = inject('cart')


</script>

<template>
  <!-- Главный контейнер с флексбокс-раскладкой и отступами между элементами -->
  <div class="flex flex-col flex-1 gap-4 justify-between" v-auto-animate>
    <!-- Итерируемся по каждому товару в корзине и рендерим компонент CartItem -->
    <CartItem
      v-for="item in cart"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      @on-click-remove="() => removeFromCart(item)"
     
    />
  </div>
</template>
