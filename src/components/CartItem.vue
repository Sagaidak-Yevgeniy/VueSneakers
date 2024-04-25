<script setup>
const emit = defineEmits(['onClickRemove'])
defineProps({
  id: Number, // Уникальный идентификатор товара (число)
  title: String, // Название товара (строка)
  price: Number, // Цена товара (число)
  imageUrl: String // URL изображения товара (строка)
})
</script>

<template>
  <!-- Внешний контейнер товара -->
  <div class="relative flex w-full border border-slate-100 rounded-xl p-4 gap-4">
    <!-- Изображение товара -->
    <img :src="imageUrl" class="w-16 h-16" :alt="title" />
    <!-- Контейнер для текстовой информации о товаре -->
    <div class="flex flex-col w-full">
      <!-- Название товара -->
      <p>{{ title }}</p>
      <!-- Контейнер для цены и кнопки "Закрыть" -->
      <div class="flex justify-between mt-5">
        <!-- Цена товара -->
        <span class="font-bold">{{ price }} тенге</span>
        <!-- Изображение кнопки "Закрыть" -->
        <img
          @click="emit('onClickRemove')"
          class="cursor-pointer opacity-30 hover:opacity-100 transition"
          src="/close.svg"
          alt="Close"
        />
      </div>
    </div>
  </div>
</template>
