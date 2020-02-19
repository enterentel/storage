
<template>
    <div>
      <div>
        <input v-model="newPizza">
        <label for="lstIngredients">Choose Ingredients:</label>
        <select v-model="selected"  multiple="multiple"  size="10">
          <option v-for="(ingredient, index) in ingredients" :key="index" v-bind:value="ingredient.id">
            {{ ingredient.name }}
          </option>
        </select>
        <span>Selected: {{ selected }}</span>
        <button v-on:click="addPizza()">Add Pizza</button>
      </div>

      <div>
        <h2>{{ title }}</h2>

        <div>
          <table>
                <thead>
                    <tr>
                      <th>{{title}}</th>
                      <th>Option</th>
                    </tr>
                </thead>
                <tbody>

                  <tr v-for="(pizzas, index) in pizzas" :key="index">
                        <td>{{pizzas.name}}</td>
                        <td><button v-on:click="deletePizza(pizzas.id)">delete</button></td>
                    </tr>
                </tbody>
          </table>
        </div>
      </div>

    </div>
</template>


<script >


  export default{

    data () {
      return {
         title:'Pizzas',
         pizzas: [],
         pizzaIngredients: [],
         newPizza: null,
         selected: [],
      }
    },
    mounted: function(){
      this.loadPizzaTable();
      this.loadPizzaIngredients();
    },
    props:{
        ingredients : [],
    },
    methods:{
        addPizza: function(){
            if(!this.newPizza){
               return;
            }
            var idPizza= Math.random();
            this.pizzas.push({id:idPizza, name: this.newPizza })
            this.newPizza = null;
            this.savePizzas();
            this.selected = [];
            //save ingredients of pizza
            this.addPizzaIngredients(idPizza);
        },
        deletePizza: function(idPizza){
            for( var i = 0; i <  this.pizzas.length; i++){
              if ( this.pizzas[i].id == idPizza) {
                this.pizzas.splice(i, 1);
              }
            }
            this.savePizzas();
            //delete ingredients of this pizza
            this.deletePizzaIngredients(idPizza);
        },
        savePizzas: function(){
          var  objParsed = JSON.stringify(this.pizzas);
          localStorage.setItem('Pizzas', objParsed);
        },
        addPizzaIngredients: function(idPizza){
            for(var i =0; i< this.selected.length; i++){
                this.pizzaIngredients.push({id:idPizza, name: this.selected[i] });
            }
            //Update state
            this.savePizzaIngredients();
        },
        deletePizzaIngredients: function(idPizza){
          for( var i = 0; i <  this.pizzaIngredients.length; i++){
              if ( this.pizzaIngredients[i].id == idPizza) {
                this.pizzaIngredients.splice(i, 1);
              }
            }
            this.savePizzaIngredients();
        },
        savePizzaIngredients: function(){
          var  objParsed = JSON.stringify(this.pizzaIngredients);
          localStorage.setItem('PizzaIngredients', objParsed);
        },
        loadPizzaTable: function(){
          if(localStorage.getItem('Pizzas')){
            try{
              this.pizzas = JSON.parse(localStorage.getItem('Pizzas'));
            }catch(e){
              localStorage.removeItem('Pizzas');
            }
          }
        },
        loadPizzaIngredients: function(){
          //Pizza Ingredients
          if(localStorage.getItem('PizzaIngredients')){
            try{
              this.pizzaIngredients = JSON.parse(localStorage.getItem('PizzaIngredients'));
            }catch(e){
              localStorage.removeItem('PizzaIngredients');
            }
          }
        }
    },

}
</script>

