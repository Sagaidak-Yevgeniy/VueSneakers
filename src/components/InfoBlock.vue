<template>
    <div class="flex flex-col items-center text-center w-72 mx-auto">
        <img width="80" height="80" :src="imageUrl" alt="info" />
        <h2 class="mt-5 text-2-xl font-medium">{{ title }}</h2>
        <p class="text-grey-400 mt-2">{{ description }}</p>
    </div>
  
</template>

<script setup>
    defineProps({
        title: String,
        description: String,
        imageUrl: String
    })
</script>

<style scoped>

</style>