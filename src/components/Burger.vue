<template>
  <div class="burgers">
  <div>
    {{ burger.name }} {{ burger.kCal }}
  </div>
  <div class="burger">
    <header>
      <h1> {{ burger.name }} </h1>
    </header>
    <img v-bind:src = "burger.url" class=burgerimage>
    <section>
    <li v-for= "t in burger" v-bind:key="t.key">
      {{t}}
    </li>

  <!--    <ul>
        <section class="allergy">
          <li><span id="gluten">Bröd</span></li>
        </section>
        <li> Nötkött </li>
        <section class="allergy">
          <li><span id="laktos">Cheddarost</span></li>
        </section>
        <li> Majonäs </li>
        <li> Senap </li>
        <li> Saltgurka </li>
        </ul> -->
      </section>
      </div>
    </div>

</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.allergy {
  text-transform: uppercase;
}

.burgerimage{
  max-width: 35%;
  height: 250px;
}

#laktos, #gluten{
  color: #ff5500;
}


/* .burgers {
  display: grid;
  grid-template-columns: auto auto auto;
} */

.burgers > div {
  background-color: black;
  text-align: center;
  padding: 20px 0;
  font-size: 30px;
}

.burger {
  overflow: auto;
  background-color: black;
  color: white;
  margin: 10pt;
  border: 2px dashed green;
  margin-top: 0pt;
  margin-bottom: 0pt;
}
</style>
