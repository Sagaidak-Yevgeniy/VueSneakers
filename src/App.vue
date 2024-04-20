<script setup>
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'
import { onMounted, watch, reactive, ref } from 'vue'
import axios from 'axios'

const items = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

// const fetchFavorites = async () => {
//   try {
//     const { data: favorites } = await axios.get(`https://c62d441103464330.mokky.dev/favorites`)
//       items.value = items.value.map(item => {
      
//         const favorite = favorites.find(favorite => favorite.parentId === item.id);
//       if (!favorite) {
//         return item;
//       }
//       return {
//         ...item,
//         isFavorite: true,
//         favoriteId: favorite.id
//         };
//     });
//     console.log(items.value)
//   } catch (error) {
//     console.log(error)
//   }
// }

const addToFavorite = async (item) => {
        item.isFavorite = true;
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get(`https://c62d441103464330.mokky.dev/items`, { params })
   
    items.value = data.map(obj=>({
      ...obj,
      isFavorite: false,
      isAdded: false
    }))
  } catch (error) {
    console.log(error)
  }
}
onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})
watch(filters, fetchItems)
</script>

<template>
  <!-- <Drawer /> -->
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
        <div class="flex gap-4">
          <select
            @change="onChangeSelect"
            id=""
            class="border border-slate-200 rounded-md px-3 py-2 outline-none"
          >
            <option value="name">По названию</option>
            <option value="price">По цене(возрастанию)</option>
            <option value="-price">По цене(убыванию)</option>
          </select>

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

      <div class="mt-10">
        <CardList :items="items" />
      </div>
    </div>
  </div>
</template>

<style scoped></style>
