<script setup>
const emit = defineEmits(['toggleDrawer']) // Определяем функцию для передачи события toggleDrawer
</script>

<template>
  <header class="flex justify-between border-b border-slate-200 px-8 py-8">
    <!-- Верхняя часть страницы с рамкой и отступами -->
    <div class="flex items-center gap-4">
      <!-- Контейнер с логотипом и текстом -->
      <img src="/logo.png" alt="image" class="w-10" />
      <!-- Логотип магазина -->
      <div>
        <h2 class="text-xl font-bold uppercase">VUE SNEAKERS</h2>
        <!-- Название магазина -->
        <p class="text-slate-400">Магазин лучших кроссовок</p>
        <!-- Описание магазина -->
      </div>
    </div>

    <ul class="flex items-center gap-10">
      <!-- Список элементов справа -->
      <li
        @click="() => emit('toggleDrawer')"
        class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black"
      >
        <img src="/cart.svg" alt="" />
        <!-- Иконка корзины -->
        <b>12205 тенге</b>
        <!-- Цена товаров в корзине -->
      </li>
      <li class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black">
        <!-- Элемент "Закладки" -->
        <img src="/heart.svg" alt="" />
        <!-- Иконка сердца -->
        <span>Закладки</span>
        <!-- Текст элемента -->
      </li>
      <li class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black">
        <!-- Элемент "Профиль" -->
        <img src="/profile.svg" alt="" />
        <!-- Иконка профиля -->
        <span>Профиль</span>
        <!-- Текст элемента -->
      </li>
    </ul>
  </header>
</template>
