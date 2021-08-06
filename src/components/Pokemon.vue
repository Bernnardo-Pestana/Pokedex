<template>


    <div class="card" id ="pokemon" >
  <div class="card-image">
    <figure>
      <img :src="this.currentImg" alt="Placeholder image">
    </figure>
  </div>
  <div class="card-content">
    <div class="media">
    
      <div class="media-content">
        <p class="title is-4">{{id}} {{name | Upper }}</p>
        <p class="subtitle is-6"> {{this.pokemon.type}} </p>
      </div>
    </div>

    <div class="content">
      <button class="button is-small" @click="mudaImg">Muda-Imagem</button>
    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios';
export default {

    //props: item que permite ser alterado dinamicamente pelo html
    props:{
        id : Number,
        name : String,
        url : String
    },
    //filters : um filtro onde vc pode definir o que sera filtrado e suas funções
    filters:{
        Upper : function( value )
        {
            var newName = value[0].toUpperCase() + value.slice(1);
            return newName;
        }

    },

    // created: função que permite a utilização do axios para fazer requisiçoes a uma api externa/interna. Nesse caso está sendo utilizada uma api de pokemon
    created: function ()
    {
        axios.get(this.url).then(data =>{
            this.pokemon.type = data.data.types[0].type.name;
            this.pokemon.front = data.data.sprites.front_default;
            this.pokemon.back = data.data.sprites.back_default;

            this.currentImg = this.pokemon.front
        }).catch(err =>
        {
            console.log(err);
        })
    },

    data(){
        return{

            isFront : true,
            currentImg: '',

            pokemon : {
                type : '',
                front: '',
                end: ''
            }
        }
    },

    methods: {

        mudaImg : function() {

            console.log("Mudar imagem")
        if(this.front)
        {

            this.isFront = false;
            this.currentImg = this.pokemon.back
        }else{
             this.isFront = true;
             this.currentImg = this.pokemon.front
        }

     }
    }
}
</script>

<style>
#pokemon{
  
  margin-top: 10px;
  float: left


}

</style>