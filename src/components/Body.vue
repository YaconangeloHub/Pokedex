<script setup lang="ts">
import { ref, watch, defineProps } from 'vue';
import axios from 'axios';

// imports de iconos de la carpeta icons en assets
import normalIcon from '../assets/icons/normal.svg';
import fireIcon from '../assets/icons/fire.svg';
import waterIcon from '../assets/icons/water.svg';
import grassIcon from '../assets/icons/grass.svg';
import electricIcon from '../assets/icons/electric.svg';
import iceIcon from '../assets/icons/ice.svg';
import fightingIcon from '../assets/icons/fighting.svg';
import poisonIcon from '../assets/icons/poison.svg';
import groundIcon from '../assets/icons/ground.svg';
import flyingIcon from '../assets/icons/flying.svg';
import psychicIcon from '../assets/icons/psychic.svg';
import bugIcon from '../assets/icons/bug.svg';
import rockIcon from '../assets/icons/rock.svg';
import ghostIcon from '../assets/icons/ghost.svg';
import dragonIcon from '../assets/icons/dragon.svg';
import darkIcon from '../assets/icons/dark.svg';
import steelIcon from '../assets/icons/steel.svg';
import fairyIcon from '../assets/icons/fairy.svg';

// Definir las props
const props = defineProps<{ pokemonName: string }>();

//importamos el fondo
import fondo from '../assets/Pokedex_fondo.png';

// Definir una referencia para los datos del Pokémon
const pokemonData = ref<any>(null);

const capitalize = (str: string): string => {
  return str.replace(/\b\w/g, char => char.toUpperCase());
};


const typeColors: { [key: string]: string } = {
  normal: 'bg-gray-400',
  fire: 'bg-orange-500',
  water: 'bg-blue-500',
  grass: 'bg-green-500',
  electric: 'bg-yellow-400',
  ice: 'bg-blue-200',
  fighting: 'bg-red-700',
  poison: 'bg-purple-500',
  ground: 'bg-yellow-600',
  flying: 'bg-indigo-400',
  psychic: 'bg-pink-500',
  bug: 'bg-green-700',
  rock: 'bg-gray-700',
  ghost: 'bg-indigo-700',
  dragon: 'bg-indigo-800',
  dark: 'bg-gray-900',
  steel: 'bg-gray-500',
  fairy: 'bg-pink-300',
  stellar: 'bg-teal-300'
};

const typeIcons: { [key: string]: string } = {
  normal: normalIcon,
  fire: fireIcon,
  water: waterIcon,
  grass: grassIcon,
  electric: electricIcon,
  ice: iceIcon,
  fighting: fightingIcon,
  poison: poisonIcon,
  ground: groundIcon,
  flying: flyingIcon,
  psychic: psychicIcon,
  bug: bugIcon,
  rock: rockIcon,
  ghost: ghostIcon,
  dragon: dragonIcon,
  dark: darkIcon,
  steel: steelIcon,
  fairy: fairyIcon,
};

// Usar watch para obtener la información del Pokémon cuando cambie el nombre
watch(() => props.pokemonName, async (newName) => {
  if (newName) {
    try {
      const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${newName.toLowerCase()}`);
      pokemonData.value = response.data;
      console.log(pokemonData.value);
    } catch (error) {
      console.error('Error al obtener los datos del Pokémon:', error);
      pokemonData.value = null;
    }
  }
});
  
// Función para establecer el color de fondo gradiente
const setCardColor = (): string => {
  const types = pokemonData.value.types.map((type: { type: { name: string } }) => type.type.name);
  const colorOne = typeColors[types[0]];
  const colorTwo = types[1] ? typeColors[types[1]] : typeColors[types[0]];
  return `radial-gradient(circle, ${colorOne} 50%, ${colorTwo} 20%)`;
};

// Función para obtener el color de fondo del nombre del Pokémon
const getNameBackgroundColor = (): string => {
  if (pokemonData.value) {
    const types = pokemonData.value.types.map((type: { type: { name: string } }) => type.type.name);
    return typeColors[types[0]];
  }
  return 'bg-white';
};
</script>

<template>
  <div class="bg-cover bg-center flex flex-col justify-start items-center h-screen" :style="{ backgroundImage: `url(${fondo})` }">
    <div class="p-4">
      <p v-if="!pokemonData" class="text-center mt-4 bg-white border-4 border-solid font-bold border-black">Ingresa el nombre de un Pokémon y presiona "Buscar".</p>
      <div v-if="pokemonData" class="mt-4 flex flex-col space-y-2 items-center">
        <h2 :class="['text-7xl border-4 border-black rounded-lg font-bold flex justify-center items-center font-serif italic', getNameBackgroundColor()]">{{ capitalize(pokemonData.name) }}</h2>
        <div :class="setCardColor()" class="relative w-64 h-64 rounded-full flex justify-center items-center overflow-hidden border-4 border-solid border-black">
          <img :src="pokemonData.sprites.front_default" :alt="`Imagen de ${capitalize(pokemonData.name)}`" class="w-48 h-48 object-contain" />
        </div>
        <ul class="space-y-5 w-full px-4 ">
          <h2 class="text-5xl font-medium text-center  border-4 border-black rounded-lg font-bold italic bg-white"><b>Tipo de Pokemon:</b></h2>
          <li v-for="type in pokemonData.types" :key="type.slot" class="flex items-center space-x-2">
            <span :class="[typeColors[type.type.name] || 'bg-gray-200', 'p-3 text-3xl flex items-center space-x-2 border rounded-md italic font-mono w-full']">
              <img :src="typeIcons[type.type.name]" :alt="`${capitalize(type.type.name)} icon`" class="w-8 h-8" />
              <span>{{ capitalize(type.type.name) }}</span>
            </span>
          </li>
        </ul>
        <ul class="text-2xl space-y-2 mt-9 w-full px-4  border-4 border-black rounded-lg font-bold italic bg-white">
          <h2 class="text-5xl font-medium text-center "><b><u>Stats:</u></b></h2>
          <li v-for="stat in pokemonData.stats" :key="stat.stat.name">
            <span class="oldstyle-nums">{{ capitalize(stat.stat.name) }} : {{ stat.base_stat }}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Estilos adicionales si es necesario */
</style>
