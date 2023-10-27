<template>
  <div class="pokemon-profile">
    <div class="pokemon-header">
      <h1>Pokemon Profile for {{ pokemon.name }}</h1>
    </div>
    <div class="background"></div>
    <div class="pokemon-details">
      <img :src="pokemon.imageUrl" :alt="pokemon.name" class="pokemon-image" />
      <div class="pokemon-info">
        <h1 class="pokemon-name">
          {{ "#" + pokemon.id + " " + pokemon.name.toUpperCase() }}
        </h1>
        <div class="attribute">
          <h2>Height</h2>
          <h3>{{ (pokemon.height / 10).toFixed(2) }} meters</h3>
        </div>
        <div class="attribute">
          <h2>Weight</h2>
          <h3>{{ (pokemon.weight / 10).toFixed(2) }} kilograms</h3>
        </div>
        <div class="attribute">
          <h2>Abilities</h2>
          <h3>{{ formatAbilities(pokemon.abilities) }}</h3>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { useRoute } from "vue-router";
import { ref, computed } from "vue";

const { id } = useRoute().params;

// Create API call to get data
const uri = "https://pokeapi.co/api/v2/pokemon/" + id;
const { data: response } = await useFetch(uri);
const pokemonInfo = ref(response.value);

// Create API call to retrieve image
const imageUrl = computed(
  () =>
    `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemonInfo.value.id}.png`
);

// Format strings
const formatAbilities = (abilities) => {
  return abilities
    .map((ability) => capitalizeFirstLetter(ability.ability.name))
    .join(", ");
};

const capitalizeFirstLetter = (string) => {
  return string.charAt(0).toUpperCase() + string.slice(1);
};

const pokemon = computed(() => {
  const info = pokemonInfo.value;
  return {
    id: info.id,
    name: capitalizeFirstLetter(info.name),
    imageUrl: imageUrl.value,
    height: info.height,
    weight: info.weight,
    abilities: info.abilities,
  };
});

definePageMeta({
  layout: "pokemons",
});
</script>

<style scoped>
.pokemon-profile {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  position: relative;
  color: #000000;
}

.background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url("../static/images/pokefield.jpeg");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  filter: blur(5px);
  z-index: -1;
}

.pokemon-header {
  margin-bottom: 20px;
}

.pokemon-details {
  display: flex;
  align-items: center;
  background-color: rgba(255, 255, 255, 0.3);
  padding-right: 50px;
}

.pokemon-image {
  width: 500px !important;
  height: 500px !important;
  margin-right: 20px;
}

.pokemon-info {
  display: flex;
  flex-direction: column;
}

.pokemon-name {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 10px;
}

.attribute {
  margin-bottom: 10px;
}

h3 {
  font-weight: bold;
  margin-bottom: 5px;
  font-family: "Open Sans", sans-serif;
  text-shadow: 3px 3px 5px rgb(255, 255, 255);
  text-rendering: pixelated;
}

h2 {
  font-weight: bold;
  font-family: "Open Sans", sans-serif;
  text-shadow: 3px 3px 5px rgb(255, 255, 255);
  text-rendering: pixelated;
}

h1 {
  font-weight: bold;
  font-family: "Open Sans", sans-serif;
  text-shadow: 3px 3px 5px rgb(255, 255, 255);
  text-rendering: pixelated;
}
</style>
