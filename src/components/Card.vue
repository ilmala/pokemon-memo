<script setup>
import frontImage from '../assets/pokemon-logo-bn.png'
defineProps({
    pokemon: Object,
    flipped: Boolean
})
const url = "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork";

</script>

<template>
    <div class="relative w-24 md:w-36 h-32 md:h-44 card-inner rounded-xl shadow-md shadow-blue-300" :class="{'flipped': flipped}">
        <div class="absolute inset-0 bg-white rounded-xl overflow-hidden p-2 card-front">
            <img :src="`${url}/${pokemon.id}.png`" alt="" class="w-full object-cover">
            <p class="absolute inset-x-0 bottom-0 py-1 font-bold text-center text-sm text-blue-800 bg-blue-200/20">{{ pokemon.name }}</p>
        </div>

        <div class="absolute inset-0 flex flex-col justify-center items-center bg-pattern rounded-xl card-back">
            <img :src="frontImage" alt="" class="h-14 md:h-20 mix-blend-darken opacity-70">
        </div>
    </div>
</template>