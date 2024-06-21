<script setup lang="ts">
import { ref } from 'vue'

// Definir una referencia para el nombre del Pokémon
const pokemonName = ref<string>('')

// Definir la función para emitir el evento
const emit = defineEmits(['searchPokemon'])

function searchPokemon() {
  emit('searchPokemon', pokemonName.value)
}
</script>

<template>
      <div class="bg-blue-400 flex flex-row  items-center h-24 px-4">
        <div class="flex justify-start items-center">
    <img src="../assets/Pokédex_logo.png" class="h-10">
  </div>
        <div class="flex flex-row justify-center items-center h-full ml-24">
    <input v-model="pokemonName" type="text" placeholder="Nombre del Pokémon" class="p-2 rounded-lg border-2 border-black" />
    <button @click="searchPokemon" class="p-2 ml-2 bg-white rounded-lg border-2 border-black">Buscar</button>
  </div>
  </div>
</template>

<style scoped>
</style>
