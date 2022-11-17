<script setup>
import { onMounted, ref, computed } from "vue";
import Card from './components/Card.vue';
import logo from './assets/pokemon-logo.png'
import {shuffle} from 'lodash';

const pokemons = [
  { id: 1, name: "bulbasaur" },
  { id: 8, name: "wartortle" },
  { id: 9, name: "blastoise" },
  { id: 6, name: "charizard" },
  { id: 7, name: "squirtle" },
  { id: 25, name: "pikachu" },
  { id: 26, name: "raichu" },
  { id: 22, name: "fearow" },
];

const pairOfPokemons = ref([])
const picked = ref([])
const firstPick = ref(null)
const secondPick = ref(null)
const founded = ref([])

const score = ref(0)
const moves = ref(0)
const over = ref(false)

onMounted(()=>{
  pairOfPokemons.value = createPairOfPokemon()
})

const createPairOfPokemon = () => {
  return shuffle([...pokemons, ...pokemons])
}

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

  if(founded.value.length === 8){
    setTimeout(()=>{
      over.value = true;
    }, 1000)
  }
}

const handlePlayAgain = () => {
  picked.value = []
  founded.value = []
  score.value = 0
  moves.value = 0
  pairOfPokemons.value = createPairOfPokemon()
  over.value = false
}

const resetPickedCard = () => {
  picked.value = []
  firstPick.value = null
  secondPick.value = null
}

</script>

<template>
<main>
  <Transition 
  enter-from-class="opacity-0"
  enter-active-class="duration-300 ease-out"
  enter-to-class="opacity-100"
  leave-from-class="opacity-100"
  leave-active-class="duration-200 ease-in"
  leave-to-class="opacity-0"
  >
    <div v-if="over" class="fixed z-10 inset-0 overflow-x-hidden overflow-y-auto flex justify-center items-center">
      <div class="absolute inset-0 bg-zinc-800/60" @click.prevent="handlePlayAgain"></div>
      <div class="relative bg-white w-full max-w-sm rounded-xl shadow-lg shadow-zinc-600/40">
        <h3 class="p-4 font-bold text-2xl text-blue-700 text-center">Game Over</h3>
        <div class="p-4 text-center bg-blue-700">
          <p class="text-white">Your score is: {{ score }}</p>
          <p class="text-white">Maked Moves: {{ moves }}</p>
        </div>
        <div class="p-4 flex justify-center">
            <button @click.prevent="handlePlayAgain" class="px-4 py-2 bg-blue-600 hover:bg-blue-700 font-semibold text-white text-sm rounded-xl shadow-md shadow-blue-600/40 transition duration-150">
              Play Again
            </button>
          </div>
      </div>
    </div>
  </Transition> 

  <div class="relative h-screen flex justify-center items-center">
    <div class="py-4">
      <h2 class="font-bold text-3xl text-blue-800 text-center flex flex-col items-center">
        <img :src="logo" alt="Pokemon memo" class="h-20">
        <span>MEMO</span>
      </h2>
      
      <div class="mt-8 flex justify-center items-center space-x-4">
        <p class="text-blue-800">Score: {{ score }}</p>
        <p class="text-blue-800">Moves: {{ moves }}</p>
      </div>

      <div class="mt-4 grid grid-cols-4 gap-4 bg-blue-200 p-4 rounded-xl">
        <div v-for="(pokemon, index) in pairOfPokemons" :key="index" class="card cursor-pointer">
          <Card 
            :pokemon="pokemon" 
            :flipped="isFlipped(index)" 
            @click.prevent="handleFlip(index)" />
        </div>
      </div>
    </div>
  </div>
</main>
</template>
