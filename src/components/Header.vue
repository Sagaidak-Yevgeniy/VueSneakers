<template>
        <header class="flex justify-between border-b border-slate-200 px-8 py-8">

            <div class="flex items-center gap-4">
                <img src="/logo.png" alt="image" class="w-10">
                <div>
                    <h2 class="text-xl font-bold uppercase">VUE SNEAKERS</h2>
                    <p class="text-slate-400">Магазин лучших кроссовок</p>
                </div>
            </div>

            <ul class="flex items-center gap-10">
                <li class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black">
                    <img src="/cart.svg" alt="">
                    <b>12205 тенге</b>
                </li>
                <li class="flex items-center cursor-pointer gap-3  text-gray-500  hover:text-black"> 
                    <img src="/heart.svg" alt="">
                    <span>Закладки</span>
                </li>
                <li class="flex items-center cursor-pointer gap-3  text-gray-500  hover:text-black">
                    <img src="/profile.svg" alt="">
                    <span>Профиль</span>
                </li>
            </ul>

        </header>
</template>