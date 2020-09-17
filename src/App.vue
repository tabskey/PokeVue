<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/sheep.png" width="80">
      <hr>
      <h4 class="is-size-4 has-text-weight-medium">Pokedex</h4>
      <input class="input is-danger is-rounded" type="text" name="" id=""
      placeholder="Catch a new pokemon by their name!" v-model="search">
      <button class="button is-primary is-light" id="searchBtn" @click="searchBy">Catch'em!</button>
      <div v-for="(poke,index) in filteredPoke" :key="poke.url"> <!-- or searchResults -->
       <Pokenent :name="poke.name" :url="poke.url" :num="index + 1"/>
      <!--<h3> {{ index ++}} {{poke.name}}</h3> -->
      </div>
     </div>
  </div>
</template>

<script>
  import axios from "axios";
  import Pokenent from './components/Pokenent'
export default {
  name: 'App',
  data(){
    return {
      pokemons: [],
      search:'',
      filteredPoke: []
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then( res => {
     // console.log(res.data.results);
     this.pokemons = res.data.results;
     this.filteredPoke = res.data.results;
     console.log(this.pokemons);
    })
  },
  components:{
    Pokenent
  },
  methods:{
    searchBy: function(){
      var pokemonMin = this.search.toLowerCase();
      this.filteredPoke = this.pokemons;
      if(this.search =='' || this.search == ' ') {
        this.filteredPoke = this.pokemons;
    }else{
      this.filteredPoke = this.pokemons.filter(pokemon => pokemon.name == pokemonMin)
    }
  }
  // computed:{
  //   searchResults: function() {
  //     var pokemonMin = this.search.toLowerCase();
  //     if(pokemonMin =='' || pokemonMin == ' ') {
  //       return this.pokemons;
  //     }else{
  //       return this.pokemons.filter(pokemon => pokemon.name == pokemonMin);
  //     }
  //   }
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
#searchBtn {
  margin-top: 2%;
}
</style>
