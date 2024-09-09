<script>
export default {
  props: {
    pokemon: Object
  },
  data() {
    return {
      inputAdivinar: '',
      esCorrecto: false
    }
  },
  computed: {
    getImage() {
      return this.pokemon.sprites.front_default
    }
  },
  methods: {
    checkAdivinanza() {
      if (this.inputAdivinar.toLowerCase() === this.pokemon.name.toLowerCase()) {
        if (!this.esCorrecto) {
          this.esCorrecto = true
          this.$emit('correcto')
        }
      } else {
        alert('Ese no es el nombre del pokemon, ¡Inténtalo de nuevo! :)')
      }
    }
  }
}
</script>

<template>
  <div class="card pokemon-card">
    <div class="pokemon-image">
      <img
        :src="getImage"
        :alt="pokemon.name"
        :style="{ filter: esCorrecto ? 'none' : 'blur(10px) grayscale(100%)' }"
      />
    </div>
    <div v-if="esCorrecto">
      <h3
        :style="{
          fontWeight: 'bold'
        }"
      >
        {{ pokemon.name }}
      </h3>
      <img
        src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTtuLvq_z6fGcatFZVwO9KBw03rgVetQA-p9Q&s"
        alt="pokemon capturado"
        width="50"
      />
    </div>
    <div v-else>
      <input
        type="text"
        v-model="inputAdivinar"
        placeholder="Ingresa un nombre"
        @keyup.enter="checkAdivinanza"
        class="form-control text-center"
      />
      <button @click="checkAdivinanza" class="mt-3 btn btn-danger">Descubrir</button>
    </div>
  </div>
</template>

<style scoped>
.pokemon-card {
  text-align: center;
  padding: 1rem;
  margin: 20px;
  box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
  border: 0;
}
.pokemon-image img {
  width: 150px;
  height: 150px;
}
</style>
