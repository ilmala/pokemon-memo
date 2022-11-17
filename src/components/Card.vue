<script setup>
defineProps({
    pokemon: Object,
    flipped: Boolean
})
const url = "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork";

</script>

<template>
    <div class="relative w-36 h-44 card-inner rounded-xl shadow-md shadow-blue-200" :class="{'flipped': flipped}">
        <div class="absolute inset-0 bg-white rounded-xl overflow-hidden p-2 card-front">
            <img :src="`${url}/${pokemon.id}.png`" alt="" class="w-full object-cover">
            <p class="absolute inset-x-0 bottom-0 py-1 font-bold text-center text-sm text-blue-800 bg-blue-200/20">{{ pokemon.name }}</p>
        </div>

        <div class="absolute inset-0 flex flex-col justify-center items-center bg-pattern rounded-xl card-back">
            <p class="font-bold text-5xl text-blue-200">P</p>
            <p class="text-sm text-blue-300">Pokemon</p>
        </div>
    </div>
</template>