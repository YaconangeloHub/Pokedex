<script setup lang="ts">
import Header from './components/Header.vue'
import Body from './components/Body.vue'
import {ref} from 'vue'

// Definir una referencia para el nombre del Pokémon
const pokemonName = ref<string>('')

// Manejar el evento emitido por Header.vue
function handleSearchPokemon(name: string) {
  pokemonName.value = name
}

/*const state = reactive({
  pokemonData: {
    name: '',
    sprites: {
      front_default: ''
    },
    types: [],
    stats: []
  },
  pokemonID: '1'
})

async function axiosPokemonData() {
  try {
    const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${state.pokemonID}`)
    state.pokemonData = response.data
    console.log(response.data)
  } catch (error) {
    console.error('Error buscando datos del Pokemon', error)
  }
}

function updatePokemonID(newID: string) {
  state.pokemonID = newID
  axiosPokemonData()
}

onMounted(() => {
  axiosPokemonData()
}) */
</script>

<template>
  <div>
    <Header @searchPokemon="handleSearchPokemon" />
    <Body :pokemonName="pokemonName" />
  </div>
</template>

<style scoped>
</style>
