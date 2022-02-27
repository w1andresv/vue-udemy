<template>
  <h2 v-if="!pokemon">Espere por favor</h2>
  <div v-else>
    <h2>Quién es este pokemon?</h2> Asiertos : {{ counter }}
    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon"></PokemonPicture>
    <PokemonOptions :pokemons="pokemons" @selection="checkAnswer"></PokemonOptions>
  </div>

</template>

<script>
import PokemonPicture from "@/components/PokemonPicture";
import PokemonOptions from "@/components/PokemonOptions";
import getPokemonsOptions from "@/api/getPokemonOptions";

export default {
  name: "PokemonPage",
  data() {
    return {
      pokemons: [],
      pokemon: null,
      showPokemon: false,
      counter: 0
    }
  },
  components: {
    PokemonPicture,
    PokemonOptions
  },
  methods: {
    async getPokemons() {
      this.pokemons = await getPokemonsOptions();
      const rnd = Math.floor(Math.random() * 4);
      this.showPokemon = false;
      this.pokemon = this.pokemons[rnd];
    },
    checkAnswer(value) {
      this.showPokemon = this.pokemon.id === value;
      if (this.showPokemon) {
        this.counter++
        setTimeout(() => {
          this.getPokemons();
        }, 500)
      } else {
        this.counter = 0
      }
    }
  },
  mounted() {
    this.getPokemons();
  }
}
</script>

<style scoped>

</style>
