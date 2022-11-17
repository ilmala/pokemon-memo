<script setup>
import frontImage from '../assets/pokemon-logo-bn.png'
defineProps({
    pokemon: Object,
    flipped: Boolean
})
const url = "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork";

</script>

<template>
    <div class="relative w-20 md:w-36 h-20 md:h-36 card-inner rounded-xl shadow-md shadow-blue-300" :class="{'flipped': flipped}">
        <div class="absolute inset-0 bg-white rounded-xl overflow-hidden pt-1 px-2 md:px-4 card-front">
            <img :src="`${url}/${pokemon.id}.png`" alt="" class="w-full object-cover">
            <p class="absolute inset-x-0 bottom-0 py-0.5 md:py-1 font-semibold capitalize tracking-wider text-center text-xs md:text-base text-blue-800 bg-blue-200/40">{{ pokemon.name }}</p>
        </div>

        <div class="absolute inset-0 flex flex-col justify-center items-center bg-pattern rounded-xl card-back">
            <img :src="frontImage" alt="" class="h-14 md:h-20 mix-blend-darken opacity-70">
        </div>
    </div>
</template>