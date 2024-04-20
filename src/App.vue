<script setup>
// Импортируем необходимые компоненты и функции из Vue и Axios
import Header from './components/Header.vue' // Импорт компонента заголовка
import CardList from './components/CardList.vue' // Импорт компонента списка карточек
import Drawer from './components/Drawer.vue' // Импорт компонента выдвижного меню
import { onMounted, watch, reactive, provide, ref } from 'vue' // Импорт необходимых функций и хуков из Vue
import axios from 'axios' // Импорт библиотеки Axios для работы с HTTP-запросами

// Создаем реактивную ссылку для хранения данных о элементах
const items = ref([]) // Создание реактивной ссылки на массив элементов
const cart = ref([]) // Создание реактивной ссылки на массив элементов корзины

const drawerVisible = ref(false) // Создание реактивной ссылки для управления видимостью выдвижного меню

const toggleDrawer = () => {
  // Функция для переключения видимости выдвижного меню
  drawerVisible.value = !drawerVisible.value // Инвертирование значения видимости
}

// Создаем реактивный объект для хранения параметров фильтрации
const filters = reactive({
  sortBy: 'title', // Свойство для хранения параметра сортировки по умолчанию (по заголовку)
  searchQuery: '' // Свойство для хранения поискового запроса по умолчанию (пустой)
})

// Функция для добавления или удаления элемента из корзины
const addToCart = (item) => {
  if (!item.isAdded) {
    // Если элемент не добавлен в корзину
    cart.value.push(item) // Добавляем элемент в корзину
    item.isAdded = true // Устанавливаем флаг isAdded в true
  } else {
    // Если элемент уже добавлен в корзину
    cart.value.splice(cart.value.indexOf(item), 1) // Удаляем элемент из корзины
    item.isAdded = false // Устанавливаем флаг isAdded в false
  }
  console.log(cart) // Выводим содержимое корзины в консоль
}

// Функция для обработки изменений в выпадающем списке сортировки
const onChangeSelect = (event) => {
  filters.sortBy = event.target.value // Устанавливаем значение параметра сортировки в соответствии с выбором пользователя
}

// Функция для обработки изменений в поле ввода поиска
const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value // Устанавливаем значение поискового запроса в соответствии с введенным пользователем
}

// Функция для загрузки избранных элементов с сервера
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
    console.log(items.value) // Выводим массив элементов с отметками избранных в консоль
  } catch (error) {
    console.log(error) // Выводим ошибку в консоль, если запрос не удался
  }
}

// Функция для добавления или удаления элемента из избранного
const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      // Если элемент не является избранным
      const obj = {
        parentId: item.id // Создаем объект для отправки на сервер с идентификатором элемента
      }
      item.isFavorite = true // Устанавливаем флаг isFavorite в true
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

// Функция для загрузки элементов с сервера в соответствии с фильтрами
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

// Функция, выполняемая при монтировании компонента
onMounted(async () => {
  await fetchItems() // Получаем элементы
  await fetchFavorites() // Получаем избранные
})

// Следим за изменениями в объекте filters и запускаем fetchItems соответственно
watch(filters, fetchItems)

provide('cart', {
  toggleDrawer, // Передаем функцию для переключения видимости выдвижного меню
  cart // Передаем массив элементов корзины
})
</script>

<template>
  <Drawer v-if="drawerVisible" />
  <!-- Выводим компонент выдвижного меню в зависимости от значения видимости -->
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header @toggle-drawer="toggleDrawer" />
    <!-- Выводим компонент заголовка и прослушиваем событие toggle-drawer -->
    <!-- Блок фильтрации и поиска -->
    <div class="p-10">
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
        <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="addToCart" />
        <!-- Передаем массив элементов и слушаем события добавления в избранное и в корзину -->
      </div>
    </div>
  </div>
</template>

<style scoped></style>
