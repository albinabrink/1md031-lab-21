<template>
  <div class="burgers">
  <div class="burger">
    <header>
      <h1> {{ burger.name }} </h1>
    </header>
    <img v-bind:src = "burger.img" class=burgerimage>
    <section>
    <ul class="allergy">
      <li> {{burger.kCal}} kCal </li>
      <li v-if="burger.lactose" id=lactose> Contains Lactose </li>
      <li v-if="burger.gluten" id=gluten> Contains Gluten </li>
    </ul>
    <div class=buttons>
    <button v-on:click=removeBurger> - </button>
    {{amountOrdered}}
    <button v-on:click="addBurger"> + </button>
    </div>
  </section>
  </div>
</div>
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },
  data: function() {
    return {
      amountOrdered: 0,
    }
  },
  methods: {
    addBurger: function () {
      this.amountOrdered += 1;
      this.$emit('orderedBurger', {name: this.burger.name,
                                 amount: this.amountOrdered}
                );
    },
    removeBurger: function () {
      if (this.amountOrdered > 0) {
      this.amountOrdered -= 1;
      this.$emit('orderedBurger', {name: this.burger.name,
                                 amount: this.amountOrdered}
                );
    }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.allergy {
  text-transform: uppercase;
}

.burgerimage{
  max-width: 100%;
  height: 100pt;
}

#lactose, #gluten{
  color: #ff5500;
}


/* .burgers {
  display: grid;
  grid-template-columns: auto auto auto;
} */

.burgers > div {
  background-color: black;
  text-align: center;
  font-size: 20px;
}

.burger {
  overflow: auto;
  background-color: black;
  color: white;
  border: 2px dashed green;
  margin-top: 0pt;
  margin-bottom: 0pt;
}

.buttons{
  margin: 10pt;
  padding: 4pt;
}

button{
  padding: 4pt;
  align: center;
}

ul {
  align: left;
}

</style>
