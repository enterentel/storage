<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>


    <!-- ingredients-->
     <div>
        <ingredients v-on:UpdateCollection="this.updateCollectionToChild"></ingredients>
     </div>

      <div>
        <pizzas :ingredients=ingredients></pizzas>
     </div>
  </div>
</template>

<script>
  import ingredients from "./ingredients.vue";
  import pizzas from "./pizzas.vue";

  export default {
    name: 'app',
    data () {
      return {
        msg: 'Welcome!!!',
        ingredients : []
      }
    },
    created:function(){
      this.loadIngredients();
    },
    mounted: function(){
      this.loadIngredients();
    },
    components:{
        ingredients: ingredients,
        pizzas:pizzas
    },
    methods: {
      updateCollectionToChild: function() {
        this.loadIngredients();
      },
      loadIngredients: function(){
          //Ingredients
          if(localStorage.getItem('ingredients')){
            try{
              this.ingredients = JSON.parse(localStorage.getItem('ingredients'));
            }catch(e){
              localStorage.removeItem('ingredients');
            }
          }
        },
    }

  }
</script>


<style>
  .example-slide {
    align-items: center;
    background-color: #666;
    color: #999;
    display: flex;
    font-size: 1.5rem;
    justify-content: center;
    min-height: 10rem;
  }
</style>
