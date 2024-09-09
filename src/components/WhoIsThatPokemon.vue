<script>
import axios from 'axios'
import PokemonCard from '@/components/PokemonCard.vue'

export default {
  components: {
    PokemonCard
  },
  data() {
    return {
      pokemons: [],
      pokemonsDescubiertos: 0
    }
  },
  async created() {
    const pokePromesa = []
    for (let i = 1; i <= 20; i++) {
      pokePromesa.push(axios.get(`https://pokeapi.co/api/v2/pokemon/${i}`))
    }
    try {
      const responses = await Promise.all(pokePromesa)
      this.pokemons = responses.map((response) => response.data)
    } catch (error) {
      console.error('No pudimos atrapar ningún pokemon 😞:', error)
    }
  },
  methods: {
    contadorPokemon() {
      this.pokemonsDescubiertos++
      if (this.pokemonsDescubiertos === this.pokemons.length) {
        alert('¡Felicidades! Has descubierto a todos los pokemones 🎉')
      }
    }
  }
}
</script>

<template>
  <div class="text-center">
    <img
      src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRn74k9jrdLZiWw0dCfb06gfj7SzsJbSBR0cQ&s"
      alt="logo pokemon"
    />
    <h1 class="mt-2">¿Quién es ese Pokémon?</h1>
    <div class="d-flex justify-content-center align-items-center">
      <img
        src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTtuLvq_z6fGcatFZVwO9KBw03rgVetQA-p9Q&s"
        alt="pokemon capturado"
        class="object-fit-cover me-2 pokebola"
      />

      <h2 class="mt-2">
        Pokemones descubiertos:
        <span
          :style="{
            color: 'red'
          }"
          >{{ pokemonsDescubiertos }}</span
        >
      </h2>
    </div>
    <div class="pokemon-container">
      <PokemonCard
        v-for="pokemon in pokemons"
        :key="pokemon.id"
        :pokemon="pokemon"
        @correcto="contadorPokemon"
      />
    </div>
  </div>
</template>

<style scoped>
.pokemon-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
  padding: 20px;
  justify-content: center;
}

.pokebola {
  width: 30px;
  height: 30px;
}
</style>
