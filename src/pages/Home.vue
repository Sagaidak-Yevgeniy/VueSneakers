<script setup>
import { ref, reactive, watch, onMounted } from 'vue'
import { inject } from 'vue'
import axios from 'axios'
import CardList from '../components/CardList.vue'
import debounce from 'lodash.debounce'

const { cart, addToCart, removeFromCart } = inject('cart')

const items = ref([])

const filters = reactive({
  sortBy: 'title', // Свойство для хранения параметра сортировки по умолчанию (по заголовку)
  searchQuery: '' // Свойство для хранения поискового запроса по умолчанию (пустой)
})

const onClickaddToPlus = (item) => {
  if (!item.isAdded) {

    addToCart(item)
  } else {

    removeFromCart(item)
  }
 
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value // Устанавливаем значение параметра сортировки в соответствии с выбором пользователя
}

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value // Устанавливаем значение поискового запроса в соответствии с введенным пользователем
}, 300)

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

const fetchFavorites = async () => {
  try {
    // Отправляем GET-запрос для получения избранных элементов
    const { data: favorites } = await axios.get(`https://c62d441103464330.mokky.dev/favorites`)
    // Обновляем массив элементов для отметки избранных
    items.value = items.value.map((item) => {
      // Проверяем, является ли текущий элемент избранным
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)
      if (!favorite) {
        // Если элемент не избранный, возвращаем его без изменений
        return item
      }
      // Если элемент избранный, обновляем его свойства
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (error) {
    console.log(error) // Выводим ошибку в консоль, если запрос не удался
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy // Создаем объект параметров запроса с указанием параметра сортировки
    }
    // Если есть поисковой запрос, добавляем его в объект params
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    // Отправляем GET-запрос для получения элементов с указанными параметрами
    const { data } = await axios.get(`https://c62d441103464330.mokky.dev/items`, { params })

    // Обновляем массив элементов полученными данными, инициализируем свойства isFavorite и isAdded
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (error) {
    console.log(error) // Выводим ошибку в консоль, если запрос не удался
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems() // Получаем элементы
  await fetchFavorites()

  items.value = items.value.map((item) => ({
    // Мапим элементы в items.value
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  })) // Пол
})

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

watch(filters, fetchItems)
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
    <div class="flex gap-4">
      <!-- Выпадающий список сортировки -->
      <select
        @change="onChangeSelect"
        id=""
        class="border border-slate-200 rounded-md px-3 py-2 outline-none"
      >
        <option value="name">По названию</option>
        <option value="price">По цене(возрастанию)</option>
        <option value="-price">По цене(убыванию)</option>
      </select>

      <!-- Поле ввода для поиска -->
      <div class="relative">
        <img src="/search.svg" alt="Search" class="w-5 h-5 absolute left-4 top-3" />
        <input
          @input="onChangeSearchInput"
          type="text"
          placeholder="Поиск..."
          class="border border-slate-200 rounded-md px-4 py-2 pl-11 pr-4 outline-none focus:border-gray-400"
        />
      </div>
    </div>
  </div>

  <!-- Выводим список карточек с элементами -->
  <div class="mt-10">
    <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickaddToPlus" />
    <!-- Передаем массив элементов и слушаем события добавления в избранное и в корзину -->
  </div>
</template>
