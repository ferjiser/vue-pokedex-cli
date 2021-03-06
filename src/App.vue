<template>
  <div>
    <header class="main-header">
        Vue - Pokedex
    </header>
    <div class="main-content" id="app" v-cloak>
        <!-- Pokemon Filters -->
        <aside class="pokedex-filters">
            <NiceInput 
                v-model="searchText" 
                placeholder="my custom placeholder" 
                type="text"
            />
            <!-- Checkboxes -->
            <p class="checkboxes-list-title">Type</p>
            <div class="checkboxes-list">
                <!-- nice-checkbox -->
                <div class="md-checkbox" v-for="(color, type) in TYPE_COLOR" :key="type">
                    <input :id="'checkbox-'+type" type="checkbox" :value="type" v-model="selectedPokemonTypes">
                    <label :for="'checkbox-'+type">
                        <i class="material-icons" :style="{color: color}">lens</i>
                        <span class="label-title">{{ type }}</span>
                    </label>
                </div>
            </div>
            <button @click="random">REORDENAR</button>
        </aside>

        <!-- Pokemons list -->
        <main class="main-view">
            <transition-group tag="article" name="slide" class="pokemons-list">
                <!-- Pokemon Card -->
                <pokemon-card v-for="pokemon in filteredPokemons"
                    :key="pokemon.name"
                    :pokemon="pokemon"
                    @remove="removePokemon"/>
            </transition-group>
        </main>
    </div>
  </div>  
</template>

<script>
import {TYPE_COLOR} from './constants'
import NiceInput from './components/NiceInput'
import PokemonCard from './components/PokemonCard'

export default {
    data() {
        return {
            searchText: '',
            TYPE_COLOR,
            selectedPokemonTypes: [],
            pokemons: [],
        };
    },
    computed: {
        filteredPokemons() {
            return this.pokemons
                .filter(pokemon => pokemon.name.includes(this.searchText)) // filter by pokemon name (searchBox)
                .filter(pokemon => { // filter by pokemon type (checkboxes)
                    if (this.selectedPokemonTypes.length === 0) return true; // no checked checkbox means no filter
                    return pokemon.types
                        .find(type => this.selectedPokemonTypes.includes(type));
                });
        },
    },
    methods: {
        removePokemon(pokemonToDelete) {
            this.pokemons = this.pokemons
                .filter(pokemon => pokemon !== pokemonToDelete);
        },
        random(){
          this.pokemons.sort(()=> Math.random()-0.5) 
      }
    },
    created() { // Component Lyfecycle Diagram https://vuejs.org/v2/guide/instance.html#Lifecycle-Diagram
        fetch('https://api.jsonbin.io/b/5ab37f77989617146bd6eb29') 
            .then(response => response.json())
            .then(pokemons => this.pokemons = pokemons);
    },
    components: {
        PokemonCard,
        NiceInput
    },
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Roboto:400,700');
@import url('https://fonts.googleapis.com/icon?family=Material+Icons');

*,
*:before,
*:after {
  box-sizing: border-box;
}

:focus {
    outline: none;
}

.material-icons {
    font-size: 20px;
}

body {
    font-family: 'Roboto', sans-serif;
    display: flex;
    min-height: 100vh;
    flex-direction: column;
    margin: 0;
}

header, article, nav, aside {
    padding: 1em;
}

.main-header {
    text-transform: uppercase;
    background: #42b883;
    color: white;
    font-size: 35px;
    padding: 0.5em;
    font-weight: 400;
}

.main-content {
    display: flex;
    flex: 1;
}

.main-view {
    overflow-y: auto;
}

.pokedex-filters {
    flex: 0 0 250px;
    display: flex;
    flex-direction: column;
    box-shadow: 0 2px 2px 0 rgba(0,0,0,0.14),
        0 1px 5px 0 rgba(0,0,0,0.12),
        0 3px 1px -2px rgba(0,0,0,0.2);
}

.checkboxes-list-title {
    margin-bottom: 0px;
}

.checkboxes-list {
    overflow-y: auto;
}

.pokemons-list {
    flex: 1;
    display: flex;
    flex-wrap: wrap;
    overflow-y: auto;
}

/* Nice checkbox */
.md-checkbox {
  position: relative;
  display: flex;
  align-items: center;
  margin: 16px 0;
  text-align: left;
}
.md-checkbox.md-checkbox-inline {
  display: inline-block;
}
.md-checkbox label {
  display: flex;
  align-items: center;
  cursor: pointer;
  text-transform: capitalize;
}

.md-checkbox label .label-title {
    margin: 0px 5px;
}

.md-checkbox label:before, .md-checkbox label:after {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
}
.md-checkbox label:before {
  width: 20px;
  height: 20px;
  background: #fff;
  border: 2px solid rgba(0, 0, 0, 0.54);
  border-radius: 2px;
  cursor: pointer;
  transition: background 0.3s;
}
.md-checkbox input[type="checkbox"] {
  outline: 0;
  margin-right: 10px;
  visibility: hidden;
}
.md-checkbox input[type="checkbox"]:checked + label:before {
  background: #4caf50;
  border: none;
}
.md-checkbox input[type="checkbox"]:checked + label:after {
  transform: rotate(-45deg);
  top: 5px;
  left: 4px;
  width: 12px;
  height: 6px;
  border: 2px solid #fff;
  border-top-style: none;
  border-right-style: none;
}
.md-checkbox input[type="checkbox"]:disabled + label:before {
  border-color: rgba(0, 0, 0, 0.26);
}
.md-checkbox input[type="checkbox"]:disabled:checked + label:before {
  background: rgba(0, 0, 0, 0.26);
}
/* End Nice checkbox */
.slide-enter{
    transform: translateX(500px);
}
.slide-enter-active{
    transition: all 1s
}
.slide-leave-to{
    transform: translateX(-5000px);
}
.slide-leave-active{
    transition: all 0.5s
}
.slide-move {   
    transition: all 1s
}
</style>
