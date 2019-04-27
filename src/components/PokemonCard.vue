<template>
  <div class="pokemon-card">
      <div class="background-wrapper">
          <div v-for="type in pokemon.types" :key="type" :style="{'background-color': typeColor[type]}"></div>
      </div>
      <span class="remove-pokemon" @click="remove"><i class="material-icons">close</i></span>
      <div class="image-wrapper">
          <img class="pokemon-image" :src="pokemon.image" alt="bulbasaur">
      </div>
      <div class="pokemon-title">
          {{ pokemon.name }}
      </div>
  </div>
</template>

<script>
export default {
    props: ['pokemon', 'typeColor'],
    template: '#pokemon-card-template', // or we can just put the html here in a string, or even JSX (build step needed)
    methods: {
        remove() {
            this.$emit('remove', this.pokemon);
        },
    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">

</style>
