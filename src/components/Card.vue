<script setup>
defineProps({
  // Идентификатор товара
  id: Number,
  // URL изображения товара
  imageUrl: String,
  // Название товара
  title: String,
  // Цена товара
  price: Number,
  // Флаг: добавлен ли товар в избранное
  isFavorite: Boolean,
  // Флаг: добавлен ли товар в корзину
  isAdded: Boolean,
  // Функция обработки нажатия кнопки "Добавить в корзину"
  onclickAdd: Function,
  // Функция обработки нажатия кнопки "Добавить в избранное"
  onclickFavorite: Function
})
</script>

<template>
  <div
    class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer transition-all duration-300 hover:-translate-y-2 hover:shadow-xl"
  >
    <!-- Иконка "Избранное" -->
    <img
      @click="onclickFavorite"
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="like 1"
      class="absolute top-8 left-8"
    />
    <!-- Изображение товара -->
    <img :src="imageUrl" alt="sneakers" />
    <!-- Название товара -->
    <p class="mt-2">{{ title }}</p>

    <!-- Цена и кнопка "Добавить в корзину" -->
    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <!-- Текст "Цена" -->
        <span class="text-slate-400">Цена:</span>
        <!-- Значение цены товара -->
        <b>{{ price }} тенге:</b>
      </div>
      <!-- Иконка "Добавить в корзину" -->
      <img @click="onclickAdd" :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="Plus" />
    </div>
  </div>
</template>
