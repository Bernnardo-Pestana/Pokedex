<template>
  <div id="app">
    <div class="column is-one-third is-offset-one-fifth">
      <h3>Bem vindo a Pokedex </h3>
      <input  class="input is-rounded" type="text"  v-model="busca">
      <button class="button is-dark button is-fullwidth is-rounded " @click="buscar">Buscar</button>

      <div v-for="(poke,index) in filteredpokemons" :key="poke.url"> 
      <Pokemon :name="poke.name" :url="poke.url" :id="index + 1" />
      </div>  
    </div>
  </div>
</template>

<script>

//
import axios from 'axios';
import Pokemon from "./components/Pokemon.vue"

export default {
  name: 'App',
  
  data(){
    return{
      pokemons : [],
      filteredpokemons : [],
      busca : ''

    }

  },

  created : function()
  {

    console.log("created funciona....")
    
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then( data=>{

      console.log("Pegando lista de pokemons");

      this.pokemons = data.data.results
      this.filteredpokemons = data.data.results


    }).catch(Err =>{
      console.log(Err)
    })

  },
    methods: {

    buscar: function()
    {
      this.filteredpokemons = this.pokemons
      if(this.busca == '' || this.busca == " ")
      {
        this.filteredpokemons = this.pokemons

        this.busca = ""
      }else
      {
        this.filteredpokemons =  this.pokemons.filter(pokemon => pokemon.name == this.busca)
        this.busca = ""
      }


    }

  },

  components: {

    Pokemon

  }


}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>

