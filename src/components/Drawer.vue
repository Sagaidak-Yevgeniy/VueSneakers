<script setup>
import DrawerHeader from './DrawerHeader.vue' // импорт компонента заголовка боковой панели
import CartListItem from './CartListItem.vue' // импорт компонента элемента списка корзины
</script>

<template>
  <!-- Полупрозрачный фон -->
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <!-- Боковая панель корзины -->
  <div class="bg-white w-96 h-full fixed top-0 right-0 z-20 p-8">
    <DrawerHeader />
    <!-- Вставка компонента заголовка -->
    <CartListItem />
    <!-- Вставка компонента элемента списка корзины -->

    <!-- Итоговая сумма и налог -->
    <div class="flex flex-col gap-4 mt-8">
      <div class="flex gap-2">
        <span>ИТОГО:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>120 000 тенге</b>
      </div>
      <div class="flex gap-2">
        <span>Налог 5%:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>120 000 тенге</b>
      </div>

      <!-- Кнопка оформления заказа -->
      <button
        disabled
        class="mt-4 transition bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 cursor-pointer hover:bg-lime-600 active:bg-lime-700"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>
