<script setup>
import { onMounted, reactive, ref, computed } from "vue";
import listPokemons from "../components/listPokemons.vue";
import CardPokemonSelected from "../components/CardPokemonSelected.vue";

let searchPokemonField = ref("");
let urlBaseSvg = ref(
  "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/"
);
let pokemons = reactive(ref());
let pokemonSelected = reactive(ref());
let loading = ref(false);

onMounted(() => {
  fetch("https://pokeapi.co/api/v2/pokemon?limit=420&offset=0")
    .then((resp) => resp.json())
    .then((resp) => (pokemons.value = resp.results));
});

const pokemonsFiltered = computed(() => {
  if (pokemons.value && searchPokemonField.value) {
    return pokemons.value.filter((pokemon) =>
      pokemon.name.toLowerCase().includes(searchPokemonField.value.toLowerCase())
    );
  }
  return pokemons.value;
});

const selectPokemon = async (pokemon) => {
  loading.value = true;
  await fetch(pokemon.url)
    .then((resp) => resp.json())
    .then((resp) => (pokemonSelected.value = resp))
    .catch((err) => alert(err))
    .finally(() => {
      loading.value = false;
    });
  console.log(pokemonSelected.value);
};
</script>

<template>
  <main>
    <div class="container text-body-secondary">
      <div class="row mt-4">
        <div class="col-sm-12 col-md-6">
          <CardPokemonSelected
            :name="pokemonSelected?.name"
            :xp="pokemonSelected?.base_experience"
            :height="pokemonSelected?.height"
            :img="pokemonSelected?.sprites.other.dream_world.front_default"
            :loading="loading"
          />
        </div>
        <div class="col-sm-12 col-md-6">
          <div class="card card-list">
            <div class="card-body row">
              <div class="mb-3">
                <label hidden for="searchPokemonField" class="form-label"
                  >Pesquisar</label
                >

                <input
                  v-model="searchPokemonField"
                  type="text"
                  class="form-control"
                  id="searchPokemonField"
                  placeholder="Pesquisar..."
                />
              </div>

              <listPokemons
                @click="selectPokemon(pokemon)"
                v-for="pokemon in pokemonsFiltered"
                :key="pokemon.name"
                :name="pokemon.name"
                :urlBaseSvg="urlBaseSvg + pokemon.url.split('/')[6] + '.svg'"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.card-list {
  overflow-y: scroll;
  max-height: 75vh;
  overflow-x: hidden;
}
</style>
