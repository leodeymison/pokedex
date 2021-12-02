<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <div id="title">
        <h3 class="is-size-4">Pokedex</h3>
        <input type="text" class="input is-hovered" placeholder="Buscar pokemon" v-model="busca">
        <button @click="buscar" class="button is-primary">Buscar</button>
      </div>
      
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon 
          :name="poke.name" 
          :url="poke.url" 
          :num="index + 1" />
      </div>
    </div>
    
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon.vue";
export default {
  name: 'App',
  
  data(){
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=100&offset=200").then(response => {
      this.pokemons = response.data.results
      this.filteredPokemons = response.data.results
    }).catch(error => {
       console.log(error)
    })
  },
  components:{
    Pokemon
  },
  
  methods: {
    buscar: function(){
      this.filteredPokemons = this.pokemons
      if(this.busca == '' || this.busca == ' '){
        this.filteredPokemons = this.pokemons
      }
      else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    }
  },
  computed: {
    // resultadoBusca: function(){
    //   if(this.busca == '' || this.busca == ' '){
    //     return this.pokemons
    //   }
    //   else{
    //     return this.pokemons.filter(pokemon => pokemon.name == this.busca)
    //   }
    // }
  }
}
</script>

<style>
#title{
  display: flex;
  justify-content: center;
  padding: 60px 0px 30px 0px;
}
#title input{
  margin-left: 10px;
}
#title button{
  margin-left: 4px;
}
</style>
