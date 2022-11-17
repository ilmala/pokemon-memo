<script setup>
import { onMounted, ref, computed } from "vue";
import Card from './components/Card.vue';
import {shuffle} from 'lodash';

const pokemons = [
  { id: 1, name: "Balbosur" },
  { id: 8, name: "Wartotle" },
  { id: 9, name: "Blasoise" },
  { id: 6, name: "Charizard" },
  { id: 7, name: "Squirtle" },
  { id: 25, name: "Pikachu" },
];

const pairOfPokemons = ref(shuffle([...pokemons, ...pokemons]))
const picked = ref([])
const firstPick = ref(null)
const secondPick = ref(null)
const founded = ref([])

const score = ref(0)
const moves = ref(0)

const isFlipped = (index) => {
  const pokemonId = pairOfPokemons.value[index]?.id
  return picked.value.includes(index) || founded.value.includes(pokemonId)
}

const handleFlip = (index) => {
  if(picked.value.includes(index) || secondPick.value) return;

  picked.value.push(index)

  firstPick.value = pairOfPokemons.value[picked.value[0]]?.id
  secondPick.value = pairOfPokemons.value[picked.value[1]]?.id

  if(!secondPick.value) return;

  if(firstPick.value === secondPick.value) {
    founded.value.push(firstPick.value)
    resetPickedCard()
    score.value += 10
  }else{
    setTimeout(()=>resetPickedCard(), 1000)
  }

  moves.value += 1;
}

const resetPickedCard = () => {
  picked.value = []
  firstPick.value = null
  secondPick.value = null
}

</script>

<template>
  <div class="h-screen flex justify-center items-center">
    <div class="py-4">
      <h2 class="font-bold text-5xl text-blue-800 text-center">Pokemon Memo</h2>
      
      <div class="mt-8 flex justify-center items-center space-x-4">
        <p class="text-blue-800">Score: {{ score }}</p>
        <p class="text-blue-800">Moves: {{ moves }}</p>
      </div>

      <div class="mt-4 grid grid-cols-4 gap-4">
        <div v-for="(pokemon, index) in pairOfPokemons" :key="index" class="card cursor-pointer">
          <Card 
            :pokemon="pokemon" 
            :flipped="isFlipped(index)" 
            @click.prevent="handleFlip(index)" />
        </div>
      </div>
    </div>
  </div>

</template>
