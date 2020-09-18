<template>
<div id="pokemon">
 <div class="card">
  <div class="card-image">
    <figure>
      <img :src="currentSprite" alt="Placeholder image">
    </figure>
  </div>
  <div class="card-content">
    <div class="media">
      <div class="media-content">
        <p class="title is-4">{{num}} {{name | upperCase}}</p>
        <div class="tags column is-half is-offset-one-quarter">
            <span class="tag">{{ pokemon.types | upperCase }}</span>
            <span class="tag" v-if="pokemon.secType">{{ pokemon.secType | upperCase}} </span>
        </div>  
      </div>

    </div>

    <div class="content">
        <button class="button is-danger is-rounded" @click="changeSprite">Change Sprite</button>
      
    </div>
  </div>
</div>
</div>
</template>

<script>
import axios from "axios";
export default {
    created: function(){
        axios.get(this.url).then(res => {
            
            this.pokemon.types = res.data.types[0].type.name;
            
            this.pokemon.front = res.data.sprites.front_default;
            this.pokemon.back = res.data.sprites.back_default;
            this.currentSprite =  this.pokemon.front;
            
         }) ,
            axios.get(this.url).then(res => {
            this.pokemon.secType = res.data.types[1].type.name;
         }).catch(function() {
            this.pokemon.secType = false; 
         })
    },
    data(){
      return {
          isFront: true,
          currentSprite: '',
          pokemon:{
              type: '',
              secType: '',
              front: '',
              back: ''
          }
      }  
    },
    props:{
        num: Number,
        name : String,
        url : String
    },
    filters:{
        upperCase: function(value) {
            var nameUpper = value[0].toUpperCase() + value.slice(1); 
            return nameUpper;
        }
    },
    methods: {
        changeSprite: function() {
            if(this.isFront) {
                this.isFront = false;
                this.currentSprite = this.pokemon.back
            }else {
                this.isFront = true;
                this.currentSprite = this.pokemon.front;
            }
        }
    }
}
</script>

<style>
#pokemon {
    margin-top: 2%;
}
</style>