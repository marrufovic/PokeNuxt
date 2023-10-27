<template>
  <div class="home-container">
    <div>
      <input type="text" v-model="searchQuery" placeholder="Search Pokémon" />
    </div>
    <h2>Pokémon List</h2>
    <ul class="pokemon-list">
      <li v-for="pokemon in filteredPokemons" :key="pokemon.name">
        <NuxtLink :to="`/${pokemon.name}`" class="pokemon-link">
          <div class="pokemon-item">
            <div class="pokemon-image">
              <img
                :src="pokemon.spriteUrl"
                :alt="pokemon.name"
                class="normal-sprite"
              />
              <img
                :src="pokemon.shinySpriteUrl"
                :alt="pokemon.name"
                class="shiny-sprite"
              />
            </div>
            <div class="pokemon-details">
              <h3>{{ "#" + pokemon.id + " " + pokemon.name.toUpperCase() }}</h3>
            </div>
          </div>
        </NuxtLink>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";

const searchQuery = ref<string>("");
const pokemons: Pokemon[] = [];

interface Pokemon {
  id: number;
  name: string;
  spriteUrl: string;
  shinySpriteUrl: string;
}

interface PokemonResult {
  name: string;
  url: string;
}

// Filter Pokemon list base on the search query
const filteredPokemons = computed(() => {
  const query = searchQuery.value.toLowerCase();
  return pokemons.filter((pokemon) =>
    pokemon.name.toLowerCase().includes(query)
  );
});

// Fetch 60 Pokemons
const response = await fetch(
  "https://pokeapi.co/api/v2/pokemon/?offset=0&limit=60"
);
const data: { results: PokemonResult[] } = await response.json();
const pokemonResults = data.results;

// Add Pokemons to data to pokemons array
pokemons.push(
  ...pokemonResults.map((pokemon: PokemonResult) => {
    const name = pokemon.name;
    const url = pokemon.url;
    const id = parseInt(url.split("/").slice(-2, -1)[0], 10);

    const spriteUrl = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`;
    const shinySpriteUrl = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/shiny/${id}.png`;

    return {
      id,
      name,
      spriteUrl,
      shinySpriteUrl,
    };
  })
);
</script>

<style scoped>
.home-container {
  max-width: 960px;
  margin: 0 auto;
  padding: 1rem;
}

h2 {
  font-size: 24px;
  margin-bottom: 1rem;
  text-align: center;
  font-family: "Open Sans", sans-serif;
}

.pokemon-list {
  list-style: none;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  max-width: 100%;
}

.pokemon-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 1rem;
  border: 1px solid #e0e0e0;
  border-radius: 5px;
  padding: 1rem;
  background-color: #0f0c0c;
  width: 200px;
  font-family: "Open Sans", sans-serif;
}

.pokemon-image {
  width: 100px;
  height: auto;
  margin-bottom: 1rem;
}

.pokemon-details {
  flex: 1;
}

.pokemon-link {
  text-decoration: none;
}

h3 {
  font-size: 20px;
  margin: 0;
  color: #e0e0e0;
  text-decoration: none;
}

p {
  font-size: 16px;
  margin: 0;
}

.pokemon-image {
  position: relative;
  width: 100px;
  height: 100px;
}

.normal-sprite {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  opacity: 1;
  transition: opacity 0.3s;
}

.shiny-sprite {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.3s;
}

.pokemon-item:hover .normal-sprite {
  opacity: 0;
}

.pokemon-item:hover .shiny-sprite {
  opacity: 1;
}

input[type="text"] {
  font-size: 18px;
  padding: 10px 15px;
}
</style>
