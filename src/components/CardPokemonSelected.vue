<script setup>
const pokemon = defineProps(["name", "xp", "height", "img", "loading"]);
</script>

<template>
  <div
    class="card CardPokemonSelected"
    :class="loading ? '' : 'animate__animated animate__flipInY'"
  >
    <img
      v-if="pokemon.name"
      :src="pokemon.img"
      height="350"
      class="card-img-top pt-2"
      :alt="pokemon.name"
    />
    <img
      v-else
      src="../assets/egg_pokemon.svg"
      height="350"
      class="card-img-top pt-2"
      alt="?????"
    />
    <div class="card-body">
      <h5 class="card-title text-center">{{ pokemon.name || "???" }}</h5>
      <hr />
      <div class="row">
        <section class="col">
          <strong>XP: </strong>
          <span>{{ pokemon.xp }}</span>
        </section>
        <section class="col">
          <strong>Altura: </strong>
          <span>{{ pokemon.height }}</span>
        </section>
      </div>
    </div>
  </div>
</template>

<style scoped>
.CardPokemonSelected {
  height: 75vh;
  background: rgb(72, 63, 251);
  background: radial-gradient(
    circle,
    rgba(251, 232, 63, 0.8) 0%,
    rgba(252, 70, 85, 0.8) 100%
  );
}
</style>
