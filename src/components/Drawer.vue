<script setup>
import InfoBlock from './InfoBlock.vue'
import DrawerHeader from './DrawerHeader.vue' // импорт компонента заголовка боковой панели
import CartListItem from './CartListItem.vue'
import { ref, computed, inject } from 'vue'
import axios from 'axios'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
  isCreatedOrder: Boolean
})

const { cart } = inject('cart')

const isCreatedOrder = ref(false)
const orderId = ref(null)

const btnDisable = computed(() =>
  props.isCreatedOrder.value ? true : props.totalPrice ? false : true
)

const createOrder = async () => {
  try {
    isCreatedOrder.value = true
    const { data } = await axios.post('https://c62d441103464330.mokky.dev/orders', {
      items: cart.value,
      totalPrice: props.totalPrice
    })
    cart.value = []
   
    orderId.value = data.id
  } catch (e) {
    console.log(e)
  } finally {
    isCreatedOrder.value = false
  }
}
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>

  <div class="bg-white w-96 h-full fixed top-0 right-0 z-20 p-8">
    <DrawerHeader />
    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пуста"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ"
        imageUrl="/package-icon.png"
      />
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен"
        :description="`Ваш заказ № ${orderId} успешно оформлен и будет передан курьерской службе`"
        imageUrl="/order-success-icon.png"
      />
    </div>
    <div v-else>
      <CartListItem />

      <div v-if="totalPrice" class="flex flex-col gap-4 mt-8">
        <div class="flex gap-2">
          <span>ИТОГО:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} тенге</b>
        </div>
        <div class="flex gap-2">
          <span>Налог 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ vatPrice }} тенге</b>
        </div>

        <button
          :disabled="btnDisable"
          @click="createOrder"
          class="mt-4 transition bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 cursor-pointer hover:bg-lime-600 active:bg-lime-700"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
